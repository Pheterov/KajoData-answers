# KajoData-answers
MySQL code to answer questions from Kajo Data Space community

--                                  ZADANIE 1
/*
SELECT
    COUNT(*)
FROM
    kd19.orders
WHERE
    (order_date < '2021.01.01' AND order_date > '2019.12.31') AND
    shipping_mode = 'Standard Class'
*/
--                                  ZADANIE 2
/*
SELECT 
    COUNT(DISTINCT ko.customer_id)
FROM
    kd19.orders ko LEFT JOIN kd19.customers kc ON ko.customer_id = kc.customer_id
WHERE
    EXTRACT(YEAR FROM ko.order_date) = 2019;
*/
--                                  ZADANIE 3
/*
SELECT
    ko.shipping_mode
    ,SUM(kop.item_quantity)
FROM
    --kd19.order_positions.item_quantity ilość sprzedanych produktów
    kd19.orders ko LEFT JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
WHERE
    EXTRACT(YEAR FROM ko.order_date) = 2021
GROUP BY 1
ORDER BY 2 DESC
*/
--                                  ZADANIE 4
/*
SELECT
    shipping_mode
    ,COUNT(ko.order_id) ile_zamowien
FROM
    kd19.orders ko
WHERE
    EXTRACT(YEAR FROM ko.order_date) = 2020
GROUP BY
    1
HAVING
    COUNT(ko.order_id) > 500
*/
--                                  ZADANIE 5
/*
SELECT
    COUNT(order_id)
    ,EXTRACT(MONTH FROM ko.order_date) number_of_month
FROM
    kd19.orders ko
WHERE
    EXTRACT(YEAR FROM ko.order_date) = 2020
GROUP BY
    2
HAVING
    COUNT(order_id) > 100
LIMIT
    3
*/
--                                  ZADANIE 6
/*
SELECT
    category
    ,SUM(kop.item_quantity) ile_sprzedanych
FROM
    kd19.orders ko LEFT JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
    LEFT JOIN kd19.products kp ON kop.product_id = kp.product_id
    LEFT JOIN kd19.product_groups kpg ON kp.group_id=kpg.group_id
WHERE
    EXTRACT(YEAR FROM ko.order_date) = 2020
GROUP BY
    1
*/
--                                  ZADANIE 7
/*
SELECT
    EXTRACT(YEAR FROM ko.order_date) AS year_,
    SUM(kop.item_quantity * kp.product_price * (1 - kop.position_discount)) AS total_income
FROM kd19.orders ko
LEFT JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
LEFT JOIN kd19.products kp ON kop.product_id = kp.product_id
GROUP BY 1
ORDER BY 1;
*/
--                                  ZADANIE 8
/*
SELECT  
    COUNT(DISTINCT ko.customer_id) customers_2021
FROM
    kd19.orders ko
WHERE
    EXTRACT(YEAR FROM ko.order_date) = 2021
    AND ko.customer_id IN
    (
        SELECT customer_id
        FROM kd19.orders
        GROUP BY 1
        HAVING COUNT(*) >= 5
    )
*/
--                                  ZADANIE 9
--                                  items/order 2021
/*
SELECT
    DISTINCT(ko.order_id) distinct_order
    ,SUM(kop.item_quantity)
FROM
    kd19.orders ko JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
WHERE
    EXTRACT(YEAR FROM ko.order_date) = 2021
GROUP BY 1
*/
--                                  avg items/order in 2021
/*
SELECT
    AVG(ab.item_q)
FROM
(
SELECT
    DISTINCT(ko.order_id) distinct_order
    ,SUM(kop.item_quantity) item_q
FROM
    kd19.orders ko JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
WHERE
    EXTRACT(YEAR FROM ko.order_date) = 2021
GROUP BY 1
) ab
*/
--                                  FINAL ANSWER
/*
SELECT
    COUNT(*) total_quantity
FROM
(
    SELECT
        DISTINCT(ko.order_id) distinct_order
        ,SUM(kop.item_quantity) items
    FROM
        kd19.orders ko JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
    WHERE
        EXTRACT(YEAR FROM ko.order_date) = 2021
    GROUP BY 1) x
WHERE
    x.items >
(
    SELECT
        AVG(ab.item_q)
    FROM
    (
    SELECT
        DISTINCT(ko.order_id) distinct_order
        ,SUM(kop.item_quantity) item_q
    FROM
        kd19.orders ko JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
    WHERE
        EXTRACT(YEAR FROM ko.order_date) = 2021
    GROUP BY 1
    ) ab
)
*/
--                                      ZADANIE 10
--                                      items/order all years
/*
SELECT
    DISTINCT(ko.order_id) distinct_order
    ,SUM(kop.item_quantity)
FROM
    kd19.orders ko JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
GROUP BY 1
*/
--                                      avg_items/order all years
/*
SELECT
    AVG(ab.item_q)
FROM
(
SELECT
    DISTINCT(ko.order_id) distinct_order
    ,SUM(kop.item_quantity) item_q
FROM
    kd19.orders ko JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
GROUP BY 1
) ab
*/
--                                      FINAL ANSWER
/*
SELECT
    ROUND(AVG(ab.item_q), 2) avg_class
FROM
(
SELECT
    DISTINCT(ko.order_id) distinct_order
    ,SUM(kop.item_quantity) item_q
FROM
    kd19.orders ko JOIN kd19.order_positions kop ON ko.order_id = kop.order_id
WHERE
    ko.shipping_mode LIKE '%Class'
GROUP BY 1
) ab
*/
