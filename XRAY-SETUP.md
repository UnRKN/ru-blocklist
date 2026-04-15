# Гайд по настройке

⚠️ **Дисклеймер:** Данный проект создан в информационных целях. Используйте ответственно и в соответствии с действующим законодательством.

---

## � Файлы для Xray

| Файл | Размер | Формат |
|------|--------|--------|
| **ru-blocklist-domain.txt** | 340 доменов | `domain:example.com` |
| **ru-blocklist-extended-domain.txt** | 460 доменов | `domain:example.com` |

### Какой выбрать?
- **ru-blocklist-domain.txt** — базовый (рекомендуется)
- **ru-blocklist-extended-domain.txt** — расширенный с региональными доменами

---

## 🚀 Установка (3 шага)

---

### Шаг 1: Скопируй файл

```bash
sudo cp ru-blocklist-domain.txt /etc/xray/
sudo chown xray:xray /etc/xray/ru-blocklist-domain.txt
sudo chmod 644 /etc/xray/ru-blocklist-domain.txt
```

### Шаг 2: Обнови config.json

```json
{
  "routing": {
    "rules": [
      {
        "type": "field",
        "domain": ["ext:file:///etc/xray/ru-blocklist-domain.txt"],
        "outboundTag": "block"
      }
    ]
  },
  "outbounds": [
    {
      "protocol": "blackhole",
      "tag": "block"
    },
    {
      "protocol": "freedom",
      "tag": "direct"
    }
  ]
}
```

### Шаг 3: Перезагрузи сервис

```bash
sudo systemctl restart xray
sudo systemctl status xray
```

---

## 📝 Примеры конфигов

### Пример 1: Простая блокировка

```json
{
  "routing": {
    "rules": [
      {
        "type": "field",
        "domain": ["ext:file:///etc/xray/ru-blocklist-domain.txt"],
        "outboundTag": "block"
      }
    ]
  }
}
```

### Пример 2: С маршрутизацией на прокси

```json
{
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "rules": [
      {
        "type": "field",
        "domain": ["ext:file:///etc/xray/ru-blocklist-domain.txt"],
        "outboundTag": "block"
      },
      {
        "type": "field",
        "domain": ["geosite:cn"],
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "network": "tcp,udp",
        "outboundTag": "proxy"
      }
    ]
  }
}
```

### Пример 3: Расширенный список

```json
{
  "routing": {
    "rules": [
      {
        "type": "field",
        "domain": ["ext:file:///etc/xray/ru-blocklist-extended-domain.txt"],
        "outboundTag": "block"
      }
    ]
  }
}
```

---

## ✅ Проверка

```bash
# Проверить синтаксис конфига
xray test -c /etc/xray/config.json

# Просмотреть логи
sudo journalctl -u xray -f

# Проверить блокировку
curl -v https://yandex.ru  # должно быть Connection refused
```

---

## 🐛 Решение проблем

### Ошибка: "Файл не найден"

```bash
ls -la /etc/xray/ru-blocklist-domain.txt
sudo chmod 644 /etc/xray/ru-blocklist-domain.txt
```

### Ошибка синтаксиса JSON

```bash
xray test -c /etc/xray/config.json
```

**Решение:** убедись что используешь файлы с суффиксом `-domain.txt` (не `-extended.txt` для базового списка)

---

**Последнее обновление:** апрель 2026