# Хакатон Яндекс Музыка: Октябрь 2023  
# Решение от команды Musical-Detectives-Team (команда №7)
**Задача хакатона:** Разработка ML-продукта, позволяющего обнаруживать каверы и/или объединять треки в кластеры каверов, а также искать оригинальные треки в цепочке каверов и оригинала.

**Достигнутые результаты:** 
* Создана модель классификации трека по признаку кавер-оригинал (**F1 = 0.7685**);
* Создана модель для группировки каверов и оригинального трека (**F1 = 0.9036** на датасете для матчинга треков);
* На основании первых двух решений предложен алгоритм поиска оригинальных треков в цепочке треков.

# Содержание репозитория:

## 1. [Блокнот с исследовательским анализом данных](EDA.ipynb)
Проведен анализ данных, выдвинуты гипотезы, предложена стратегия решения поставленных задач.

## 2. [Блокнот с переводом текстов](translator.ipynb)
В данном блокноте представлен код взаимодействия с API Yandex Translator. Весь текст приведён к одному языку.

## 3. [Блокнот с предобработкой данных](preprocessing.ipynb)
В данном блокноте проведена предобработка данных с учетом особенностей, выявленных при исследовательском анализе.

## 4. [Блокнот с моделингом для задачи классификации трека по признаку кавер-оригинал](modeling_task_1.ipynb)
Разработана модель для прогноза является ли данный трек кавером или оригиналом. Проведён анализ признаков и подбор гиперпараметров. Получена метрика F1 = 0.7249.

## 5. [Блокнот с моделингом для задачи группировки каверов и оригинального трека](modeling_task_2.ipynb)
Разработана модель для получения группы треков являющимися каверами и оригиналом. Предложена модель состоящая из двух уровней:
1. На первом уровне, при помощи FAISS будем искать k ближайших соседей для каждого трека-запроса.
2. На основании списка полученных претендентов строим модель (CatBoostClassifier), которая будет принимать решения являются ли треки каверами друг для друга.

## 6. [Блокнот с моделингом для задачи поиска оригинального трека в треков каверов и оригинала](modeling_task_3.ipynb)
Предложена и реализована следующая логика решения задачи: 
* сначала проводится анализ цепочки каверов и при отсутствии цепочки как таковой (модель из второй задачи не нашла похожие треки в базе), будем считать сам трек оригиналом;
* если цепочка треков была обнаружена, то получаем самый ранний из треков на основании года, полученному из isrc (по факту год регистрации трека);
* если несколько треков соответствуют самому раннему году, либо если есть треки, для которых год неизвестен, то отдаём их для прогноза модели, обученной для решения задачи классификации кавер / оригинал, и по predict_proba выбираем тот, в котором модель больше уверена.

## 7. [Блокнот с общими выводами по работе](conclusions.ipynb)
В данном блокноте объединены разные этапы проделанной работы, кратко указаны достигнутые результаты на каждом из этапов.

## 8. [Папка с визуализацией данных ydata_profiling](profile_df)
Предоставлены результаты визуализации данных по датасетам на этапе EDA.

# Состав команды:
[Елена Кравцова](https://github.com/ElenaKravtsova20) - PM команды  
[Алексей Томашевский](https://github.com/TomashA1980) - DS команды  
[Татьяна Вдовина](https://github.com/vdovinati) - DS команды  
[Ярослав Князев](https://github.com/Yaroslav-Kn) - DS команды
