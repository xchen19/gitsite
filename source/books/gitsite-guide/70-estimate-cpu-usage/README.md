# CPU
在 Kubernetes 的 Pod 中监控单个文件处理命令的 CPU 消耗，可以使用以下方法：

---

### **方法一：通过 `kubectl exec` 和 `top`**
1. **进入目标 Pod**：
   ```bash
   kubectl exec -it <pod_name> -- /bin/bash
   ```
   或者：
   ```bash
   kubectl exec -it <pod_name> -- /bin/sh
   ```

2. **在 Pod 内运行 `top`**：
   如果 Pod 内包含 `top` 命令，运行：
   ```bash
   top
   ```
   - 找到运行中的文件处理命令（如 `python`）。
   - 观察其 `%CPU` 列的占用情况。

3. **安装 `procps` 工具（如果 `top` 不可用）**：
   如果 `top` 没有安装，可以尝试安装相关工具（需要 Pod 基础镜像支持包管理器）：
   ```bash
   apt-get update && apt-get install -y procps
   ```
   然后运行 `top` 或其他监控工具。

---

### **方法二：通过 `kubectl exec` 和 `ps`**
1. **进入 Pod**：
   ```bash
   kubectl exec -it <pod_name> -- /bin/bash
   ```

2. **使用 `ps` 查看进程**：
   ```bash
   ps aux
   ```
   找到目标进程（如 `python` 或文件处理命令），记录其进程 ID (`PID`)。

3. **监控进程 CPU 使用**：
   使用以下命令实时监控 CPU 消耗：
   ```bash
   top -p <PID>
   ```
   或：
   ```bash
   while true; do ps -p <PID> -o %cpu,%mem,cmd; sleep 1; done
   ```

---

### **方法三：使用 `kubectl top pod`**
1. **检查 Pod 的整体资源消耗**：
   ```bash
   kubectl top pod <pod_name> -n <namespace>
   ```
   输出示例：
   ```
   NAME          CPU(cores)   MEMORY(bytes)
   <pod_name>    200m         128Mi
   ```
   - 可以用此信息推测单个任务的资源消耗，结合并发度分配适当资源。

---

### **方法四：通过 Sidecar 容器运行监控工具**
如果需要长期监控或更详细的资源分析，可以添加一个 Sidecar 容器运行监控工具（如 `htop` 或 `pidstat`）：

1. **定义 Sidecar 容器**：
   在 Pod 的 YAML 中添加一个运行监控工具的容器：
   ```yaml
   containers:
   - name: monitoring
     image: ubuntu
     command: ["sleep", "infinity"]
     resources:
       requests:
         cpu: 100m
         memory: 128Mi
       limits:
         cpu: 200m
         memory: 256Mi
   ```

2. **进入 Sidecar 容器**：
   ```bash
   kubectl exec -it <pod_name> -c monitoring -- /bin/bash
   ```

3. **安装并运行监控工具**：
   安装并运行工具，如：
   ```bash
   apt-get update && apt-get install -y sysstat
   pidstat -p <PID> 1
   ```

---

### **方法五：通过 Prometheus 和 Metrics Server**
1. **启用 Metrics Server**：
   确保 Kubernetes 集群已安装 Metrics Server。
   ```bash
   kubectl top pod <pod_name>
   ```

2. **结合 Prometheus 监控**：
   - 安装 Prometheus 和 Grafana。
   - 配置指标采集规则，监控 Pod 的 CPU 使用情况。

---

### **方法六：通过 Kubernetes Logs 分析资源消耗**
修改任务脚本，打印处理文件的时间和资源消耗：
1. 修改脚本，记录处理时间：
   ```python
   import time
   import os

   start_time = time.time()
   # 执行文件处理逻辑
   end_time = time.time()

   print(f"CPU: {os.cpu_percent()}, Time Taken: {end_time - start_time}")
   ```
2. 查看 Pod 日志：
   ```bash
   kubectl logs <pod_name>
   ```

---

### **结论**
- **轻量监控**：使用 `kubectl exec` 和 `top/ps`。
- **实时分析**：使用 `pidstat` 或在脚本中打印 CPU 和时间。
- **长期监控**：通过 Prometheus 和 Metrics Server。

根据单任务 CPU 消耗结果，可以调整 Pod 的并发度与资源分配。

