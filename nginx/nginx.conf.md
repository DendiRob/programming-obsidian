
##  Типы описания директив в конфиге

1. Простая

```
access_log /var/log/nginx/access.log main; 
```

1. Блочная 

```
events {
	worker_connections 1024;
}
```

>[!info]
Если у блочной директивы внутри фигурных скобок можно задать другие директивы, то это называется контекстом



## Директивы

### main 
Это файл nginx.conf. Его никак не называют

### http 
Отвечает за обработку запроса по протоколу http
