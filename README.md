# Домашнее задание к занятию «Настройка приложений и управление доступом в Kubernetes»

### Примерное время выполнения задания

120 минут

### Цель задания

Научиться:
- Настраивать конфигурацию приложений с помощью **ConfigMaps** и **Secrets**
- Управлять доступом пользователей через **RBAC**

Это задание поможет вам освоить ключевые механизмы Kubernetes для работы с конфигурацией и безопасностью. Эти навыки необходимы для уверенного администрирования кластеров в реальных проектах. На практике навыки используются для:
- Хранения чувствительных данных (Secrets)
- Гибкого управления настройками приложений (ConfigMaps) 
- Контроля доступа пользователей и сервисов (RBAC)

------

## **Подготовка**
### **Чеклист готовности**
- Установлен Kubernetes (MicroK8S, Minikube или другой)
- Установлен `kubectl`
- Редактор для YAML-файлов (VS Code, Vim и др.)
- Утилита `openssl` для генерации сертификатов

------

### Инструменты, которые пригодятся для выполнения задания

1. [Инструкция](https://microk8s.io/docs/getting-started) по установке MicroK8S
2. [Инструкция](https://minikube.sigs.k8s.io/docs/start/) по установке Minikube
3. [Инструкция](https://kubernetes.io/docs/tasks/tools/) по установке kubectl
4. [Инструкция](https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools) по установке VS Code

### Дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/concepts/configuration/secret/) Secret.
2. [Описание](https://kubernetes.io/docs/concepts/configuration/configmap/) ConfigMap.
3. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool.
4. [Описание](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) RBAC.
5. [Пользователи и авторизация RBAC в Kubernetes](https://habr.com/ru/company/flant/blog/470503/).
6. [RBAC with Kubernetes in Minikube](https://medium.com/@HoussemDellai/rbac-with-kubernetes-in-minikube-4deed658ea7b).

------

## **Задание 1: Работа с ConfigMaps**
### **Задача**
Развернуть приложение (nginx + multitool), решить проблему конфигурации через ConfigMap и подключить веб-страницу.

### **Шаги выполнения**
1. **Создать Deployment** с двумя контейнерами
   - `nginx`
   - `multitool`
3. **Подключить веб-страницу** через ConfigMap
4. **Проверить доступность**

### **Что сдать на проверку**
- Манифесты:
  - `deployment.yaml`
  - `configmap-web.yaml`
- Скриншот вывода `curl` или браузера

<img width="634" height="131" alt="image" src="https://github.com/user-attachments/assets/186d8a6f-7d08-40b7-b88a-9b3160291175" />

<img width="564" height="110" alt="image" src="https://github.com/user-attachments/assets/58a67cd3-a415-4b89-9343-314f0e06454a" />




---
## **Задание 2: Настройка HTTPS с Secrets**  
### **Задача**  
Развернуть приложение с доступом по HTTPS, используя самоподписанный сертификат.

### **Шаги выполнения**  
1. **Сгенерировать SSL-сертификат**
```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key -out tls.crt -subj "/CN=myapp.example.com"
```

<img width="943" height="127" alt="image" src="https://github.com/user-attachments/assets/5951a308-87c7-4b39-a50b-b49acd77a10e" />


2. **Создать Secret**
3. **Настроить Ingress**
4. **Проверить HTTPS-доступ**

### **Что сдать на проверку**  
- Манифесты:
  - `secret-tls.yaml`
  - `ingress-tls.yaml`
- Скриншот вывода `curl -k`


Создаем Secret для сертификата {kubectl create secret tls tls-secret --cert=tls.crt --key=tls.key} (тогда он хранится в подкорке с именем tls-secret) 
или можем сами сделать требуемый файл самостоятельно:

```
apiVersion: v1
kind: Secret
metadata:
  name: tls-secret
type: kubernetes.io/tls
data:
  tls.crt: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURHekNDQWdPZ0F3SUJBZ0lVQlR3d0FsaU55ZVlDVWFSaTdhUGgvT0htKzc4d0RRWUpLb1pJaHZjTkFRRUwKQlFBd0hURWJNQmtHQTFVRUF3d1NiWGt0WVhCd0xtVjRZVzF3YkdVdVkyOXRNQjRYRFRJMU1Ea3lOREE1TXpjeApObG9YRFRJMk1Ea3lOREE1TXpjeE5sb3dIVEViTUJrR0ExVUVBd3dTYlhrdFlYQndMbVY0WVcxd2JHVXVZMjl0Ck1JSUJJakFOQmdrcWhraUc5dzBCQVFFRkFBT0NBUThBTUlJQkNnS0NBUUVBc1ZibzVmZFlVRGFJdm5LK21sVXUKTjY4d2t4R3QwMk1raWx2Tmk4QUxSVjB4SkRLWGRmSWQvN3ZsN3dpUDNoeU9EdFpGaUthVDlXT3hScTdWYWxwYwo4MjcvOVNBbklvWlRXRExMRkF0U2hXcmJpZ1hCVFFjV29xekRaMUpLZ3cvRk93bEp3ZFZwdUQ2SEhYM0FnN1BWCjl3Mk5BOW9ibGp1Q01qV1Jkc1V2TzNLaGJzR2xiMlhLdkVXS1pETGdrNElkd2o0T1hPMGxtV0o0UE9WaG1OaEgKbFF5UFp1ejBCajk3ZFF3elNQa2dEeGtuaE12MmxKUGtTNWJtb1FLczZUMjMxRXJac05Qb3VjeTNDOGV4cGhDbApIUlVJa3JlZkQ1aU5oTTVjeS9keS9MeUV3NFQ4UkY4K1RJU1ZBRHdtKytRUnNvNzhxeFdZYVZ3R0JmMnpLMTVqClp3SURBUUFCbzFNd1VUQWRCZ05WSFE0RUZnUVVGV1BZUStyTEVmWXhEMW91WjlzUXF5c1d0SzR3SHdZRFZSMGoKQkJnd0ZvQVVGV1BZUStyTEVmWXhEMW91WjlzUXF5c1d0SzR3RHdZRFZSMFRBUUgvQkFVd0F3RUIvekFOQmdrcQpoa2lHOXcwQkFRc0ZBQU9DQVFFQWN4aWNGb0NSNXcwUzE1QjdESTRkb3hBbGFBSDJ1Q3kvWTVHdGNsSjZ0S3FZCkIrb0ZjdjdneW91aVVzcFBaNHhDKzhiTW4wTm9LUnpOUytabFQrVXNsTzhwQmVuUnFnL3RmTGxaMnJGWXM2cFoKOEFZcHFUekhGRnpuQnkrMUdSQUw4aGhWRDdscW1teTdYbE1WNDlrZXZ3ZHdNeHVRTDAzdGlHREZEa2NRYksxagowY3h4ZGQ5aHF0MkRhdHd0c3hKUytXc1JuMmdMb3hsaWV1aVFENEJHanJIZW4xdUdpUFp2dXdUcUJRV2dtRllQCmhCSVdEMTZuTWRXRWx4bVE2b1ZRRXZZcmdMa0tYd1pyUjAwZEIrQi95aWVRaUtIRmNOSlNBZWdzVFJ2d3VyalUKN29mOTU2RW1BZ2QxSWxkWS9sYmNlTktJV0U4bUR1d3c1MEFSdWs0RWh3PT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
  tls.key: LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tCk1JSUV2Z0lCQURBTkJna3Foa2lHOXcwQkFRRUZBQVNDQktnd2dnU2tBZ0VBQW9JQkFRQ3hWdWpsOTFoUU5vaSsKY3I2YVZTNDNyekNURWEzVFl5U0tXODJMd0F0RlhURWtNcGQxOGgzL3UrWHZDSS9lSEk0TzFrV0lwcFAxWTdGRwpydFZxV2x6emJ2LzFJQ2NpaGxOWU1zc1VDMUtGYXR1S0JjRk5CeGFpck1OblVrcUREOFU3Q1VuQjFXbTRQb2NkCmZjQ0RzOVgzRFkwRDJodVdPNEl5TlpGMnhTODdjcUZ1d2FWdlpjcThSWXBrTXVDVGdoM0NQZzVjN1NXWlluZzgKNVdHWTJFZVZESTltN1BRR1AzdDFERE5JK1NBUEdTZUV5L2FVaytSTGx1YWhBcXpwUGJmVVN0bXcwK2k1ekxjTAp4N0dtRUtVZEZRaVN0NThQbUkyRXpsekw5M0w4dklURGhQeEVYejVNaEpVQVBDYjc1Qkd5anZ5ckZaaHBYQVlGCi9iTXJYbU5uQWdNQkFBRUNnZ0VCQUlUOGdhYUNkKzJmRjZpSjc3bFlpMUlpeERCbG40N1gyRDBSWHZDZjBWUHcKOExzT1hWcUxlRWVncG1zOFpuYlB2eEFOU3hPUXAzL3JoTE5XeEovK3A1bTk4Wi9tdnJJN1BTRDA1aWxmM0VCRQp3K0diTXp3S1JzVXkvVTNyL2dpT3VQN3VsaXNQV1RwYldZT2FHOVluOUJwU1JSOVFYa09vMkpmQ2FCVkRCWXB6CnlsUlYrR0o2dm9ZOTFtQ0JTK2RCajh1WndVZWhuZzdYMnRyNVAzZk1haitQejVEZ2lSZ0Z3V1VweHlGVEdaMEIKdStUbnBvUE45YUJKcUxsdU4rSHRGbzlMTnZXakhKSEwrUExVYzZMdnk5bTRQUHFBN1Q1QlMyRTZJVDk0VUkxUQpjSXFkaGVEeGNIbEJRN3hFQm40c2FRbHhTWlNad0hVS3ptUEV3Z08zUnFFQ2dZRUEySFdwaXpYb0RzTUt0T1hyClluelRvRUd5RVZBcnpoKy9XaTJvUlZseVdEODhIRmRXZ2NmcE1KQnA4Wno0UjBRdnJHcVJtamo3K29UL0hkcGQKM3RBalBGWkVmQTV4RjRBZndPYUNDZHQ4Ym1yOWNSNWJNanpwSXhkTHdKRTJhSUlmRll3a25DUWRuZTlqY3p6ZwpjZmYzNGl4TmlOc1hlOUFtMGE0aVR1emc2cmtDZ1lFQTBidmR5aHU0b2M4WkNQSVk0V3UzTWJTZzRkeTVUNk54CjM1YlpOZDdOdlM3eDBwbW45WDhITG9mYklHUW85aUVGQ3NNeTZqdHJtOUVwRVV6S2FEQmtsU2o4QWs1bGJsMWcKQldvRFJSaFBaaW1OMzQyZ0FmQnZvT3ZaNFAxdnRUM0hpMlF2QmV6UTdaNmVNYThtRDZrb0pSNGdici83MWNpYQpWWmdoTkF4YUx4OENnWUVBcXFzekw5RWtGQ2VTbElsSUs1SlNaZVFHbTRJRDEvVE9Nak1YbnY1a210SFkrbHVlCm1KdGY4R3VkTE9UZ0dZallzZkFndDJIQXc0a0RnYTFBSUVNcDFSUUwwV2l0b0tMajVudVpBbDZ0WUg0NU1HeUgKNlRkL2RxeVNqTld4K1hySE9YMFRESTJwVUhLRWprTHNrTSs4QWZkK2RxNlFlSTNwWGFBWDZ2VDRiZmtDZ1lCMwp5MStHUmtreUd2Rkl1OGRjVWtNajMvRVlzUk1qbXM2N0VCVm5BS2p4Q3ZSUy96TUJOUm9zQ0tzdm1DWVJWNURpCnNkWE9GanlEbG5kbml6MzlQczdrcDdFeHZBZVJmMElPTlp4Q2hmMHI5RVkxejFYNlpaUE5EWW00U2VuWlVyMDgKTCsvdjZYRDRtR1h4S1FLTFpXb3BzVWlER2FORlc3eFRjWDVkbFVTWnJ3S0JnRzVGWjIwdlduQVdPVnZmcU5WQgpMOHhkNmErS0FybU5BZkNYb3NrNjJickQ4R09zSWZVMG1jU1VMdlMveUtKQWU5RU5Dd201SllDdE5YMkxJZ0pxCmpRQXJwNXFrVGQ0djAvcmYyRG1XRWJsbDVGUC9XeHJUb2tJMGxDT3R3TlBVUE8yS3A5OUg4UzBiVUNpZmZBT2UKT3cwQ1ZtVEVRMy9KdFFER2xpMDlZelNRCi0tLS0tRU5EIFBSSVZBVEUgS0VZLS0tLS0K
# TLS в base64 вместе с -----BEGIN CERTIFICATE----- и -----END CERTIFICATE-----
```

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: https-ingress
  annotations:
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
spec:
  tls:
  - hosts:
    - my-app.example.com
    secretName: tls-secret
  rules:
  - host: my-app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: main-service
            port:
              number: 443
```

```
apiVersion: v1
kind: Service
metadata:
  name: main-service
spec:
  selector:
    app: main
  ports:
    - protocol: TCP
      port: 443
      targetPort: 443
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: main
spec:
  replicas: 1
  selector:
    matchLabels:
      app: main
  template:
    metadata:
      labels:
        app: main
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 443
        volumeMounts:
        - name: nginx-config
          mountPath: /etc/nginx/conf.d/default.conf
          subPath: default.conf
        - name: web-html
          mountPath: /usr/share/nginx/html
        - name: cert-volume
          mountPath: /etc/nginx/ssl
      volumes:
      - name: nginx-config
        configMap:
          name: nginx-https-config
      - name: web-html
        configMap:
          name: nginx-html
      - name: cert-volume
        secret:
          secretName: tls-secret
```

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-https-config
data:
  default.conf: |
    server {
        listen       443 ssl;
        server_name  localhost;

        ssl_certificate      /etc/nginx/ssl/tls.crt;
        ssl_certificate_key  /etc/nginx/ssl/tls.key;

        location / {
            root   /usr/share/nginx/html;
            index  index.html;
        }
    }
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-html
data:
  index.html: |
    <html>
    <body>
    test
    </body>
    </html>
```
по ip
<img width="1040" height="235" alt="image" src="https://github.com/user-attachments/assets/dd82224c-028a-47a2-b889-197c169086d2" />

по имени, сертификт есть
<img width="810" height="502" alt="image" src="https://github.com/user-attachments/assets/218b6357-03eb-4ea6-9137-e810b277b3fd" />








---
## **Задание 3: Настройка RBAC**  
### **Задача**  
Создать пользователя с ограниченными правами (только просмотр логов и описания подов).

### **Шаги выполнения**  
1. **Включите RBAC в microk8s**
```bash
microk8s enable rbac
```
2. **Создать SSL-сертификат для пользователя**
```bash
openssl genrsa -out developer.key 2048
openssl req -new -key developer.key -out developer.csr -subj "/CN={ИМЯ ПОЛЬЗОВАТЕЛЯ}"
openssl x509 -req -in developer.csr -CA {CA серт вашего кластера} -CAkey {CA ключ вашего кластера} -CAcreateserial -out developer.crt -days 365
```
3. **Создать Role (только просмотр логов и описания подов) и RoleBinding**
4. **Проверить доступ**

### **Что сдать на проверку**  
- Манифесты:
  - `role-pod-reader.yaml`
  - `rolebinding-developer.yaml`
- Команды генерации сертификатов
- Скриншот проверки прав (`kubectl get pods --as=developer`)

---
## Шаблоны манифестов с учебными комментариями
### **1. Deployment с ConfigMap (nginx + multitool)**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: nginx-config # ПОДКЛЮЧЕНИЕ ConfigMap
          mountPath: /etc/nginx/conf.d
      volumes:
      - name: nginx-config
        configMap:
          name: nginx-config # УКАЖИТЕ имя созданного ConfigMap
```
### **2. ConfigMap для веб-страницы**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: web-content # ИЗМЕНИТЕ: Укажите имя ConfigMap
  namespace: default # ОПЦИОНАЛЬНО: Укажите namespace, если не default
data:
  # КЛЮЧЕВОЙ МОМЕНТ: index.html будет подключен как файл
  index.html: |
    <!DOCTYPE html>
    <html>
    <head>
      <title>Страница из ConfigMap</title> # ИЗМЕНИТЕ: Заголовок страницы
    </head>
    <body>
      <h1>Привет от Kubernetes!</h1> # ДОБАВЬТЕ: Свой контент страницы
    </body>
    </html>
```

### **3. Secret для TLS-сертификата**
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: tls-secret # ИЗМЕНИТЕ при необходимости
type: kubernetes.io/tls
data:
  tls.crt: # ЗАМЕНИТЕ на base64-код сертификата (cat tls.crt | base64 -w 0)
  tls.key: # ЗАМЕНИТЕ на base64-код ключа (cat tls.key | base64 -w 0)
```
### **4. Role для просмотра подов**
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-viewer # ИЗМЕНИТЕ: Название роли
  namespace: default # ВАЖНО: Role работает только в указанном namespace
rules:
- apiGroups: [""] # КЛЮЧЕВОЙ МОМЕНТ: "" означает core API group
  resources: # РАЗРЕШЕННЫЕ РЕСУРСЫ:
    - pods # Доступ к просмотру подов
    - pods/log # Доступ к логам подов
  verbs: # РАЗРЕШЕННЫЕ ДЕЙСТВИЯ:
    - get # Просмотр отдельных подов
    - list # Список всех подов
    - watch # Мониторинг изменений
    - describe # Просмотр деталей
# ДОПОЛНИТЕЛЬНО: Можно добавить больше правил для других ресурсов
```
---

## **Правила приёма работы**
1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать:
   - Скриншоты вывода команд `kubectl`
   - Скриншоты результатов выполнения
   - Тексты манифестов или ссылки на них
3. Для заданий с TLS приложите команды генерации сертификатов

## **Критерии оценивания задания**
1. Зачёт: Все задачи выполнены, манифесты корректны, есть доказательства работы (скриншоты).
2. Доработка (на доработку задание направляется 1 раз): основные задачи выполнены, при этом есть ошибки в манифестах или отсутствуют проверочные скриншоты.
3. Незачёт: работа выполнена не в полном объёме, есть ошибки в манифестах, отсутствуют проверочные скриншоты. Все попытки доработки израсходованы (на доработку работа направляется 1 раз). Этот вид оценки используется крайне редко.

## **Срок выполнения задания**  
1. 5 дней на выполнение задания.
2. 5 дней на доработку задания (в случае направления задания на доработку).
