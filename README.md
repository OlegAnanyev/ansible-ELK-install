# Ansible-проект для установки и конфигурирования ELK-стека

## Задача
Задача в том, чтобы создать плейбук, который:
* установит Java, Elasticsearch, Kibana, Logstash на нужные хосты
* добавит переменные окружения для всех продуктов
* сконфигурирует Elasticsearch так, чтобы к нему можно было обращаться по ip-адресу с внешних хостов
* сконфигурирует Kibana так, чтобы она также была доступна извне
* создаст простой конфиг Logstash для отправки логов /tmp/access_log в Elasticsearch, установленный на другой хост

**Важное замечание: в данном проекте многие вещи намеренно сделаны не самым оптимальным для продакшена образом с целью более полного использования возможностей Ansible и наиболее эффективного изучения данного инструмента.**

## Перед запуском плейбука
В директории files/ должны уже находиться архивы с бинарниками, устанавливаемых продуктов.
Можно было бы скачивать их из интернета прямо в процессе выполнения плейбука, но это медленно. Если есть такая необходимость, то можно воспользоваться модулем `get_url`.

```bash
elasticsearch-7.10.1-linux-x86_64.tar.gz
jdk-15.0.1_linux-x64_bin.tar.gz
kibana-7.10.1-linux-x86_64.tar.gz
logstash-7.10.1-linux-x86_64.tar.gz
```

## Запуск плейбука для установки и конфигурирования Java, Elasticsearch, Kibana, Logstash

```bash
ansible-playbook -i inventory/prod.yml --ask-pass --ask-become-pass site.yml
```

## Запуск установленных сервисов

```bash
#на хосте с эластиком и кибаной
elasticsearch -d -p pid && kibana
```

```bash
#на хосте с логстешем
logstash -f /opt/logstash/7.10.1/config/logstash.conf
```

## Проверка работоспособности
* тут можно посмотреть, жив ли эластик: http://IP-ADRESS-ELASTIC-KIBANA-HOST:9200/
* тут можно посмотреть, работает ли кибана: http://IP-ADRESS-ELASTIC-KIBANA-HOST:5601/
