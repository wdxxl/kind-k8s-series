
### 1. 准备 简单环境
```
kubectl apply -f assets/application-cpu/deployment.yaml 

kubectl apply -f assets/application-cpu/traffic-generator.yaml 
```

### 2. 验证HPA - 需要依赖 MetricsServer
```
kubectl scale deploy/application-cpu --replicas 2
kubectl autoscale deploy/application-cpu --cpu-percent=50 --min=1 --max=10
kubectl get hpa/application-cpu  -owide
kubectl describe hpa/application-cpu
```

### 3. 增加流量 查看效果
```
# get a terminal to the traffic-generator
kubectl exec -it traffic-generator sh

# install wrk
apk add --no-cache wrk

# simulate some load
wrk -c 5 -t 5 -d 99999 -H "Connection: Close" http://application-cpu
```

### 4. HPA 效果验证 良好