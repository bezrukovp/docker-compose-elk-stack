# elk-stack

##Коробка содержит:

- Elk
- Curator
- ElastAlert

Контейнер elk расширяет образ [sebp/elk](https://hub.docker.com/r/sebp/elk/) на github это проект [elk-docker](https://github.com/spujadas/elk-docker). Данный образ хорошо [документирован](https://elk-docker.readthedocs.io).

###Logstash содержит фильтры для:

- syslog
- nginx: access log
- apache2: access log
- postfix
- mysql: query, error, slow log
- percona: slow log
- mariadb: error, slow log

###Установка:
После клонирования репозитория в корне проекта нужно переименовать файл `.env.example` в файл `.env` и если требуется сгенерить ключи, подкрутить параметры окружения.
> Чтобы новые ключи не попали под vcs предусмотрена папка `local` её можно положить в любой части проекта и её содержимое не попадёт под контроль.

После этого можно запускать контейнеры `docker-compose up -d`.  На 5601 порту будет висеть Kibana, а на 9201 ElasticHQ.

#####Далее переходим к настройке источника логов

На сервере устанавливаем и настраиваем filebeat, как настраивать подробно описано [тут](https://elk-docker.readthedocs.io/#forwarding-logs-filebeat).

Поддерживаемые `document_type` можно посмотреть в папке `./elk/filters`

Logstash требует авторизации по tls поэтому требуется загрузить ключ `logstash-beats.crt` из папки заданной параметром `LOGSTASH_KEYS_DIR` на сервер и в конфигурации output в logstash в опции tls указать `certificate_authorities` путь к данному файлу с ключём.

Заливаем в папку конфигурации filebeat шаблон индекса по образцу: `./.filebeat/fb-app.template.json` и выполняем его загрузку в elk командой:
```
curl -XPUT 'http://docker:9201/_template/fb-app?pretty' -d@/etc/filebeat/fb-app.template.json
```

Запускаем filebeat `service filebeat start`