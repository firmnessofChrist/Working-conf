# Практическое занятие №3. Конфигурационные языки

П.Н. Советов, РТУ МИРЭА

Разобраться, что собой представляют программируемые конфигурационные языки (Jsonnet, Dhall, CUE).

## Задача 1

Реализовать на Jsonnet приведенный ниже пример в формате JSON. Использовать в реализации свойство программируемости и принцип DRY.
Итак, скрипт на Jsonnet будет выглядеть следующим образом:

// Определяем шаблон для групп
local groupPrefix = "ИКБО-";
local groupYear = "-20";

local groups = [groupPrefix + std.str(num) + groupYear for num in std.range(1, 24)];

// Определяем студентов
local students = [
  { age: 19, group: "ИКБО-4-20", name: "Иванов И.И." },
  { age: 18, group: "ИКБО-5-20", name: "Петров П.П." },
  { age: 18, group: "ИКБО-5-20", name: "Сидоров С.С." },
  { age: 20, group: "ИКБО-6-20", name: "Смирнов С.С." }  // Четвертый студент
];

// Основная структура
{
  groups: groups,
  students: students,
  subject: "Конфигурационное управление",
}

JSON - вывод для этого скрипта будет следующим:


![image](https://github.com/user-attachments/assets/e68c472c-6d60-4fc5-853e-baac02e34493)


![image](https://github.com/user-attachments/assets/7cdece35-7ccb-4486-8c3b-96c241e986ca)


![image](https://github.com/user-attachments/assets/f3b4799e-31d8-4394-89ea-accf9a48c33b)


Этот JSON выводит массив групп, студентов с их возрастом и группой, а также название предмета

## Задача 2

Реализовать на Dhall приведенный ниже пример в формате JSON. Использовать в реализации свойство программируемости и принцип DRY.

```
{
  "groups": [
    "ИКБО-1-20",
    "ИКБО-2-20",
    "ИКБО-3-20",
    "ИКБО-4-20",
    "ИКБО-5-20",
    "ИКБО-6-20",
    "ИКБО-7-20",
    "ИКБО-8-20",
    "ИКБО-9-20",
    "ИКБО-10-20",
    "ИКБО-11-20",
    "ИКБО-12-20",
    "ИКБО-13-20",
    "ИКБО-14-20",
    "ИКБО-15-20",
    "ИКБО-16-20",
    "ИКБО-17-20",
    "ИКБО-18-20",
    "ИКБО-19-20",
    "ИКБО-20-20",
    "ИКБО-21-20",
    "ИКБО-22-20",
    "ИКБО-23-20",
    "ИКБО-24-20"
  ],
  "students": [
    {
      "age": 19,
      "group": "ИКБО-4-20",
      "name": "Иванов И.И."
    },
    {
      "age": 18,
      "group": "ИКБО-5-20",
      "name": "Петров П.П."
    },
    {
      "age": 18,
      "group": "ИКБО-5-20",
      "name": "Сидоров С.С."
    },
    <добавьте ваши данные в качестве четвертого студента>
  ],
  "subject": "Конфигурационное управление"
} 
```

Мы имеем следующий скрипт на DHALL:

let Group = Text
let Student = { age : Natural, group : Group, name : Text }

let groups : List Group =
      let prefix = "ИКБО-"
      let year = "-20"
      List.map (λ(x: Natural) → prefix ++ Natural/show x ++ year) (List.range 1 25)

let students : List Student =
      [ { age = 19, group = "ИКБО-4-20", name = "Иванов И.И." }
      , { age = 18, group = "ИКБО-5-20", name = "Петров П.П." }
      , { age = 18, group = "ИКБО-5-20", name = "Сидоров С.С." }
      , { age = 20, group = "ИКБО-6-20", name = "Смирнов С.С." }  // Четвертый студент
      ]

in { groups = groups, students = students, subject = "Конфигурационное управление" }

Для этого скрипта json-вывод будет аналогичным со скриптом на Jsonnet:


![image](https://github.com/user-attachments/assets/360ad43e-39ee-4e9f-84a3-2a41bc90e296)


![image](https://github.com/user-attachments/assets/dc17d3ea-5620-42ad-a392-f6e56287377b)


![image](https://github.com/user-attachments/assets/70c1a675-5602-4f5b-b8d9-c06368619397)


Для решения дальнейших задач потребуется программа на Питоне, представленная ниже. Разбираться в самом языке Питон при этом необязательно.

```Python
import random


def parse_bnf(text):
    '''
    Преобразовать текстовую запись БНФ в словарь.
    '''
    grammar = {}
    rules = [line.split('=') for line in text.strip().split('\n')]
    for name, body in rules:
        grammar[name.strip()] = [alt.split() for alt in body.split('|')]
    return grammar


def generate_phrase(grammar, start):
    '''
    Сгенерировать случайную фразу.
    '''
    if start in grammar:
        seq = random.choice(grammar[start])
        return ''.join([generate_phrase(grammar, name) for name in seq])
    return str(start)


BNF = '''
E = a
'''

for i in range(10):
    print(generate_phrase(parse_bnf(BNF), 'E'))

```

Реализовать грамматики, описывающие следующие языки (для каждого решения привести БНФ). Код решения должен содержаться в переменной BNF:

## Задача 3

Язык нулей и единиц.

```
10
100
11
101101
000
```

Чтобы реализовать грамматику, описывающую язык нулей и единиц, мы можем использовать следующую БНФ (Бэкуса-Наура):

БНФ
makefile
Копировать код
S = 0 S | 1 S | 0 | 1
Эта грамматика означает, что строка может начинаться с 0 или 1, за которым следует снова строка, или может состоять только из одного символа 0 или 1.

имеем измененный код на Python:

import random


def parse_bnf(text):
    '''
    Преобразовать текстовую запись БНФ в словарь.
    '''
    grammar = {}
    rules = [line.split('=') for line in text.strip().split('\n')]
    for name, body in rules:
        grammar[name.strip()] = [alt.split() for alt in body.split('|')]
    return grammar


def generate_phrase(grammar, start):
    '''
    Сгенерировать случайную фразу.
    '''
    if start in grammar:
        seq = random.choice(grammar[start])
        return ''.join([generate_phrase(grammar, name) for name in seq])
    return str(start)


BNF = '''
S = 0 S | 1 S | 0 | 1
'''

for i in range(10):
    print(generate_phrase(parse_bnf(BNF), 'S'))

Символ S может быть заменён на 0 или 1, за которым следует снова S, или на просто 0 или 1, что позволяет генерировать строки из нулей и единиц любой длины.
Код генерирует десять случайных строк, состоящих из нулей и единиц, используя указанную грамматику.


![image](https://github.com/user-attachments/assets/74663209-7562-4f33-ab0c-cceb4ae8a94d)


Каждый запуск программы будет генерировать разные строки, так как выбор символов и их порядок происходит случайным образом. Вывод будет состоять из различных комбинаций 0 и 1, длина которых также будет варьироваться.

## Задача 4

Язык правильно расставленных скобок двух видов.

```
(({((()))}))
{}
{()}
()
{}
```

Чтобы описать язык правильно расставленных скобок двух видов (круглых () и фигурных {}), можно использовать следующую БНФ (Бэкуса-Наура):

БНФ
mathematica
Копировать код
E = ( E ) | { E } | ε
Эта грамматика означает, что строка может состоять из:

пары круглых скобок, содержащих другую корректную строку,
пары фигурных скобок, содержащих другую корректную строку,
или быть пустой строкой (ε).

мы имеем следующий измененный код на Python:

import random


def parse_bnf(text):
    '''
    Преобразовать текстовую запись БНФ в словарь.
    '''
    grammar = {}
    rules = [line.split('=') for line in text.strip().split('\n')]
    for name, body in rules:
        grammar[name.strip()] = [alt.split() for alt in body.split('|')]
    return grammar


def generate_phrase(grammar, start):
    '''
    Сгенерировать случайную фразу.
    '''
    if start in grammar:
        seq = random.choice(grammar[start])
        return ''.join([generate_phrase(grammar, name) for name in seq])
    return str(start)


BNF = '''
E = ( E ) | { E } | ε
'''

for i in range(10):
    print(generate_phrase(parse_bnf(BNF), 'E'))


![image](https://github.com/user-attachments/assets/28afdc02-ef59-4cba-af5a-85fcb8ecd258)


Каждый запуск программы будет генерировать разные корректные строки с учетом заданной грамматики.

## Задача 5

Язык выражений алгебры логики.

```
((~(y & x)) | (y) & ~x | ~x) & x
y & ~(y)
(~(y) & y & ~y)
~x
~((x) & y | (y) | (x)) & x | x | (y & ~y)
```

Чтобы описать язык выражений алгебры логики, можно использовать следующую БНФ (Бэкуса-Наура):

БНФ
mathematica
Копировать код
E = E | E & E | E | E | ~E | (E)
Эта грамматика описывает:

E — логическое выражение.
| — логическое "ИЛИ".
& — логическое "И".
~ — логическое "НЕ".
(E) — выражение в скобках.

Мы имеем измененный код программы на Python:

import random


def parse_bnf(text):
    '''
    Преобразовать текстовую запись БНФ в словарь.
    '''
    grammar = {}
    rules = [line.split('=') for line in text.strip().split('\n')]
    for name, body in rules:
        grammar[name.strip()] = [alt.split() for alt in body.split('|')]
    return grammar


def generate_phrase(grammar, start):
    '''
    Сгенерировать случайную фразу.
    '''
    if start in grammar:
        seq = random.choice(grammar[start])
        return ''.join([generate_phrase(grammar, name) for name in seq])
    return str(start)


BNF = '''
E = E | E & E | E | ~E | ( E ) | x | y
'''

for i in range(10):
    print(generate_phrase(parse_bnf(BNF), 'E'))
При выполнении кода могут быть получены разные комбинации логических выражений, например:


![image](https://github.com/user-attachments/assets/bb383483-83c2-439b-b245-3c02d10868a4)



## Полезные ссылки

Configuration complexity clock: https://mikehadlow.blogspot.com/2012/05/configuration-complexity-clock.html

Json: http://www.json.org/json-ru.html

Язык Jsonnet: https://jsonnet.org/learning/tutorial.html

Язык Dhall: https://dhall-lang.org/

Учебник в котором темы построения синтаксических анализаторов (БНФ, Lex/Yacc) изложены подробно: https://ita.sibsutis.ru/sites/csc.sibsutis.ru/files/courses/trans/LanguagesAndTranslationMethods.pdf

Полезные материалы для разработчика (очень рекомендую посмотреть слайды и прочие ссылки, все это актуально и для других тем нашего курса): https://habr.com/ru/company/JetBrains-education/blog/547768/
