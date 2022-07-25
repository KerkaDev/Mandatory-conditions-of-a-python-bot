# Обязательные условия бота на питона



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
