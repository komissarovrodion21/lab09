[![Build Status](https://travis-ci.org/komissarovrodion21/lab08.svg?branch=master)](https://travis-ci.org/komissarovrodion21/lab08)
## Laboratory work VI

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **Catch**

```ShellSession
$ open https://github.com/philsquared/Catch
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab08** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
#устанавливаем значение переменной GITHUB_USERNAME
$ export GITHUB_USERNAME=<имя_пользователя>
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab05 projects/lab08 #клонирование репозитория 5 лабораторной в локальный каталог 6 лабораторной
$ cd projects/lab08 #выбираем директорию lab08
$ git remote remove origin #отключаемся от удаленного репозитория 5 лабораторной
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08 #подключаемся к удаленному репозиторию 6 лабораторной
```

```ShellSession
$ mkdir tests #создаем каталог tests
$ wget https://github.com/philsquared/Catch/releases/download/v1.9.3/catch.hpp -O tests/catch.hpp #устанавливаем библиотеку для модульного тестирования на языке С++ catch.hpp
$ cat > tests/main.cpp <<EOF #вносим изменения в main.cpp, подключая к нему catch.hpp
#define CATCH_CONFIG_MAIN
#include "catch.hpp"
EOF
```

```ShellSession
$ §sed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\ #добавляем опцию option(BUILD_TESTS "Build tests" OFF) в файл CMakeLists.txt
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF #добавляем настройки в CMakeLists.txt

if(BUILD_TESTS)
	enable_testing()
	file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
	add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
	target_link_libraries(check \${PROJECT_NAME} \${DEPENDS_LIBRARIES})
	add_test(NAME check COMMAND check "-s" "-r" "compact" "--use-colour" "yes") 
endif()
EOF
```
Изменения в test1.cpp
```ShellSession
$ cat >> tests/test1.cpp <<EOF #вносим изменения в test1.cpp
#include "catch.hpp"
#include <print.hpp>

TEST_CASE("output values should match input values", "[file]") {
  std::string text = "hello";
  std::ofstream out("file.txt");
  
  print(text, out);
  out.close();
  
  std::string result;
  std::ifstream in("file.txt");
  in >> result;
  
  REQUIRE(result == text);
}
EOF
```
CMake
```ShellSession
$ cmake -H. -B_build -DBUILD_TESTS=ON #-H. устанавливаем каталог,-B_build указывает директорию для собираемых файлов,-D - заменяет команду set
$ cmake --build _build  #--build _build создает бинарное дерево проекта
$ cmake --build _build --target test #--target указывает необходимые для обработки цели
```

```ShellSession
$ sed -i 's/lab05/lab08/g' README.md #вносим изменения в файле README.md
$ sed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml #вносим изменения в файле .travis.yml
$ sed -i '/cmake --build _build --target install/a\ #вносим изменения в файле .travis.yml
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
```
Отображаем предупреждения или ошибки в файле .travis.yml
```ShellSession
$ travis lint
```

```ShellSession
$ git add . #добавляем все отредактированные файлы в подтвержденные
$ git commit -m"added tests" #создаем коммит
$ git push origin master #выгружаем локальную репозиторий в удаленный репозиторий шестой лабораторной
```

```ShellSession
$ travis login --auto #авторизуемся своим GITHUB аккаунтом
$ travis enable #включаем репозиторий в Travis
```

```ShellSession
$ mkdir artifacts #создаем каталог artifacts
$ open https://github.com/${GITHUB_USERNAME}/lab08 #открываем репозиторий шестой лабораторной на GitHub
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=06
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Google Test](https://github.com/google/googletest)

```
Copyright (c) 2017 Братья Вершинины
```
