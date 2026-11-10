# SQL. Часть 2

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.
```sql
SELECT
    s.last_name   AS last_name,
    s.first_name  AS first_name,
    ci.city       AS city,
    COUNT(c.customer_id) AS customers_count
FROM store st
JOIN staff    s  ON s.store_id  = st.store_id
JOIN address  a  ON a.address_id = st.address_id
JOIN city     ci ON ci.city_id   = a.city_id
JOIN customer c  ON c.store_id   = st.store_id
GROUP BY st.store_id, s.staff_id, s.last_name, s.first_name, ci.city
HAVING COUNT(c.customer_id) > 300;
```
Скриншот вывода выполнения кода
[](./SQL-2/Z-1.png)


### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
```sql
SELECT COUNT(*) AS films_above_avg
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```
Скриншот вывода выполнения кода
[](./SQL-2/Z-2.png)


### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
```sql
SELECT
    DATE_FORMAT(p.payment_date, '%Y-%m') AS payment_month,
    SUM(p.amount)                        AS total_amount,
    COUNT(DISTINCT r.rental_id)          AS rentals_count
FROM payment p
JOIN rental  r ON r.rental_id = p.rental_id
GROUP BY DATE_FORMAT(p.payment_date, '%Y-%m')
ORDER BY total_amount DESC
LIMIT 1;
```
Скриншот вывода выполнения кода
[](./SQL-2/Z-3.png)
