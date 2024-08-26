1. SQL Error: [22001][1292] Data truncation: Truncated incorrect DOUBLE value
<details>
  <summary>Explaination</summary>
    Description:
    bug with determine column type when CREATE TABLE
    
    How to repeat:
    mysql> CREATE TEMPORARY TABLE tmp_1 (id INT) SELECT 1;
    Query OK, 1 row affected (0.00 sec)
    Records: 1  Duplicates: 0  Warnings: 0
    mysql> SELECT id
        -> FROM tmp_1
        -> WHERE id = 'aaa'
        -> ;
    Empty set, 1 warning (0.01 sec)
    mysql> CREATE TEMPORARY TABLE tmp_2
        -> SELECT id
        -> FROM tmp_1
        -> WHERE id = 'aaa'
        -> ;
    ERROR 1292 (22007): Truncated incorrect DOUBLE value: 'aaa'
    
    [30 Aug 2021 12:30] MySQL Verification Team
    Hi Mr. Mmmmmm,
    
    Thank you for your bug report.
    
    However, this is not a bug. This is, actually, the expected behaviour.
    
    You are trying to compare an integer to string. In that case, as explained in our Manual, our server converts both values to the floating point type. Since 'aaa' can not be converted to the floating point, you get an error. 
    
    Not a bug.
</details>
