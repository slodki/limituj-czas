find_package(Gettext REQUIRED)

add_custom_command(OUTPUT ${PROJECT_NAME}.pot
  COMMAND bash --dump-po-strings "${PROJECT_SOURCE_DIR}/${PROJECT_NAME}" >${PROJECT_NAME}.pot
  COMMENT "Generating .pot file"
  MAIN_DEPENDENCY "${PROJECT_SOURCE_DIR}/${PROJECT_NAME}")

file(GLOB POFiles RELATIVE "${CMAKE_CURRENT_BINARY_DIR}" *.po)
foreach(POFile ${POFiles})
  get_filename_component(CurrentFile "${POFile}" NAME_WE)
  add_custom_command(OUTPUT ${CurrentFile}.mo
    COMMAND ${GETTEXT_MSGMERGE_EXECUTABLE} --quiet --update --force-po --lang=${CurrentFile} --backup=none -s "${POFile}" ${PROJECT_NAME}.pot
    COMMAND ${GETTEXT_MSGFMT_EXECUTABLE} --check-format --check-domain -o ${CurrentFile}.mo "${POFile}"
    COMMENT "Generating ${CurrentFile} translation"
    MAIN_DEPENDENCY "${POFile}"
    DEPENDS ${PROJECT_NAME}.pot)

  install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${CurrentFile}.mo"
    DESTINATION share/locale/${CurrentFile}/LC_MESSAGES
    RENAME ${PROJECT_NAME}.mo)

  list(APPEND MOFiles "${CMAKE_CURRENT_BINARY_DIR}/${CurrentFile}.mo")
endforeach()

add_custom_target(Translations ALL DEPENDS ${MOFiles}
    COMMENT "Generated translations")
