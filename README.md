# ПРИМЕНЕНИЕ СИСТЕМ ИСКУССТВЕННОГО ИНТЕЛЛЕКТА В ИГРАХ И ИНТЕРАКТИВНЫХ ПРИЛОЖЕНИЯХ. ИНТЕГРАЦИЯ ОБУЧЕННОЙ СИСТЕМЫ МАШИННОГО ОБУЧЕНИЯ В ИГРОВОЙ ПРОЕКТ. [in GameDev]
Отчет по лабораторной работе #4 выполнил:
- Кутявин Данил Сергеевич
- РИ-210945
Отметка о выполнении заданий (заполняется студентом):

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
### Познакомиться с программными средствами для создания системы искусственного интеллекта и их применение совместно с Unity.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR.
### Ход работы:

1. Обучаю перцептрон операции OR:

![Снимок экрана (121)](https://user-images.githubusercontent.com/103362515/205010984-2da86e89-060e-4858-9ef9-3396c447ed57.png)
    На данном снимке видно, что перцептрон научился вычислять после 3х эпох обучения.

2. Обучаю перцептрон операции AND:
![Снимок экрана (122)](https://user-images.githubusercontent.com/103362515/205011526-e718b71c-7434-4034-8a0d-10f471c54420.png)
   На данном снимке видно, что перцептрон научился вычислять после 6и эпох обучения.

3. Обучаю перцептрон операции NAND:
![Снимок экрана (123)](https://user-images.githubusercontent.com/103362515/205011751-c8e905bb-488e-4d78-9e51-dd9cc038467c.png)
   На данном снимке видно, что перцептрон научился вычислять после 7и эпох обучения.

4. Обучаю перцептрон операции XOR:
![Снимок экрана (124)](https://user-images.githubusercontent.com/103362515/205012042-098050fb-600b-4c80-8559-d0f4312c9628.png)
   На данном снимке видно, что перцептрон не может научиться производить вычисления даже после 1000 эпох обучения для данной операции, так как она не является линейной.

## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.
### Ход работы:
1. График зависимости при операции OR:

![image](https://user-images.githubusercontent.com/103362515/205036549-f2206d04-2340-4e1c-acd9-cf7e02269b35.png)

2. График зависимости при операции AND:

![image](https://user-images.githubusercontent.com/103362515/205037902-d1f9404d-2304-4a86-9eba-ccedab81b080.png)




## Задание 3
### Изучить код на Python и ответить на вопросы:


## Выводы


## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
