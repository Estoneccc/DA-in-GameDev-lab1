# РАЗРАБОТКА СИСТЕМЫ МАШИННОГО ОБУЧЕНИЯ [in GameDev]
Отчет по лабораторной работе #2 выполнил:
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
Ознакомиться с основными операторами языка Python на примере реализации линейной регрессии.

## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity.
Ход работы:
1. Создайте новый пустой 3D проект на Unity. Скачайте папку с ML агентом. Вы найдете ее в облаке с исходными файлами к лабораторной работе – ml-agents-release_19. В созданный проект добавьте ML Agent, выбрав Window - Package Manager - Add Package from disk.
- Последовательно добавьте .json – файлы:
- ml-agents-release_19 / com,unity.ml-agents / package.json
- ml-agents-release_19 / com,unity.ml-agents.extensions / package.json

![Снимок экрана (86)](https://user-images.githubusercontent.com/103362515/198092096-a6076eeb-d91a-4b09-b378-db4c640e7be7.png)

Если все сделано правильно, то во вкладке с компонентами (Components) внутри Unity вы увидите строку ML Agent.

![Снимок экрана (87)](https://user-images.githubusercontent.com/103362515/198092738-969946be-74eb-48b1-822a-3744e69d1535.png)

2. Далее пишем серию команд для создания и активации нового ML-агента, а также для скачивания необходимых библиотек:
- mlagents 0.28.0;
- torch 1.7.1;

![Снимок экрана (88)](https://user-images.githubusercontent.com/103362515/198093121-38ad6a4d-dedf-47e3-8f47-1f46e7e86ada.png)

![Снимок экрана (89)](https://user-images.githubusercontent.com/103362515/198093153-8e0f73a3-0c37-4d00-a257-985525901845.png)

3. Создайте на сцене плоскость, куб и сферу. Создайте простой C# скрипт-файл и подключите его к сфере:
- В скрипт-файл RollerAgent.cs добавьте код, опубликованный в материалах лабораторных работ
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8 - 4, 0.5f, Random.value * 8 - 4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if (distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```
![Снимок экрана (90)](https://user-images.githubusercontent.com/103362515/198094441-479cd5f6-df6f-4992-8093-e0ce09948146.png)

- Объекту «сфера» добавить компоненты Rigidbody, Decision Requester, Behavior Parameters и настройте их:

![Снимок экрана (91)](https://user-images.githubusercontent.com/103362515/198094525-04438ae8-e5d2-4713-b3d2-1e1c621bbf27.png)

4. В корень проекта добавьте файл конфигурации нейронной сети, доступный в папке с файлами проекта.
```c#
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```
- Запустите работу ml-агента

![Снимок экрана (93)](https://user-images.githubusercontent.com/103362515/198095337-e439fe07-e455-4f01-a779-13fdda2f58cf.png)

- Вернитесь в проект Unity, запустите сцену, проверьте работу ML-Agent’a. Сделайте 3, 9, 27 копий модели «Плоскость-Сфера-Куб», запустите симуляцию сцены и наблюдайте за результатом обучения модели.

![Снимок экрана (94)](https://user-images.githubusercontent.com/103362515/198095621-484763fa-5259-41c3-b4d4-a0de2b5e8f6c.png)
![Снимок экрана (95)](https://user-images.githubusercontent.com/103362515/198095644-5742c00a-78ed-4d51-b296-a2a737615457.png)
![Снимок экрана (96)](https://user-images.githubusercontent.com/103362515/198095661-531c72e2-855d-4b96-8cef-7d35edd15f27.png)

- После завершения обучения проверьте работу модели.

![Снимок экрана (98)](https://user-images.githubusercontent.com/103362515/198096482-1b452d19-6fef-4fc5-a0c1-9441b35d95dc.png)
![Снимок экрана (99)](https://user-images.githubusercontent.com/103362515/198096514-08b39042-62ea-424a-bcfc-60f74b95373f.png)

- Выводы: 
## Задание 2
### В разделе "ход работы" пошагово выполнять каждый пункт с описанием и примером реализации задачи по теме лабораторной работы.
### Ход работы.



## Задание 3
### Изучить код на Python и ответить на вопросы:

1. Должна ли величина loss стремиться к нулю при изменении исходных данных? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ.

Величина loss не стремится к нулю при изменении исходных данных.

![Снимок экрана (69)](https://user-images.githubusercontent.com/103362515/192792518-711254f6-2a4b-4200-848b-ea652e663789.png)
![Снимок экрана (70)](https://user-images.githubusercontent.com/103362515/192792553-348fc5c3-239f-4ddf-a79e-3e9139583154.png)

2. Какова роль параметра Lr? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ. В качестве эксперимента можете изменить значение параметра.

В зависимости от параметра Lr меняется угол наклона графика и его направление, либо он растет, либо убывает.

![Снимок экрана (71)](https://user-images.githubusercontent.com/103362515/192794183-a645cbbe-4d3d-42a8-840a-d94dfbcdc27c.png)
![Снимок экрана (72)](https://user-images.githubusercontent.com/103362515/192794212-80179a9b-1bfc-4bd8-9718-56d8ba6ad8ec.png)


## Выводы
В ходе проделанной работы я получил навыки работы с Unity, google.colab, Anaconda. Ознакомился с основными операторами языка Python на примере реализации линейной регрессии.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
