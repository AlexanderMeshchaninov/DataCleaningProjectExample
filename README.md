# Статистические методы поиска выбросов
![](/Images/data_cleaning.png)

## Понятие выброса
Одним из этапов очистки данных это поиск выбросов.

***Выброс или аномалия в данных*** - это наблюдение, которое существенно выбивается из общего распределения и сильно отличается, визуально на графиках, или в таблице от других данных.

В данном разделе рассматриваются два статистических метода поиска выбросов:
1. Метод межквартиального размаха;
2. Метод Z-отклонений (метод сигм).

## Метод межквартильного размаха

![](/Images/boxplot.png)

### Описание алгоритма метода:
1. Вычислить 25-ую и 75-ую квантили (1 и 3 квартили) - $Q_{25}$ и $Q_{75}$ для признака, который мы исследуем.
2. Вычислить межквартильное расстояние:
    * $IRQ = Q_{75}-Q_{25}$
3. Определить верхнюю и нижнюю границы Тьюки:
    * $bound_{upper} = Q_{75} + 1.5*IQR$
    * $bound_{lower} = Q_{25} - 1.5*IQR$
4. Найти наблюдения, которые выходят за пределы границ.

### **Недостатки метода:**

Данный метод требует, чтобы признак, на основе которого происходит поиск выбросов, был распределен нормально.

### **Модификация метода:**

Можно воспользоваться методами преобразования данных, например, логарифмированием (log_scale), чтобы попытаться свести распределение к нормальному или хотя бы к симметричному результату.

Также можно добавить вручную количество квартильных размахов в левую и правую сторону распределений.

## Метод Z-отклонений (метод сигм)

Правило трех сигм гласит: что, если распределение данных является нормальным, то 99.73% лежат в интервале:
$(\mu-3 \sigma$ , $\mu+3 \sigma)$,

где:

* $\mu$ - математическое ожидание (для выборки это среднее значение)

* $\sigma$ - стандартное отклонение

Наблюдения, которые лежат за пределами этого интервала будут считаться выбросами.

![](/Images/sigma_method.png)

### **Алгоритм метода:**

1. Вычислить среднее и стандартное отклонение $\mu$ и $\sigma$ для признака, который мы исследуем;
2. Определить верхнюю и нижнюю границы:
    * $bound_{upper} = \mu - 3 * \sigma$
    * $bound_{lower} = \mu + 3 * \sigma$
3. Найти наблюдения, которые выходят за пределы границ.

### **Недостатки метода:**

Метод требует, чтобы признак, на основе которого происходит поиск выбросов, был распределен нормально.

### **Модификация метода:**

Можно попробовать воспользоваться методами преобразования данных, например, логарифмированием, чтобы попытаться свести распределение к нормальному или хотя бы к симметрии. 

Также можно добавить вручную количество стандартных отклонений в левую и правую сторону распределений.

## Реализация методов

Методы реализованы в виде функций (на языке Python) find_outliers_z_score() и find_outliers_iqr(). 
Функции находятся в файле find_outliers.py

## Пример использования

Обязательными аргументами функций, реализующих методы поиска выбросов являются:
* data (pandas.DataFrame): набор входных данных (таблица)
* feature (str): имя признака, на основе которого происходит поиск выбросов

Использование подходов без модификаций:
```python
# Метод z-отклонений
from Outliers_lib.find_outliers import find_outliers_z_score

outliers_z_score, cleaned_z_score = find_outliers_z_score(data, feature)
```

```python
# Метод межквартильного размаха
from Outliers_lib.find_outliers import find_outliers_iqr

outliers_iqr, cleaned_iqr = find_outliers_iqr(data, feature)
```

Использование методов с предварительным логарифмированием и добавлением вариативности разброса:

```python
# Метод z-отклонений
outliers_z_score, cleaned_z_score = find_outliers_z_score(data, feature, left=2, right=2, log=True)
```

```python
# Метод межквартильного размаха
outliers_iqr, cleaned_iqr = find_outliers_iqr(data, feature, left=2, right=2, log=True)
```

Использование методов с предварительным логарифмированием:

```python
# Метод z-отклонений
outliers_z_score, cleaned_z_score = find_outliers_z_score(data, feature, log=True)
```
```python
# Метод межквартильного размаха
outliers_iqr, cleaned_iqr = find_outliers_iqr(data, feature, log=True)
```

## Использованные инструменты и библиотеки
* numpy (1.26.4)
* pandas (2.2.1)

## Дополнительные источники:
* [Нормальное распределение](https://ru.wikipedia.org/wiki/Нормальное_распределение)
* [Метод межквартильного размаха](https://recture.ru/common/chto-takoe-pravilo-mezhkvartilnogo-razmaha/)
* [Правило трех сигм](https://wiki.loginom.ru/articles/3-sigma-rule.html)