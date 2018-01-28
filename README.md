[![Build Status](https://travis-ci.org/komissarovrodion21/lab08.svg?branch=master)](https://travis-ci.org/komissarovrodion21/lab08)
## Laboratory work IX

Данная лабораторная работа посвещена изучению процесса создания пакета на примере **Github Release**

```ShellSession
$ open https://help.github.com/articles/creating-releases/
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab09** на сервисе **GitHub**
- [X] 2. Ознакомиться со ссылками учебного материала
- [X] 3. Получить токен для доступа к репозиториям сервиса **GitHub**
- [X] 4. Сгенерировать GPG ключ и добавить его к аккаунту сервиса **GitHub**
- [X] 5. Выполнить инструкцию учебного материала
- [X] 6. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Делаем первоначальные настройки, добавляя значения переменным окружения
```ShellSession
$ export GITHUB_TOKEN=<полученный_токен> #устанавливаем значение переменной окружения GITHUB_TOKEN
$ export GITHUB_USERNAME=komissarovrodion21 #устанавливаем значение переменной окружения GITHUB_USERNAME
```
Проводим первоначальные настройки, устанавливаем github-release
```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
$ go get github.com/aktau/github-release
```
Проводим первоначальные настройки для соединения с репозиторием девятой лабораторной работы
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab08 projects/lab09 #клонирование удаленного репозитория восьмой лабораторной в локальный каталог девятой лабораторной
$ cd projects/lab09 #меняем директорию на lab09
$ git remote remove origin #отключаемся от удаленного репозитория восьмой лабораторной
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab09 #подключаемся к удаленному репозиторию девятой лабораторной

```
Вносим изменения в README.md
```ShellSession
$ gsed -i 's/lab08/lab09/g' README.md #вносим изменения в README.md
```
Работа с CMake
```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
#-H. устанавливаем каталог в который сгенерируется файл CMakeLists.txt,-B_build указывает директорию для собираемых файлов,-D - заменяет команду set
$ cmake --build _build --target package #--build _build создает бинарное дерево проекта,--target указывает необходимые для обработки цели

```
Работа с Travis
```ShellSession
$ travis login –auto #авторизуемся своим GITHUB аккаунтом
$ travis enable #включаем репозиторий в Travis

```

```ShellSession
$ git tag -s v0.1.0.0 #создаем тэг с GPG сигнатурой
$ git tag -v v0.1.0.0 #проверяем GPG сигнатуру созданного нами тэга 
$ git push origin master –tags #отправляем на удаленную ветку все теги локальной ветки

```
Работа с github-release
```ShellSession
//Вывод версии github-release
$ github-release --version
$ github-release info -u ${GITHUB_USERNAME} -r lab09
#Делаем релиз задавая параметры
$ github-release release \
#Пользователя
    --user ${GITHUB_USERNAME} \
#Репозитория
    --repo lab09 \
#Используемого тага
    --tag v0.1.0.0 \
#Имени
    --name "libprint" \
#Описания релиза
    --description "my first release"
```
Работа с github-release
```ShellSession
$ export PACKAGE_OS=`uname -s` PACKAGE_ARCH=`uname -m` #устанавливаем значение переменной окружения PACKAGE_OS и PACKAGE_ARCH
$ export PACKAGE_FILENAME=print-${PACKAGE_OS}-${PACKAGE_ARCH}.tar.gz #устанавливаем значение переменной окружения PACKAGE_FILENAME
#Загружаем наш релиз устанавливая параметры
$ github-release upload \
#Имени пользователя
    --user ${GITHUB_USERNAME} \
#Репозитория    
    --repo lab09 \
#Тэга
    --tag v0.1.0.0 \
#Имени пакета
    --name "${PACKAGE_FILENAME}" \
#Используемого файла
    --file _build/*.tar.gz
```

```ShellSession
$ github-release info -u ${GITHUB_USERNAME} -r lab09
#Устанавливаем пакет по ссылке
$ wget https://github.com/${GITHUB_USERNAME}/lab09/releases/download/v0.1.0.0/${PACKAGE_FILENAME}
#Разархивируем его
$ tar -ztf ${PACKAGE_FILENAME}
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=09
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Create Release](https://help.github.com/articles/creating-releases/)
- [Get GitHub Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
- [Signing Commits](https://help.github.com/articles/signing-commits-with-gpg/)
- [Go Setup](http://www.golangbootcamp.com/book/get_setup)
- [github-release](https://github.com/aktau/github-release)

```
Copyright (c) 2017 Братья Вершинины
