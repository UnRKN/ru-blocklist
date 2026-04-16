# Техническая документация: Формат geosite.dat

## Общее описание

Файлы `.dat` (geosite.dat) — это бинарные файлы в формате Protocol Buffer (protobuf), используемые V2Ray и XRay для хранения географических данных о доменах. Это оптимизированный формат для быстрой загрузки и поиска доменов.

## Структура Protocol Buffer

### Определение (geosite.proto)

```protobuf
message Domain {
  string value = 1;
  uint32 type = 2;
}

message GeoSite {
  string country_code = 1;
  repeated Domain domain = 2;
}

message GeoSiteList {
  repeated GeoSite entry = 1;
}
```

### Поля Domain

- **value** (string): Доменное имя (например, `yandex.ru`)
- **type** (uint32): Тип домена:
  - `1` = plain (простой домен, точное совпадение)
  - `2` = regex (регулярное выражение)
  - `3` = domain (домен с поддоменами, **рекомендуется**)
  - `4` = full (полный домен)

### Поля GeoSite

- **country_code** (string): Код страны/региона (например, `RU` для России)
- **domain** (repeated Domain): Массив доменов

### Поля GeoSiteList

- **entry** (repeated GeoSite): Массив записей GeoSite

## Пример структуры в файле

```
GeoSiteList {
  entry: [
    {
      country_code: "RU"
      domain: [
        { value: "yandex.ru", type: 3 }
        { value: "sber.ru", type: 3 }
        { value: "vk.com", type: 3 }
        ...
      ]
    }
  ]
}
```

## Использование в XRay

### Синтаксис в конфигурации

```json
{
  "routing": {
    "rules": [
      {
        "type": "field",
        "domain": ["ext:file://путь/к/файлу.dat:RU"],
        "outboundTag": "блокировка"
      }
    ]
  }
}
```

### Формат: `ext:file://path:tag`

- `ext:` — префикс (обязательно в нижнем регистре)
- `file://` — протокол (обязательно)
- `path` — абсолютный или относительный путь к файлу `.dat`
- `tag` — название тега (должно совпадать с `country_code` в файле)

### Примеры

#### Linux/MacOS
```
ext:file:///etc/xray/ru-blocklist-extended-domain.dat:RU
ext:file:///usr/local/share/xray/ru-blocklist.dat:RU
```

#### Windows
```
ext:file://C:\\Program Files\\xray\\ru-blocklist-extended-domain.dat:RU
```

#### Относительный путь (от каталога конфигурации)
```
ext:file://resources/ru-blocklist-extended-domain.dat:RU
```

## Генерация файлов

### Используемый скрипт: `geosite_generator.py`

Скрипт преобразует текстовые файлы со списками доменов в bинарный формат geosite.dat.

**Алгоритм:**
1. Читает доменные имена из текстовых файлов
2. Удаляет префикс `domain:` если присутствует
3. Фильтрует пустые строки и комментарии
4. Кодирует каждый домен в формат Protocol Buffer с типом `3` (domain)
5. Группирует домены под тегом `RU`
6. Записывает результат в бинарный файл

**Пример использования:**
```bash
python3 geosite_generator.py
```

Скрипт автоматически обработает все файлы:
- `ru-blocklist-domain.txt` → `ru-blocklist-domain.dat`
- `ru-blocklist-extended-domain.txt` → `ru-blocklist-extended-domain.dat`
- `ru-blocklist.txt` → `ru-blocklist.dat`
- `ru-blocklist-ext.txt` → `ru-blocklist-ext.dat`

## Сравнение форматов

| Аспект | geosite.dat | Текстовый файл |
|--------|-------------|----------------|
| **Размер** | Компактный (5-8 KB) | Больший (10-15 KB) |
| **Скорость загрузки** | Быстрая | Медленнее |
| **Человеческая читаемость** | Нет (бинарный) | Да |
| **Простота редактирования** | Нет, нужна регенерация | Да, текстовый редактор |
| **Совместимость** | V2Ray, XRay | V2Ray, XRay, другие |
| **Рекомендуется** | ✅ Для production | Для разработки |

## Техническая справка

### Protobuf Wire Format

Каждое поле в protobuf кодируется как:
- **Tag** (variable-length integer): Идентификатор поля
- **Wire Type** (3 бита в tag):
  - `0` = Varint
  - `2` = Length-delimited
- **Value** (зависит от типа)

### Примеры кодирования

Домен `yandex.ru` с типом `3`:

```
Domain.value = "yandex.ru"  → Tag: 1, WireType: 2 (length-delimited)
Domain.type = 3             → Tag: 2, WireType: 0 (varint)
```

Кодированный результат (hex):
```
0a 0a "yandex.ru" 10 03
```

Где:
- `0a` = Tag 1, WireType 2
- `0a` = Длина строки (10 байт)
- `yandex.ru` = 9 байт (+ 1 байт для кодирования длины)
- `10` = Tag 2, WireType 0
- `03` = Значение 3

## Ресурсы

- [V2Ray Protocol Buffer Reference](https://www.v2ray.com/)
- [Protocol Buffers Documentation](https://developers.google.com/protocol-buffers)
- [XRay Configuration Reference](https://xtls.github.io/en/)

## Поддержка

Для вопросов или проблем с файлами `.dat`:
1. Убедитесь, что используется правильный путь к файлу
2. Проверьте, что тег совпадает с `country_code` в файле (обычно `RU`)
3. Попробуйте использовать абсолютный путь вместо относительного
4. Проверьте права доступа к файлу
