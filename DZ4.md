Задание 4:

Разработать ассемблер и интерпретатор на python для учебной виртуальной машины 
(УВМ). Система команд УВМ представлена далее.
Для ассемблера необходимо разработать читаемое представление команд
УВМ. Ассемблер принимает на вход файл с текстом исходной программы, путь к
которой задается из командной строки. Результатом работы ассемблера является
бинарный файл в виде последовательности байт, путь к которому задается из
командной строки. Дополнительный ключ командной строки задает путь к файлулогу, в котором хранятся ассемблированные инструкции в духе списков
“ключ=значение”, как в приведенных далее тестах.
Интерпретатор принимает на вход бинарный файл, выполняет команды УВМ
и сохраняет в файле-результате значения из диапазона памяти УВМ. Диапазон
также указывается из командной строки.
Форматом для файла-лога и файла-результата является csv.
Необходимо реализовать приведенные тесты для всех команд, а также
написать и отладить тестовую программу.

A B C
Биты 0—3 Биты 4—10 Биты 11—24
15 Адрес Константа


Размер команды: 5 байт. Операнд: поле C. Результат: регистр по адресу,
которым является поле B.
Тест (A=15, B=9, C=265):
0x9F, 0x48, 0x08, 0x00, 0x00
Чтение значения из памяти


A B C
Биты 0—3 Биты 4—10 Биты 11—35
8 Адрес Адрес


Размер команды: 5 байт. Операнд: значение в памяти по адресу, которым
является поле C. Результат: регистр по адресу, которым является поле B.
Тест (A=8, B=94, C=962):
0xE8, 0x15, 0x1E, 0x00, 0x00
Запись значения в память


A B C
Биты 0—3 Биты 4—10 Биты 11—35
7 Адрес Адрес


Размер команды: 5 байт. Операнд: регистр по адресу, которым является поле
B. Результат: значение в памяти по адресу, которым является поле C.
Тест (A=7, B=69, C=841):
0x57, 0x4C, 0x1A, 0x00, 0x00
 

Бинарная операция: вычитание
A B C D

Биты 0—3 Биты 4—10 Биты 11—17 Биты 18—33
9 Адрес Адрес Смещение

Размер команды: 5 байт. Первый операнд: регистр по адресу, которым
является поле B. Второй операнд: значение в памяти по адресу, которым является
сумма адреса (регистр по адресу, которым является поле C) и смещения (поле D).
Результат: регистр по адресу, которым является поле B.
Тест (A=9, B=115, C=69, D=667):
0x39, 0x2F, 0x6E, 0x0A, 0x00
Тестовая программа
Выполнить поэлементно операцию вычитание над вектором длины 7 и числом
156. Результат записать в исходный вектор.


