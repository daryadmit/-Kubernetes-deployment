# 🚀 Kubernetes Deployment: WebApp

[🔗 Официальная документация Kubernetes](https://kubernetes.io/docs/home/)

## 🧩 Описание назначения деплоя и приложения

Проект предназначен для **развёртывания веб-приложения** в кластере **Kubernetes** с помощью ресурса **Deployment**.  
Этот деплой демонстрирует, как можно быстро развернуть контейнерное приложение и управлять его жизненным циклом через Kubernetes.

Приложение `webapp` — это пример простого веб-сервиса (например, на **Nginx**), который обрабатывает HTTP-запросы.  
Deployment обеспечивает стабильную работу приложения, создаёт несколько реплик и автоматически восстанавливает их в случае сбоя.  
Service открывает к ним доступ внутри кластера.

> 💡 *Kubernetes Deployment гарантирует высокую доступность приложения и удобство при обновлениях без простоя.*

---

## ⚙️ Параметры деплоя

| Параметр | Значение | Описание |
|-----------|-----------|----------|
| `appName` | `webapp` | Имя приложения |
| `replicas` | `3` | Количество реплик Pod'ов |
| `image` | `nginx:latest` | Docker-образ контейнера |
| `containerPort` | `80` | Порт, на котором работает приложение |
| `serviceType` | `ClusterIP` | Тип сервиса для внутреннего доступа |
| `namespace` | `default` | Пространство имён в Kubernetes |

---

## 🧾 Пример YAML-манифеста

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp-deployment
  labels:
    app: webapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  selector:
    app: webapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
