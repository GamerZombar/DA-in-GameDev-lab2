# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Романов Вадим Юрьевич
- РИ210950
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
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity

## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity. При выполнении задания используйте видео-материалы и исходные данные, предоставленные преподавателя курса.
Ход работы:
- **Собсна, результатом работы является чуть-ли не точная копия дейсвий преподавателя курса, поэтому код из видео-инструкции предоставлять не буду, в связи его громоздкости, но скриншоты основных дейсвий прилагаю ниже:**

![Screenshot_2](https://user-images.githubusercontent.com/58142149/194958895-c899c5aa-a495-40c5-82b1-3c662efbb814.png)
![Screenshot_4](https://user-images.githubusercontent.com/58142149/194959054-c632179e-3e7f-4626-8bc6-8f83dd5780fa.png)
![Screenshot_1](https://user-images.githubusercontent.com/58142149/194959066-1ffa18d1-fbc2-4c0b-a349-a118e362d780.png)
![Screenshot_3](https://user-images.githubusercontent.com/58142149/194959077-12401419-b148-45c7-9929-4928d9d45941.png)


____


## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1
Ход работы:
- Создал новую гугл-таблицу "LinearRegression" и изменил код линейной регрессии из Лабораторной работы №1, чтобы данные не только выводились на экран, но и записывались в гугл-таблицу.
```py
import numpy as np
import gspread
# import matplotlib.pyplot as plt  #  За ненадобностью - убрал


def model(a, b, x):
    return a*x+b


def loss_function(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    return (0.5/num) * (np.square(prediction - y)).sum()


def optimize(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    da = (1.0/num) * ((prediction - y)*x).sum()
    db = (1.0/num) * ((prediction - y).sum())
    a -= Lr*da
    b -= Lr*db
    return a, b


def iterate(a, b, x, y, times):
    for i in range(times):
        a, b = optimize(a, b, x, y)
    return a, b


a = np.random.rand(1)
print(a)
b = np.random.rand(1)
print(b)
Lr = 0.000001

x = np.array([3, 21, 22, 34, 54, 34, 55, 67, 89, 99])
y = np.array([2, 22, 24, 65, 79, 82, 55, 130, 150, 199])


a, b = iterate(a, b, x, y, 100)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
# plt.scatter(x, y)
# plt.plot(x, prediction)
# plt.show()


gc = gspread.service_account(filename='versatile-now-365110-a9b1366ab2f0.json')
sh = gc.open("LinearRegression")
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
        sh.sheet1.update(('B' + str(i)), str(tempInf))
        print(tempInf)
```

![Screenshot_5](https://user-images.githubusercontent.com/58142149/194960207-dc73c247-09ad-4869-a899-046f8f57520b.png)


____

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2
Ход работы:
- С нуля уж не стал делать одно и то же, поэтому просто изменил код скрипта, отвечающий за воспроизведение звуков, в зависимости от данных. 
```cs
     void Update()
    {
        if (dataSet.Count == 0 || dataSet.Count <= i){
            return;
        }
        
        if (dataSet["Mon_" + i.ToString()] <= 200 & statusStart == false & 1 != dataSet.Count){
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 200 & dataSet["Mon_" + i.ToString()] < 1000 & statusStart == false & 1 != dataSet.Count){
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        
        if (dataSet["Mon_" + i.ToString()] >= 1000 & statusStart == false & 1 != dataSet.Count){
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1uGsokpbfe9YWQUjgl2YnhgCUdCIHzmnJGHz_dhqLyhQ/values/Лист1?key=AIzaSyAX8ysCLFqIPEyPKk8-n1CFU4pRrW5gpiQ");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[1]));  // selectRow[2] -> selectRow[1]
        }
    }
```
![image](https://user-images.githubusercontent.com/58142149/194961982-3ef2dbaf-9789-46e0-9da4-b7535d8704c0.png)

____

## Выводы

- Интересная лаб. работа, с не менее интересной реализацией. Было здорово! Особенно, когда сразу пишешь на нескольких языках... это в то же время и странно, и очень кайфово. В общем, респектос за лабу!
- Отдельный лайк за диалоги из Stronghold Crusader =)
