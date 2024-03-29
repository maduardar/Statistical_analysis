# Статистический анализ ранга счастья в разных странах
## Структура работы:
1.  [Постановка задачи:](#par1)
    1. [Актуальность темы исследования;](#par1.1)
    2. [Описание выбранных объектов и показатели;](#par1.2)
2.	[Предварительный анализ данных:](#par2)
    1. [Вычисление, описание и анализ дескриптивных статистических переменных;](#par2.1)
    2. [Графическое представление данных;](#par2.2)
3.	[Кластерный анализ:](#par3)
    1. [Ранжирование объектов по выбранным показателям;](#par3.1)
    2. [Аномальные наблюдения;](#par3.2)
    3. [Диаграммы рассеяния;](#par3.3)
    4. [Выводы;](#par3.4)
    5. [Анализ стран, попавших в выборку;](#par3.5)
    6. [Классификация с помощью иерархических кластер-процедур:](#par3.6)
        1. Метод ближнего соседа;
        2. Метод дальнего соседа;
        3. Метод центра тяжести;
        4. Метод средней связи;
        5. Метод Уорда (Warda);
    7. Сравнение полученных результатов;
    8. Таблица результатов
    9. Дендограммы;
    10. Анализ количества кластеров;
    11. Классификация объектов с помощью метода k-средних:
        1. Задание количества кластеров на которое целесообразно разбить исследуемую совокупность;
        2. Таблица средних значений кластеров;
        3. График средних значений показателей по кластерам;
        4. Характеристика каждого из выделенных кластеров;
        5. Таблица классификации;
        6. Матрица расстояний между центрами кластеров;
4. Дискриминантный анализ:
    1. Выделение наблюдений, подлежащих дискриминации;
    2. Дискриминантный анализ;
    3. Выражение для дискриминантной функции;
    4. Оценка значимости дискриминантной функции (по коэффициенту Уилкса);
    5. Определение относительного вклада каждой переменной в формирование классов;
    6. Определение средних значений дискриминантной функции по группам;
    7. Группы классифицируемых объекты и вероятности, с которыми объекты входят в эти группы;
    8. Значимость различий средних значений дискриминантной функции в двух группах;
    9. Качество дискриминантного анализа (на основании результатов таблицы Eigenvalue);
    10. Целесообразность проведения дискриминантного анализа по представленным данным.

## Постановка задачи <a name="par1"></a>
В своей работе я буду анализировать уровень счастья в разных странах.
Данные были взяты с сайта [worldhappiness.report](http://worldhappiness.report/ed/2017/).
### Актуальность темы исследования <a name="par1.1"></a>
[Всемирный доклад о счастье](https://ru.wikipedia.org/wiki/Всемирный_доклад_о_счастье) - это исторический обзор состояния глобального счастья.
Первый Всемирный доклад о счастье был опубликован в апреле 2012 года в поддержку Совещания высокого уровня ООН по вопросам счастья и благополучия. С тех пор мир прошел долгий путь. Растущее счастье считается надлежащей мерой социального прогресса и целью государственной политики. Интересно применить задачу кластеризации к таким данным, посмотреть, что общего у стран, находящихся в одном кластере: географическое положение? схожий политический режим? или что-то другое? Чтобы ответить на эти вопросы, присупим к анлизу данных. 
### Описание выбранных объектов и показатели <a name="par1.2"></a>
Для оценки национального счастья используются 6 факторов: ВВП на душу населения (*GDP*), социальная поддержка (*Social support*), ожидаемая продолжительность жизни (*Healthy life expectancy*), свобода граждан самостоятельно принимать жизненно важные решения (*Freedom*), щедрость(*Generosity*) и отношение к коррупции(*Perceptions of corruption*). Каждый фактор оценивается по 10-и бальной шкале. Каждая страна также сравнивается с гипотетической страной под названием «Антиутопия». Антиутопия представляет самые низкие национальные средние значения для каждой ключевой переменной и вместе с остаточной ошибкой используется в качестве эталона регрессии (столбец *Dystopia*).
Данные, выбранные мной для анализа, имеют следующую структуру:

Это таблица 155 x 12. Ниже приведены первые пять строк таблицы:

|Country |Happiness Rank|Happiness Score	|Whisker (high)	|Whisker (low)	|GDP	|Social support	|Health Life Expectancy	|Freedom |Generosity	|Trust (Government Corruption)| Dystopia|
|---------|---|---|---|---|---|---|---|---|---|---|---|
|Norway|1|7.537	|7.594445|7.479556|1.616463|1.533524|0.796667|0.635423|0.362012|0.315964|2.277027|
|Denmark	|2	|7.522	|7.581728	|7.462272	|1.482383	|1.551122|0.792566|0.626007|0.355280|0.400770|2.313707|
|Iceland	|3	|7.504	|7.622030	|7.385970	|1.480633	|1.610574|0.833552|0.627163|0.475540|0.153527|2.322715|
|Switzerland	|4	|7.494	|7.561772	|7.426227	|1.564980	|1.516912|0.858131|0.620071|0.290549|0.367007|2.276716|
|Finland	|5	|7.469	|7.527542	|7.410458	|1.443572	|1.540247|0.809158|0.617951|0.245483|0.382612|2.430182|

Первый столбец таблицы содержит название страны. Далее, для каждой страны приведены следующие данные:

*  **Happiness Rank** -- *ранг счастья* -- позиция, на которой находится страна в рейтинге счастья (целое число от 1 до 155);
* **Happiness Score** -- *счастье* -- метрика, впервые введённая в 2015 году путём опроса. Людям задавали вопрос: «Как бы вы оценили свое счастье по шкале от 0 до 10, где 10 - самый счастливый?» В этом столбце находятся средние показатели результатов для каждой страны (действительное число от 0 до 10);
* **Whisker (high)** -- верхний (третий) квартиль (действительное число от 0 до 10);
* **Whisker (low)** -- нижний (первый) квартиль (действительное число от 0 до 10);

В остальных столбцах (их значения описаны выше) находится вес, с которым каждый показатель входит в формулу, по которой вычисляется показатель счастья.

## Предварительный анализ данных<a name="par2"></a>

Все вычисления можно найти в файле [Cluster analysis.ipynb](https://github.com/maduardar/Statistical_analysis/blob/master/Cluster%20analysis.ipynb).

### Вычисление, описание и анализ дескриптивных статистических переменных<a name="par2.1"></a>
Основные статистики приведены в таблице:

|   | Happiness Rank | Happiness Score | Whisker (high) | Whisker (low) | GDP| Social support | Healthy life expectancy | Freedom    | Generosity | Perceptions of corruption | Dystopia|
|----------------|-----------------|----------------|---------------|------------|----------------|-------------------------|------------|------------|---------------------------|------------|------------|
|среднее          | 78.000000       | 5.354019       | 5.452326      | 5.255713   | 0.984718       | 1.188898                | 0.551341   | 0.408786   | 0.246883                  | 0.123120   | 1.850238   |
| с.к.о.            | 44.888751       | 1.131230       | 1.118542      | 1.145030   | 0.420793       | 0.287263                | 0.237073   | 0.149997   | 0.134780                  | 0.101661   | 0.500028   |
| min            | 1.000000        | 2.693000       | 2.864884      | 2.521116   | 0.000000       | 0.000000                | 0.000000   | 0.000000   | 0.000000                  | 0.000000   | 0.377914   |
| 25%            | 39.500000       | 4.505500       | 4.608172      | 4.374955   | 0.663371       | 1.042635                | 0.369866   | 0.303677   | 0.154106                  | 0.057271   | 1.591291   |
| 50%            | 78.000000       | 5.279000       | 5.370032      | 5.193152   | 1.064578       | 1.253918                | 0.606042   | 0.437454   | 0.231538                  | 0.089848   | 1.832910   |
| 75%            | 116.500000      | 6.101500       | 6.194600      | 6.006527   | 1.318027       | 1.414316                | 0.723008   | 0.516561   | 0.323762                  | 0.153296   | 2.144654   |
| max            | 155.000000      | 7.537000       | 7.622030      | 7.479556   | 1.870766       | 1.610574                | 0.949492   | 0.658249   | 0.838075                  | 0.464308   | 3.117485   |

### Графическое представление данных <a name="par2.2"></a>
Во-первых, мы попытаемся понять корреляцию между какими-то переменными. Для этого сначала вычислим корреляционную матрицу среди переменных и, для наглядности построим, её как тепловую карту.

![correlation](https://github.com/maduardar/Statistical_analysis/blob/master/Images/corr.png)

Мы получили тепловую карту корреляции между переменными. Более светлый оттенок представляет собой высокую корреляцию. Мы можем видеть, что оценка счастья сильно коррелирует с ВВП на душу населения,  социальной поддержкой и продолжительность жизни. Самая слабая корреляция -- со щедростью.

А теперь изобразим индекс счастья разных стран на карте мира.<a name="pic1">
<img src="https://github.com/maduardar/Statistical_analysis/blob/master/Images/Plot%201.png">

## Кластерный анализ <a name="par3"></a>
### Ранжирование объектов по выбранным показателям <a name="par3.1"></a>
Исходные данные уже проранжированны (столбец *Happiness rank (ранг счастья)*). Данные упорядоченны по возрастанию значений в этом столбце.
### Аномальные наблюдения <a name="par3.2"></a>
По всей видимости, данные уже предобработанные и сглаженные. Аномальных наблюдений не выявлено.
### Диаграммы рассеяния <a name="par3.3"></a>
Итак, мы [выяснили](#par2.2), что оценка счастья (Happiness score) коррелирует с ВВП на душу населения (GDP) и слабо зависит от щедрости (Generosity). Построим диаграммы рассеяния для этих двух пар показателей:
<img src="https://github.com/maduardar/Statistical_analysis/blob/master/Images/diag1.png"> <img src="https://github.com/maduardar/Statistical_analysis/blob/master/Images/diag2.png">
### Выводы <a name="par3.4"></a>
По графику наглядно видно, что объекты *оценка счастья* и *ВВП на душу населения* действительно имеют сильную зависимость, в то время как между *оценкой счастья* и *щедростью* никакой линейной зависимости не наблюдается.
### Анализ стран, попавших в выборку <a name="par3.5"></a>
В выборку попали большинство крупнейших стран мира. Глядя на распределение индекса счастья на [карте мира](#pic1), можно заметить, что в Северной, Южной Америке, Австралии и Европе люди более счастливы, чем в Азии и Африке.
### Классификация с помощью иерархических кластер-процедур <a name="par3.6"></a>
