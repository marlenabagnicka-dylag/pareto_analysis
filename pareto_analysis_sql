WITH filtered_data AS (
SELECT
    product,
    actual_time,
    standard_time,
    efficiency_pct BETWEEN 5 AND 500 AS efficiency_valid
FROM
     `eliminacja-rozbieznosci.efektywnosc.efficiency_table_2_0`
),
product_summary as(
SELECT
    product,
    SUM(actual_time) as total_actual_time,
    SUM(standard_time) as total_standard_time
FROM
    filtered_data
WHERE
    efficiency_valid=TRUE
GROUP BY
    product
    )
SELECT
    product,
    ROUND(total_actual_time,0) AS total_actual_time,
    ROUND(total_standard_time * 100 / NULLIF(total_actual_time, 0),2) AS efficiency_pct_weighted,
    ROUND(product_summary.total_actual_time*100/sum(product_summary.total_actual_time) over(),2) as time_share_pct,
   ROUND(sum(total_actual_time) over (order by total_actual_time desc ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)/sum(total_actual_time) over(),4) as cumulative_share
FROM product_summary
--QUALIFY cumulative_share<=0.8
ORDER BY total_actual_time desc


