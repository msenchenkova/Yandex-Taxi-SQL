# 🚕 Яндекс.Такси

## Консоль
<details>
<summary>Выяснила, какие запросы шли с определенного IP-адреса</summary>  


>Команда, которой удалось получить нужные логи: 
```
cat logs/2019/12/* | grep "^233.201"
```
>Логи:
```
233.201.188.154 - - [18/12/2019:21:46:01 +0000] "DELETE /events HTTP/1.1" 403 3971
233.201.182.9 - - [21/12/2019:21:56:20 +0000] "PATCH /users HTTP/1.1" 400 4118
```
***
</details>

<details>
<summary>Сохранила в отдельный файл логи, которые были записаны в период, когда был обнаружен баг</summary>  

>1. Команды, которыми создала директории:
```
mkdir bug1
mkdir bug1/events
```

>2. Команда, которой выбирала запросы за период проявления бага 30.12.2019 и 31.12.2019 с 21:30:00 до 21:39:59:
```
cat logs/2019/12/* | grep "3./12/2019:21:3" >> ./bug1/main.txt;
```

>3. Команды, которыми сохранила логи в файлы 400.txt и 500.txt из main.txt: 
```
cat bug1/main.txt | grep " 400 " >> bug1/events/400.txt;
cat bug1/main.txt | grep " 500 " >> bug1/events/500.txt;
```
***
</details>

## База данных

* По плану на линию обслуживания должно было выйти 10550 автомобилей
<details>
<summary>Выяснила сколько такси вышло на линии на самом деле</summary>

>Количество автомобилей:
```
5500
```

>Запрос, которым удалось найти число автомобилей: 
```
SELECT COUNT(DISTINCT vehicle_id) AS unique_id
FROM cabs;
```
***
</details>

<details>
  <summary>Посчитала количество автомобилей в каждой компании, отсортировала и вывела те компании, в которых меньше 100 автомобилей</summary>

  >Запрос, которым удалось решить задачу:
```
SELECT COUNT(vehicle_id) AS cnt,company_name
FROM cabs
GROUP BY company_name
HAVING COUNT(vehicle_id)<100
ORDER BY cnt DESC;
```

>Список компаний с числом автомобилей меньше 100:
```
cnt  |                 company_name                 
-----+----------------------------------------------
  97 | Nova Taxi Affiliation Llc
  89 | Patriot Taxi Dba Peace Taxi Associat
  85 | Blue Diamond
  81 | Checker Taxi Affiliation
  80 | Chicago Medallion Management
  69 | Chicago Independents
  67 | 24 Seven Taxi
  60 | Checker Taxi
  55 | American United
  53 | Chicago Medallion Leasing INC
  49 | Top Cab Affiliation
  48 | KOAM Taxi Association
  38 | Chicago Taxicab
  34 | Norshore Cab
  20 | Gold Coast Taxi
  20 | Metro Group
  18 | Service Taxi Association
  14 | 5 Star Taxi
   8 | American United Taxi Affiliation
   8 | Metro Jet Taxi A
   7 | Setare Inc
   5 | Leonard Cab Co
   1 | 4615 - 83503 Tyrone Henderson
   1 | 5062 - 34841 Sam Mestas
   1 | 4623 - 27290 Jay Kim
   1 | 5997 - 65283 AW Services Inc.
   1 | 2092 - 61288 Sbeih company
   1 | 1469 - 64126 Omar Jada
   1 | 2733 - 74600 Benny Jona
   1 | 2192 - 73487 Zeymane Corp
   1 | 5006 - 39261 Salifu Bawa
   1 | 3556 - 36214 RC Andrews Cab
   1 | 3721 - Santamaria Express, Alvaro Santamaria
   1 | 2809 - 95474 C & D Cab Co Inc.
   1 | 2241 - 44667 - Felman Corp, Manuel Alonso
   1 | 3620 - 52292 David K. Cab Corp.
   1 | 2823 - 73307 Lee Express Inc
   1 | 6057 - 24657 Richard Addo
   1 | 6742 - 83735 Tasha ride inc
   1 | 1085 - 72312 N and W Cab Co
   1 | 3591 - 63480 Chuks Cab
   1 | 0118 - 42111 Godfrey S.Awir
   1 | 6574 - Babylon Express Inc.
   1 | 3094 - 24059 G.L.B. Cab Co
   1 | 5874 - 73628 Sergey Cab Corp.
   1 | 6743 - 78771 Luhak Corp
   1 | 5074 - 54002 Ahzmi Inc
   1 | 3623 - 72222 Arrington Enterprises
   1 | 4053 - 40193 Adwar H. Nikola
   1 | Chicago Star Taxicab
   1 | 3011 - 66308 JBL Cab Inc.
(51 rows)
```
***
</details>

* В приложении такси рассчитывается коэффициент стоимости поездки в зависимости от погодных условий

<details>
  <summary>Сделала выборку для проверки правильности расчёт коэффициента</summary>

  >Запрос, которым удалось решить задачу:
```
SELECT ts,CASE
WHEN description LIKE '%rain%' OR description LIKE '%storm%'
THEN 'Bad' ELSE 'Good' END AS weather_conditions
FROM weather_records
WHERE ts BETWEEN '2017-11-05 00:00:00' AND '2017-11-06 00:00:00';
```

>Таблица с данными за указанный период:
```
         ts          | weather_conditions 
---------------------+--------------------
 2017-11-05 00:00:00 | Good
 2017-11-05 01:00:00 | Bad
 2017-11-05 02:00:00 | Good
 2017-11-05 03:00:00 | Good
 2017-11-05 04:00:00 | Bad
 2017-11-05 05:00:00 | Bad
 2017-11-05 06:00:00 | Good
 2017-11-05 07:00:00 | Good
 2017-11-05 08:00:00 | Good
 2017-11-05 09:00:00 | Good
 2017-11-05 10:00:00 | Good
 2017-11-05 11:00:00 | Good
 2017-11-05 12:00:00 | Good
 2017-11-05 13:00:00 | Good
 2017-11-05 14:00:00 | Bad
 2017-11-05 15:00:00 | Good
 2017-11-05 16:00:00 | Bad
 2017-11-05 17:00:00 | Good
 2017-11-05 18:00:00 | Bad
 2017-11-05 19:00:00 | Bad
 2017-11-05 20:00:00 | Bad
 2017-11-05 21:00:00 | Good
 2017-11-05 22:00:00 | Good
 2017-11-05 23:00:00 | Good
 2017-11-06 00:00:00 | Good
(25 rows)
```
***
</details>

* После обновления ПО таксопарки стали сообщать, что прибыль, которую они получают, не сходится с данными, которые отдаёт приложение
<details>
  <summary>Получила выборку с количеством поездок каждого таксопарка за 15 и 16 ноября 2017 года</summary>

  >Запрос, которым удалось решить задачу:
```
SELECT cabs.company_name AS company_name, COUNT(trips.trip_id) AS trips_amount
FROM cabs
INNER JOIN trips ON trips.cab_id = cabs.cab_id
WHERE trips.start_ts BETWEEN '2017-11-15 00:00:00' AND '2017-11-16 23:59:00'
GROUP BY company_name
ORDER BY trips_amount DESC;
```

>Таблица с данными за указанный период:
```
 company_ name                               | trips_amount
---------------------------------------------+--------------
Flash Cab                                    |19558
Taxi Affiliation Services                    |11422
Medallion Leasin                             |10367
Yellow Cab                                   |9888
Taxi Affiliation Service Yellow              |9299
Chicago Carriage Cab Corp                    |9181
City Service                                 |8448
Sun Taxi                                     |7701
Star North Management LLC                    |7455
Blue Ribbon Taxi Association Inc.            |5953
Choice Taxi Association                      |5015
Globe Taxi                                   |4383
Dispatch Taxi Affiliation                    |3355
Nova Taxi Affiliation Llc                    |3175
Patriot Taxi Dba Peace Taxi Associat         |2235
Checker Taxi Affiliation                     |2216
Blue Diamond                                 |2070
Chicago Medallion Management                 |1955
24 Seven Taxi                                |1775
Chicago Medallion Leasing INC                |1607
Checker Taxi                                 |1486
American United                              |1404
Chicago Independents                         |1296
KOAM Taxi Association                        |1259
Chicago Taxicab                              |1014
Top Cab Affiliation                          |978
Gold Coast Taxi                              |428
Service Taxi Association                     |402
5 Star Taxi                                  |310
303 Taxi                                     |250
Setare Inc                                   |230
American United Taxi Affiliation             |210
Leonard Cab Co                               |147
Metro Jet Taxi A                             |146
Norshore Cab                                 |127
6742 - 83735 Tasha ride inc                  |39
3591 - 63480 Chuks Cab                       |37
1469 - 64126 Omar Jada                       |36
6743 - 78771 Luhak Corp                      |33
0118 - 42111 Godfrey S.Awir                  |33
6574 - Babylon Express Inc.                  |31
Chicago Star Taxicab                         |29
1085 - 72312 N and W Cab Co                  |29
2809 - 95474 C & D Cab Co Inc.               |29
2092 - 61288 Sbeih company                   |27
3011 - 66308 JBL Cab Inc.                    |25
3620 - 52292 David K. Cab Corp.              |21
4615 - 83503 Tyrone Henderson                |21
3623 - 72222 Arrington Enterprises           |20
5074 - 54002 Ahzmi Inc                       |16
2823 - 73307 Lee Express Inc                 |15
4623 - 27290 Jay Kim                         |15
3721 - Santamaria Express, Alvaro Santamaria |14
5006 - 39261 Salifu Bawa                     |14
2192 - 73487 Zeymane Corp                    |14
6057 - 24657 Richard Addo                    |13
5997 - 65283 AW Services Inc.                |12
Metro Group                                  |11
5062 - 34841 Sam Mestas                      |8
4053 - 40193 Adwar H. Nikola                 |7
2733 - 74600 Benny Jona                      |7
5874 - 73628 Sergey Cab Corp.                |5
2241 - 44667 - Felman Corp, Manuel Alonso    |3
3556 - 36214 RC Andrews Cab                  |2
(64 rows)

</details>
