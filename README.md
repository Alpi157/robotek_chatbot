**Robotek Chatbot**

Это чат-бот для WhatsApp с использованием библиотеки whatsapp-chatbot-python. Бот реализует логику выбора языка (русский/казахский), отвечает на вопросы об образовательных курсах в школе робототехники «Роботек», учитывает семестры, базу знаний, а также умеет распознавать голосовые сообщения через Whisper.

**1. Подготовка окружения**

Склонируйте репозиторий (или скачайте архив с файлами проекта):

git clone https://github.com/yourusername/yourrepo.git

(либо используйте GitHub Desktop, либо скачайте .zip-архив и распакуйте его)

Перейдите в папку проекта:

cd yourrepo

Создайте виртуальное окружение (опционально, но рекомендуется):

python -m venv venv
source venv/bin/activate  # macOS/Linux
.\venv\Scripts\activate   # Windows

Установите зависимости:

pip install -r requirements.txt

В результате будут установлены необходимые пакеты: whatsapp-chatbot-python и прочие зависимости.


**2. Настройка переменных и ключей**

В файле main.py есть несколько важных переменных:

openai.api_key = 'апи когда будет сюда' Замените строку

на реальный ключ (например, "sk-...").


**3. Запуск бота**

После корректной настройки ключей и установки зависимостей достаточно запустить:

python main.py

В консоли появится лог:

[DEBUG] Запуск бота...
2025-XX-XX XX:XX:XX:whatsapp-chatbot-python:INFO:Deleted old incoming notifications.
2025-XX-XX XX:XX:XX:whatsapp-chatbot-python:INFO:Started receiving incoming notifications.
...

Это означает, что бот начал прослушивать входящие сообщения.


**4. Файлы проекта**

main.py

Основной скрипт, содержащий всю логику бота:

Подключение к GPT
Настройка Green API
Обработчики входящих (@bot.router.message()) и исходящих сообщений (@bot.router.outgoing_message())
Логика определения языка, выдачи приветствия, передачи запроса менеджеру и т.д.

requirements.txt

Перечень зависимостей (библиотек Python), необходимых для работы бота.

known_clients.json

Файл, в котором хранится список (set) известных клиентов (чтобы отправлять приветствие только один раз). Если файл отсутствует, он будет создан автоматически при первой записи.

README.md

Текущий файл с инструкциями по запуску.


**Изменение модели**

В коде установлено:
MODEL = "gpt-4o-mini"

Можно заменить на другие более мощные модели, например:
MODEL = "gpt-4o"





**Развёртывание бота на VPS**

Предположим, у вас есть любой VPS с установленной операционной системой Linux (Ubuntu, Debian или другая). Пример ниже будет рассматриваться на примере Ubuntu, но общая логика применима и к другим дистрибутивам.

Подготовьте VPS

Убедитесь, что на сервере установлен Python 3.7+ (лучше 3.9 или 3.10). Проверить версию можно командой:

python3 --version

Если Python не установлен, установите:

sudo apt-get update
sudo apt-get install python3 python3-pip

При желании можно установить Git для клонирования репозитория:

sudo apt-get install git

Скопируйте проект на сервер

Вариант A: Клонировать напрямую из GitHub:

git clone https://github.com/yourusername/yourrepo.git
cd yourrepo

Вариант B: Скопировать файлы (включая main.py, requirements.txt, и т.д.) по SSH/SCP или любым другим удобным способом (например, FileZilla, WinSCP).
Убедитесь, что все файлы лежат в одной папке.
Создайте виртуальное окружение (опционально, но рекомендуется)
Так вы сможете «изолировать» зависимости проекта.

python3 -m venv venv
source venv/bin/activate
При этом в консоли появится префикс (venv).

Установите зависимости
Выполните команду:

pip install -r requirements.txt

Убедитесь, что все пакеты (включая whatsapp-chatbot-python, openai и другие) установились без ошибок.

Настройте переменные (в файле main.py):

openai.api_key = "ваш апи"

Запустите бота
На сервере (в папке проекта):

python main.py

В консоли вы увидите что-то вроде:

[DEBUG] Запуск бота...
2025-XX-XX XX:XX:XX:whatsapp-chatbot-python:INFO:Deleted old incoming notifications.
2025-XX-XX XX:XX:XX:whatsapp-chatbot-python:INFO:Started receiving incoming notifications.
...
Это означает, что бот запущен и слушает входящие уведомления.

Проверьте работу


**Автозапуск и постоянная работа**

Если вы хотите, чтобы бот автоматически запускался при перезагрузке сервера, можно воспользоваться systemd, Supervisor или pm2. Ниже пример с systemd:

Создайте юнит-файл, например /etc/systemd/system/robotekbot.service:

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

Замените /путь/до/yourrepo на реальный путь к вашему проекту, а также уточните User=.

Активируйте и запустите службу:

sudo systemctl daemon-reload
sudo systemctl enable robotekbot
sudo systemctl start robotekbot

Проверьте статус:

systemctl status robotekbot
Если всё в порядке, сервис будет работать в фоне. При сбое или ребуте сервера он автоматически перезапустится.


