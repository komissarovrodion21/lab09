## Laboratory work IV

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
#устанавливаем значение переменной GITHUB_USERNAME
$ export GITHUB_USERNAME=<имя_пользователя>
```
Настройки для соединения с репозиторием четвертой лабораторной работы
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab03.git lab04 #клонирование репозитория lab03 в lab04
$ cd lab04 #выбираем директорию lab04
$ git remote remove origin #отключаемся от удаленного репозитория 3 лабораторной
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04.git #подключаемся к удаленному репозиторию 4 лабораторной
```
Подготовка CMake
```ShellSession
$ g++ -I./include -std=c++11 -c sources/print.cpp #добавляем print.cpp в среду обработки
$ ls print.o #проверяем файл print.o
$ ar rvs print.a print.o #создаем архив print.a с print.o
$ file print.a #проверяем файл print.a
$ g++ -I./include -std=c++11 -c examples/example1.cpp #добавляем example1.cpp в среду обработки
$ ls example1.o #проверяем наличие файла example1.o
$ g++ example1.o print.a -o example1 #собираем проект 
$ ./example1 && echo #запускаем проект и печатаем строку
```

```ShellSession
$ g++ -I./include -std=c++11 -c examples/example2.cpp #добавляем example2.cpp в среду обработки
$ ls example2.o #проверяем файл example2.o
$ g++ example2.o print.a -o example2 #собираем проект 
$ ./example2 #запускаем
$ cat log.txt && echo #записываем в log.txt и выводим на экран содержимое файла
```
Удаление файлов 
```ShellSession
$ rm -rf example1.o example2.o print.o #удаляем объектные файлы
$ rm -rf print.a #удаляем архив
$ rm -rf example1 example2 #удаляем example1 и example2
$ rm -rf log.txt #удаляем log.txt
```
Работа с CMakeLists.txt
```ShellSession
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.0)#проверка версии CMake
project(print) #название проекта
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11) #подключение 11-го стандарта
set(CMAKE_CXX_STANDARD_REQUIRED ON) #активация стандарта
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp) #создание статической библиотеки print
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include) #добавляем путь к include для заголовочных файлов
EOF
```

```ShellSession
$ cmake -H. -B_build #сборка проекта в католок
$ cmake --build _build #сборка и линовка проекта
```
Создаем исполняемые файлы
```ShellSession
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp) #создаем файл с именем example1
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp) #создаем файл с именем example2
EOF
```
Линковка программ example1 и example2 с библиотекой print
```ShellSession
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```
Собираем проект
```ShellSession
$ cmake --build _build #запускаем сборку в каталоге _build
$ cmake --build _build --target print #сборка цели print
$ cmake --build _build --target example1 #Сборка example1
$ cmake --build _build --target example2 #Сборка example2
```

```ShellSession
$ ls -la _build/libprint.a  
$ _build/example1 && echo #сборка и вывод на экран файла
hello
$ _build/example2 #сборка example2
$ cat log.txt && echo
hello
```
Скачиваем файл CMakeLists.txt из репозитория lab04
```ShellSession
$ git clone https://github.com/tp-labs/lab04 tmp #копирование
$ mv -f tmp/CMakeLists.txt . #перемещение в tmp/CMakeLists.txt
$ rm -rf tmp #удаляем файл tmp
```
Настройки CMake
```ShellSession
$ cat CMakeLists.txt #выводим файл
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install #сборка проекта с -DCMAKE_INSTALL_PREFIX
$ cmake --build _build --target install #сборка проекта print
$ tree _install #выводим в консоль структуру проекта
```
Отправка изменений в Github
```ShellSession
$ git add CMakeLists.txt #добавляем CMakeLists.txt в подтвержденные файлы
$ git commit -m"added CMakeLists.txt" #создаем коммит 
$ git push origin master #выгружаем файлы
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=04
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2017 Братья Вершинины
```
