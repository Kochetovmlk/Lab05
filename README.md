## Laboratory work V

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **GTest**

```sh
$ open https://github.com/google/googletest
```


## Homework

### Задание
1. Создайте `CMakeList.txt` для библиотеки *banking*.
2. Создайте модульные тесты на классы `Transaction` и `Account`.
    * Используйте mock-объекты.
    * Покрытие кода должно составлять 100%.
3. Настройте сборочную процедуру на **TravisCI**.
4. Настройте [Coveralls.io](https://coveralls.io/).


## Work report
1) git clone https://github.com/Kochetovmlk/Lab05.git
   cd Lab05
   
2) touch CMakeLists.txt : 
   cmake_minimum_required(VERSION 3.12)

   project(banking)

   set(CMAKE_CXX_STANDARD 17)

   add_library(banking STATIC
    banking.cpp
   )

   add_executable(main main.cpp)
   target_link_libraries(main banking
   
3) mkdir tests в директории Lab05
   cd tests
   touch CMakeLists.txt :
   cmake_minimum_required(VERSION 3.12)

   project(tests)

   set(CMAKE_CXX_STANDARD 17)

   add_executable(tests
     test_banking.cpp
   )

   target_link_libraries(tests banking)
   
4) cd ..

   touch .travis.yml : 
   language: cpp
   compiler: gcc
   script:
  - cmake .
  - cmake --build .
  - cmake --build . --target test
 5) После чего я зарегистрировался на сайте coveralls.io и добавил репозиторий Lab05 в список отслеживаемых.

 6) Установил утилиту lcov :
    sudo apt-get install lcov
    
 7) touch CMakeLists.txt.coverage : 
    cmake_minimum_required(VERSION 3.12)

   project(coverage)

   set(CMAKE_CXX_STANDARD 17)

   add_library(coverage_config INTERFACE)

   option(CODE_COVERAGE "Enable coverage reporting" ON)

   if(CODE_COVERAGE)
       message(STATUS "Enabling code coverage")
       target_compile_options(coverage_config INTERFACE -O0 -g --coverage)
       target_link_options(coverage_config INTERFACE --coverage)
   endif()

   add_executable(tests_coverage
       test_banking.cpp
   )

   target_link_libraries(tests_coverage banking coverage_config)
   
8) В файле CMakeLists.txt добавил следующую строку перед командой add_library(banking STATIC ...) :

   set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/CMakeModules)

9) В файле .travis.yml добавил следующие команды после команды cmake --build . :
   - cmake -DCODE_COVERAGE=ON -B . -S .
   - cmake --build . --target tests_coverage
   - lcov --directory . --capture --output-file coverage.info
   - lcov --remove coverage.info '/usr/*' "${HOME}"'/.cache/*' --output-file coverage.info
   - lcov --list coverage.info
   - coveralls-lcov --repo-token ${COVERALLS_TOKEN} coverage.info
   - 
10) Загрузил все изменения в мой репозиторий на GitHub.

   git add .
   git commit -m "Configure build and coverage"
   git push origin master
   
   ## Conclusion
   
   Проект Lab05 должен быть настроен для сборки, выполнения модульных тестов и отслеживания покрытия кода с помощью Travis CI и Coveralls.io.


## Links

- [C++ CI: Travis, CMake, GTest, Coveralls & Appveyor](http://david-grs.github.io/cpp-clang-travis-cmake-gtest-coveralls-appveyor/)
- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Catch](https://github.com/catchorg/Catch2)


