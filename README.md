## Laboratory work III

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** и с лиценцией **MIT**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Задаем параметры для Git
```ShellSession
$ export GITHUB_USERNAME=komissarovrodion21 #задаем значение переменной GITHUB_USERNAME
$ export GITHUB_EMAIL=komissarovrodion1@gmail.com #задаем значение переменной GITHUB_EMAIL
$ alias edit=nano #cоздаем алиас редактором nano
```
Работаем сдирректориями и создаем репозиторий
```ShellSession
$ mkdir lab3 && cd lab3 #cоздаем директорию lab3 и переходим в нее
$ git init #cоздаем репозиторий git
$ git config --global user.name ${GITHUB_USERNAME} #устанавливаем параметры user.name
$ git config --global user.email ${GITHUB_EMAIL} #устанавливаем параметры user.email
$ git config -e --global #открываем файл
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab3 #добавляем удаленный репозиторий по ссылке
$ git pull origin master #копируем содержимое удаленного репозитория в локальный
$ touch README.md #создаем файл
$ git status #отображаем файлы в репозиториях(подтвержденные и неподтвержденные)
$ git add README.md #добавляем этот файл в подтвержденные
$ git commit -m"added README.md" #отправляем файл в удаленный репозиторий 
$ git push origin master #сохраняем изменения на удаленном репозитории
```

Добавить на сервисе **GitHub** в репозитории **lab03** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/
*install*/
*.swp
```
Копируем содержимое и открываем журнал всех изменений 
```ShellSession
$ git pull origin master #копируем содержимое удаленного репозитория в локальный
$ git log #открываем журнал всех изменений 
```
Работа с директориями, создание файла и добавление в него информации
```ShellSession
$ mkdir sources #cоздаем каталог sources
$ mkdir include #cоздаем каталог include
$ mkdir examples #cоздаем каталог examples
#создаем файлы и добавляем в них код
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out) {
  out << text;
}

void print(const std::string& text, std::ofstream& out) {
  out << text;
}
EOF
```
Создание файла и добавление в него информации
```ShellSession
$ cat > include/print.hpp <<EOF
#include <string>
#include <fstream>
#include <iostream>

void print(const std::string& text, std::ostream& out = std::cout);
void print(const std::string& text, std::ofstream& out);
EOF
```
Создание файла и добавление в него информации
```ShellSession
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv) {
  print("hello");
}
EOF
```
Создание файла и добавление в него информации
```ShellSession
$ cat > examples/example2.cpp <<EOF
#include <fstream>
#include <print.hpp>

int main(int argc, char** argv) {
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```
Вносим изменения в файл
```ShellSession
$ edit README.md
```
Отправляем файлы в Github
```ShellSession
$ git status #проверяем статус файлов
$ git add . 
$ git commit -m"added sources" #отправляем данные 
$ git push origin master #вливаем локальные изменения в удаленный репозиторий
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=3
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```
