# lab05
> alias gsed=sed
> cd ${GITHUB_USERNAME}/workspace
> pushd . # Сохраняет текущую директорию в стеке директорий
~/matveech99/workspace ~
> source scripts/activate
~/matveech99/workspace




> git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05
Клонирование в «projects/lab05»...
remote: Enumerating objects: 35, done.
remote: Counting objects: 100% (35/35), done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 35 (delta 8), reused 28 (delta 7), pack-reused 0 (from 0)
Получение объектов: 100% (35/35), 16.39 КиБ | 381.00 КиБ/с, готово.
Определение изменений: 100% (8/8), готово.
> cd projects/lab05
> git remote remove origin
> git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
~/m/w/p/lab05 main >                                                     rb 3.2.2 13:31:32

Клонируем репозиторий из 4 лабораторной в локальную папку, а затем переходим в нее. 
Затем удаляем привязку к репозиторию лабы 4, чтобы привязать к 5-ой.



























> mkdir third-party
> git submodule add https://github.com/google/googletest third-party/gtest
Клонирование в «/home/matvey/matveech99/workspace/projects/lab05/third-party/gtest»...
remote: Enumerating objects: 28002, done.
remote: Counting objects: 100% (243/243), done.
remote: Compressing objects: 100% (154/154), done.
remote: Total 28002 (delta 153), reused 91 (delta 88), pack-reused 27759 (from 4)
Получение объектов: 100% (28002/28002), 13.52 МиБ | 4.30 МиБ/с, готово.
Определение изменений: 100% (20745/20745), готово.
> cd third-party/gtest && git checkout release-1.8.1 && cd ../..
Примечание: переключение на «release-1.8.1».

Вы сейчас в состоянии «отсоединённого указателя HEAD». Можете осмотреться,
внести экспериментальные изменения и зафиксировать их, также можете
отменить любые коммиты, созданные в этом состоянии, не затрагивая другие
ветки, переключившись обратно на любую ветку.

Если хотите создать новую ветку для сохранения созданных коммитов, можете
сделать это (сейчас или позже), используя команду switch с параметром -c.
Например:> mkdir third-party
> git submodule add https://github.com/google/googletest third-party/gtest
Клонирование в «/home/matvey/matveech99/workspace/projects/lab05/third-party/gtest»...
remote: Enumerating objects: 28002, done.
remote: Counting objects: 100% (243/243), done.
remote: Compressing objects: 100% (154/154), done.
remote: Total 28002 (delta 153), reused 91 (delta 88), pack-reused 27759 (from 4)
Получение объектов: 100% (28002/28002), 13.52 МиБ | 4.30 МиБ/с, готово.
Определение изменений: 100% (20745/20745), готово.
> cd third-party/gtest && git checkout release-1.8.1 && cd ../..
Примечание: переключение на «release-1.8.1».

Вы сейчас в состоянии «отсоединённого указателя HEAD». Можете осмотреться,
внести экспериментальные изменения и зафиксировать их, также можете
отменить любые коммиты, созданные в этом состоянии, не затрагивая другие
ветки, переключившись обратно на любую ветку.

Если хотите создать новую ветку для сохранения созданных коммитов, можете
сделать это (сейчас или позже), используя команду switch с параметром -c.
Например:

  git switch -c <новая-ветка>

Или отмените эту операцию с помощью:

  git switch -

Отключите этот совет, установив переменную конфигурации
advice.detachedHead в значение false

HEAD сейчас на 2fe3bd99 Merge pull request #1433 from dsacre/fix-clang-warnings
> 
> git add third-party/gtest
> git commit -m"added gtest framework"
[main 4e28427] added gtest framework
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest

  git switch -c <новая-ветка>

Или отмените эту операцию с помощью:

  git switch -

Отключите этот совет, установив переменную конфигурации
advice.detachedHead в значение false

HEAD сейчас на 2fe3bd99 Merge pull request #1433 from dsacre/fix-clang-warnings
> 
> git add third-party/gtest
> git commit -m"added gtest framework"
[main 4e28427] added gtest framework
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest

Создаём директорию third-party для хранения gtest.
Переходим в директорию подмодуля (gtest).
Фиксируем ссылку на конкретный коммит подмодуля (в данном случае — release-1.8.1) в индексе Git.
Затем создаём коммит с сообщением «added gtest framework»


> gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
sed: невозможно прочитать CMakeLists.txt: Нет такого файла или каталога
> git status
Текущая ветка: main
нечего коммитить, нет изменений в рабочем каталоге
> git add CMakeLists.txt
> git commit -m"Initial commit"
[main 411f59c] Initial commit
 1 file changed, 36 insertions(+)
 create mode 100644 CMakeLists.txt
> gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
> cat >> CMakeLists.txt <<EOF

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
EOF
~/m/w/p/lab05 main !1 >     

Добавляем BUILD_TESTS в CMakeLists.txt
СmakeLists.txt нужно добавить вручную, так как его нет в коммите 4 лабы. 
> mkdir tests
> cat > tests/test1.cpp <<EOF
#include <print.hpp>

#include <gtest/gtest.h>

TEST(Print, InFileStream)
{
  std::string filepath = "file.txt";
  std::string text = "hello";
  std::ofstream out{filepath};

  print(text, out);
  out.close();

  std::string result;
  std::ifstream in{filepath};
  in >> result;

  EXPECT_EQ(result, text);
}
EOF
~/m/w/p/lab05 main !1 ?1 >  

Создаём тестовый файл test1.cpp для проверки функции print с использованием Google Test. 





> cmake -B _build -DBUILD_TESTS=ON
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
CMake Deprecation Warning at third-party/gtest/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at third-party/gtest/googlemock/CMakeLists.txt:42 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at third-party/gtest/googletest/CMakeLists.txt:49 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Warning (dev) at third-party/gtest/googletest/cmake/internal_utils.cmake:239 (find_package):
  Policy CMP0148 is not set: The FindPythonInterp and FindPythonLibs modules
  are removed.  Run "cmake --help-policy CMP0148" for policy details.  Use
  the cmake_policy command to set the policy and suppress this warning.

Call Stack (most recent call first):
  third-party/gtest/googletest/CMakeLists.txt:84 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Found PythonInterp: /usr/bin/python3 (found version "3.12.3") 
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE  
-- Configuring done (0.4s)
-- Generating done (0.0s)
-- Build files have been written to: /home/matvey/matveech99/workspace/projects/lab05/_build
> cmake --build _build
[  8%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 16%] Linking CXX static library libprint.a
[ 16%] Built target print
[ 25%] Building CXX object third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
In file included from /home/matvey/matveech99/workspace/projects/lab05/third-party/gtest/googletest/src/gtest-all.cc:42:
/home/matvey/matveech99/workspace/projects/lab05/third-party/gtest/googletest/src/gtest-death-test.cc: In function ‘bool testing::internal::StackGrowsDown()’:
/home/matvey/matveech99/workspace/projects/lab05/third-party/gtest/googletest/src/gtest-death-test.cc:1224:24: warning: ‘dummy’ may be used uninitialized [-Wmaybe-uninitialized]
 1224 |   StackLowerThanAddress(&dummy, &result);
      |   ~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~
/home/matvey/matveech99/workspace/projects/lab05/third-party/gtest/googletest/src/gtest-death-test.cc:1214:13: note: by argument 1 of type ‘const void*’ to ‘void testing::internal::StackLowerThanAddress(const void*, bool*)’ declared here
 1214 | static void StackLowerThanAddress(const void* ptr, bool* result) {
      |             ^~~~~~~~~~~~~~~~~~~~~
/home/matvey/matveech99/workspace/projects/lab05/third-party/gtest/googletest/src/gtest-death-test.cc:1222:7: note: ‘dummy’ declared here
 1222 |   int dummy;
      |       ^~~~~
[ 33%] Linking CXX static library libgtest.a
[ 33%] Built target gtest
[ 41%] Building CXX object third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Linking CXX static library libgtest_main.a
[ 50%] Built target gtest_main
[ 58%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[ 66%] Linking CXX executable check
[ 66%] Built target check
[ 75%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 83%] Linking CXX static library libgmock.a
[ 83%] Built target gmock
[ 91%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library libgmock_main.a
[100%] Built target gmock_main
cmake -H. -B_build -DBUILD_TESTS=ON - Конфигурирует проект с помощью Cmake.
cmake --build _build - Запускает сборку проекта в директории _build.
cmake --build _build --target test - Запускает цель test, которая выполняет все зарегистрированные тесты через Ctest.













> _build/check
Running main() from /home/matvey/matveech99/workspace/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from Print
[ RUN      ] Print.InFileStream
[       OK ] Print.InFileStream (0 ms)
[----------] 1 test from Print (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (0 ms total)
[  PASSED  ] 1 test.
> cmake --build _build --target test -- ARGS=--verbose


Running tests...
UpdateCTestConfiguration  from :/home/matvey/matveech99/workspace/projects/lab05/_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/home/matvey/matveech99/workspace/projects/lab05/_build/DartConfiguration.tcl
Test project /home/matvey/matveech99/workspace/projects/lab05/_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: check
1: Test command: /home/matvey/matveech99/workspace/projects/lab05/_build/check
1: Working Directory: /home/matvey/matveech99/workspace/projects/lab05/_build
1: Test timeout computed to be: 10000000
1: Running main() from /home/matvey/matveech99/workspace/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
1: [==========] Running 1 test from 1 test case.
1: [----------] Global test environment set-up.
1: [----------] 1 test from Print
1: [ RUN      ] Print.InFileStream
1: [       OK ] Print.InFileStream (0 ms)
1: [----------] 1 test from Print (0 ms total)
1: 
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test case ran. (0 ms total)
1: [  PASSED  ] 1 test.
1/1 Test #1: check ............................   Passed    0.00 sec
100% tests passed, 0 tests failed out of 1
Total Test time (real) =   0.00 sec
~/m/w/p/lab05 main !1 ?3 >  
Запускает исполняемый файл тестов check напрямую
Запускает тесты через систему Ctest
> gsed -i 's/lab04/lab05/g' README.md
> gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml
> gsed -i '/cmake --build _build --target install/a\
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
 git add .travis.yml
> git add tests
> git add -p
diff --git a/CMakeLists.txt b/CMakeLists.txt
index 96a361e..54b9abd 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -4,6 +4,7 @@ set(CMAKE_CXX_STANDARD 11)
 set(CMAKE_CXX_STANDARD_REQUIRED ON)
 
 option(BUILD_EXAMPLES "Build examples" OFF)
+option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
 
(1/2) Индексировать этот блок [y,n,q,a,d,j,J,g,/,e,?]? y
@@ -34,3 +35,20 @@ install(TARGETS print
 
 install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
 install(EXPORT print-config DESTINATION cmake)
+
+if(BUILD_TESTS)
+  enable_testing()
+  
+  
+  # Решение 1: Отключаем -Werror для gtest (добавьте эту строку)
+  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-error=maybe-uninitialized")
+  
+  # Подключаем gtest (уже с отключенной ошибкой)
+  add_subdirectory(third-party/gtest)
+    
+  
+  file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
+  add_executable(check ${${PROJECT_NAME}_TEST_SOURCES})
+  target_link_libraries(check ${PROJECT_NAME} gtest_main)
+  add_test(NAME check COMMAND check)
+endif()
(2/2) Индексировать этот блок [y,n,q,a,d,K,g,/,e,?]? y
<stdin>:20: trailing whitespace.
  
<stdin>:21: trailing whitespace.
  
<stdin>:24: trailing whitespace.
  
<stdin>:27: trailing whitespace.
    
<stdin>:28: trailing whitespace.
  
warning: 5 строк добавили ошибки в пробельных символах.

diff --git a/README.md b/README.md
index 954e4d0..efc0ba2 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-# lab04
\ No newline at end of file
+# lab05
\ No newline at end of file
(1/1) Индексировать этот блок [y,n,q,a,d,e,?]? git commit -m"added tests"
Нет других блоков для перехода
@@ -1 +1 @@
-# lab04
\ No newline at end of file
+# lab05
\ No newline at end of file
(1/1) Индексировать этот блок [y,n,q,a,d,e,?]? y

> git commit -m"added tests"
[main f481be0] added tests
 3 files changed, 38 insertions(+), 1 deletion(-)
 create mode 100644 tests/test1.cpp
> git push origin master
error: src refspec master ничему не соответствует
error: не удалось отправить некоторые ссылки в «https://github.com/matveech99/lab05»
> git status
Текущая ветка: main
Неотслеживаемые файлы:
  (используйте «git add <файл>...», чтобы добавить в то, что будет включено в коммит)
	_build/
	file.txt

индекс пуст, но есть неотслеживаемые файлы
(используйте «git add», чтобы проиндексировать их)
> 
> git add .
> git commit -m"added tests"
> sudo apt install gnome-screenshot
[sudo] пароль для matvey: 
Чтение списков пакетов… Готово
Построение дерева зависимостей… Готово
Чтение информации о состоянии… Готово         
Следующие пакеты устанавливались автоматически и больше не требуются:
  libpkcs11-helper1t64 python3-netifaces
Для их удаления используйте «sudo apt autoremove».
Следующие НОВЫЕ пакеты будут установлены:
  gnome-screenshot
Обновлено 0 пакетов, установлено 1 новых пакетов, для удаления отмечено 0 пакетов, и 89 пакетов не обновлено.
Необходимо скачать 174 kB архивов.
После данной операции объём занятого дискового пространства возрастёт на 1 115 kB.
Пол:1 http://ru.archive.ubuntu.com/ubuntu noble/universe amd64 gnome-screenshot amd64 41.0-2build2 [174 kB]
Получено 174 kB за 0с (1 738 kB/s)     
Выбор ранее не выбранного пакета gnome-screenshot.
(Чтение базы данных … на данный момент установлено 253185 файлов и каталогов.)
Подготовка к распаковке …/gnome-screenshot_41.0-2build2_amd64.deb …
Распаковывается gnome-screenshot (41.0-2build2) …
Настраивается пакет gnome-screenshot (41.0-2build2) …
Обрабатываются триггеры для man-db (2.12.0-4build2) …
Обрабатываются триггеры для libglib2.0-0t64:i386 (2.80.0-6ubuntu3.2) …
Обрабатываются триггеры для libglib2.0-0t64:amd64 (2.80.0-6ubuntu3.2) …
Обрабатываются триггеры для bamfdaemon (0.5.6+22.04.20220217-0ubuntu5) …
Rebuilding /usr/share/applications/bamf-2.index...
Обрабатываются триггеры для desktop-file-utils (0.27-2build1) …
Обрабатываются триггеры для hicolor-icon-theme (0.17-2) …
Обрабатываются триггеры для gnome-menus (3.36.0-1.1ubuntu3) …
> sudo apt install gnome-screenshot
Чтение списков пакетов… Готово
Построение дерева зависимостей… Готово
Чтение информации о состоянии… Готово         
Уже установлен пакет gnome-screenshot самой новой версии (41.0-2build2).
Следующие пакеты устанавливались автоматически и больше не требуются:
  libpkcs11-helper1t64 python3-netifaces
Для их удаления используйте «sudo apt autoremove».
Обновлено 0 пакетов, установлено 0 новых пакетов, для удаления отмечено 0 пакетов, и 89 пакетов не обновлено.
> sleep 20s && gnome-screenshot --file artifacts/screenshot.png
