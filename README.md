# Kubernetes Deployment для веб-приложения

> Цель проекта — обеспечить масштабируемое, отказоустойчивое и легко обновляемое развертывание веб-приложения в кластере Kubernetes.

Этот Deployment управляет жизненным циклом подов с контейнеризованным приложением, гарантируя заданное количество реплик, автоматическое восстановление при сбоях и плавные обновления без простоя.

---

## 📋 Параметры Deployment

В таблице ниже перечислены основные параметры манифеста и их значения по умолчанию:

| Параметр                     | Описание                                      | Значение по умолчанию     |
|-----------------------------|-----------------------------------------------|---------------------------|
| replicas                  | Количество запущенных подов                   | 3                       |
| image                     | Образ контейнера приложения                   | nginx:1.25              |
| containerPort             | Порт, на котором слушает приложение           | 80                      |
| resources.requests.cpu    | Запрашиваемые CPU-ресурсы на под              | 100m                    |
| resources.requests.memory | Запрашиваемая память на под                   | 128Mi                   |
| resources.limits.cpu      | Максимальные CPU-ресурсы на под               | 200m                    |
| resources.limits.memory   | Максимальная память на под                    | 256Mi                   |
| strategy.type             | Стратегия обновления                          | RollingUpdate           |

> 💡 Совет: Настройте лимиты ресурсов в соответствии с нагрузкой вашего приложения, чтобы избежать нехватки ресурсов или их избыточного резервирования.

---

## 📄 Пример манифеста

Ниже приведён полный YAML-манифест для развертывания приложения:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app-deployment
  labels:
    app: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: web-app
        image: nginx:1.25
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "200m"
      restartPolicy: Always
