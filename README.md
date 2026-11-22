# ДЗ на 6 баллов

Приложение со счётчиком упаковано в Docker. Исходники не изменены.
Для запуска необходим Docker с Compose. Команды выполняются из корня проекта.

## Запуск без Compose

```powershell
docker build -t my-app .
docker run -d --name box3 -p 127.0.0.1:8080:8000 -e GREETING="Hello world!" my-app
```

Приложение: http://localhost:8080.

Проверка счётчика и просмотр логов:

```powershell
curl.exe http://localhost:8080/
curl.exe -X POST http://localhost:8080/increment
docker logs box3
```

Логи проверки сохранены в [logs.txt](logs.txt).

Перед запуском Compose нужно освободить порт:

```powershell
docker stop box3
```

## Запуск через Compose

Создать файл настроек и запустить приложение:

```powershell
Copy-Item .env.example .env
docker compose up -d --build
```

Настройки в `.env`:

- `HOST_PORT` — внешний порт приложения, по умолчанию `8080`.
- `GREETING` — текст приветствия.

После изменения настроек выполнить `docker compose up -d`.

## Проверка сохранения данных

SQLite хранит счётчик в `/data/counter.db`.
Именованный volume `counter-data` подключён к `/data`.

Счётчик увеличен несколькими POST-запросами. Затем выполнены:

```powershell
docker compose down
docker compose up -d
```

После пересоздания контейнера значение счётчика сохранилось.
Следующий POST-запрос увеличил его на один.

Результаты: [до пересоздания](before.json) и
[после пересоздания](after.json).
