[![CI](https://github.com/shirmanovak410-ops/lab09/actions/workflows/ci.yml/badge.svg)](https://github.com/shirmanovak410-ops/lab09/actions/workflows/ci.yml)
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
$ cmake -H. -B_build
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
$ cmake --build _build
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
[ 75%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[100%] Linking CXX executable demo
[100%] Built target demo
$ cd _build
$ cpack -G "TGZ"
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/ksu/shirmanovak410-ops/workspace/projects/lab09/_build/print-0.1.0.0-Linux.tar.gz generated.
$ ls -la *.tar.gz
-rw-rw-r-- 1 ksu ksu 6161 мая 26 00:58 print-0.1.0.0-Linux.tar.gz
$ cd ..
```
9) Создание тега и отправка на GitHub:
```bash
$ git tag v0.1.0.0
$ git push origin main --tags
```
10) Создание релиза через github-release и загрузка артефактов в релиз:
```bash
$ export PACKAGE_OS=`uname -s`
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09$ export PACKAGE_ARCH=`uname -m`
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09$ export PACKAGE_FILENAME=print-${PACKAGE_OS}-${PACKAGE_ARCH}.tar.gz
ksu@skalab3:~/shirmanovak410-ops/workspace/projects/lab09$ github-release release \
>  --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "libprint" \
    --description "my first release"
$ github-release upload \
>  --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "${PACKAGE_FILENAME}" \
    --file _build/*.tar.gz
$ github-release info -u ${GITHUB_USERNAME} -r lab09
tags:
- v0.1.0.0 (commit: https://api.github.com/repos/shirmanovak410-ops/lab09/commits/467dbb0999031436af788249688e71bd7f9e2097)
releases:
- v0.1.0.0, name: 'libprint', description: 'my first release', id: 329084080, tagged: 17/05/2026 at 13:33, published: 25/05/2026 at 22:05, draft: ✗, prerelease: ✗
  - artifact: print-Linux-x86_64.tar.gz, downloads: 0, state: uploaded, type: application/octet-stream, size: 2.5 kB, id: 429682565
$ wget https://github.com/${GITHUB_USERNAME}/lab09/releases/download/v0.1.0.0/${PACKAGE_FILENAME}
--2026-05-26 01:05:59--  https://github.com/shirmanovak410-ops/lab09/releases/download/v0.1.0.0/print-Linux-x86_64.tar.gz
Распознаётся github.com (github.com)… 20.26.156.215
Подключение к github.com (github.com)|20.26.156.215|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 302 Found
Адрес: https://release-assets.githubusercontent.com/github-production-release-asset/1249582529/242d45ee-8724-4bee-9474-d88034edc67e?sp=r&sv=2018-11-09&sr=b&spr=https&se=2026-05-25T22%3A56%3A07Z&rscd=attachment%3B+filename%3Dprint-Linux-x86_64.tar.gz&rsct=application%2Foctet-stream&skoid=96c2d410-5711-43a1-aedd-ab1947aa7ab0&sktid=398a6654-997b-47e9-b12b-9515b896b4de&skt=2026-05-25T21%3A55%3A15Z&ske=2026-05-25T22%3A56%3A07Z&sks=b&skv=2018-11-09&sig=9CITet%2B%2FIc%2FFZWgF%2BEVzsAZUoqMDRGr0C5o4bk6qArE%3D&jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmVsZWFzZS1hc3NldHMuZ2l0aHVidXNlcmNvbnRlbnQuY29tIiwia2V5Ijoia2V5MSIsImV4cCI6MTc3OTc0NzA2MCwibmJmIjoxNzc5NzQ2NzYwLCJwYXRoIjoicmVsZWFzZWFzc2V0cHJvZHVjdGlvbi5ibG9iLmNvcmUud2luZG93cy5uZXQifQ.pH8dq1FgZ40gbUkhOOxEaBL1CbRRTyaVJ1RGH_R0FtY&response-content-disposition=attachment%3B%20filename%3Dprint-Linux-x86_64.tar.gz&response-content-type=application%2Foctet-stream [переход]
--2026-05-26 01:06:00--  https://release-assets.githubusercontent.com/github-production-release-asset/1249582529/242d45ee-8724-4bee-9474-d88034edc67e?sp=r&sv=2018-11-09&sr=b&spr=https&se=2026-05-25T22%3A56%3A07Z&rscd=attachment%3B+filename%3Dprint-Linux-x86_64.tar.gz&rsct=application%2Foctet-stream&skoid=96c2d410-5711-43a1-aedd-ab1947aa7ab0&sktid=398a6654-997b-47e9-b12b-9515b896b4de&skt=2026-05-25T21%3A55%3A15Z&ske=2026-05-25T22%3A56%3A07Z&sks=b&skv=2018-11-09&sig=9CITet%2B%2FIc%2FFZWgF%2BEVzsAZUoqMDRGr0C5o4bk6qArE%3D&jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmVsZWFzZS1hc3NldHMuZ2l0aHVidXNlcmNvbnRlbnQuY29tIiwia2V5Ijoia2V5MSIsImV4cCI6MTc3OTc0NzA2MCwibmJmIjoxNzc5NzQ2NzYwLCJwYXRoIjoicmVsZWFzZWFzc2V0cHJvZHVjdGlvbi5ibG9iLmNvcmUud2luZG93cy5uZXQifQ.pH8dq1FgZ40gbUkhOOxEaBL1CbRRTyaVJ1RGH_R0FtY&response-content-disposition=attachment%3B%20filename%3Dprint-Linux-x86_64.tar.gz&response-content-type=application%2Foctet-stream
Распознаётся release-assets.githubusercontent.com (release-assets.githubusercontent.com)… 185.199.109.133, 185.199.110.133, 185.199.111.133, ...
Подключение к release-assets.githubusercontent.com (release-assets.githubusercontent.com)|185.199.109.133|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 200 OK
Длина: 2497 (2,4K) [application/octet-stream]
Сохранение в: «print-Linux-x86_64.tar.gz»

print-Linux-x86_64 100%[================>]   2,44K  --.-KB/s    за 0s

2026-05-26 01:06:00 (25,5 MB/s) - «print-Linux-x86_64.tar.gz» сохранён [2497/2497]
```
