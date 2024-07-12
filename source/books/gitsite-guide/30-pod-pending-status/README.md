# 24-07-11 pod pending status

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
