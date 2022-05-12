# snowflake_uplod_performance



* Raw Data  : 
    * Data Size(Rows Num) : 
        - 187G(14억건)
    * File lise :
        - 's3://databricks-manure/tpc-ds/store_sales/store_sales_1_8.dat',
        - 's3://databricks-manure/tpc-ds/store_sales/store_sales_2_8.dat',
        - 's3://databricks-manure/tpc-ds/store_sales/store_sales_3_8.dat',
        - 's3://databricks-manure/tpc-ds/store_sales/store_sales_4_8.dat',
        - 's3://databricks-manure/tpc-ds/store_sales/store_sales_5_8.dat',
        - 's3://databricks-manure/tpc-ds/store_sales/store_sales_6_8.dat',
        - 's3://databricks-manure/tpc-ds/store_sales/store_sales_7_8.dat'



* Data load #1 ( Folder path)

    <pre>
    use database tpcds;
    truncate tpcds.store_sales;
    use schema public;
    copy into tpcds.store_sales from '@TPCDS_STAGE/store_sales/' file_format = (type = csv field_delimiter='|' error_on_column_count_mismatch=false VALIDATE_UTF8=false );
    </pre>


    - 30분35초

* Data load #2 

    <pre>
    COPY INTO tpcds.store_sales
    FROM @TPCDS_STAGE/store_sales/
    FILES = ('store_sales_1_8.dat',
    'store_sales_2_8.dat',
    'store_sales_3_8.dat',
    'store_sales_4_8.dat',
    'store_sales_5_8.dat',
    'store_sales_6_8.dat',
    'store_sales_7_8.dat')

    file_format = (type = csv field_delimiter='|' error_on_column_count_mismatch=false VALIDATE_UTF8=false )
    ;
    </pre>

    - 27분26초

* Data Load #3

  - 7개 쿼리 동시 수행
  - 27분



* Data Load #4

 - Spark Connector
 <pre>
text = spark.read.csv("s3a://databricks-manure/tpc-ds/store_sales/", sep ='|', header = False,  inferSchema='True')
text2 = text.withColumnRenamed("_c0","ss_sold_date_sk") \
        .withColumnRenamed("_c1","ss_sold_time_sk") \
        .withColumnRenamed("_c2","ss_item_sk")\
        .withColumnRenamed("_c3","ss_customer_sk")\
        .withColumnRenamed("_c4","ss_cdemo_sk")\
        .withColumnRenamed("_c5","ss_hdemo_sk")\
        .withColumnRenamed("_c6","ss_addr_sk")\
        .withColumnRenamed("_c7","ss_store_sk")\
        .withColumnRenamed("_c8","ss_promo_sk")\
        .withColumnRenamed("_c9","ss_ticket_number")\
        .withColumnRenamed("_c10","ss_quantity")\
        .withColumnRenamed("_c11","ss_wholesale_cost")\
        .withColumnRenamed("_c12","ss_list_price")\
        .withColumnRenamed("_c13","ss_sales_price")\
        .withColumnRenamed("_c14","ss_ext_discount_amt")\
        .withColumnRenamed("_c15","ss_ext_sales_price")\
        .withColumnRenamed("_c16","ss_ext_wholesale_cost")\
        .withColumnRenamed("_c17",F"ss_ext_list_price")\
        .withColumnRenamed("_c18","ss_ext_tax")\
        .withColumnRenamed("_c19","ss_coupon_amt")\
        .withColumnRenamed("_c20","ss_net_paid")\
        .withColumnRenamed("_c21","ss_net_paid_inc_tax")\
        .withColumnRenamed("_c22","ss_net_profit")

sfOptions = { 
"sfURL" : "atixoaj-dev_manure.snowflakecomputing.com", 
"sfAccount" : "atixoaj-dev_manure", 
"sfUser" : "manure", 
"sfPassword" : "XXXXX", 
"sfDatabase" : "TPCDS", 
"sfSchema" : "TPCDS", 
"sfWarehouse" : "DEMO_MANURE_S", 
"sfRole" : "SYSADMIN", 
}

SNOWFLAKE_SOURCE_NAME = "net.snowflake.spark.snowflake"
 
text2.write.format(SNOWFLAKE_SOURCE_NAME).options(**sfOptions).option("dbtable", "store_sales_test").mode("overwrite").save()

데이터 전송에 30분 가량 소요.

 <pre>


# 참고
* https://interworks.com/blog/2020/03/04/zero-to-snowflake-multi-threaded-bulk-loading-with-python/

