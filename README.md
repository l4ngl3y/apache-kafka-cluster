
# Ansible Kafka cluster

Ansible роль для развертывания кластера Kafka.


## TL;DR

### Подготовка inventory
Первоначально нужно создать в папке ```inventory``` новую среду для развертывания, за основу можно взять ```dev-cluster```.
В этой папке находится описание хостов и переменных относящихся к новой среды развертывания.  

### Подготовка нод кластера
Роль подготавливает хосты для установки компонентов кластеров Apache Kafka и Apache Zookeper.
Предварительно перед запуском этой роли необходимо произвести настройки: 
* DNS записей на NS серверах инфраструктуры
* Подготовить блочное устройство для хранения данных Apache Kafka - произвести разбивку на разделы и отформатировать эти разделы.
```
ansible-playbook -i inventory/dev-cluster/hosts.yaml playbook.yaml --tags 'preinstall'
```
### Установка kafka кластера
```
ansible-playbook -i inventory/dev-cluster/hosts.yaml playbook.yaml --tags 'install-kafka'
```
### Настройка kafka кластера
```
ansible-playbook -i inventory/dev-cluster/hosts.yaml playbook.yaml --tags 'configure-kafka'
```
### Установка zookeeper кластера
```
ansible-playbook -i inventory/dev-cluster/hosts.yaml playbook.yaml --tags 'install-zookeeper'
```
### Настройка zookeeper кластера
```
ansible-playbook -i inventory/dev-cluster/hosts.yaml playbook.yaml --tags 'configure-zookeeper'
```
### Установка docker engine
Docker engine - требуется для запуска вспомогательного ПО Apache Kafka - вчастности UI приложения.
```
ansible-playbook -i inventory/dev-cluster/hosts.yaml playbook.yaml --tags 'install-docker' --limit 'kafka-01-dev'
```

## Environment Variables
Описание переменные находящихся в файле [all.yaml](inventory/dev-cluster/group_vars/all.yaml)

| Name                         | Value                                                  |
|------------------------------|--------------------------------------------------------|
| `system_packages`            | `acl, mc, htop, atop, iotop, rsync ,curl, wget, unzip` |
| `kafka_version`              | `3.3.1` версия дистрибутива                            |
| `zookeeper_version`          | `3.8.0` версия дистрибутива                            |
| `http_proxy`                 | `""`                                                   |
| `https_proxy`                | `""`                                                   |
| `default_replication_factor` | `2` фактор репликации, для новых топиков               |
| `num_partitions`             | `16` колличество партиция для новых топиков            |
| `log_retention_hours`        | `48` интервал хранения данных                          |
| `log_dirs`                   | `/var/lib/kafka` путь к хранению данных                |
| `ansible_ssh_extra_args`     | `-o StrictHostKeyChecking=no`                          | 
| `ansible_user`               | `""`                                                   |
| `ansible_ssh_private_key_file`| `""`                                                   |  

## Tech Stack

**Client:** Ansible

**Server:** Kafka, Zookeeper
