[![CI](https://github.com/shirmanovak410-ops/lab08/actions/workflows/ci.yml/badge.svg)](https://github.com/shirmanovak410-ops/lab08/actions/workflows/ci.yml)
# Lab09 
Данная лабораторная работа посвещена изучению процесса создания артефактов на примере Github Release.
Этапы выполнения:
1) Настройка переменных окружения:
```bash
$ export GITHUB_USERNAME=shirmanovak410-ops
$ export GITHUB_TOKEN=мой токен
$ export PACKAGE_MANAGER=sudo apt-get install
$ export GPG_PACKAGE_NAME=gpg
```
2)  Установка необходимых утилит:
```bash
$ sudo apt-get update && sudo apt-get install -y xclip
Пол:1 http://security.debian.org/debian-security trixie-security InRelease [43,4 kB]
Пол:2 http://deb.debian.org/debian trixie InRelease [140 kB]
Пол:3 http://deb.debian.org/debian trixie-updates InRelease [47,3 kB]
Пол:4 http://deb.debian.org/debian trixie/main Sources [10,5 MB]
Пол:5 http://security.debian.org/debian-security trixie-security/main Sources [157 kB]
Пол:6 http://security.debian.org/debian-security trixie-security/main amd64 Packages [171 kB]
Пол:7 http://security.debian.org/debian-security trixie-security/main Translation-en [108 kB]
Пол:8 http://deb.debian.org/debian trixie/main amd64 Packages [9 671 kB]
Пол:9 http://deb.debian.org/debian trixie/main Translation-en [6 485 kB]
Получено 27,4 MB за 6с (4 846 kB/s)
Чтение списков пакетов… Готово
N: Репозиторий «http://deb.debian.org/debian trixie InRelease» изменил значение поля «Version» с «13.4» на «13.5»
Чтение списков пакетов… Готово
Построение дерева зависимостей… Готово
Чтение информации о состоянии… Готово
Следующие НОВЫЕ пакеты будут установлены:
  xclip
Обновлено 0 пакетов, установлено 1 новых пакетов, для удаления отмечено 0 пакетов, и 228 пакетов не обновлено.
Необходимо скачать 21,3 kB архивов.
После данной операции объём занятого дискового пространства возрастёт на 63,5 kB.
Пол:1 http://deb.debian.org/debian trixie/main amd64 xclip amd64 0.13-4 [21,3 kB]
Получено 21,3 kB за 0с (70,7 kB/s)
Выбор ранее не выбранного пакета xclip.
(Чтение базы данных … на данный момент установлен 147201 файл и каталог.)
Подготовка к распаковке …/xclip_0.13-4_amd64.deb …
Распаковывается xclip (0.13-4) …
Настраивается пакет xclip (0.13-4) …
Обрабатываются триггеры для man-db (2.13.1-1) …
Сканирование процессов...
Сканирование образов linux...

Запущено ядро последней версии.

Службы не требуют перезапуска.

Контейнеры не требуют перезапуска.

В сеансах пользователей нет устаревших
 процессов.

$ alias gsed=sed
$ alias pbcopy='xclip -selection clipboard'
$ alias pbpaste='xclip -selection clipboard -o'
$ cd ~/shirmanovak410-ops/workspace
$ source scripts/activate
```
3) Установка github-release:
```bash
$ go install github.com/github-release/github-release@latest
go: downloading github.com/github-release/github-release v0.11.0
go: downloading github.com/dustin/go-humanize v1.0.1
go: downloading github.com/voxelbrain/goptions v0.0.0-20180630082107-58cddc247ea2
go: downloading github.com/kevinburke/rest v0.0.0-20250718180114-1a15e4f2364f
go: downloading github.com/tomnomnom/linkheader v0.0.0-20180905144013-02ca5825eb80
$ github-release --version
github-release v0.11.0
```
4)  Клонирование и подготовка репозитория:
```bash
$ git clone https://github.com/shirmanovak410-ops/lab08 projects/lab09
Клонирование в «projects/lab09»...
remote: Enumerating objects: 214, done.
remote: Counting objects: 100% (214/214), done.
remote: Compressing objects: 100% (118/118), done.
remote: Total 214 (delta 76), reused 185 (delta 60), pack-reused 0 (from 0)
Получение объектов: 100% (214/214), 53.35 КиБ | 3.14 МиБ/с, готово.
Определение изменений: 100% (76/76), готово.
$ cd projects/lab09
$ git remote remove origin
$ git remote add origin https://github.com/shirmanovak410-ops/lab09
$ gsed -i 's/lab08/lab09/g' README.md
```
5) Настройка GPG-ключа для подписи тегов:
```bash
$ gpg --full-generate-key
gpg (GnuPG) 2.4.7; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Выберите тип ключа:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (только для подписи)
  (14) Existing key from card
Ваш выбор? 1
длина ключей RSA может быть от 1024 до 4096.
Какой размер ключа Вам необходим? (3072) 4096
Запрошенный размер ключа - 4096 бит
Выберите срок действия ключа.
         0 = не ограничен
      <n>  = срок действия ключа - n дней
      <n>w = срок действия ключа - n недель
      <n>m = срок действия ключа - n месяцев
      <n>y = срок действия ключа - n лет
Срок действия ключа? (0) 0
Срок действия ключа не ограничен
Все верно? (y/N) y

GnuPG должен составить идентификатор пользователя для идентификации ключа.

Ваше полное имя: shirmanovak410-ops
Адрес электронной почты: Ska2007msk@mail.ru
Примечание: ksu
Вы выбрали следующий идентификатор пользователя:
    "shirmanovak410-ops (ksu) <Ska2007msk@mail.ru>"

Сменить (N)Имя, (C)Примечание, (E)Адрес; (O)Принять/(Q)Выход? O
Необходимо получить много случайных чисел. Желательно, чтобы Вы
в процессе генерации выполняли какие-то другие действия (печать
на клавиатуре, движения мыши, обращения к дискам); это даст генератору
случайных чисел больше возможностей получить достаточное количество энтропии.
Необходимо получить много случайных чисел. Желательно, чтобы Вы
в процессе генерации выполняли какие-то другие действия (печать
на клавиатуре, движения мыши, обращения к дискам); это даст генератору
случайных чисел больше возможностей получить достаточное количество энтропии.
gpg: сертификат отзыва записан в '/home/ksu/.gnupg/openpgp-revocs.d/2F7B296EC4545D58578DE8C36790791721020AE5.rev'.
открытый и секретный ключи созданы и подписаны.
```
6) Ключ добавлен на GitHub
7) В конец файла CMakeLists.txt добавлены настройки для создания пакетов:
```bash
 cat >> CMakeLists.txt << 'EOF'
> include(InstallRequiredSystemLibraries)
set(CPACK_PACKAGE_NAME "print")
set(CPACK_PACKAGE_VERSION "0.1.0.0")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Static C++ library for printing")
set(CPACK_GENERATOR "TGZ")
include(CPack)
EOF
```
8) Сборка пакета:
 ```bash
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09$ cmake -H. -B_build
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
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
-- Configuring done (0.9s)
-- Generating done (0.0s)
-- Build files have been written to: /home/ksu/shirmanovak410-ops/workspace/projects/lab09/_build
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09$ cmake --build _build
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
[ 75%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[100%] Linking CXX executable demo
[100%] Built target demo
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09$ cd _build
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09/_build$ cpack -G "TGZ"
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/ksu/shirmanovak410-ops/workspace/projects/lab09/_build/print-0.1.0.0-Linux.tar.gz generated.
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09/_build$ ls -la *.tar.gz
-rw-rw-r-- 1 ksu ksu 6161 мая 26 00:58 print-0.1.0.0-Linux.tar.gz
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09/_build$ cd ..
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09$ git tag v0.1.0.0
```
