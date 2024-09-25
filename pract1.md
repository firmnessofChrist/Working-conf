# Практическое занятие №1. Введение, основы работы в командной строке

П.Н. Советов, РТУ МИРЭА

Научиться выполнять простые действия с файлами и каталогами в Linux из командной строки. Сравнить работу в командной строке Windows и Linux.

## Задача 1

Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd (вам понадобится grep).

![image](https://github.com/user-attachments/assets/054a5bb2-f983-48c8-a165-094e736d185a)

## Задача 2

Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов, как показано в примере ниже:

```
[root@localhost etc]# cat /etc/protocols ...
142 rohc
141 wesp
140 shim6
139 hip
138 manet
```

![image](https://github.com/user-attachments/assets/9f852690-75a9-4c61-a7aa-dcbaf78248a3)

## Задача 3

Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!):

```
[root@localhost ~]# ./banner "Hello from RTU MIREA!"
+-----------------------+
| Hello from RTU MIREA! |
+-----------------------+
```

![image](https://github.com/user-attachments/assets/b8f93929-9945-4413-816b-97b1cb320396)

Перед отправкой решения проверьте его в ShellCheck на предупреждения.

## Задача 4

Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).

Пример для hello.c:

```
h hello include int main n printf return stdio void world
```

![image](https://github.com/user-attachments/assets/e888d4dd-ebad-4c01-bd08-f3ffdd905378)

## Задача 5

Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).

Например, пусть программа называется reg:
#!/usr/bin/env python3

import sys

def main():
    print("Команда успешно зарегистрирована!")

if __name__ == "__main__":
    main()
В результате для banner задаются правильные права доступа и сам banner копируется в /usr/local/bin.
![image](https://github.com/user-attachments/assets/5d940109-bdcb-407d-9a9a-cc4c0ae31f8f)

## Задача 6

Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.

import os
import sys

# Функция для проверки наличия комментария в первой строке файла
def check_first_line_for_comment(file_path):
    try:
        with open(file_path, 'r') as file:
            first_line = file.readline().strip()

            # Проверяем на наличие комментария для каждого типа файлов
            if file_path.endswith('.c') or file_path.endswith('.js'):
                # Комментарии в стиле C/JS (// или /*)
                if first_line.startswith('//') or first_line.startswith('/*'):
                    return True
            elif file_path.endswith('.py'):
                # Комментарии в стиле Python (#)
                if first_line.startswith('#'):
                    return True
        return False
    except Exception as e:
        print(f"Ошибка при чтении файла {file_path}: {e}")
        return False

# Функция для поиска файлов в директории и проверки их на наличие комментария в первой строке
def check_files_in_directory(directory):
    try:
        if not os.path.isdir(directory):
            print(f"Ошибка: {directory} не является директорией.")
            return

        # Проходим по всем файлам в директории
        for filename in os.listdir(directory):
            filepath = os.path.join(directory, filename)

            # Проверяем файлы с расширениями .c, .js и .py
            if os.path.isfile(filepath) and (filename.endswith('.c') or filename.endswith('.js') or filename.endswith('.py')):
                if check_first_line_for_comment(filepath):
                    print(f"Файл {filename} содержит комментарий в первой строке.")
                else:
                    print(f"Файл {filename} НЕ содержит комментарий в первой строке.")
    except Exception as e:
        print(f"Произошла ошибка: {e}")

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Использование: python3 script.py <директория>")
    else:
        directory = sys.argv[1]
        check_files_in_directory(directory)


создаем файлы и проверяем комментарии в каждом из них

![image](https://github.com/user-attachments/assets/eddc313d-7d47-4b92-b08b-799965498cf6)

## Задача 7

Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).
Как работает программа:
Хеширование файлов: Программа вычисляет MD5-хеш для каждого файла, прочитывая его поблочно, чтобы избежать проблем с большими файлами.
Сравнение файлов: Все файлы, которые имеют одинаковый хеш, считаются дубликатами.
Рекурсивный обход каталогов: Программа использует os.walk(), чтобы рекурсивно обойти все файлы в указанной директории и её подкаталогах.
код программы:

import os
import hashlib

# Функция для вычисления хеша файла
def get_file_hash(file_path):
    hasher = hashlib.md5()  # Используем MD5 для вычисления хеша файла
    try:
        with open(file_path, 'rb') as file:
            while chunk := file.read(8192):  # Читаем файл порциями по 8KB
                hasher.update(chunk)
    except Exception as e:
        print(f"Ошибка при чтении файла {file_path}: {e}")
        return None
    return hasher.hexdigest()

# Функция для поиска файлов-дубликатов
def find_duplicate_files(directory):
    if not os.path.isdir(directory):
        print(f"Ошибка: {directory} не является директорией.")
        return

    # Словарь для хранения хеша и списка файлов с одинаковым содержимым
    file_hashes = {}

    # Проходим по всем файлам и подкаталогам
    for root, _, files in os.walk(directory):
        for filename in files:
            filepath = os.path.join(root, filename)

            # Вычисляем хеш содержимого файла
            file_hash = get_file_hash(filepath)

            if file_hash:
                # Если такой хеш уже есть, добавляем файл в список дубликатов
                if file_hash in file_hashes:
                    file_hashes[file_hash].append(filepath)
                else:
                    file_hashes[file_hash] = [filepath]

    # Выводим найденные дубликаты
    duplicates_found = False
    for file_list in file_hashes.values():
        if len(file_list) > 1:
            duplicates_found = True
            print("Найдены дубликаты:")
            for file in file_list:
                print(file)
            print()  # Пустая строка для разделения групп дубликатов

    if not duplicates_found:
        print("Дубликаты не найдены.")

if __name__ == "__main__":
    if len(os.sys.argv) != 2:
        print("Использование: python3 find_duplicates.py <директория>")
    else:
        directory = os.sys.argv[1]
        find_duplicate_files(directory)
создаем директорию и файлы

программа успешно справилась с поиском дубликатов:

![image](https://github.com/user-attachments/assets/fa5e868a-eac8-4ed9-870b-fe4dd50da8ef)



## Задача 8

Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.

код программы:

import os
import sys
import tarfile

def find_files_with_extension(directory, extension):
    # Список для хранения найденных файлов
    files_to_archive = []

    # Рекурсивно обходим директорию и ищем файлы с нужным расширением
    for root, _, files in os.walk(directory):
        for file in files:
            if file.endswith(extension):
                file_path = os.path.join(root, file)
                files_to_archive.append(file_path)
    
    return files_to_archive

def create_tar_archive(files, archive_name):
    try:
        # Создаём tar архив
        with tarfile.open(archive_name, "w") as tar:
            for file in files:
                tar.add(file, arcname=os.path.basename(file))
        print(f"Архив '{archive_name}' успешно создан.")
    except Exception as e:
        print(f"Ошибка при создании архива: {e}")

if __name__ == "__main__":
    if len(sys.argv) != 4:
        print("Использование: python3 archive_files.py <директория> <расширение> <имя_архива>")
    else:
        directory = sys.argv[1]
        extension = sys.argv[2]
        archive_name = sys.argv[3]

        # Находим файлы с заданным расширением
        files_to_archive = find_files_with_extension(directory, extension)

        if files_to_archive:
            # Создаём архив с найденными файлами
            create_tar_archive(files_to_archive, archive_name)
        else:
            print(f"Файлы с расширением '{extension}' не найдены.")
Поиск файлов: Функция find_files_with_extension() рекурсивно обходит заданную директорию и находит все файлы с указанным расширением.
Создание архива: Функция create_tar_archive() создаёт tar-архив и добавляет в него все найденные файлы.
Аргументы:
Первый аргумент: путь к директории, где искать файлы.
Второй аргумент: расширение файлов, которые нужно найти (например, .txt).
Третий аргумент: имя создаваемого архива (например, archive.tar).

создаем директорию и архивируем

![image](https://github.com/user-attachments/assets/92dc6ff5-e187-4d63-9bc8-ec83ae7f53e7)





        
## Задача 9

Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.
Создадим скрипт на Python и создадим два тестовых файла intput.txt и output.txt


![image](https://github.com/user-attachments/assets/c8c89016-0ed5-48e8-a9ac-0a94af041502)


далее мы можем увидеть, что программа обработала файл


![image](https://github.com/user-attachments/assets/62d824aa-36e1-4207-9b17-f844e4b51a9c)


сравним исходник и полученный файлы:


![image](https://github.com/user-attachments/assets/6f233c52-26a4-404a-9e81-b1b3768d85f0)


![image](https://github.com/user-attachments/assets/a3a3086e-31c6-4fee-84e0-24574700c70f)



программа:

import sys

def replace_spaces_with_tabs(input_file, output_file):
    try:
        # Открываем входной файл для чтения
        with open(input_file, 'r') as infile:
            content = infile.read()

        # Заменяем 4 пробела на символ табуляции
        content = content.replace('    ', '\t')

        # Открываем выходной файл для записи
        with open(output_file, 'w') as outfile:
            outfile.write(content)

        print(f"Замена завершена. Результат сохранен в {output_file}")
    except FileNotFoundError:
        print(f"Ошибка: файл {input_file} не найден.")
    except Exception as e:
        print(f"Произошла ошибка: {e}")

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Использование: python3 script.py <input_file> <output_file>")
    else:
        input_file = sys.argv[1]
        output_file = sys.argv[2]
        replace_spaces_with_tabs(input_file, output_file)

## Задача 10

Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром. 

скрипт на Python 
import os
import sys

def find_empty_files(directory):
    try:
        # Проверяем, существует ли директория
        if not os.path.isdir(directory):
            print(f"Ошибка: {directory} не является директорией.")
            return

        # Проходим по всем файлам в директории
        empty_files = []
        for filename in os.listdir(directory):
            filepath = os.path.join(directory, filename)

            # Проверяем, что это файл и он пустой (размер 0 байт) и имеет расширение .txt
            if os.path.isfile(filepath) and os.path.getsize(filepath) == 0 and filename.endswith('.txt'):
                empty_files.append(filename)

        # Выводим результат
        if empty_files:
            print("Пустые текстовые файлы:")
            for file in empty_files:
                print(file)
        else:
            print("В данной директории нет пустых текстовых файлов.")
    
    except Exception as e:
        print(f"Произошла ошибка: {e}")

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Использование: python3 script.py <директория>")
    else:
        directory = sys.argv[1]
        find_empty_files(directory)

создаем директорию:
mkdir my_directory
и пустые файлы в ней:
touch my_directory/file1.txt
touch my_directory/file2.txt
touch my_directory/file3.txt


![image](https://github.com/user-attachments/assets/14a40948-f7d2-4b22-99af-9134de3280d3)


после команды python3 directory.py my_directory


![image](https://github.com/user-attachments/assets/305fba22-49ae-4ab6-80fc-350f619adf36)


программа выводит пустые файлы
## Полезные ссылки

Линукс в браузере: https://bellard.org/jslinux/

ShellCheck: https://www.shellcheck.net/

Разработка CLI-приложений

Общие сведения

https://ru.wikipedia.org/wiki/Интерфейс_командной_строки
https://nullprogram.com/blog/2020/08/01/
https://habr.com/ru/post/150950/

Стандарты

https://www.gnu.org/prep/standards/standards.html#Command_002dLine-Interfaces
https://www.gnu.org/software/libc/manual/html_node/Argument-Syntax.html
https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html

Реализация разбора опций

Питон

https://docs.python.org/3/library/argparse.html#module-argparse
https://click.palletsprojects.com/en/7.x/
