Вариант №19



Задание №1
Разработать эмулятор для языка оболочки ОС. Необходимо сделать работу
эмулятора как можно более похожей на сеанс shell в UNIX-подобной ОС.
Эмулятор должен запускаться из реальной командной строки, а файл с
виртуальной файловой системой не нужно распаковывать у пользователя.
Эмулятор принимает образ виртуальной файловой системы в виде файла формата
zip. Эмулятор должен работать в режиме CLI.
Конфигурационный файл имеет формат xml и содержит:
• Имя пользователя для показа в приглашении к вводу.
• Имя компьютера для показа в приглашении к вводу.
• Путь к архиву виртуальной файловой системы.
• Путь к стартовому скрипту.
Стартовый скрипт служит для начального выполнения заданного списка
команд из файла.
Необходимо поддержать в эмуляторе команды ls, cd и exit, а также
следующие команды:
1. cp.
2. uname.
Все функции эмулятора должны быть покрыты тестами, а для каждой из
поддерживаемых команд необходимо написать 3 теста.



Задание №2
Разработать инструмент командной строки для визуализации графа
зависимостей, включая транзитивные зависимости. Сторонние программы или
библиотеки для получения зависимостей использовать нельзя.
Зависимости определяются для git-репозитория. Для описания графа
зависимостей используется представление PlantUML. Визуализатор должен
выводить результат на экран в виде кода.
94
Построить граф зависимостей для коммитов, в узлах которого содержатся
сообщения. Граф необходимо строить для ветки с заданным именем.
Конфигурационный файл имеет формат xml и содержит:
• Путь к программе для визуализации графов.
• Путь к анализируемому репозиторию.
• Путь к файлу-результату в виде кода.
• Имя ветки в репозитории.
Все функции визуализатора зависимостей должны быть покрыты тестами.



Задание №3

Разработать инструмент командной строки для учебного конфигурационного
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
95
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



Задание №4
Разработать ассемблер и интерпретатор для учебной виртуальной машины
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


![Снимок экрана 2024-10-09 163607](https://github.com/user-attachments/assets/922056af-bdae-4958-9b40-e4d0c7d7f0a3)



![image](https://github.com/user-attachments/assets/bc2c808f-34d8-43ca-b5d5-acd566e26432)






