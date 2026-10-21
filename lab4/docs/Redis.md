# Redis в n8n

В этой части n8n подключён к Redis 7, который запущен в Docker. Создан ключ, прочитано его значение, затем ключ удалён и проверено, что его больше нет.

## 1. Запуск Redis

```bash
docker run -d --name redis -p 6379:6379 redis:7
```

| Параметр | Значение |
| --- | --- |
| Образ | `redis:7` |
| Имя контейнера | `redis` |
| Порт | `6379:6379` |

## 2. Credentials в n8n

В n8n созданы credentials типа **Redis**:

| Параметр | Значение |
| --- | --- |
| Host | `host.docker.internal` |
| Port | `6379` |

Хост `host.docker.internal` нужен, потому что `localhost` внутри контейнера n8n указывает на сам контейнер, а Redis опубликован на порту хоста.

## 3. Workflow

Ключ: `kazak`, значение: `Lab4 Redis test value`.

### 3.1. Создание и чтение ключа

Файл: [flow-09-redis.json](../flows/flow-09-redis.json)

`Manual Trigger` → `Redis (Set)` → `Redis (Get)`

1. `Set` записывает ключ `kazak` со значением `Lab4 Redis test value`.
2. `Get` читает ключ `kazak` и возвращает сохранённую строку.

![Создание и чтение ключа](../screenshots/08-redis-set-get.png)

### 3.2. Удаление и проверка отсутствия ключа

Файл: [flow-10-redis.json](../flows/flow-10-redis.json)

`Manual Trigger` → `Redis (Delete)` → `Redis (Exists)`

1. `Delete` удаляет ключ `kazak`.
2. `Exists` проверяет наличие ключа и возвращает `false`, то есть ключа в Redis больше нет.

![Удаление и проверка отсутствия ключа](../screenshots/09-redis-delete-exists.png)