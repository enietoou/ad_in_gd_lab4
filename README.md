# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил:
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
Поработать с перцептроном.

## Задание 1
OR | дать комментарии о корректности работы
AND | дать комментарии о корректности работы
NAND | дать комментарии о корректности работы
XOR | дать комментарии о корректности работы
OR:
<img width="1440" alt="image" src="https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/0c8754da-151d-4500-ac43-ddfe14361133">
-тренировка проходит быстро, начиная +-4 поколения перестаёт выдавать ошибки.
AND:
<img width="1440" alt="image" src="https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/5ae8efce-ffbf-4ab0-b39c-296018394b7c">
-работает медленне, чем в OR, начиная с +-6 поколения перестаёт выдавать ошибки.
NAND:
<img width="1440" alt="image" src="https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/542ff074-0184-43dd-9dda-e964851383c0">
-по скорости примерно как AND, с +-6 поколения перестаёт выдавать ошибки.
XOR:
<img width="1440" alt="image" src="https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/3e4bfd66-5e85-4a61-b2ef-82e50420322b">
-перцептрон не смог обучиться, отсутствия ошибок не удаётся добиться.

## Задание 2
Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.
-На основании полученных данных я построил таблицу и график:
![image](https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/e17764a6-8e9e-40a0-ba8e-68d72dd99aa2)
-Выбор количества эпох завит от сложности задачи и уровня желаемой точности. Например для XOR количества эпох не хватаило и обучении закончилось с недостаточно точными весами, вследствии задача не была решена.

## Задание 3
Построить визуальную модель работы перцептрона на сцене Unity.
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class Per_visual : MonoBehaviour
{
    private Renderer cubeRenderer;
    [SerializeField] private int cubeFirst;
    [SerializeField] private int cubeSecond;

    // Start is called before the first frame update
    void Start()
    {
        cubeRenderer = GetComponent<Renderer>();
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    private void OnTriggerEnter(Collider other)
    {
        if (SceneManager.GetActiveScene().name == "OR") OR();
        if (SceneManager.GetActiveScene().name == "AND") AND();
        if (SceneManager.GetActiveScene().name == "NAND") NAND();
        if (SceneManager.GetActiveScene().name == "XOR") XOR();
        Destroy(other.gameObject);
    }

    private void OR() 
    {
        if (cubeFirst == 1 || cubeSecond == 1){
            SetCubeColor(Color.white);
        }
        else {
            SetCubeColor(Color.black);
        }
    }
    private void AND() 
    {
        if (cubeFirst == 1 && cubeSecond == 1){
            SetCubeColor(Color.white);
        }
        else {
            SetCubeColor(Color.black);
        }
    }
    private void NAND() 
    {
        if (!(cubeFirst == 1 && cubeSecond == 1)){
            SetCubeColor(Color.white);
        }
        else {
            SetCubeColor(Color.black);
        }
    }
    private void XOR() 
    {
        if ((cubeFirst == 1 || cubeSecond == 1) && !(cubeFirst == 1 && cubeSecond == 1)){
            SetCubeColor(Color.white);
        }
        else {
            SetCubeColor(Color.black);
        }
    }

    void SetCubeColor(Color color)
    {
        cubeRenderer.material.color = color;
    }

}
```
Черный куб - 0, белый - 1, начальное положение для всех сцен:
![image](https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/175c80c5-191f-4d47-8346-cf877f735c72)
Визуализация OR:
![image](https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/d3507d75-cbc0-477e-ba32-41a2122f83a6)
Визуализация AND:
![image](https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/506a02ae-c57d-4757-8076-704fcc6bf1e2)
Визуализация NAND:
![image](https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/db7a31bf-8a4b-4a01-8761-a056d56ac80a)
Визуализация XOR:
![image](https://github.com/enietoou/ad_in_gd_lab4/assets/74960429/615574de-a577-4465-a881-ec226c1355e4)


## Выводы
В ходе выполнения лабораторной работы я изучил работу перцептронов и успешно реализовал их визуализацию в среде Unity.


**BigDigital Team: Denisov | Fadeev | Panov**
