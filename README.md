# Как развернуть бота на python в Docker на сервере 


Я расскажу как развернуть бота на питоне на удаленном сервере в Docker 

Будем считать, что уже купили сервер. Обычно хватает самой базовой конфигурации. 


## Установка софта 

Подключились по ssh и ставим софт 

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y htop tmux python3-pip python-is-python3 neofetch python3-venv git vim curl wget net-tools docker.io docker-compose
sudo apt update
sudo apt install -y language-pack-ru
sudo update-locale LANG=ru_RU.UTF-8
```

## Настройка Docker в проекте


В корне нашего приложения создаем файл с названием Dockerfile 

С таким содержимым 

```Dockerfile
FROM python:3.10
WORKDIR /app

# Устанавливаем зависимости
COPY requirements.txt /app/requirements.txt

RUN pip install -r requirements.txt

# Копируем исходный код в образ
COPY . /app

# Указываем команду, которая будет запускать бота (скорее всего надо заменить имя файла)
CMD ["python", "bot.py"]
```

Также создадим файл `docker-compose.yml`

```yaml
version: '3'
services:
  app:
    build: .
    restart: always
```

    Так приложение не будет сохранять прогресс при перезапуске (руками)

Если нужно добавить сохранение файлов, тогда изменим содержимое файла `docker-compose.yml`

```yaml
version: '3'
services:
  app:
    build: .
    restart: always
    volumes:
      - ./docker-app:/app
```



## Загрузка кода на сервер 

1. Код можно загрузить на Git, а после скачать его через git clone 
2. Скопировать файлы на сервер. Можно воспользоваться например filezilla (https://filezilla.ru/). Или аналогами. Я загружаю файлы используя команду scp. 


## Запуск кода на сервере
Нужно перейти в нужную директорию на сервере.
```bash
cd ПУТЬ/ДО/ДИРЕКТОРИИ
```

Нам нужно окозаться в одной папке с `docker-compose.yml`


После чего запускам команду 
```bash
docker-compose up --build -d 
```


## Остановка и перезапуск бота

Остановить бота 

```bash
docker-compose down
``` 

Перезапустить / обратно включить / пересообрать 

```bash
docker-compose up --build -d 
```
