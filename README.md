# Проект: Kubernetes deployment

## Описание проекта

> "Управление приложениями в Kubernetes требует чёткого понимания deployment-процессов и их документации"

Данный проект демонстрирует **деплой веб-приложения** в кластере Kubernetes. Приложение представляет собой **REST API сервис** для управления пользовательскими данными.

### Назначение деплоя
Этот deployment обеспечивает:

- **Масштабируемость** приложения
- **Отказоустойчивость** через репликацию
- **Автоматическое восстановление** при сбоях
- **Равномерное распределение** нагрузки

## 📊 Параметры Deployment

| Параметр | Значение | Описание |
|----------|----------|-----------|
| `replicas` | 3 | Количество реплик приложения |
| `image` | `nginx:1.25-alpine` | Docker образ приложения |
| `containerPort` | 80 | Порт контейнера |
| `memory` | 128Mi | Лимит памяти |
| `cpu` | 250m | Лимит CPU |
| `strategy.type` | RollingUpdate | Стратегия обновления |

### Дополнительные настройки

<details>
<summary><b>Расширенные параметры</b></summary>

- **Переменные окружения:**
  - `APP_ENV=production`
  - `LOG_LEVEL=info`

- **Проверки здоровья:**
  - Liveness probe: `/healthz`
  - Readiness probe: `/readyz`

</details>

## 🚀 YAML манифест deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app-deployment
  namespace: production
  labels:
    app: web-app
    environment: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: web-app
        version: v1.2.0
    spec:
      containers:
      - name: web-app
        image: nginx:1.25-alpine
        ports:
        - containerPort: 80
          protocol: TCP
        env:
        - name: APP_ENV
          value: "production"
        - name: LOG_LEVEL
          value: "info"
        resources:
          requests:
            memory: "64Mi"
            cpu: "100m"
          limits:
            memory: "128Mi"
            cpu: "250m"
        livenessProbe:
          httpGet:
            path: /healthz
            port: 80
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /readyz
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
  - `LOG_LEVEL=info`
