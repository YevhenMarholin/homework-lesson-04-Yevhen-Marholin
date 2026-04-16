# homework-lesson-04-Yevhen-Marholin

## Docker Compose для course-app

Було створено `docker-compose.yml` для застосунку `apps/course-app`.

## Dockerfile

Було створено файл `Dockerfile` для застосунку `apps/course-app`, який використовується для збірки образу та запуску контейнера.

```dockerfile
FROM python:3.12-alpine

WORKDIR /src

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8080

CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

## Compose файл

```yaml
services:
  app:
    build: .
    container_name: course-app
    ports:
      - "8080:8080"
    environment:
      APP_STORE: redis
      APP_REDIS_URL: redis://redis:6379/0
    depends_on:
      - redis

  redis:
    image: redis:7-alpine
    container_name: course-app-redis
```

## Запуск

```bash
docker compose up --build
```

або

```bash
docker compose up -d --build
```

## Перевірка

Відкрити в браузері:

- http://localhost:8080
- http://localhost:8080/healthz

```bash
curl http://localhost:8080
curl http://localhost:8080/healthz
```

## Змінні середовища

```env
APP_STORE=redis
APP_REDIS_URL=redis://redis:6379/0
```

## Додатковий сервіс

У Compose файл додано другий сервіс `redis` як зовнішнє сховище для застосунку.

## Перевірка роботи

```bash
docker compose ps
docker compose logs
```
```
 /workspaces/homework-lesson-04-Yevhen-Marholin/course-app (main) $ docker compose up --build
[+] Building 0.9s (13/13) FINISHED                                                                                    
 => [internal] load local bake definitions                                                                       0.0s
 => => reading from stdin 561B                                                                                   0.0s
 => [internal] load build definition from Dockerfile                                                             0.0s
 => => transferring dockerfile: 247B                                                                             0.0s
 => [internal] load metadata for docker.io/library/python:3.12-alpine                                            0.5s
 => [auth] library/python:pull token for registry-1.docker.io                                                    0.0s
 => [internal] load .dockerignore                                                                                0.0s
 => => transferring context: 2B                                                                                  0.0s
 => [1/5] FROM docker.io/library/python:3.12-alpine@sha256:54a808cea192d0875e5a384b1ad57316bbd0b410a4d95ef1f040  0.0s
 => => resolve docker.io/library/python:3.12-alpine@sha256:54a808cea192d0875e5a384b1ad57316bbd0b410a4d95ef1f040  0.0s
 => [internal] load build context                                                                                0.0s
 => => transferring context: 462B                                                                                0.0s
 => CACHED [2/5] WORKDIR /src                                                                                    0.0s
 => CACHED [3/5] COPY requirements.txt .                                                                         0.0s
 => CACHED [4/5] RUN pip install --no-cache-dir -r requirements.txt                                              0.0s
 => [5/5] COPY . .                                                                                               0.0s
 => exporting to image                                                                                           0.1s
 => => exporting layers                                                                                          0.0s
 => => exporting manifest sha256:51021f9db53cd55803db65a669786bde3a1391b2288543613cc22e51934b73fb                0.0s
 => => exporting config sha256:439429709b090fe326529425aa91828c730dc57b4ae80729fdaf04e3bcbb45a0                  0.0s
 => => exporting attestation manifest sha256:61732fb77df71e149290ad8f7de80b70746e71750bcd7e34f999f2ab7dbaf429    0.0s
 => => exporting manifest list sha256:919bbb56a0447df8bebc80b579c0e3fd670f35bead0f736bcc6c2ba6ffc7a9e8           0.0s
 => => naming to docker.io/library/course-app-app:latest                                                         0.0s
 => => unpacking to docker.io/library/course-app-app:latest                                                      0.0s
 => resolving provenance for metadata file                                                                       0.0s
[+] Running 2/2
 ✔ course-app-app        Built                                                                                   0.0s 
 ✔ Container course-app  Recreated                                                                               0.1s 
Attaching to course-app, course-app-redis
course-app-redis  | 1:C 16 Apr 2026 15:47:06.661 # WARNING Memory overcommit must be enabled! Without it, a background save or replication may fail under low memory condition. Being disabled, it can also cause failures without low memory condition, see https://github.com/jemalloc/jemalloc/issues/1328. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
course-app-redis  | 1:C 16 Apr 2026 15:47:06.661 * oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
course-app-redis  | 1:C 16 Apr 2026 15:47:06.661 * Redis version=7.4.8, bits=64, commit=00000000, modified=0, pid=1, just started
course-app-redis  | 1:C 16 Apr 2026 15:47:06.661 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
course-app-redis  | 1:M 16 Apr 2026 15:47:06.661 * Increased maximum number of open files to 10032 (it was originally set to 1024).
course-app-redis  | 1:M 16 Apr 2026 15:47:06.661 * monotonic clock: POSIX clock_gettime
course-app-redis  | 1:M 16 Apr 2026 15:47:06.662 * Running mode=standalone, port=6379.
course-app-redis  | 1:M 16 Apr 2026 15:47:06.663 * Server initialized
course-app-redis  | 1:M 16 Apr 2026 15:47:06.663 * Loading RDB produced by version 7.4.8
course-app-redis  | 1:M 16 Apr 2026 15:47:06.663 * RDB age 8 seconds
course-app-redis  | 1:M 16 Apr 2026 15:47:06.663 * RDB memory usage when created 0.90 Mb
course-app-redis  | 1:M 16 Apr 2026 15:47:06.663 * Done loading RDB, keys loaded: 0, keys expired: 0.
course-app-redis  | 1:M 16 Apr 2026 15:47:06.664 * DB loaded from disk: 0.000 seconds
course-app-redis  | 1:M 16 Apr 2026 15:47:06.664 * Ready to accept connections tcp
course-app        | INFO:     Started server process [1]
course-app        | INFO:     Waiting for application startup.
course-app        | INFO:     Application startup complete.
course-app        | INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
course-app        | INFO:     172.18.0.1:56198 - "GET / HTTP/1.1" 200 OK
course-app        | INFO:     172.18.0.1:56210 - "GET /api/messages?limit=50 HTTP/1.1" 200 OK
course-app        | INFO:     172.18.0.1:56214 - "GET /favicon.ico HTTP/1.1" 404 Not Found
course-app        | INFO:     172.18.0.1:41724 - "GET /healthz HTTP/1.1" 200 OK
course-app        | INFO:     172.18.0.1:41726 - "GET /readyz HTTP/1.1" 200 OK
course-app        | INFO:     172.18.0.1:41740 - "GET /healthz HTTP/1.1" 200 OK
course-app        | INFO:     172.18.0.1:54218 - "GET /stress?seconds=10&background=true HTTP/1.1" 200 OK


w Enable Watch
```
## Висновок

- описано Compose файл для `apps/course-app`
- перевірено доступність застосунку
- перевірено `/healthz`
- додано сервіс `redis`
- додано змінні середовища
- Compose працює без помилок
![alt text](image.png)