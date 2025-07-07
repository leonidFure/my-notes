Redis поддерживает **скрипты на Lua**, что позволяет:
- Выполнять сложные операции атомарно.
- Избегать гонок данных.
- Минимизировать сетевые вызовы.
Пример:
```lua
redis.call('SET', 'counter', 1)
redis.call('INCR', 'counter')
return redis.call('GET', 'counter')
```
Вызов в CLI:
```shell
EVAL "return redis.call('GET', 'counter')" 0
```

==TODO дополнить информацию про это (Думаю не очень юзабельный кейс, так что не срочно)==