---
Ansible-проект для установки и конфигурирования ELK-стека
---

Так запускаем плейбук:
```
ansible-playbook -i inventory/prod.yml --ask-pass --ask-become-pass site.yml
```

После прокатки плейбука так стартуем сервисы и проверяем:

```
#на хосте с эластиком и кибаной
elasticsearch -d -p pid && kibana
```

```
#на хосте с логстешем
logstash -f /opt/logstash/7.10.1/config/logstash.conf
```

Когда всё запущено:
```
#тут можно посмотреть, жив ли эластик
http://IP-ADRESS-ELASTIC-KIBANA-HOST:9200/

#тут можно посмотреть, работает ли кибана
http://IP-ADRESS-ELASTIC-KIBANA-HOST:5601/
```