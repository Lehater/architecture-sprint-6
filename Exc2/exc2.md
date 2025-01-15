# Обязательная часть задания:

## Динамическая маршрутизация на основании показателей утилизации памяти

### Напишите манифест развёртывания (Deployment) Kubernetes для запуска тестового приложения.

[deployment.yaml](task1/deployment.yaml)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: scaletest-deployment
  labels:
    app: scaletest
spec:
  replicas: 1
  selector:
    matchLabels:
      app: scaletest
  template:
    metadata:
      labels:
        app: scaletest
    spec:
      containers:
        - name: scaletest-container
          image: shestera/scaletestapp:latest
          ports:
            - containerPort: 8080
          resources:
            limits:
              memory: "30Mi"
            requests:
              memory: "20Mi"

```

### Напишите и примените манифест сервиса (Service) для доступа к приложению.

[service.yaml](task1/service.yaml)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: scaletest-service
spec:
  type: LoadBalancer
  selector:
    app: scaletest
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 32222
```

### Настройте динамическую маршрутизацию 
на основании показателей утилизации оперативной памяти с помощью Horizontal Pod Autoscaler (HPA).

[hpa.yaml](task1/hpa.yaml)
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: scaletest-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: scaletest-deployment
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 80
```

### Сделайте скриншоты дашборда
которые покажут, что количество реплик базы данных поменялось в ответ на сгенерированную нагрузку.

нагрузка 1000 пользователей
![нагрузка 1000 пользователей](task1/pics/Screenshot%20from%202025-01-15%2019-59-26.png)

нагрузка 2000 пользователей
![нагрузка 2000 пользователей](task1/pics/Screenshot%20from%202025-01-15%2020-06-50.png)