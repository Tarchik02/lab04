# Отчет ЛР3

# Task 1

Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории `formatter_lib`. В этой директории находятся файлы для статической библиотеки `formatter`. Создайте `CMakeList.txt` в директории `formatter_lib`, с помощью которого можно будет собирать статическую библиотеку formatter


Содержимое файла CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter)

add_library(formatter STATIC formatter.cpp)
target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```

- Команда cmake_minimum_required указывает подходящую минимальную версию Cmake
- Команды set(CMAKE_CXX_STANDARD 11), set(CMAKE_CXX_STANDARD_REQUIRED ON) устанавливают значения стандартных переменных в Cmake(в данном случае CMAKE_CXX_STANDARD и CMAKE_CXX_STANDARD_REQUIRED)
- Команда project(formatter) создает проект formatter, к которому можно подключать библиотеки, исполняемы файлы и т.д.
- Команда add_library(formatter STATIC formatter.cpp) создает статическую библиотеку из указываемых файлов
- Команда target_include_directories связывает библиотеку formatter и CMAKE_CURRENT_SOURCE_DIR


Проверяем работоспособность CMake:
```
$ cmake -H. -B_build
$ cmake --build _build

```

# Task 2

У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием `CMakeList.txt` для статической библиотеки formatter, ваш руководитель поручает заняться созданием `CMakeList.txt` для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter

Содержимое файла CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_ex)

add_library(formatter_ex STATIC formatter_ex.cpp)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib ${CMAKE_CURRENT_BINARY_DIR}/formatter)
target_include_directories(formatter_ex PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

target_link_libraries(formatter_ex formatter)
```

- Команда add_subdirectory(../formatter_lib formatter) включает в сборку директорию с библиотекой formatter
- Команда target_link_libraries(formatter_ex formatter) связывает библиотеку formatter с formatter_ex
- Остальные команды аналогичны первому заданию


Проверяем работоспособность CMake:
```
$ cmake -H. -B_build
$ cmake --build _build
```

# Task 3
Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой `formatter_ex`, вам необходимо создать два `CMakeList.txt` для двух простых приложений:

- `hello_world`, которое использует библиотеку `formatter_ex`;
- `solver`, приложение которое испольует статические библиотеки `formatter_ex` и `solver_lib`

Удачной стажировки!


### hello_world.cpp



Содержимое файла CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(hello_world_example)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib ${CMAKE_CURRENT_BINARY_DIR}/formatter_ex)
add_executable(example hello_world.cpp)

target_link_libraries(example formatter_ex)
```


Проверяем работоспособность CMake:
```
$ cmake -H. -B_build
$ cmake --build _build
```


Демонстрируем исполнение программы:
```
$ cmake --build _build --target example
