# Robotek Chatbot

**Robotek Chatbot** — это чат-бот для WhatsApp, созданный с использованием библиотеки `whatsapp-chatbot-python`. Он реализует выбор языка (русский/казахский), отвечает на вопросы, учитывает семестры и базу знаний, а также распознаёт голосовые сообщения через Whisper от OpenAI.

---

## Содержание

- [Подготовка окружения](#подготовка-окружения)
- [Настройка переменных и ключей](#настройка-переменных-и-ключей)
- [Запуск бота](#запуск-бота)
- [Файлы проекта](#файлы-проекта)
- [Изменение модели](#изменение-модели)
- [Развёртывание бота на VPS](#развёртывание-бота-на-vps)
- [Автозапуск и постоянная работа](#автозапуск-и-постоянная-работа)

---

## Подготовка окружения

1. Клонируйте репозиторий (или скачайте архив с файлами проекта):

   ```bash
   git clone https://github.com/Alpi157/robotek_chatbot.git
   ```

2. Перейдите в папку проекта:

   ```bash
   cd robotek_chatbot
   ```

3. Создайте виртуальное окружение (опционально, но рекомендуется):

   ```bash
   python -m venv venv
   ```

   **Активация виртуального окружения:**

   - macOS/Linux:
     ```bash
     source venv/bin/activate
     ```
   - Windows:
     ```bash
     .\venv\Scripts\activate
     ```

4. Установите зависимости:

   ```bash
   pip install -r requirements.txt
   ```

---

## Настройка переменных и ключей

В файле `main.py` необходимо задать API-ключ:

```python
openai.api_key = "ваш_ключ"
```

Замените строку на актуальный ключ OpenAI (`sk-...`).

---

## Запуск бота

После установки зависимостей и настройки ключей выполните:

```bash
python main.py
```

Если всё работает корректно, вы увидите в консоли:

```
[DEBUG] Запуск бота...
...:whatsapp-chatbot-python:INFO:Started receiving incoming notifications.
```

---

## Файлы проекта

- `main.py` — основной скрипт с логикой:
  - Интеграция с GPT
  - Подключение к Green API
  - Обработка входящих и исходящих сообщений
  - Языковая логика, приветствия, маршрутизация сообщений

- `requirements.txt` — список зависимостей Python

- `known_clients.json` — список клиентов, которым уже отправлено приветствие. Создаётся автоматически.

- `README.md` — текущая документация

---

## Изменение модели

По умолчанию в коде используется:

```python
MODEL = "gpt-4o-mini"
```

При необходимости можно заменить, например:

```python
MODEL = "gpt-4o"
```

---

## Развёртывание бота на VPS

Пример для Ubuntu, применимо к другим дистрибутивам.

### Подготовка

Убедитесь, что установлен Python 3.7+:

```bash
python3 --version
```

Если Python не установлен:

```bash
sudo apt-get update
sudo apt-get install python3 python3-pip
```

Установите Git при необходимости:

```bash
sudo apt-get install git
```

### Скопируйте проект

- **Вариант A**: Клонировать с GitHub

  ```bash
  git clone https://github.com/Alpi157/robotek_chatbot.git
  cd robotek_chatbot
  ```

- **Вариант B**: Скопируйте файлы через SCP, WinSCP и т.д.

### Создание и активация виртуального окружения

```bash
python3 -m venv venv
source venv/bin/activate
```

### Установка зависимостей

```bash
pip install -r requirements.txt
```

### Настройка ключа

Задайте OpenAI API-ключ в `main.py`.

### Запуск

```bash
python main.py
```

В консоли появится:

```
[DEBUG] Запуск бота...
...:whatsapp-chatbot-python:INFO:Started receiving incoming notifications.
```

---

## Автозапуск и постоянная работа

Для автозапуска можно использовать `systemd`.

### Пример systemd-сервиса

Создайте файл `/etc/systemd/system/robotekbot.service`:

```ini
[Unit]
Description=Robotek Chatbot
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/путь/до/yourrepo
ExecStart=/путь/до/yourrepo/venv/bin/python /путь/до/yourrepo/main.py
Restart=always

[Install]
WantedBy=multi-user.target
```

**Замените** `/путь/до/yourrepo` на путь к вашему проекту и имя пользователя.

### Активация сервиса

```bash
sudo systemctl daemon-reload
sudo systemctl enable robotekbot
sudo systemctl start robotekbot
```

Проверка статуса:

```bash
systemctl status robotekbot
```

Если всё настроено корректно, бот будет работать в фоне и автоматически перезапускаться при сбоях или перезагрузке сервера.
