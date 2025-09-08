<center>

Федеральное государственное бюджетное  
образовательное учреждение высшего образования  

**«Сибирский государственный университет  
телекоммуникаций и информатики»**

<br>
<br>

---

## Лабораторная работа 1


---
<br>
<br>
</center>

<div align="right">

Выполнил:  
Студент группы **ИА-233**  
**Гурачевский Никита**  

«___» __________ 2025 г.  

</div>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

<center>

**Новосибирск 2025**

</center>

---

## План отчёта

1. Установка библиотек **libzmq** и **czmq** из исходников  
2. Создание структуры проекта и инициализация Git-репозитория  
3. Конфигурация **CMake** и сборка исходников `client.c` и `server.c`  
4. Запуск клиент-серверного приложения  
5. Анализ сетевого взаимодействия в **Wireshark**  
6. Размещение проекта на GitHub  
7. Выводы

---

### 1. Установка библиотек libzmq и czmq

Сначала были установлены необходимые пакеты для сборки:

```bash
sudo apt update
sudo apt install -y build-essential cmake pkg-config git

```

Далее — клонирование исходников:
```bash

git clone https://github.com/zeromq/libzmq.git
git clone https://github.com/zeromq/czmq.git

```

Сборка и установка libzmq:

```bash cd libzmq
mkdir build && cd build
cmake ..
make -j4
sudo make install
sudo ldconfig
```

Аналогично — czmq:

```bash
cd ../../czmq
mkdir build && cd build
cmake ..
make -j4
sudo make install
sudo ldconfig

```
После установки библиотеки доступны через pkg-config.

### 2. Создание структуры проекта

Был создан репозиторий и структура:

```bash
mkdir lab1
cd lab1
mkdir src include tests
touch .gitignore CMakeLists.txt
git init
```

Структура:
```bash
lab1/
├── include/
├── src/
│   ├── client.c
│   └── server.c
├── tests/
│   ├── CMakeLists.txt
│   └── README.md
├── CMakeLists.txt
└── .gitignore
```
3. Конфигурация CMake и сборка исходников

Содержимое CMakeLists.txt:
```bash
cmake_minimum_required(VERSION 3.10)
project(MyZMQApp C)

set(CMAKE_C_STANDARD 99)

find_package(PkgConfig REQUIRED)
pkg_check_modules(ZMQ REQUIRED libzmq)
pkg_check_modules(CZMQ REQUIRED libczmq)

include_directories(${ZMQ_INCLUDE_DIRS} ${CZMQ_INCLUDE_DIRS})

add_executable(server src/server.c)
target_link_libraries(server ${ZMQ_LIBRARIES} ${CZMQ_LIBRARIES})

add_executable(client src/client.c)
target_link_libraries(client ${ZMQ_LIBRARIES} ${CZMQ_LIBRARIES})

```
Сборка выполняется командами:
```bash
mkdir build
cd build
cmake ..
make
```

Результат:
```bash
[ 50%] Built target server
[100%] Built target client
```
4. Запуск клиент-серверного приложения

Для проверки запускаются два процесса:

./server
./client


На стороне сервера отображается приём сообщений, на стороне клиента — отправка.

Соединение проверено командой:
```bash
ss -ntp | grep 5555
```

Вывод:
```bash
ESTAB 0 0 127.0.0.1:5555    127.0.0.1:50690 users:(("server",pid=5939,fd=10))
ESTAB 0 0 127.0.0.1:50690   127.0.0.1:5555  users:(("client",pid=6569,fd=9))
```