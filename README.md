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
![Screenshot from 2022-05-28 13-32-10](https://user-images.githubusercontent.com/59205554/170821618-ef4fe2fc-5d82-4b98-b178-9f1b42de1a58.png)

