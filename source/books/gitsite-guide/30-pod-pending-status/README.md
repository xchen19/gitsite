# 24-07-11 airflow

1. pod pending status
检查Pod的状态和事件：
kubectl describe pod <pod-name> -n <namespace>

这会显示Pod的详细信息，包括事件日志，可以帮助确定问题的原因

发现branch写错。

Events:
  Type     Reason     Age                   From     Message
  ----     ------     ----                  ----     -------
  Normal   Scheduled  6m1s                  stork    Successfully assigned sdssapc-dev/apc-airflow-web-6b55d9c5f6-mmf64 to uls-stg01-gen1-6ccd67ddf-sgtrd
  Normal   Pulling    3m35s (x4 over 5m6s)  kubelet  Pulling image "ibp-op-artifa01.wdc.com:6629/sdss_apc/airflow:test_kafka"
  Warning  Failed     3m33s (x4 over 5m4s)  kubelet  Failed to pull image "ibp-op-artifa01.wdc.com:6629/sdss_apc/airflow:test_kafka": rpc error: code = NotFound desc = failed to pull and unpack image "ibp-op-artifa01.wdc.com:6629/sdss_apc/airflow:test_kafka": failed to resolve reference "ibp-op-artifa01.wdc.com:6629/sdss_apc/airflow:test_kafka": ibp-op-artifa01.wdc.com:6629/sdss_apc/airflow:test_kafka: not found
  Warning  Failed     3m33s (x4 over 5m4s)  kubelet  Error: ErrImagePull
  Warning  Failed     3m18s (x6 over 5m3s)  kubelet  Error: ImagePullBackOff
  Normal   BackOff    2s (x20 over 5m3s)    kubelet  Back-off pulling image "ibp-op-artifa01.wdc.com:6629/sdss_apc/airflow:test_kafka"

2. unfound module
Broken DAG: [/opt/airflow/dags/repo/src/airflow/dag_kafka.py] Traceback (most recent call last):
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File "/opt/airflow/dags/repo/src/airflow/dag_kafka.py", line 6, in <module>
    from kafka.admin import KafkaAdminClient, NewTopic
ModuleNotFoundError: No module named 'kafka'

solution:
Need to install the kafka-python library in the Airflow environment.

Here are the steps to install the kafka-python library:

Step 1: Install kafka-python Library
If you have access to the command line of the environment where Airflow is running, you can install the kafka-python library using pip:

bash
Copy code
pip install kafka-python
Step 2: Add to Airflow Docker Image
If you are using a Docker container for Airflow, you will need to add the installation command to your Dockerfile. For example:

Dockerfile
Copy code
## Example Dockerfile

FROM apache/airflow:2.3.3

USER root

## Install kafka-python library
RUN pip install kafka-python

USER airflow
Then, rebuild your Docker image and redeploy it.

Step 3: Requirements File
Alternatively, if you manage dependencies using a requirements.txt file, you can add kafka-python to this file:

text
Copy code
## requirements.txt
kafka-python
Then, ensure this file is referenced in your Airflow setup to install dependencies.

Step 4: Verify Installation
After installing the library, verify that the module can be imported successfully:

Open a Python shell in the same environment where Airflow is running:

bash
Copy code
python
Try importing the kafka module:

python
Copy code
from kafka.admin import KafkaAdminClient, NewTopic
If no error occurs, the installation is successful, and your DAG should no longer show the ModuleNotFoundError.

Step 5: Restart Airflow
After installing the required module, restart your Airflow services to ensure the new dependencies are loaded.

bash
Copy code
## Commands to restart Airflow services
airflow scheduler restart
airflow webserver restart
By following these steps, the ModuleNotFoundError: No module named 'kafka' error should be resolved, and your DAG should be able to import the kafka module without any issues.
