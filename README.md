CODE USED 

# dbt_snowflake_combination
dbt_snowflake_combination

/*
with customers as (

    select
        id as customer_id,
        first_name,
        last_name

    from raw.jaffle_shop.customers

),

orders as (

    select
        id as order_id,
        user_id as customer_id,
        order_date,
        status

    from raw.jaffle_shop.orders

),

customer_orders as (

    select
        customer_id,

        min(order_date) as first_order_date,
        max(order_date) as most_recent_order_date,
        count(order_id) as number_of_orders

    from orders

    group by 1

),

final as (

    select
        customers.customer_id,
        customers.first_name,
        customers.last_name,
        customer_orders.first_order_date,
        customer_orders.most_recent_order_date,
        coalesce(customer_orders.number_of_orders, 0) as number_of_orders

    from customers

    left join customer_orders using (customer_id)

)

select * from final
*/


-- Clean Up
-- drop warehouse if exists transforming;
-- drop database if exists  raw;
-- drop database if exists analytics;

-- select MAIN.* from (
--     select count(1),'STG_CUSTOMERS' as LoadedInto from ANALYTICS.DBT_RAKESHVELCHURI.STG_CUSTOMERS
--     UNION ALL
--     select count(1),'STG_ORDERS' as LoadedInto  from ANALYTICS.DBT_RAKESHVELCHURI.STG_ORDERS
--     UNION ALL
--     select count(1),'CUSTOMERS' as LoadedInto  from ANALYTICS.DBT_RAKESHVELCHURI.CUSTOMERS
-- ) MAIN


-- select * from ANALYTICS.DBT_RAKESHVELCHURI.STG_CUSTOMERS
-- select * from ANALYTICS.DBT_RAKESHVELCHURI.STG_ORDERS
-- select * from ANALYTICS.DBT_RAKESHVELCHURI.CUSTOMERS

-- select * from RAW.JAFFLE_SHOP.CUSTOMERS
/*
ID,FIRST_NAME,LAST_NAME
*/

-- select * from RAW.JAFFLE_SHOP.ORDERS
/*
ID, USER_ID,ORDER_DATE,STATUS,_ETL_LOADED_AT
*/

-- select * from RAW.STRIPE.PAYMENT
/*
ID, ORDERID, PAYMENTMETHOD,STATUS,AMOUNT,CREATED,_BATCHED_AT
*/

<!-- SELECT
     A.ID
    ,A.ORDERID
    ,A.PAYMENTMETHOD
    ,A.STATUS
    ,A.AMOUNT
    ,A.CREATED
    ,A._BATCHED_AT
    ,B.USER_ID
    ,B.ORDER_DATE
    ,B.STATUS
    ,B._ETL_LOADED_AT
    ,C.FIRST_NAME
    ,C.LAST_NAME
FROM RAW.STRIPE.PAYMENT A 
LEFT OUTER JOIN RAW.JAFFLE_SHOP.ORDERS B ON A.ORDERID = B.ID
LEFT OUTER JOIN RAW.JAFFLE_SHOP.CUSTOMERS C ON B.USER_ID = C.ID -->