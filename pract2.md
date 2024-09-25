### Задача 1: Вывести служебную информацию о пакете matplotlib

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



### Задача 2:  Вывести служебную информацию о пакете express (JavaScript)
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


**Задача 3**:
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

   после запуска скрипта появляется  png - шник

    ![image](https://github.com/user-attachments/assets/d1d54ed9-4b33-4da0-b713-b5c0b6fe4a45)


