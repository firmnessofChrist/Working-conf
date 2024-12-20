Задание 3:

итак, Разработать инструмент командной строки на python для учебного конфигурационного 
языка, синтаксис которого приведен далее. Этот инструмент преобразует текст из
входного формата в выходной. Синтаксические ошибки выявляются с выдачей
сообщений.
Входной текст на языке json принимается из файла, путь к которому задан
ключом командной строки. Выходной текст на учебном конфигурационном
языке попадает в стандартный вывод.
Однострочные комментарии:
; Это однострочный комментарий
Словари:
{
 имя = значение,
 имя = значение,
 имя = значение,
 ...
}
Имена:
[_a-zA-Z][_a-zA-Z0-9]*
Значения:
• Числа.
• Строки.
• Словари.
Строки:
@"Это строка"
Объявление константы на этапе трансляции:
set имя = значение
Вычисление константы на этапе трансляции:
#[имя]
Результатом вычисления константного выражения является значение.
Все конструкции учебного конфигурационного языка (с учетом их
возможной вложенности) должны быть покрыты тестами. Необходимо показать 3
примера описания конфигураций из разных предметных областей.

План решения данной задачи:

Шаг 1: Парсинг JSON-файла
Программа должна:

Чтение JSON-файла, путь к которому передаётся через аргументы командной строки.
Конвертация JSON-данных в формат конфигурационного языка.
Шаг 2: Обработка синтаксиса
Необходимо:

Обрабатывать значения, такие как числа, строки (со специальным синтаксисом @"...), и словари.
Реализовать конструкцию для объявления константы set имя = значение.
Поддерживать вычисление значения константы на этапе трансляции #[имя].
Включить поддержку однострочных комментариев.
Шаг 3: Валидация и обработка синтаксических ошибок
Проверка на корректность имён: имена должны соответствовать регулярному выражению [_a-zA-Z][_a-zA-Z0-9]*.
Обработка ошибок, если в JSON-файле есть значения или структуры, не соответствующие правилам.
Шаг 4: Тестирование
Программа должна покрываться тестами, включая:

Синтаксически корректные и некорректные примеры.
Проверку обработки чисел, строк, словарей и констант.


для решения данной задачи были реализованы следующие скрипты:



![image](https://github.com/user-attachments/assets/578b2112-5aa6-4261-ac20-ea59182626e8)



переходим к нашему проекту и проверяем json файл:


![image](https://github.com/user-attachments/assets/b4875d98-7e01-4617-b674-96558284fd2a)



теперь после удачной компиляции программы создаем входные данные в input.json



![image](https://github.com/user-attachments/assets/18b61980-bbb5-412d-b06b-48babb628fd9)




и видим вывод в файле output.json




![image](https://github.com/user-attachments/assets/49ac5935-25bb-4f19-bd67-dfecf85d2884)



итак, тест 1: конфигурация для веб сервиса:




![image](https://github.com/user-attachments/assets/93fdf172-fc25-427f-a4bc-6d5a20b374b8)




тест 2: Конфигурация игрового приложения




![image](https://github.com/user-attachments/assets/dda03a75-9e2d-4e55-a754-52e5d2e56626)




тест 3: Конфигурация для системы умного дома



![image](https://github.com/user-attachments/assets/0eb4952d-eda9-46dc-a166-56f0ea50513c)




было сделано три теста, проект полностью поддерживается.
