> git remote add origin https://github.com/${GITHUB_USERNAME}/lab06
> gsed -i '/project(print)/a\
set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")
' CMakeLists.txt
> gsed -i '/project(print)/a\
set(PRINT_VERSION\
  \${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
> gsed -i '/project(print)/a\
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
> gsed -i '/project(print)/a\
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
> gsed -i '/project(print)/a\
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
> gsed -i '/project(print)/a\
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
> git diff


Результат git diff:
diff --git a/CMakeLists.txt b/CMakeLists.txt
index 0bd75e9..9b283ad 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -8,6 +8,13 @@ option(BUILD_TESTS "Build tests" OFF)
 option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
+set(PRINT_VERSION_MAJOR 0)
+set(PRINT_VERSION_MINOR 1)
+set(PRINT_VERSION_PATCH 0)
+set(PRINT_VERSION_TWEAK 0)
+set(PRINT_VERSION
+  ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
+set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
 
 add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

   1.gsed -i '/project(print)/a\ set(PRINT_VERSION_MAJOR 0)'
        ◦ Добавляет строку set(PRINT_VERSION_MAJOR 0) после project(print).
        ◦ Устанавливает основную версию (major) в 0.
    2. gsed -i '/project(print)/a\ set(PRINT_VERSION_MINOR 1)'
        ◦ Добавляет строку set(PRINT_VERSION_MINOR 1) после project(print).
        ◦ Устанавливает дополнительную версию (minor) в 1.
    3. gsed -i '/project(print)/a\ set(PRINT_VERSION_PATCH 0)'
        ◦ Добавляет строку set(PRINT_VERSION_PATCH 0) после project(print).
        ◦ Устанавливает версию патча (patch) в 0.
    4. gsed -i '/project(print)/a\ set(PRINT_VERSION_TWEAK 0)'
        ◦ Добавляет строку set(PRINT_VERSION_TWEAK 0) после project(print).
        ◦ Устанавливает дополнительную корректировку (tweak) в 0.
    5. gsed -i '/project(print)/a\ set(PRINT_VERSION ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})'
        ◦ Собирает полную версию из отдельных компонентов в формате MAJOR.MINOR.PATCH.TWEAK (например, 0.1.0.0).
    6. gsed -i '/project(print)/a\ set(PRINT_VERSION_STRING "v${PRINT_VERSION}")'
        ◦ Создает строку версии с префиксом v (например, v0.1.0.0).


 
> touch DESCRIPTION && edit DESCRIPTION
zsh: command not found: edit
> touch DESCRIPTION && nano DESCRIPTION
> touch ChangeLog.md
> export DATE="`LANG=en_US date +'%a %b %d %Y'`"
> cat > ChangeLog.md <<EOF
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF

Создаём файл DESCRIPTION и зыписываем в него изменения. 
Создаём файл для записи изменений и переводим раскладку 





> cat > CPackConfig.cmake <<EOF
include(InstallRequiredSystemLibraries)
EOF

Это CMake-команда, которая подключает стандартный модуль InstallRequiredSystemLibraries. Этот модуль автоматически проверяет, какие системные библиотеки нужны для работы программы, и включает их в пакет 






> cat >> CPackConfig.cmake <<EOF
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")
EOF

Эта команда дописывает в конец файла CPackConfig.cmake настройки для системы упаковки CPack, используя ранее определённые переменные версии (PRINT_VERSION_*) и другие параметры.
> cat >> CPackConfig.cmake <<EOF

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF

Эта команда дописывает в конец файла CPackConfig.cmake две важные настройки, которые связывают файлы лицензии и README с генерируемым пакетом.









cat >> CPackConfig.cmake <<EOF

set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
Эта команда добавляет в конец файла CPackConfig.cmake специфичные для RPM настройки упаковки.






cat >> CPackConfig.cmake <<EOF

set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
Устанавливает имя пакета в формате lib<имя>-dev
Указывает релиз пакета

> cat >> CPackConfig.cmake <<EOF

include(CPack)
EOF

Эта команда добавляет ключевую директиву include(CPack) в конец файла CPackConfig.cmake, активируя систему сборки пакетов Cpack.






cat >> CMakeLists.txt <<EOF

include(CPackConfig.cmake)
EOF

Эта команда добавляет строку include(CPackConfig.cmake) в конец файла CMakeLists.txt, подключая конфигурационный файл CPack к основному процессу сборки.


> gsed -i 's/lab05/lab06/g' README.md
> git add .
> git commit -m"added cpack config"
[main b7f3d8e] added cpack config
 5 files changed, 82 insertions(+), 28 deletions(-)
 create mode 100644 CPackConfig.cmake
 create mode 100644 ChangeLog.md
 create mode 100644 DESCRIPTION
> git tag v0.1.0.0
> git push origin master --tags
error: src refspec master ничему не соответствует
error: не удалось отправить некоторые ссылки в «https://github.com/matveech99/lab06»
> git push origin main --tags
Перечисление объектов: 183, готово.
Подсчет объектов: 100% (183/183), готово.
При сжатии изменений используется до 16 потоков
Сжатие объектов: 100% (106/106), готово.
Запись объектов: 100% (183/183), 1.24 МиБ | 2.41 МиБ/с, готово.
Всего 183 (изменений 59), повторно использовано 173 (изменений 56), повторно использовано пакетов 0
remote: Resolving deltas: 100% (59/59), done.
To https://github.com/matveech99/lab06
 * [new tag]         v0.1.0.0 -> v0.1.0.0
 ! [rejected]        main -> main (fetch first)
error: не удалось отправить некоторые ссылки в «https://github.com/matveech99/lab06»
подсказка: Updates were rejected because the remote contains work that you do not
подсказка: have locally. This is usually caused by another repository pushing to
подсказка: the same ref. If you want to integrate the remote changes, use
подсказка: 'git pull' before pushing again.
подсказка: See the 'Note about fast-forwards' in 'git push --help' for details.
> git push --force origin main --tags
Всего 0 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
To https://github.com/matveech99/lab06
 + 191ff30...b7f3d8e main -> main (forced update)
> cmake -H. -B_build
CMake Error: The current CMakeCache.txt directory /home/matvey/matveech99/workspace/projects/lab06/_build/CMakeCache.txt is different than the directory /home/matvey/matveech99/workspace/projects/lab05/_build where CMakeCache.txt was created. This may result in binaries being created in the wrong place. If you are not sure, reedit the CMakeCache.txt
CMake Error: The source "/home/matvey/matveech99/workspace/projects/lab06/CMakeLists.txt" does not match the source "/home/matvey/matveech99/workspace/projects/lab05/CMakeLists.txt" used to generate cache.  Re-run cmake with a different source directory.
> 
> cmake -H. -B_build
CMake Error: The current CMakeCache.txt directory /home/matvey/matveech99/workspace/projects/lab06/_build/CMakeCache.txt is different than the directory /home/matvey/matveech99/workspace/projects/lab05/_build where CMakeCache.txt was created. This may result in binaries being created in the wrong place. If you are not sure, reedit the CMakeCache.txt
CMake Error: The source "/home/matvey/matveech99/workspace/projects/lab06/CMakeLists.txt" does not match the source "/home/matvey/matveech99/workspace/projects/lab05/CMakeLists.txt" used to generate cache.  Re-run cmake with a different source directory.
> 
> rm -rf _build
> cmake -H. -B_build
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- The C compiler identification is GNU 13.3.0
-- The CXX compiler identification is GNU 13.3.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Error at /usr/share/cmake-3.28/Modules/CPack.cmake:685 (message):
  CPack license resource file:
  "/home/matvey/matveech99/workspace/projects/lab06/LICENSE" could not be
  found.
Call Stack (most recent call first):
  /usr/share/cmake-3.28/Modules/CPack.cmake:690 (cpack_check_file_exists)
  CPackConfig.cmake:24 (include)
  CMakeLists.txt:64 (include)


-- Configuring incomplete, errors occurred!
> cmake --build _build
gmake: Makefile: Нет такого файла или каталога
gmake: *** Нет правила для сборки цели «Makefile».  Останов.
> touch LICENSE
> cmake -H. -B_build
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/matvey/matveech99/workspace/projects/lab06/_build
> cmake --build _build
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
> cd _build
> cpack -G "TGZ"
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/matvey/matveech99/workspace/projects/lab06/_build/print-0.1.0.0-Linux.tar.gz generated.
> cd ..
> cmake -H. -B_build -DCPACK_GENERATOR="TGZ"

CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/matvey/matveech99/workspace/projects/lab06/_build
> cmake --build _build --target package
[100%] Built target print
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/matvey/matveech99/workspace/projects/lab06/_build/print-0.1.0.0-Linux.tar.gz generated.
> mkdir artifacts
> mv _build/*.tar.gz artifacts
> tree artifacts
artifacts
└── print-0.1.0.0-Linux.tar.gz

1 directory, 1 file
~/m/workspace/p/lab06 main !107 ?7 >         
