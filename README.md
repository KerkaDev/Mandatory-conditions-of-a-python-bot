# Обязательные условия бота на Python



**Environment**

1. При разработки бота мы часто имеем дело с какой-либо секретной информацией, различными токенами и паролями (API-ключами, секретами веб-форм). "Хардкодить" эту информацию, а тем более сохранять в публично доступной системе контроля версий это очень плохая идея.
Для этого в корневой папке проекта создаем файл '.env' и храним всю важную информацию там.

```python
# Telegram
export BOT_TOKEN=5555555555:AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
export ADMIN_ID=5555555555

#DataBase Postgre
export DB_NAME=Kerka
export DB_USER=Kerka
export DB_PASSWORD=Kerka
export DB_HOST=192.168.1.1
export DB_PORT=5432
```

Получить доступ к переменным, можно просто
```python
import os

bot = Bot(token=os.getenv("BOT_TOKEN"))
```

Перед запуском проекта, не забудьте активировать окружение
```shell
. ./.env
```

**Config**   

2. В корневой папке проекта создаем файл config.py . Там мы и будем хранить все остальные данные и настройки. Например: Dispatcher, Bot, SQLConection и тд.


**Logging** 

3. Многие программисты используют оператор print для лечения багов, но вы можете использовать логирование для этих целей. Использование лога также более чистый метод, если вы не хотите просматривать весь свой код, чтобы удалить все операторы print. Мы требуем логировать каждый уровень в отдельный файл и в консоль

```python
import logging
import sys

from aiogram import Dispatcher, Bot, types
from aiogram.contrib.middlewares.logging import LoggingMiddleware
from aiogram.contrib.fsm_storage.memory import MemoryStorage

# Loging config
logger = logging.getLogger()
logger.setLevel(logging.INFO)
formatterError = logging.Formatter(
    "[%(asctime)s] - [%(levelname)s] - [%(name)s(%(filename)s) - %(funcName)s%(lineno)d] "
    "- %(message)s\n[%(processName)s:%(process)d] [%(threadName)s:%(thread)d] - %("
    "pathname)s\n"
)
formatter = logging.Formatter("[%(asctime)s] - [%(levelname)s] -->  %(message)s")

os.mkdir('logs')
stdout_handler = logging.StreamHandler(sys.stdout)
stdout_handler.setLevel(logging.DEBUG)
stdout_handler.setFormatter(formatter)

file_handler_debug = logging.FileHandler("logs/debug.log")
file_handler_debug.setLevel(logging.DEBUG)
file_handler_debug.setFormatter(formatter)

file_handler_warning = logging.FileHandler("logs/warning.log")
file_handler_warning.setLevel(logging.WARNING)
file_handler_warning.setFormatter(formatterError)

file_handler_errors = logging.FileHandler("logs/error.log")
file_handler_errors.setLevel(logging.ERROR)
file_handler_errors.setFormatter(formatterError)

file_handler_info = logging.FileHandler("logs/info.log")
file_handler_info.setLevel(logging.INFO)
file_handler_info.setFormatter(formatter)

file_handler_warn = logging.FileHandler("logs/warn.log")
file_handler_warn.setLevel(logging.WARN)
file_handler_warn.setFormatter(formatter)

file_handler_critical = logging.FileHandler("logs/critical.log")
file_handler_critical.setLevel(logging.CRITICAL)
file_handler_critical.setFormatter(formatterError)

file_handler_fatal = logging.FileHandler("logs/fatal.log")
file_handler_fatal.setLevel(logging.FATAL)
file_handler_fatal.setFormatter(formatterError)

logger.addHandler(file_handler_debug)
logger.addHandler(file_handler_warning)
logger.addHandler(file_handler_errors)
logger.addHandler(file_handler_info)
logger.addHandler(file_handler_warn)
logger.addHandler(file_handler_critical)
logger.addHandler(file_handler_fatal)
logger.addHandler(stdout_handler)

bot = Bot(token=os.getenv("BOT_TOKEN"), parse_mode=types.ParseMode.HTML)
dp = Dispatcher(bot, storage=MemoryStorage())
dp.middleware.setup(LoggingMiddleware())
```
Если в обрабатываете try except блок, то можно так же в ручную ввыводить лог
```python
# DB connection

try:
    connection = psycopg2.connect(
        database=os.getenv("DB_NAME"),
        user=os.getenv("DB_USER"),
        password=os.getenv("DB_PASSWORD"),
        host=os.getenv("DB_HOST"),
        port=os.getenv("DB_PORT"),
    )
    connection.set_isolation_level(ISOLATION_LEVEL_AUTOCOMMIT)
    connection.autocommit = True
except Exception as _ex:
    logging.exception(f"Error while working with PostgreSQL - {_ex}")
```
