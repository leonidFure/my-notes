Redis Cluster – это механизм горизонтального масштабирования и отказоустойчивости, позволяющий распределять данные между несколькими узлами. Он устраняет необходимость в одном единственном мастере и автоматически балансирует нагрузку.
## Основные принципы
- **Шардирование (Sharding)** – данные распределяются по нодам с помощью **хеш-слотов**.
- **Отказоустойчивость** – если один мастер выходит из строя, его реплика становится новым мастером.
- **Автоматический failover** – кластер автоматически обнаруживает и устраняет сбои.
- **Множественные мастера** – каждый узел отвечает за свою часть данных, нет одного главного мастера.
- **Асинхронная репликация** – каждый мастер может иметь один или несколько слейвов.
## Архитектура
Redis Cluster состоит из **минимум 3 мастер-узлов** и **желательно 3 реплик**, итого 6 узлов.
### Компоненты кластера
- **Мастеры (Master Nodes)** – хранят данные и обрабатывают запросы.
- **Реплики (Replica Nodes)** – резервные копии мастеров (failover).
- **Хеш-слоты (Hash Slots)** – механизм распределения ключей по узлам (всего **16 384** слота).
Каждый ключ попадает в конкретный слот, рассчитываемый по формуле:

```ini
SLOT = CRC16(key) % 16384
```
## Механизм отказоустойчивости
Если мастер выходит из строя, его реплика автоматически становится новым мастером.
### Как Redis определяет сбой?
1. Узлы пингуют друг друга (`PING` / `PONG`).
2. Если мастер **не отвечает > cluster-node-timeout**, он помечается как "подозрительный".
3. Если большинство узлов согласны, что мастер недоступен → происходит **failover**.

### Docker compose файл для Redis Cluster

```yaml
version: '3'

services:
  redis-node-1:
    image: redis:7
    container_name: redis-node-1
    ports:
      - "7000:6379"
    command: ["redis-server", "--cluster-enabled", "yes", "--cluster-config-file", "/data/nodes.conf", "--cluster-node-timeout", "5000", "--appendonly", "yes"]
    volumes:
      - redis-data-1:/data

  redis-node-2:
    image: redis:7
    container_name: redis-node-2
    ports:
      - "7001:6379"
    command: ["redis-server", "--cluster-enabled", "yes", "--cluster-config-file", "/data/nodes.conf", "--cluster-node-timeout", "5000", "--appendonly", "yes"]
    volumes:
      - redis-data-2:/data

  redis-node-3:
    image: redis:7
    container_name: redis-node-3
    ports:
      - "7002:6379"
    command: ["redis-server", "--cluster-enabled", "yes", "--cluster-config-file", "/data/nodes.conf", "--cluster-node-timeout", "5000", "--appendonly", "yes"]
    volumes:
      - redis-data-3:/data

  redis-node-4:
    image: redis:7
    container_name: redis-node-4
    ports:
      - "7003:6379"
    command: ["redis-server", "--cluster-enabled", "yes", "--cluster-config-file", "/data/nodes.conf", "--cluster-node-timeout", "5000", "--appendonly", "yes"]
    volumes:
      - redis-data-4:/data

  redis-node-5:
    image: redis:7
    container_name: redis-node-5
    ports:
      - "7004:6379"
    command: ["redis-server", "--cluster-enabled", "yes", "--cluster-config-file", "/data/nodes.conf", "--cluster-node-timeout", "5000", "--appendonly", "yes"]
    volumes:
      - redis-data-5:/data

  redis-node-6:
    image: redis:7
    container_name: redis-node-6
    ports:
      - "7005:6379"
    command: ["redis-server", "--cluster-enabled", "yes", "--cluster-config-file", "/data/nodes.conf", "--cluster-node-timeout", "5000", "--appendonly", "yes"]
    volumes:
      - redis-data-6:/data

volumes:
  redis-data-1:
  redis-data-2:
  redis-data-3:
  redis-data-4:
  redis-data-5:
  redis-data-6:
```

### Создание кластера
```bash
redis-cli --cluster create redis-node-1:6379 redis-node-2:6379 redis-node-3:6379 redis-node-4:6379 redis-node-5:6379 redis-node-6:6379 --cluster-replicas 1
```

