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
        ![image](https://github.com/user-attachments/assets/10d67179-18b7-4895-8759-e491652933b1)

## Задача 8

Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.

## Задача 9

Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.

## Задача 10

Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром. 

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
