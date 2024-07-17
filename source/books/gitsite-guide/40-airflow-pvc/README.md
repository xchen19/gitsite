# airflow variable
1. 查找存的位置：
kubectl get pods -n namespace

2. 进入pod
kubectl exec -it airflow-postgresql-0 -n namespace -- /bin/bash

3. 连接数据库
\c airflow

4. 查看variable
SELECT * FROM variable;

