![grafana-6001](https://user-images.githubusercontent.com/59205554/170952943-d11c303c-17ed-4394-baf9-38e7af6508a0.jpeg)

# Grafana

Grafana - це платформа для візуалізації моніторингу та аналізу даних

Для встновлення Grafana і робочої схеми ми повині також встановити такі прогарами як Prometheus і Node-exporte

## Заходимо в root користувача
```
sudo su

```

## Спочатку оновлюємо пакети
```
sudo apt update && sudo apt upgrade -y

```
![Image text](https://github.com/cybernekit/RouterSetupGuide/blob/main/img/Screenshot%20from%202022-05-17%2016-49-11.png)
## Устанавливаем docker
(не обов'язково якщо встановлений docker)
```
sudo apt-get install ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y

```
## Завантажуємо docker-compose.yml
```
cd ~
git clone https://github.com/cybernekit/Grafana-Prometheus.git
cd ~/Grafana-Prometheus

```
![Screenshot from 2022-05-28 13-24-48](https://user-images.githubusercontent.com/59205554/170821373-17d41ca2-0a57-4721-a64d-1dce8ee9f8a3.png)

## Створюємо контейнер
```
docker swarm init
docker stack deploy -c ~/Grafana-Prometheus/docker-compose.yml monitoring

```
![Screenshot from 2022-05-28 13-27-26](https://user-images.githubusercontent.com/59205554/170821426-25288648-174f-4687-a245-08a4746925a9.png)

![Screenshot from 2022-05-28 13-25-13](https://user-images.githubusercontent.com/59205554/170821366-794d7c42-8f30-43fb-8281-30aa0b98c5b5.png)

## Провіряємо чи запустилися контейнери
```
docker ps

```

![Screenshot from 2022-05-28 13-35-21](https://user-images.githubusercontent.com/59205554/170821748-022e38d8-d824-465a-8979-334cff2ca31f.png)

# Налаштування конфігурації prometheus
Переходимо до конфігурацюї promtheus. Файл називадться `promtheus.yml` і запінюємо його на файл з правильною конфігурацюєю.
```
cd /var/lib/docker/volumes/monitoring_prom-configs/_data
rm prometheus.yml
wget https://github.com/cybernekit/-Guide-for-monitoring-blockPi-node/blob/main/prometheus.yml

```

### Перезапускаємо prometheus
Для того щоб перезапустити прометеус нам потрібно дізнатися id контейнера
```
sudo docker ps

```
![Знімок екрану з 2022-07-17 21-07-32](https://user-images.githubusercontent.com/102728347/179419613-5d30a29e-0927-4d8d-a89c-fac291d63473.png)

```
sudo docker restart <CONTAINER ID>
```
Все ви підняли prometheus. 
Тепер щоб провірити чи він в справному стані, ми водимо в браузері.
```
http://<your_address_grafana>:9090
```
(Error -> Якщо виводить на екран що сторінку не знайдено зачекайте трішки і спробуйте знову увести свій адрес з портом)

Натискамо Status -> Targets

![Screenshot from 2022-07-17 21-41-11](https://user-images.githubusercontent.com/59205554/179420255-daccece1-7331-46db-8c78-23782db8740b.png)

Якщо у вас такий вигляд то все підключилося і правильно працює 

![Screenshot from 2022-07-17 21-35-04](https://user-images.githubusercontent.com/59205554/179420112-7eab3fb3-e3fb-4bc7-9afb-b3537a66414f.png)


## Тепер ми заходимо в графану 
Відкриваємо google на своєму компютері або на сервері. 

Водимо адрес через браузер:
```
http://<IP_address>:3000
```
Відкриваємо адрес через командну строку:
```
Your_ip_address=
```
```
google-chrome $Your_ip_address:3000

```

### Водимо login -> admin i password -> admin і після цього водимо новий пароль
Після цього ви будете в графані

![Знімок екрану з 2022-05-28 20-21-33](https://user-images.githubusercontent.com/59205554/170854500-9ef1d80c-6d52-47e8-a5e2-ceda1089cff3.png)



### Слідуєм послідовності налаштування -> add data source -> Prometheus
![Знімок екрану з 2022-05-28 20-27-50](https://user-images.githubusercontent.com/59205554/170854513-8c596b84-5bd8-4721-bac5-e9b4f654b6d6.png)

* У строці URL водимо http://prometheus:9090
* Зберігаємо

![Знімок екрану з 2022-05-28 20-27-58](https://user-images.githubusercontent.com/59205554/170854532-39963456-30ee-4dd6-9912-725ae2b425f7.png)


## Далі завантажуємо json файл за цією адресою
Через браузері:
https://grafana.com/api/dashboards/1860/revisions/27/download

Через командну строку:
```
wget https://grafana.com/api/dashboards/1860/revisions/27/download
```
### Заходимо знову в grafana і слідуєм вже такій послідовності dashbord -> manage -> import

* Вставляємо в ```Import via panel json``` весь скопійований файл
* Натискаємо Load
* Вибираємо у виділинці Prometheus  prometheus і натискаємо import

![Знімок екрану з 2022-05-29 09-10-19](https://user-images.githubusercontent.com/59205554/170854796-f8b8424f-ef28-4df9-8f65-33a229d19e69.png)


