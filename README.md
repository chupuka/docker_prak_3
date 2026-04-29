# Docker X11 Applications

Практическая работа по развертыванию оконных приложений в Docker с использованием X11.

## Требования

- Docker Desktop
- Xming (или другой X-сервер для Windows)
- X11 приложения

## Структура

```
dockerkaradam/
├── calc/           # Java калькулятор
│   ├── Dockerfile
│   └── calc.jar
├── xlogo/         # Логотип X
│   └── Dockerfile
└── xeyes/        # Глазеющие глаза
    └── Dockerfile
```

## Сборка и запуск

### 1. Настройка Xming

Xming должен быть запущен на дисплее :0

### 2. Разрешить подключения

```powershell
xhost +host.docker.internal
```

### 3. Сборка образов

```powershell
# Калькулятор
docker build -t dockerkaradam-calc ./calc

# xlogo
docker build -t dockerkaradam-xlogo ./xlogo

# xeyes
docker build -t dockerkaradam-xeyes ./xeyes
```

### 4. Запуск контейнеров

```powershell
# Калькулятор
docker run -it --rm -e DISPLAY=host.docker.internal:0 dockerkaradam-calc

# xlogo
docker run -it --rm -e DISPLAY=host.docker.internal:0 dockerkaradam-xlogo

# xeyes
docker run -it --rm -e DISPLAY=host.docker.internal:0 dockerkaradam-xeyes
```

## Описание Dockerfile

### Калькулятор (calc/Dockerfile)

| Компонент | Назначение |
|----------|-----------|
| ubuntu:latest | Базовый образ |
| default-jre | Среда выполнения Java |
| x11-apps | X11 приложения |
| xauth | Аутентификация X сессии |
| COPY calc.jar | Копирование приложения |
| ENV DISPLAY | Переменная окружения X сервера |
| CMD java -jar | Запуск приложения |

### xlogo (xlogo/Dockerfile)

| Компонент | Назначение |
|----------|-----------|
| ubuntu:latest | Базовый образ |
| x11-apps | Содержит xlogo |
| xauth | Аутентификация X сессии |
| ENV DISPLAY | Переменная окружения X сервера |
| CMD xlogo | Запуск приложения |

### xeyes (xeyes/Dockerfile)

| Компонент | Назначение |
|----------|-----------|
| ubuntu:latest | Базовый образ |
| x11-apps | Содержит xeyes |
| xauth | Аутентификация X сессии |
| ENV DISPLAY | Переменная окружения X сервера |
| CMD xeyes | Запуск приложения |

## Notes

- `host.docker.internal` работает на Windows и Mac с Docker Desktop
- На Linux используется `/tmp/.X11-unix` и `-v` для проброса сокета
- `xhost +host.docker.internal` разрешает подключения от Docker