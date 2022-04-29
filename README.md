# snowflake_uplod_performance


* Raw Data  : 
    Data Size : 
        187G
    File lise :
        's3://databricks-manure/tpc-ds/store_sales/store_sales_1_8.dat',
        's3://databricks-manure/tpc-ds/store_sales/store_sales_2_8.dat',
        's3://databricks-manure/tpc-ds/store_sales/store_sales_3_8.dat',
        's3://databricks-manure/tpc-ds/store_sales/store_sales_4_8.dat',
        's3://databricks-manure/tpc-ds/store_sales/store_sales_5_8.dat',
        's3://databricks-manure/tpc-ds/store_sales/store_sales_6_8.dat',
        's3://databricks-manure/tpc-ds/store_sales/store_sales_7_8.dat'



Data load ( Folder path)
<pre>
use database tpcds;
truncate tpcds.store_sales;
use schema public;
copy into tpcds.store_sales from '@TPCDS_STAGE/store_sales/' file_format = (type = csv field_delimiter='|' error_on_column_count_mismatch=false VALIDATE_UTF8=false );
</pre>
