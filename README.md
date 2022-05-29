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
(не обязательно если установленный docker)
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

## Тепер ми заходимо в графану 
Відкриваємо google на своєму компютері або на сервері

### Водимо адрес

* http://<IP_address>:3000

Через командну строку 
```
Your_ip_address=
```
```
google-chrome $Your_ip_address:3000

```

Водимо login -> admin i password -> admin і після цього водимо новий пароль
Ось що ви побачите

Слідуєм послідовності налаштування -> add data source -> Prometheus

У строці URL водимо http://prometheus:9090
f
Зберігаємо
f

Далі завантажуємо json файл за цією адресою

https://grafana.com/api/dashboards/1860/revisions/27/download

якщо через командну стромку

wget https://grafana.com/api/dashboards/1860/revisions/27/download

Заходимо знову в grafana клацаєм dashbord -> manage -> import

Вставляємо в Import via panel json весь скопійований файл

Натискаємо Load

Вибираємо в Prometheus prometheus і натискаємо імпорт



