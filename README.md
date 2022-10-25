# СБОР, ОБРАБОТКА И ВИЗУАЛИЗАЦИЯ ТЕСТОВОГО НАБОРА ДАННЫХ [in GameDev]
Отчет по лабораторной работе #2 выполнил:
- Кутявин Данил Сергеевич
- РИ-210945

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity.

## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity. 
Ход работы:
- В облачном сервисе google console подключить API для работы с google sheets и google drive.

![Снимок экрана (77)](https://user-images.githubusercontent.com/103362515/195139567-bef0b2a6-693f-4952-bb05-a02f4255a246.png)
![Снимок экрана (76)](https://user-images.githubusercontent.com/103362515/195139613-799a3ded-ebb3-4b7d-8bff-fe1b0e4c52cb.png)

- Реализовать запись данных из скрипта на python в google-таблицу. Данные описывают изменение темпа инфляции на протяжении 11 отсчётных периодов, с учётом стоимости игрового объекта в каждый период.

![Снимок экрана (78)](https://user-images.githubusercontent.com/103362515/195142763-093755d4-e56b-46f5-9672-e07645ffa891.png)
![Снимок экрана (79)](https://user-images.githubusercontent.com/103362515/195142784-8ff146e1-ac51-432a-8506-cd2b2dad33da.png)
![Снимок экрана (80)](https://user-images.githubusercontent.com/103362515/195142810-aa4af7eb-ba63-44c2-823f-e3b1c19bbda0.png)

- Создать новый проект на Unity, который будет получать данные из google-таблицы, в которую были записаны данные в предыдущем пункте.

![Снимок экрана (81)](https://user-images.githubusercontent.com/103362515/195143134-da65d7a7-289e-4c3e-9629-8ca64502b4f5.png)
![Снимок экрана (83)](https://user-images.githubusercontent.com/103362515/195143419-e490eaad-a9d8-431d-ac80-47cb8a2e892a.png)
![Снимок экрана (82)](https://user-images.githubusercontent.com/103362515/195143166-654dfea5-b2a5-47dc-8f52-c74072fb6a31.png)


## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1. 
 Ход работы.
```py
import numpy as np
import gspread

x = [3, 21, 22, 34, 54, 34, 55, 67, 89, 99]
x = np.array(x)
y = [2, 22, 24, 65, 79, 82, 55, 130, 150, 199]
y = np.array(y)

def model(a, b, x):
    return a * x + b


def loss_function(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    return (0.5 / num) * (np.square(prediction - y)).sum()

def optimize(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    da = (1.0 / num) * ((prediction - y) * x).sum()
    db = (1.0 / num) * ((prediction - y).sum())
    a = a - Lr * da
    b = b - Lr * db
    return a, b

def iterate(a, b, x, y, times):
    for i in range(times):
        a, b = optimize(a, b, x, y)
    return a, b

a = np.random.rand(1)
b = np.random.rand(1)
Lr = 0.000001

gc = gspread.service_account(filename='unitydatascience-365017-3a71cb3a7da2.json')
sh = gc.open("UnitySheets")
price = np.random.randint(2000, 10000, 11)
mon = list(range(1, 11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        a, b = iterate(a, b, x, y, 100)
        prediction = model(a, b, x)
        loss = loss_function(a, b, x, y)
        tempInf = str(loss)
        tempInf = tempInf.replace('.', ',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)
```
![Снимок экрана (84)](https://user-images.githubusercontent.com/103362515/195164670-806c0ffc-7be2-4bb3-ae58-e36ca107ad9d.png)


## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2.
Ход работы.
```py
if (dataSet["Mon_" + i.ToString()] <= 300 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 300 & dataSet["Mon_" + i.ToString()] < 1000 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] >= 1000 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
```

![Снимок экрана (85)](https://user-images.githubusercontent.com/103362515/195168653-39cc00e2-cc9e-4ea4-98dc-f9f705c8956b.png)


## Выводы
В ходе проделанной работе я получил навыки в связи Python - Google-Sheets – Unity, мне понравилось анализировать данные и применять это в практике.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
