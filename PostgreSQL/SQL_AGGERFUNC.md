# Агрегатные функции

**Агрегатные функции** помогают вычислять сводные значения для группы строк

Давайте познакомимся с нашими покемонами и данными о них!
* id	- уникальный идентификатор
* name	- имя
* type1	- основной тип
* type2	- дополнительный тип
* hp	- количество очков здоровья
* attack	- показатели атаки
* defense	- показатели защиты
* speed	- показатели скорости

Для начала получим все основные типы покемонов.
```sql
SELECT /*выбор*/
    type1 /*столбец type1*/
FROM sql.pokemon /*из таблицы sql.pokemon*/
```
Чтобы получить **уникальные** значения из столбца, воспользуемся ключевым словом *DISTINCT*.
```sql
SELECT DISTINCT /*выбрать уникальные значения*/
    type1 /*столбец type1*/
FROM sql.pokemon /*из таблицы sql.pokemon*/
```
Мы можем применять DISTINCT и для нескольких столбцов.

Получим, например, все уникальные пары основного и дополнительного типов для покемонов.
```sql
SELECT DISTINCT /*выбрать уникальные значения*/
    type1, /*столбец type1*/
    type2 /*столбец type2*/
FROM sql.pokemon /*из таблицы sql.pokemon*/
```
**Обратите внимание!** *DISTINCT* пишется только один раз, в начале списка получаемых столбцов.

Давайте посчитаем количество строк в таблице. Для этого применим агрегатную функцию COUNT.

```sql
SELECT
    COUNT (*)
FROM sql.pokemon
```
COUNT считает строки, а звёздочка (*) в аргументе функции означает, что считаются все строки, которые возвращает запрос.

Если в аргументе функции указать название столбца, функция обработает **только строки с непустым значением**.

Узнаем число покемонов, которые имеют дополнительный тип
```sql
SELECT
    COUNT(type2)
FROM sql.pokemon
```
Внутри функции COUNT мы можем также применять DISTINCT, чтобы вычислить количество уникальных значений.
```sql
SELECT /*выбор*/
    COUNT(DISTINCT type1) /*функция подсчёта строк; уникальные значения столбца type1*/
FROM sql.pokemon /*из таблицы sql.pokemon*/
```
### Назовём основные агрегатные функции, с которыми нам предстоит работать:

* COUNT — вычисляет число непустых строк;
* SUM — вычисляет сумму;
* AVG — вычисляет среднее;
* MAX — вычисляет максимум;
* MIN — вычисляет минимум.

Разумеется, вы можете использовать в запросе фильтрацию строк с помощью WHERE, чтобы получить агрегированное значение только для отдельных строк.

Узнаем, какое среднее количество очков здоровья у покемонов-драконов (то есть тех, у кого основной тип — Dragon)
```sql
SELECT
    AVG(hp)
FROM sql.pokemon
WHERE type1 = 'Dragon'
```
Кроме того, мы можем применять несколько агрегатных функций в одном запросе.
```sql
SELECT /*выбор*/
    COUNT(*) AS "всего травяных покемонов", /*подсчёт всех строк; назначить алиас "всего травяных покемонов"*/
    COUNT(type2) AS "покемонов с дополнительным типом", /*подсчёт непустых строк в столбце type2; назначить алиас "покемонов с дополнительным типом"*/
    AVG(attack) AS "средняя атака", /*среднее значение столбца attack; назначить алиас "средняя атака"*/
    AVG(defense) AS "средняя защита" /*среднее значение столбца defense; назначить алиас "средняя защита"*/
FROM sql.pokemon /*из таблицы sql.pokemon*/
WHERE type1 = 'Grass'/*при условии, что значение столбца type1 содержит grass*/
```
Напишите запрос, который выведет:

* количество покемонов (столбец pokemon_count),
* среднюю скорость (столбец avg_speed),
* максимальное и минимальное число очков здоровья (столбцы max_hp и min_hp)
* для электрических (Electric) покемонов, имеющих дополнительный тип и показатели атаки или защиты больше 50.
```sql
SELECT
  COUNT(*) AS pokemon_count,
  AVG(speed) AS avg_speed,
  MAX(hp) AS max_hp,
  MIN(hp) AS min_hp
FROM sql.pokemon
WHERE type1 = 'Electric' 
  AND type2 IS NOT NULL 
  AND (attack > 50 or defense > 50)
```
С полным перечнем существующих агрегатных функций вы можете ознакомиться в [официальной документации](https://postgrespro.ru/docs/postgrespro/11/functions-aggregate).