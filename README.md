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


# 참고
* https://interworks.com/blog/2020/03/04/zero-to-snowflake-multi-threaded-bulk-loading-with-python/

