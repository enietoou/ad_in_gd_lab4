# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил:
- Бобоев Азизджон Рахматович
- РИ220935
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

## Цель работы
Ознакомиться с основными операторами языка Python на примере реализации линейной регрессии.

## Задание 1
Выберите одну из компьютерных игр, приведите скриншот её геймплея и краткое описание концепта игры. Выберите одну из игровых переменных в игре (ресурсы, внутри игровая валюта, здоровье персонажей и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.
Игра: Elden Ring
![image](https://github.com/enietoou/ad_in_gd_lab2/assets/74960429/1d70302c-8d00-4ab3-b529-76823e1af2ba)

Описание концепта:
Elden Ring - это action RPG, разработанная компанией FromSoftware при участии писателя Хидетаки Миядзаки и писателя Джорджа Мартина. Игра предложит открытый мир, насыщенный опасностями и загадками, а также уникальную систему боя.

Выбранная игровая переменная: Руны (внутриигровая валюта)

Роль в игре:
Руны в Elden Ring служат внутриигровой валютой, необходимой для приобретения нового снаряжения, улучшения оружия, покупки зелий и многого другого. Игроки могут зарабатывать руны, убивая врагов, выполняя задания и исследуя игровой мир.

Условия изменения / появления и диапазон допустимых значений:
Руны могут появляться при убийстве врагов, завершении квестов, а также в результате продажи предметов. Диапазон допустимых значений рун может быть, например, от 1 до 100 000, в зависимости от сложности задач и уровня врагов.

Схема экономической модели:
![Без имени-1](https://github.com/enietoou/ad_in_gd_lab2/assets/74960429/e14aefe4-cc9e-4be5-abf5-1b988148dc88)


## Задание 2

С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в выбранной игре (в качестве таких переменных может выступать игровая валюта, ресурсы, здоровье и т.д.). Средствами google-sheets визуализируйте данные в google-таблице (постройте график, диаграмму и пр.) для наглядного представления выбранной игровой величины.

Ход работы:

-Настроить доступ к google-таблице по api, написать код генерации данных с помощью Python, используя gspread.
-Передать данные в таблицу, построить график и диаграмму, используя переданные данные.

```py
import gspread
import numpy as np
gc = gspread.service_account(filename='eng-node-405014-6823cc5359d6.json')
sh = gc.open("Workshop2")
runes_data = np.random.randint(100, 1000, 11)
mon = list(range(1,11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        tempInf = ((runes_data[i-1]-runes_data[i-2])/runes_data[i-2])*100
        tempInf = str(tempInf)
        tempInf = tempInf.replace('.',',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(runes_data[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)
```
![image](https://github.com/enietoou/ad_in_gd_lab2/assets/74960429/d4582440-96ed-47e9-9f53-3f254039cf04)

## Задание 3
Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.
- Написать скрипт для воспроизведения звуков, в зависимости от значения в таблице.
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string,float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;

    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1phKoA08fmPVlKE2o7iZQhPN_IAmg5jgUCGOb9P-65bk/values/Лист1?key=AIzaSyAPmdysxqygxo359FknWrGquIVqohH5SNk");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```
<img width="1440" alt="image" src="https://github.com/enietoou/ad_in_gd_lab2/assets/74960429/8c5ff940-e2aa-4ec8-a4ba-79db27d49f89">

## Выводы

В ходе данной лабораторной работы я овладел навыками визуализации данных из таблицы Google Sheets в Unity, освоил методы их представления, а также изучил работу со звуковыми эффектами в Unity с использованием скриптов на языках Python и C#. Мне удалось настроить условия воспроизведения звуков, а затем прослушать эти звуки в соответствии с данными из Google-таблицы, полученными при помощи скрипта на языке Python.


**BigDigital Team: Denisov | Fadeev | Panov**
