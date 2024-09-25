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

def check_comment(file_path):
    with open(file_path, 'r') as file:
        first_line = file.readline().strip()
        
        if file_path.endswith('.c'):
            if first_line.startswith('//') or first_line.startswith('/*'):
                return f"{file_path}: комментарий найден"
            else:
                return f"{file_path}: комментарий не найден"
        
        elif file_path.endswith('.js'):
            if first_line.startswith('//'):
                return f"{file_path}: комментарий найден"
            else:
                return f"{file_path}: комментарий не найден"
        
        elif file_path.endswith('.py'):
            if first_line.startswith('#'):
                return f"{file_path}: комментарий найден"
            else:
                return f"{file_path}: комментарий не найден"
        
        else:
            return f"{file_path}: неподдерживаемый формат"

# Перебираем все файлы с заданными расширениями в текущем каталоге
for filename in os.listdir('.'):
    if filename.endswith(('.c', '.js', '.py')):
        result = check_comment(filename)
        print(result)
        
![image](https://github.com/user-attachments/assets/d5b8865c-c6bd-46bf-beac-f7d66d75b4b1)

## Задача 7

Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).
import os
import hashlib
from collections import defaultdict

def hash_file(file_path):
    """Возвращает хеш файла."""
    hasher = hashlib.md5()  # Используем MD5 для хеширования
    try:
        with open(file_path, 'rb') as f:
            while chunk := f.read(8192):
                hasher.update(chunk)
        return hasher.hexdigest()
    except Exception as e:
        print(f"Ошибка при открытии файла {file_path}: {e}")
        return None

def find_duplicates(directory):
    """Находит дубликаты файлов в заданном каталоге."""
    file_hashes = defaultdict(list)

    for root, _, files in os.walk(directory):
        for file in files:
            file_path = os.path.join(root, file)
            file_hash = hash_file(file_path)
            if file_hash:
                file_hashes[file_hash].append(file_path)

    # Выводим дубликаты
    duplicates = {hash_value: paths for hash_value, paths in file_hashes.items() if len(paths) > 1}
    return duplicates

if __name__ == "__main__":
    path_to_search = input("Введите путь для поиска дубликатов: ")
    duplicates = find_duplicates(path_to_search)

    if duplicates:
        print("Найденные дубликаты:")
        for hash_value, paths in duplicates.items():
            print(f"\nХеш: {hash_value}")
            for path in paths:
                print(f"  {path}")
    else:
        print("Дубликаты не найдены.")
        Мы создали 2 файла: 12.txt и 22.txt имеющие следующие строки: "12345" и "123456"



![image](https://github.com/user-attachments/assets/211e3b6c-ffe8-43e7-b06b-c6d25cc988d9)
## Задача 8
![image](https://github.com/user-attachments/assets/ed4fe389-b4a2-4a82-ae6a-1b33b6070c93)

![image](https://github.com/user-attachments/assets/bb3fe2bd-5122-4dc4-bf8e-5a5b7080aa8d)

Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.
import os
import tarfile
import argparse

def find_files_with_extension(directory, extension):
    """Находит все файлы с заданным расширением в указанном каталоге."""
    matched_files = []
    for root, _, files in os.walk(directory):
        for file in files:
            if file.endswith(extension):
                matched_files.append(os.path.join(root, file))
    return matched_files

def create_tar_archive(files, archive_name):
    """Создает архив tar из списка файлов."""
    with tarfile.open(archive_name, 'w') as tar:
        for file in files:
            tar.add(file, arcname=os.path.relpath(file, start=os.path.dirname(files[0])))

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Архивирует файлы с заданным расширением.")
    parser.add_argument("directory", help="Путь к каталогу для поиска файлов.")
    parser.add_argument("extension", help="Расширение файлов для архивации (например, .txt).")
    parser.add_argument("archive_name", help="Имя выходного архива (например, archive.tar).")
    
    args = parser.parse_args()

    files_to_archive = find_files_with_extension(args.directory, args.extension)

    if files_to_archive:
        create_tar_archive(files_to_archive, args.archive_name)
        print(f"Архив '{args.archive_name}' успешно создан с {len(files_to_archive)} файлами.")
    else:
        print("Файлы с указанным расширением не найдены.")





        
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
