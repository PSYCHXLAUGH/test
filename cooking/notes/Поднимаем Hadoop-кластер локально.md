#docker #hadoop #HDFS #git #linux 

https://www.youtube.com/watch?v=ny2w5zImqvA
#### Клонируем

```
git clone https://github.com/big-data-europe/docker-hadoop
```
####  Редактируем конфигурацию для развертывания

Здесь предлагается сразу развернуть несколько сервисов как связанных с HDFS так и с YARN. Развертывать будем именно HDFS
namenode - ключевые компоненты системы

```yaml
version: "3"

services:
  namenode:
    image: bde2020/hadoop-namenode:2.0.0-hadoop3.2.1-java8
    container_name: namenode (имя контейнера)
    restart: always
    ports: (проброс портов во вне чтобы мы могли зайти)
      - 9870:9870
      - 9000:9000
    volumes:
      - hadoop_namenode:/hadoop/dfs/name
    environment:
      - CLUSTER_NAME=test имя кластера
    env_file:
      - ./hadoop.env

  datanode:
    image: bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8
    container_name: datanode
    restart: always
    volumes:
      - hadoop_datanode:/hadoop/dfs/data
    environment:
      SERVICE_PRECONDITION: "namenode:9870"
    env_file:
      - ./hadoop.env
  
  resourcemanager: (демон, который управляет ресурсами)
    image: bde2020/hadoop-resourcemanager:2.0.0-hadoop3.2.1-java8
    container_name: resourcemanager
    restart: always
    environment:
      SERVICE_PRECONDITION: "namenode:9000 namenode:9870 datanode:9864"
    env_file:
      - ./hadoop.env

  nodemanager1: (то же что и ставится на физические тачки вместе с datanode. Нужен только там где есть вычисления над данными)
    image: bde2020/hadoop-nodemanager:2.0.0-hadoop3.2.1-java8
    container_name: nodemanager
    restart: always
    environment:
      SERVICE_PRECONDITION: "namenode:9000 namenode:9870 datanode:9864 resourcemanager:8088"
    env_file:
      - ./hadoop.env
  
  historyserver: (для mapreduce)
    image: bde2020/hadoop-historyserver:2.0.0-hadoop3.2.1-java8
    container_name: historyserver
    restart: always
    environment:
      SERVICE_PRECONDITION: "namenode:9000 namenode:9870 datanode:9864 resourcemanager:8088"
    volumes:
      - hadoop_historyserver:/hadoop/yarn/timeline
    env_file:
      - ./hadoop.env
  
volumes:
  hadoop_namenode:
  hadoop_datanode:
  hadoop_historyserver:
```
####  Убираем лишние
- historyserver
- nodemanager1
- resourcemanager

```
version: "3"

services:
  namenode:
    image: bde2020/hadoop-namenode:2.0.0-hadoop3.2.1-java8
    container_name: namenode
    restart: always
    ports:
      - 9870:9870
      - 9000:9000
    volumes:
      - hadoop_namenode:/hadoop/dfs/name
    environment:
      - CLUSTER_NAME=test
    env_file:
      - ./hadoop.env

  datanode1:
    image: bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8
    container_name: datanode1
    restart: always
    volumes:
      - hadoop_datanode1:/hadoop/dfs/data
    environment:
      SERVICE_PRECONDITION: "namenode:9870"
    env_file:
      - ./hadoop.env

  datanode2:
    image: bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8
    container_name: datanode2
    restart: always
    volumes:
      - hadoop_datanode2:/hadoop/dfs/data
    environment:
      SERVICE_PRECONDITION: "namenode:9870"
    env_file:
      - ./hadoop.env

  datanode3:
    image: bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8
    container_name: datanode3
    restart: always
    volumes:
      - hadoop_datanode3:/hadoop/dfs/data
    environment:
      SERVICE_PRECONDITION: "namenode:9870"
    env_file:
      - ./hadoop.env

volumes:
  hadoop_namenode:
  hadoop_datanode1:
  hadoop_datanode2:
  hadoop_datanode3:
```

#### Запуск

```
sudo docker-compose up
```

Создается сеть где будет работать наш кластер + namenode запускаются
Теперь мы можем зайти на веб версию

```
http://127.0.0.1:9870
```

####  Проверим запущенные контейнеры

```
docker ps
```

```
CONTAINER ID   IMAGE                                             COMMAND                  CREATED          STATUS                    PORTS                                                                                  NAMES
f2a76c7f08eb   bde2020/hadoop-namenode:2.0.0-hadoop3.2.1-java8   "/entrypoint.sh /run…"   45 minutes ago   Up 21 minutes (healthy)   0.0.0.0:9000->9000/tcp, :::9000->9000/tcp, 0.0.0.0:9870->9870/tcp, :::9870->9870/tcp   namenode
2b80df595d83   bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8   "/entrypoint.sh /run…"   45 minutes ago   Up 21 minutes (healthy)   9864/tcp                                                                               datanode3
9a8fa3d21915   bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8   "/entrypoint.sh /run…"   45 minutes ago   Up 21 minutes (healthy)   9864/tcp                                                                               datanode1
0cf5c5d55b20   bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8   "/entrypoint.sh /run…"   45 minutes ago   Up 21 minutes (healthy)   9864/tcp                                                                               datanode2

```

Во вне смотрит только один контейнер это можно понять по 45 minutes ago   Up 21 minutes (healthy)   0.0.0.0:9000->9000/tcp,

Именно через эту неймноду мы будем писать в hdfs

# Запишем файл в hdfs

Перемещаем файл в контейнер

```
sudo docker cp <path> <container>:/
```


В данном случае файл будет добавлен в root директорию
Посмотреть добавился ли файл можно в Browse Directory
Подключение к контейнеру

```
sudo docker exec -it <container> /bin/bash
```

#### Кладем в hdfs

```
hdfs dfs -put <file> <path>
```

####  Читаем файлы из hdfs

```
hdfs dfs -cat <file>
```