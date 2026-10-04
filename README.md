# SNMP-модуль для диагностики и управления коммутаторами L2

Программный модуль на Python для диагностики и управления коммутаторами уровня L2 по протоколу SNMP (на примере D-Link DES-3028). Архитектура предусматривает расширение на другие модели.

## Возможности

- Идентификация модели коммутатора по `sysObjectID` и `sysDescr`
- Управление коммутатором: IP, маска, шлюз, management VLAN, время,   save, reboot, reset, очистка счётчиков
- Таблицы MAC (FDB) и ARP, Flood FDB
- VLAN: создание, удаление, tagged/untagged, управление PVID
- ACL Ethernet и Packet Content, включая фильтрацию по IP источника
- DHCP relay и Option 82
- Управление портами: admin state, speed/duplex, flow control, MDIX, cable diagnostic, port security, loopback detection
- Ограничение трафика: bandwidth control, traffic control, traffic segmentation
- Статистика порта: байты, пакеты unicast/multicast/broadcast, ошибки CRC, утилизация
- Trusted hosts
- Расширение на новые модели через `oid.yaml` без изменения ядра

## Стек

- Python 3.14
- asyncio
- pysnmp 7.1 (асинхронный HLAPI)
- pydantic 2.13 (валидация запросов)
- PyYAML, python-dotenv, icmplib

## Структура

```
src/
├── snmp_client.py         # ядро: get / set / bulkWalk, идентификация, ошибки
├── L2_switch_client.py    # прикладной клиент: VLAN, ACL, FDB, DHCP relay, порты
├── L2_switch_handler.py   # фасад: удобный интерфейс + валидация
├── schemas.py             # Pydantic-схемы запросов
├── const.py               # константы, community, типы
├── snmp_exceptions.py     # коды ответа и исключения
├── oid.yaml               # декларативное описание OID и моделей
└── snmp.py                # примеры использования
```

Примеры сгруппированы по блокам: `switch_config_example`, `trusted_host_config_example`, `acl_config_example`, `vlan_config_example`, `fdb_flood_fdb_example`, `port_management_example`, `port_security_config_example`, `port_statistics_packet_error_example` и др.

**Примечание**:

Проект предназначен для работы в сети интернет-провайдера. Запуск модуля сторонним пользователем в текущей реализации не предусмотрен.

## Справочник по MIB

Описание поддерживаемых MIB, проверенных OID, команд SNMP и особенностей DES-3028 — в `mib_info.md`.