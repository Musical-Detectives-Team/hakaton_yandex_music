# Хакатон Яндекс Музыка: Октябрь 2023
**Задача хакатона:** Разработка ML-продукта, позволяющего обнаруживать каверы и/или объединять треки в кластеры каверов, а также искать оригинальные треки в цепочке каверов.

**Достигнутые результаты:** 
* Создана модель классификации трека по признаку кавер-некавер (**F1 = 0.7249**);
* Создана модель для группировки каверов и исходного трека (**F1 = 0.9036**);
* На основании первых двух решений предложен алгоритм поиска исходного трека в цепочке каверов.

# Содержание репозитория:

## 1. [Блокнот с исследовательским анализом данных](EDA.ipynb)
Проведен анализ данных, выдвинуты гипотезы, предложена стратегия решения поставленных задач.

## 2. [Блокнот с переводом текстов](translator.ipynb)
В данном блокноте представлен код взаимодействия с API Yandex Translator. Весь текст приведён к одному языку.

## 3. [Блокнот с предобработкой данных](preprocessing.ipynb)
В данном блокноте проведена предобработка данных с учетом особенностей, выявленных при исследовательском анализе.

## 4. [Блокнот с моделингом для задачи классификации трека по признаку кавер-некавер](modeling_task_1.ipynb)
Разработана модель для прогноза является ли данный трек кавером или оригиналом. Проведён анализ признаков и подбор гиперпараметров. Получена метрика f1 = 0.7249.

## 5. [Блокнот с моделингом для задачи группировки каверов и исходного трека](modeling_task_2.ipynb)
Разработана модель для получения группы треков являющимися каверами. Предложена модель состоящая из двух уровней:
1. На первом уровне, при помощи FAISS будем икать k ближайших соседей для каждого трека-запроса.
2. На основании списка полученных претендентов строим модель (CatBoostClassifier), которая будет принимать решения являются ли треки каверами друг для друга.

## 6. [Блокнот с моделингом для задачи поиска исходного трека в цепочке каверов](modeling_task_3.ipynb)
Предложена и реализована следующая логика решения задачи: 
* сначала проводится анализ цепочки каверов и при отсутствии цепочки как таковой (модель из второй задачи не нашла похожие треки в базе), будем считать сам трек оригиналом;
* если цепочка треков была обнаружена, то получаем самый ранний из треков на основании года, полученному из isrc (по факту год регистрации трека);
* если несколько треков соответствуют самому раннему году, либо если есть треки, для которых год неизвестен, то отдаём их для прогноза модели, обученной для решения задачи классификации кавер / оригинал, и по predict_proba выбираем тот, в котором модель больше уверена.

## 7. [Блокнот с общими выводами по работе](conclusions.ipynb)
В данном блокноте синтегрированны разные этапы проделанной работы, кратко указаны достигнутые результаты на каждом из этапов.
