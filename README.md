# Проект: Kubernetes deployment

## 🧩 Описание назначения деплоя и приложения

Данный проект предназначен для **развёртывания веб-приложения** в кластере **Kubernetes** с помощью ресурса **Deployment**.  
Деплой обеспечивает:
- автоматическое управление количеством реплик приложения,
- устойчивость к сбоям (автовосстановление Pod’ов),
- удобное обновление версии образа без простоя.

Приложение `webapp` — это простое веб-приложение (например, на **Flask** или **Nginx**), которое отвечает на HTTP-запросы.  
Оно используется как пример для демонстрации принципов деплоя и взаимодействия сервисов внутри кластера Kubernetes.

> 💡 *Deployment позволяет гарантировать, что всегда работает нужное количество экземпляров приложения, а Service обеспечивает к ним стабильный доступ.*

---

## ⚙️ Параметры деплоя

| Параметр | Значение | Описание |
|-----------|-----------|----------|
| `appName` | `webapp` | Имя приложения |
| `replicas` | `3` | Количество реплик (Pod'ов) |
| `image` | `nginx:latest` | Используемый Docker-образ |
| `containerPort` | `80` | Порт контейнера |
| `serviceType` | `ClusterIP` | Тип Kubernetes-сервиса |
| `namespace` | `default` | Пространство имён |

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
