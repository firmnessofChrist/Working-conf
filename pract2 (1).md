# Практическое занятие №2. Менеджеры пакетов

П.Н. Советов, РТУ МИРЭА

Разобраться, что представляет собой менеджер пакетов, как устроен пакет, как читать версии стандарта semver. Привести примеры программ, в которых имеется встроенный пакетный менеджер.

## Задача 1

 скрипт на python:

import matplotlib
import pkg_resources

def print_package_info(package_name):
    try:
        # Получаем информацию о пакете
        package = pkg_resources.get_distribution(package_name)

        print(f"Имя пакета: {package.project_name}")
        print(f"Версия: {package.version}")
        print(f"Местоположение: {package.location}")
        print(f"Зависимости: {package.requires()}")
    except pkg_resources.DistributionNotFound:
        print(f"Пакет '{package_name}' не найден.")

if __name__ == "__main__":
    package_name = "matplotlib"
    print_package_info(package_name)

    # Вывод версии напрямую из matplotlib
    print(f"Версия matplotlib: {matplotlib.__version__}")

Импорт библиотек: Мы импортируем matplotlib и pkg_resources.
Получение информации: Используем pkg_resources.get_distribution() для получения информации о пакете.
Вывод данных: Выводим имя пакета, его версию, местоположение и зависимости.
Вывод версии напрямую: Мы также выводим версию matplotlib, используя matplotlib.__version__.


![image](https://github.com/user-attachments/assets/402dc920-d1c5-444c-8090-b842d4c8d82c)


Если вы хотите установить matplotlib без использования менеджера пакетов, вы можете скачать пакет из репозитория PyPI и установить его вручную. Вот шаги, которые можно выполнить:

Перейдите на страницу PyPI для matplotlib: matplotlib на PyPI
Скачайте пакет:
На странице выберите нужную версию и скачайте .tar.gz или .whl файл.
Распакуйте архив: Если вы скачали .tar.gz, распакуйте его:


![image](https://github.com/user-attachments/assets/d91e26e1-abeb-411f-83e5-988739c25bb8)


Перейдите в директорию: Зайдите в распакованную директорию cd matplotlib-X.Y.Z
и Установите пакет: Выполните установку с помощью команды:python3 setup.py install



## Задача 2

создание новой директории проекта: 
mkdir express_info
cd express_info

инициализируем пакет: 


![image](https://github.com/user-attachments/assets/4d4f116d-ae47-4b5d-a253-ac4f20cf1c03)


устанавливаем: npm install express

создание файла: nano express_info.js

код крипта:

const express = require('express');

// Получаем информацию о пакете express
const packageInfo = require('./node_modules/express/package.json');

console.log(`Имя пакета: ${packageInfo.name}`);
console.log(`Версия: ${packageInfo.version}`);
console.log(`Описание: ${packageInfo.description}`);
console.log(`Главный файл: ${packageInfo.main}`);
console.log(`Лицензия: ${packageInfo.license}`);
console.log(`Авторы: ${packageInfo.author}`);
console.log(`Зависимости: ${JSON.stringify(packageInfo.dependencies, null, 2)}`);

запуск скрипта: node express_info.js


![image](https://github.com/user-attachments/assets/aa07db6c-0e96-4ae1-bd8e-f4ef15846269)

## Задача 3

Сформировать graphviz-код и получить изображения зависимостей matplotlib и express.

код скрипта:

import sys
import re
import requests
from graphviz import Digraph


def get_dependencies(package_name):
    dependencies = set()
    try:
        response = requests.get(f"https://pypi.org/pypi/{package_name}/json")
        if response.status_code == 200:
            data = response.json()
            info = data.get("info")
            requires_dist = info.get("requires_dist", [])

            for dist in requires_dist:
                if 'extra' not in dist:
                    dependency = extract_package_name(dist)
                    dependencies.add(dependency)
        else:
            print(f"Пакет {package_name} не найден на PyPI!")
            sys.exit(1)
    except Exception:
        print("", end="")

    return dependencies


def extract_package_name(dependency):

    match = re.match(r"([a-zA-Z0-9._-]+)", dependency)
    if match:
        return match.group(1).strip()
    return None


def create_dependency_graph(package_name):
    graph = Digraph(format='png')
    # visited = set()

    def add_dependencies(package_name):
        dependencies = get_dependencies(package_name)
        for dependency in dependencies:
            # if dependency not in visited:
            #     visited.add(dependency)
            graph.node(dependency)
            graph.edge(package_name, dependency)
            add_dependencies(dependency)

    graph.node(package_name)
    add_dependencies(package_name)
    return graph


if __name__ == '__main__':
    if len(sys.argv) != 2:
        print("Incorrect arguments!")
        sys.exit(1)

    print("Running dependencies_graph.py...")
    package_name = sys.argv[1]
    dependency_graph = create_dependency_graph(package_name)
    print(dependency_graph.source)
    dependency_graph.render('dependencies_graph')

    import graphviz

# Создаем граф для express
dot = graphviz.Digraph(comment='Express Dependencies', format='png')

# Основной узел
dot.node('A', 'express')

# Зависимости express
dependencies = [
    'body-parser',
    'cookie-parser',
    'express-session',
    'multer',
    'morgan',
    'serve-favicon',
    'compression',
]

# Добавляем узлы для зависимостей
for dep in dependencies:
    dot.node(dep, dep)

# Добавляем связи
for dep in dependencies:
    dot.edge('A', dep)

# Отображаем граф
dot.render('express_dependencies_graph', cleanup=True)
dot.view()


   после запуска скрипта появляется  png - шник




![image](https://github.com/user-attachments/assets/d1d54ed9-4b33-4da0-b713-b5c0b6fe4a45)


![image](https://github.com/user-attachments/assets/6e18a565-e787-482c-815c-284a38701dd9)





## Задача 4

**Следующие задачи можно решать с помощью инструментов на выбор:**

* Решатель задачи удовлетворения ограничениям (MiniZinc).
* SAT-решатель (MiniSAT).
* SMT-решатель (Z3).

Изучить основы программирования в ограничениях. Установить MiniZinc, разобраться с основами его синтаксиса и работы в IDE.

Решить на MiniZinc задачу о счастливых билетах. Добавить ограничение на то, что все цифры билета должны быть различными (подсказка: используйте all_different). Найти минимальное решение для суммы 3 цифр.

Определение переменных: Мы будем использовать 6 переменных для представления цифр в шестизначном номере билета.
Ограничение на сумму: Сумма первых трех переменных должна быть равна сумме последних трех.
Ограничение на уникальность: Все 6 цифр должны быть различными

код скрипта:

include "globals.mzn"; 
 
array[1..6] of var 0..9: digits; 
constraint all_different(digits); 
 
var int: sum_first = sum(digits[1..3]); 
var int: sum_last = sum(digits[4..6]); 
 
constraint sum_first = sum_last; 
solve minimize sum_first;



после запуска модели мы видим ответ на задачу " Найти минимальное решение для суммы 3 цифр."



![image](https://github.com/user-attachments/assets/b984b36e-9980-4379-b65b-93a223e51047)





## Задача 5

Решить на MiniZinc задачу о зависимостях пакетов для рисунка, приведенного ниже.

![](images/pubgrub.png)

## Задача 6

Решить на MiniZinc задачу о зависимостях пакетов для следующих данных:

% Определение всех пакетов и их зависимостей
enum PACKAGES = {root, foo1_0, foo1_1, left, right, shared1_0, shared2_0, target1_0, target2_0};

% Зависимости пакетов
array[PACKAGES] of set of PACKAGES: dependencies = [
    {foo1_0, target2_0}, % root 1.0.0
    {left, right},       % foo 1.1.0
    {},                  % foo 1.0.0
    {shared1_0},        % left 1.0.0
    {},                  % right 1.0.0
    {target1_0},        % shared 2.0.0
    {},                  % shared 1.0.0
    {},                  % target 2.0.0
    {}                   % target 1.0.0
];

% Массив для хранения установленных пакетов
array[PACKAGES] of var bool: installed;

% Ограничения на зависимости
constraint forall(p in PACKAGES) (
    installed[p] -> forall(dep in dependencies[p]) (installed[dep])
);

% Устанавливаем корневой пакет
constraint installed[root] = true;

% Ограничения на версии пакетов
constraint installed[left] = true; % Установим left
constraint installed[right] = false; % Не устанавливаем right

% Найдем решение, минимизируя количество установленных пакетов
solve minimize sum(installed);

% Выводим решение
output [
    "Установленные пакеты: ", 
    show([p | p in PACKAGES where installed[p]]), 
    "\n",
    "Left установлен: ", show(installed[left]), "\n",
    "Right установлен: ", show(installed[right]), "\n"
];


Определение пакетов: Мы определяем перечисление PACKAGES, в котором перечислены все пакеты.
Зависимости: Мы создаем массив dependencies, где для каждого пакета указаны его зависимости.
Установленные пакеты: Мы используем переменные типа bool, чтобы обозначить, установлен пакет или нет.
Ограничения: Указываем, что если пакет установлен, то все его зависимости также должны быть установлены.
Установка корневого пакета: Корневой пакет root должен быть установлен.
Поиск решения: Мы решаем задачу, минимизируя количество установленных пакетов.


![image](https://github.com/user-attachments/assets/4083b13a-7d7b-4305-bd36-ea55a5140435)




## Задача 7

Представить задачу о зависимостях пакетов в общей форме. Здесь необходимо действовать аналогично реальному менеджеру пакетов. То есть получить описание пакета, а также его зависимости в виде структуры данных. Например, в виде словаря. В предыдущих задачах зависимости были явно заданы в системе ограничений. Теперь же систему ограничений надо построить автоматически, по метаданным.

шаги выпронения:

Определить структуру данных: Создать словарь, где каждый пакет будет представлен своим именем, а значение будет еще одним словарем с метаданными (версии и зависимости).
Сформировать систему ограничений: На основе зависимостей, указанных в словаре, автоматически генерировать ограничения, аналогично тому, как это делает менеджер пакетов.

скрипт программы:

% Перечисление пакетов
enum PACKAGES = {root, foo_1_0_0, foo_1_1_0, left, right, shared_1_0_0, shared_2_0_0, target_1_0_0, target_2_0};

% Структура данных: версии и зависимости пакетов
array[PACKAGES] of int: version = 
    [100, 100, 110, 100, 100, 100, 200, 100, 200]; % Представление версий в числовом формате

array[PACKAGES] of set of PACKAGES: dependencies = 
    [  {foo_1_0_0, target_2_0}, % root 1.0.0
       {left, right},          % foo 1.1.0
       {},                     % foo 1.0.0
       {shared_1_0_0},        % left 1.0.0
       {shared_1_0_0},        % right 1.0.0
       {},                     % shared 2.0.0
       {target_1_0_0},        % shared 1.0.0
       {},                     % target 2.0.0
       {}];                   % target 1.0.0

% Массив для хранения установленных пакетов
array[PACKAGES] of var bool: installed;

% Ограничения на зависимости
constraint
    forall(p in PACKAGES) (
        installed[p] -> forall(dep in dependencies[p]) (installed[dep])
    );

% Устанавливаем корневой пакет
constraint installed[root] = true;

% Найдем решение, минимизируя количество установленных пакетов
solve minimize sum(installed);

% Выводим решение
output [
    "Установленные пакеты: ", show([p | p in PACKAGES where installed[p]]), "\n"
];


что будет выводить данная программа:

![image](https://github.com/user-attachments/assets/cadccb80-99ae-48c2-85f9-e7f3e037a2ab)




## Полезные ссылки

Semver: https://devhints.io/semver

Удовлетворение ограничений и программирование в ограничениях: http://intsys.msu.ru/magazine/archive/v15(1-4)/shcherbina-053-170.pdf

Скачать MiniZinc: https://www.minizinc.org/software.html

Документация на MiniZinc: https://www.minizinc.org/doc-2.5.5/en/part_2_tutorial.html

Задача о счастливых билетах: https://ru.wikipedia.org/wiki/%D0%A1%D1%87%D0%B0%D1%81%D1%82%D0%BB%D0%B8%D0%B2%D1%8B%D0%B9_%D0%B1%D0%B8%D0%BB%D0%B5%D1%82
