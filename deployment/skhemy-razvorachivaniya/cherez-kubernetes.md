---
icon: life-ring
title: Через Kubernetes
order: 1
---

Данная статья описывает способ разворачивания Бипиума через **Kubernetes**.

## Введение

Развертывание приложения в Kubernetes (K8s) позволяет обеспечить его высокую доступность, автоматическое масштабирование и удобное управление контейнерами в изолированной среде.

### Термины, сокращения и обозначения

{% table header="row" %}

---

*  {% colwidth=[220] %}

   Термин

*  {% colwidth=[575] %}

   Описание

---

*  {% colwidth=[220] %}

   Kubernetes (K8s)

*  {% colwidth=[575] %}

   Платформа оркестрации контейнеров, предназначенная для автоматического развертывания, масштабирования и управления приложениями.

---

*  {% colwidth=[220] %}

   Кластер

*  {% colwidth=[575] %}

   Набор серверов (узлов), объединенных в единый Kubernetes-кластер.

---

*  {% colwidth=[220] %}

   Namespace

*  {% colwidth=[575] %}

   Логическое пространство имен для изоляции ресурсов внутри кластера.

---

*  {% colwidth=[220] %}

   Deployment

*  {% colwidth=[575] %}

   Ресурс Kubernetes, отвечающий за создание, обновление и масштабирование подов.

---

*  {% colwidth=[220] %}

   Service

*  {% colwidth=[575] %}

   Ресурс Kubernetes, предоставляющий постоянную точку доступа к группе подов.

---

*  {% colwidth=[220] %}

   Pod

*  {% colwidth=[575] %}

   Минимальная единица развертывания в Kubernetes. Содержит один или несколько контейнеров.

---

*  {% colwidth=[220] %}

   Ingress

*  {% colwidth=[575] %}

   Ресурс Kubernetes, обеспечивающий доступ к сервисам извне по HTTP/HTTPS.

---

*  {% colwidth=[220] %}

   kubectl

*  {% colwidth=[575] %}

   Консольная утилита для управления Kubernetes-кластером.

---

*  {% colwidth=[220] %}

   Манифест

*  {% colwidth=[575] %}

   YAML-файл, описывающий ресурсы Kubernetes.

---

*  {% colwidth=[220] %}

   YAML

*  {% colwidth=[575] %}

   Формат описания конфигурационных файлов Kubernetes.

---

*  {% colwidth=[220] %}

   ConfigMap

*  {% colwidth=[575] %}

   Ресурс Kubernetes для хранения параметров конфигурации приложения.

---

*  {% colwidth=[220] %}

   SSL/TLS

*  {% colwidth=[575] %}

   Протоколы защищенного сетевого соединения.

---

*  {% colwidth=[220] %}

   Let's Encrypt

*  {% colwidth=[575] %}

   Центр сертификации, предоставляющий бесплатные SSL/TLS-сертификаты.

{% /table %}

### Ссылки

-  Kubernetes

   <https://kubernetes.io/>

-  Документация Kubernetes

   <https://kubernetes.io/docs/>

-  Инструменты установки kubernetes

   <https://kubernetes.io/docs/tasks/tools/>

-  NGINX Ingress Controller

   <https://kubernetes.github.io/ingress-nginx/>

-  cert-manager

   <https://cert-manager.io/docs/>

-  PostgreSQL

   <https://www.postgresql.org/docs/>

-  Redis

   <https://redis.io/docs/>

## Подготовка

### Пререквизиты

Перед началом развертывания убедитесь, что ваше окружение соответствует следующим требованиям:

-  **Готовый кластер Kubernetes**: У вас должен быть [настроен](https://kubernetes.io/docs/tasks/tools/#kubeadm) и запущен кластер kubernetes (версии 1.24 или выше).

-  **Доступ к кластеру**: На вашей рабочей машине [установлена](https://kubernetes.io/docs/tasks/tools/#kubectl) утилита `kubectl`, настроенная на подключение к целевому кластеру (файл `~/.kube/config`).

-  **Права доступа**: Учетная запись должна иметь права на создание ресурсов (`Deployment`, `Service`, `StatefulSet` `ConfigMap`) в целевом пространстве имен (Namespace).

## Подготовка конфигурационных файлов

Создайте директорию `k8s` в корне вашего проекта и сохраните в ней следующие файлы манифестов.

### Конфигурация (ConfigMap)

Создайте файл `k8s/configmap.yaml` для хранения переменных окружения:

```yaml
# ==========================================
# Переменные для bpium
# ==========================================
apiVersion: v1
kind: ConfigMap
metadata:
  name: bpium-config
  namespace: default
data:
  DB_CONNECTION_STRING: "postgres://postgres:kuber_password_123@postgres-service:5432/bpium"
  HOST: "domen.ru"        # Необходимо указать домен сервера для доступа из внешних ресурсов
  S3_HOST: "domen.ru"     # Необходимо указать домен сервера для доступа из внешних ресурсов
  S3_PORT: "443"
  S3_HTTPS: "true"
  BPM_HOST: "bpm-service"
  BPM_PORT: "2030"
---
# ==========================================
# Переменные для bpm
# ==========================================
apiVersion: v1
kind: ConfigMap
metadata:
  name: bpm-config
  namespace: default
data:
  BPM_QUEUE_HOST: "redis-service"
---
# ==========================================
# Переменные для bpium-s3
# ==========================================
apiVersion: v1
kind: ConfigMap
metadata:
  name: s3-config
  namespace: default
data:
  S3_HOST: "127.0.0.1"
```

### Манифест Бипиума и сервера исполнения процессов (BPM)

Создайте файл `k8s/deployment.yaml`:

```yaml
# ==========================================
# bpium
# ==========================================
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bpium
  namespace: default
  labels:
    app: bpium
spec:
  replicas: 2         # Здесь можно указать кол-во реплик
  selector:
    matchLabels:
      app: bpium
  template:
    metadata:
      labels:
        app: bpium
    spec:
      containers:
      - name: bpium
        image: bpiumdocker/bpium:latest
        ports:
        - containerPort: 80
        envFrom:
        - configMapRef:
            name: bpium-config
---
# ==========================================
# bpium-service
# ==========================================
apiVersion: v1
kind: Service
metadata:
  name: bpium-service
  namespace: default
spec:
  selector:
    app: bpium
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
---
# ==========================================
# bpm
# ==========================================
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bpm
  namespace: default
  labels:
    app: bpm
spec:
  replicas: 2         # Здесь можно указать кол-во реплик
  selector:
    matchLabels:
      app: bpm
  template:
    metadata:
      labels:
        app: bpm
    spec:
      containers:
      - name: bpm
        image: bpiumdocker/bpm:latest
        ports:
        - containerPort: 2030
        envFrom:
        - configMapRef:
            name: bpm-config
---
# ==========================================
# bpm-service 
# ==========================================
apiVersion: v1
kind: Service
metadata:
  name: bpm-service
  namespace: default
spec:
  selector:
    app: bpm
  ports:
    - protocol: TCP
      port: 2030
      targetPort: 2030
  type: ClusterIP
```

### Манифест базовой инфраструктуры (PostgreSQL и Redis)

Создайте файл `k8s/infrastructure.yaml`:

```yaml
# ==========================================
# PostgreSQL
# ==========================================
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgres
  namespace: default
spec:
  serviceName: "postgres-service"
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
      - name: postgres
        image: postgres:14-alpine
        env:
        - name: POSTGRES_USER
          value: "postgres"
        - name: POSTGRES_PASSWORD
          value: "kuber_password_123"
        - name: POSTGRES_DB
          value: "bpium"
        ports:
        - containerPort: 5432
          name: postgres
        volumeMounts:
        - name: postgres-persistent-storage
          mountPath: /var/lib/postgresql/data
  volumeClaimTemplates:
  - metadata:
      name: postgres-persistent-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 50Gi              # Объем диска для базы данных
---
apiVersion: v1
kind: Service
metadata:
  name: postgres-service
  namespace: default
spec:
  selector:
    app: postgres
  ports:
    - protocol: TCP
      port: 5432
      targetPort: 5432
---
# ==========================================
# 2. КЭШ: Redis (Deployment)
# ==========================================
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis:7-alpine
        ports:
        - containerPort: 6379
---
apiVersion: v1
kind: Service
metadata:
  name: redis-service
  namespace: default
spec:
  selector:
    app: redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
```

### Манифест объектного хранилища (bpium-s3)

Создайте файл `k8s/objectStorage.yaml`

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: bpium-s3
  namespace: default
spec:
  serviceName: "bpium-s3-service"
  replicas: 1
  selector:
    matchLabels:
      app: bpium-s3
  template:
    metadata:
      labels:
        app: bpium-s3
    spec:
      containers:
      - name: bpium-s3
        image: bpiumdocker/s3:latest
        ports:
        - containerPort: 2020
        envFrom:
        - configMapRef:
            name: s3-config
        volumeMounts:
        - name: s3-persistent-storage
          mountPath: /bpiumpac/storage       # Путь к хранилищу файлов внутри контейнера
  volumeClaimTemplates:
  - metadata:
      name: s3-persistent-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 50Gi                # Объем диска под пользовательские файлы
---
apiVersion: v1
kind: Service
metadata:
  name: bpium-s3-service
  namespace: default
spec:
  selector:
    app: bpium-s3
  ports:
    - protocol: TCP
      port: 2020
      targetPort: 2020
```

### Манифест маршрутизации и безопасности (Ingress & TLS)

Создайте файл `k8s/ingress-tls.yaml`

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: your-email@example.com            # Укажите Вашу почту
    privateKeySecretRef:
      name: letsencrypt-prod-account-key
    solvers:
    - http01:
        ingress:
          class: nginx
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: bpium-ingress
  namespace: default
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - yourdomain.com                     # Укажите Ваш домен
    secretName: bpium-tls-secret
  rules:
  - host: yourdomain.com                 # Укажите Ваш домен
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: bpium-service
            port:
              number: 80
      - path: /storage
        pathType: Prefix
        backend:
          service:
            name: bpium-s3-service
            port:
              number: 2020
```

## Применение манифестов в кластере

Выполните команды в терминале для отправки конфигураций в ваш Kubernetes-кластер.

1. **Перейдите в папку с манифестами:**

   ```
   cd k8s
   ```

2. **Примените манифест Tigera Calico:**

   ```
   kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml
   ```

3. **Примените манифест Calico:**

   ```
   kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/custom-resources.yaml
   ```

4. **Примените манифест Cert-manager:**

   ```
   kubectl create namespace cert-manager
   kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.5.4/cert-manager.yaml
   ```

5. **Примените манифест ConfigMap:**

   ```
   kubectl apply -f configmap.yaml
   ```

6. **Примените манифест Infrastructure:**

   ```
   kubectl apply -f infrastructure.yaml
   ```

   :::note 

   Подождите 1-2 минуты, пока под postgres перейдет в статус Running, прежде чем снова применять манифесты.

   :::

7. **Примените манифест Deployment:**

   ```
   kubectl apply -f deployment.yaml
   ```

8. **Примените манифест ObjectStorage:**

   ```
   kubectl apply -f objectStorage.yaml
   ```

9. **Примените манифест Ingress:**

   ```
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/baremetal/deploy.yaml
   ```

   Затем введите:

   ```
   kubectl patch deployment ingress-nginx-controller -n ingress-nginx --type='json' -p='[{"op": "add", "path": "/spec/template/spec/hostNetwork", "value": true}]'
   ```

10. **Примените манифест Ingress-tls (правила маршрутизации):**

    ```
    kubectl apply -f ingress-tls.yaml
    ```

## Проверка статуса развертывания

Убедитесь, что все компоненты успешно запустились и работают корректно.

**Проверка подов (Pods):**

```
kubectl get pods -A
```

*Статус всех подов должен измениться на* `Running`*.*