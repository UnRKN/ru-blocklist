# 🚫 Список РУ-доменов для блокировки в VPN

<div align="center">

**❤️ Если помогло — поделитесь репозиторием и поставьте звезду**

Список доменов будет пополняться и исправляться. Открыт к любым дополнениям, критике и пожеланиям.
</div>

> Актуальный список доменов российских сервисов, сливающих информацию об использовании VPN
> 
> ⚠️ **Дисклеймер:** Проект создан в информационных целях. Используйте ответственно и согласно законодательству.

## 🚀 Быстрый старт

### Скачивание файлов

#### Рекомендуемый способ: Использование .dat файлов

```bash
# Базовый (без региональных доменов)
wget https://raw.githubusercontent.com/UnRKN/ru-blocklist/refs/heads/main/ru-blocklist-domain.dat

# Расширенный (с региональными доменами)
wget https://raw.githubusercontent.com/UnRKN/ru-blocklist/refs/heads/main/ru-blocklist-extended-domain.dat
```

### ⚙️ Пример использования

#### Пример 1:

Используйте формат `ext:file://` с файлами `.dat` в формате geosite:

```json
{
  "routing": {
    "rules": [
      {
        "type": "field",
        "domain": ["ext:file:///etc/xray/ru-blocklist-extended-domain.dat:RU"],
        "outboundTag": "block"
      }
    ]
  }
}
```

#### Пример 2 (файл был скопирован прямо в /root/)

```json
  "routing": {
    "rules": [
      {
        "type": "field",
        "domain": [
          "ext:ru-blocklist-extended-domain.dat:RU"
        ],
        "outboundTag": "BLOCK"
      }
    ]
  }
```

**Формат:** `ext:файл:тег`
- `ext:` — префикс (всегда в нижнем регистре)
- `файл` — путь к .dat файлу в директории ресурсов
- `тег` — название тега в файле (например, `RU`)

Файл хранится в каталоге ресурсов XRay, обычно `/etc/xray/` на Linux или `C:\Program Files\xray\` на Windows.

#### Вариант 2: Использование текстовых списков (старый способ)

Если ваша версия XRay поддерживает загрузку из текстовых файлов:

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

## 📁 Какие есть файлы?

### 🔄 Форматы файлов

Файлы доступны в двух форматах:

1. **geosite.dat** — бинарный формат Protocol Buffer (рекомендуется)
   - Компактный размер
   - Быстрая загрузка
   - Используется в XRay/V2Ray напрямую
   - Формат: `ext:file://path/to/file.dat:TAG`

2. **Текстовые файлы** — простой текстовый формат
   - Легко редактировать
   - Легко проверять содержимое
   - Может быть медленнее при загрузке

### 📋 Список файлов

#### Вариант 1: Основной список (без региональных доменов)

| Файл | Формат | Размер | Описание |
|------|--------|--------|----------|
| **ru-blocklist-domain.dat** | geosite.dat | 5.8 KB | БЕЗ региональных, формат geosite (рекомендуется) |
| **ru-blocklist.dat** | geosite.dat | 5.8 KB | БЕЗ региональных, формат geosite |
| **ru-blocklist-domain.txt** | `domain:example.com` | 340 доменов | БЕЗ региональных доменов, с "domain:" перед каждым |
| **ru-blocklist.txt** | `example.com` | 340 доменов | БЕЗ региональных доменов и без "domain:" |

#### Вариант 2: Расширенный список (с региональными доменами)

| Файл | Формат | Размер | Описание |
|------|--------|--------|----------|
| **ru-blocklist-extended-domain.dat** | geosite.dat | 7.8 KB | С региональными, формат geosite (рекомендуется) |
| **ru-blocklist-ext.dat** | geosite.dat | 7.8 KB | С региональными, формат geosite |
| **ru-blocklist-extended-domain.txt** | `domain:example.com` | 460 доменов | С региональными доменами, с "domain:" перед каждым |
| **ru-blocklist-ext.txt** | `example.com` | 460 доменов | С региональными доменами и без "domain:" |

---

**Файлы содержит один домен на строку в следующих форматах:**

```text
domain:example.com
```
или
```text
example.com
```

---

#### Скачивание raw-ссылкой:

```bash
# Без поддоменов
wget https://raw.githubusercontent.com/UnRKN/ru-blocklist/main/russian-anti-vpn-domains-no-subdomains.txt

# Расширенный список
wget https://raw.githubusercontent.com/UnRKN/ru-blocklist/main/russian-anti-vpn-domains-extended.txt
```

## 📊 Статистика по компаниям

Детализированный список основных доменов (без поддоменов) для каждой компании:

| Компания | Доменов | Примеры |
|----------|---------|---------|
| **Yandex** | 59 | yandex.ru, yandex.com, yandex.kz, yandex.by, yandexcloud.net, … |
| **Sber** | 37 | sber.ru, sberbank.ru, sbercloud.ru, sbrf.ru, sbermarket.ru, … |
| **Tinkoff** | 36 | tinkoff.ru, tinkoff.com, t-bank.ru, tbank.ua, tinkoffbank.ru, … |
| **Wildberries** | 33 | wildberries.ru, wbcdn.net, wildberries.kz, wildberries.com, … |
| **MTS** | 34 | mts.ru, mts-bank.ru, mtsbank.ru, mts.kz, mts.by, … |
| **Ozon** | 27 | ozon.ru, ozon.com, ozonbank.ru, ozon.kz, ozon.by, … |
| **VK** | 26 | vk.com, vk.ru, vkontakte.com, vk-cdn.net, vkid.ru, … |
| **Остальные** | ~161 | Госсайты, СМИ и прочее |

**Итого:**
- 📌 **7 крупных компаний** в основной таблице
- 📦 **460+ доменов** в расширенном списке
- 🌍 Включены региональные домены (.ru, .com, .kz, .by, .ua и т.д.)

## 🔧 Регенерация файлов geosite.dat

Если вам нужно обновить файлы `.dat` после изменения списка доменов, используйте скрипт `geosite_generator.py`:

```bash
python3 geosite_generator.py
```

Скрипт автоматически сконвертирует все текстовые списки (`.txt`) в формат geosite.dat (`.dat`).

**Требования:**
- Python 3.6+
- Никаких внешних зависимостей (встроенные модули только)

**Как это работает:**
1. Читает домены из текстовых файлов (`ru-blocklist-*.txt`)
2. Конвертирует их в формат Protocol Buffer (protobuf)
3. Создает бинарные файлы (`.dat`) в том же каталоге
4. Эти файлы готовы к использованию с XRay/V2Ray

**Форматы доменов в `.dat` файлах:**
- Тип `3` = `domain` (стандартный, совпадает с доменом и всеми поддоменами)
- Тег: `RU` (для идентификации набора доменов)

<details>
<summary><b>🔍 Полный список доменов по компаниям (нажмите, чтобы развернуть)</b></summary>

| # | Компания | Кол-во | Домены |
|---|----------|--------|---------|
| 1 | **MTS** | 34 | `mts-bank.ru`, `mts-link.ru`, `mts.ai`, `mts.am`, `mts.by`, `mts.it`, `mts.kz`, `mts.md`, `mts.ru`, `mts.ua`, `mtsbank.ru`, `mts.cloud`, `mts.digital`, `mts.education`, `mts.global`, `mts.group`, `mts.info`, `mts.io`, `mts.news`, `mts.online`, `mts.org`, `mts.plus`, `mts.pro`, `mts.shop`, `mts.tech`, `mts.today`, `mts.tools`, `mts.travel`, `mts.tv`, `mts.video`, `mts.web`, `mts.work`, `mtsru.net`, `mtstelecom.ru` |
| 2 | **Ozon** | 27 | `ozon-api.com`, `ozon.ae`, `ozon.am`, `ozon.az`, `ozon.by`, `ozon.cloud`, `ozon.com`, `ozon.de`, `ozon.express`, `ozon.ge`, `ozon.io`, `ozon.kg`, `ozon.kz`, `ozon.md`, `ozon.ru`, `ozon.shop`, `ozon.store`, `ozon.tech`, `ozonbank.ru`, `ozoncloud.com`, `ozonfinance.ru`, `ozonfintech.ru`, `ozonlogistics.ru`, `ozonmart.ru`, `ozonshop.ru`, `ozontech.io`, `ozon.uz` |
| 3 | **Sber** | 37 | `sbbol.ru`, `sber.ai`, `sber.auto`, `sber.ch`, `sber.f`, `sber.group`, `sber.net`, `sber.online`, `sber.pro`, `sber.ru`, `sber.store`, `sber.vin`, `sber.wine`, `sberbank.com`, `sberbank.ru`, `sbercapital.com`, `sbercard.ru`, `sbercloud.ru`, `sberdevices.ru`, `sberbiz.ru`, `sberhealth.ru`, `sberid.ru`, `sberinsurance.ru`, `sberlabs.ru`, `sberlogistics.ru`, `sbermarket.ru`, `sbermegamarket.ru`, `sberonline.ru`, `sberpm.ru`, `sbersafe.ru`, `sbersolutions.ru`, `sbertech.ru`, `sberuniversity.ru`, `sberx.ru`, `sberzvuk.ru`, `sbrf.com`, `sbrf.ru` |
| 4 | **Tinkoff** | 36 | `t-bank.ru`, `tbank.by`, `tbank.kz`, `tbank.ru`, `tbank.ua`, `tinkoff.ai`, `tinkoff.by`, `tinkoff.com`, `tinkoff.digital`, `tinkoff.finance`, `tinkoff.flights`, `tinkoff.global`, `tinkoff.homes`, `tinkoff.io`, `tinkoff.legal`, `tinkoff.media`, `tinkoff.news`, `tinkoff.online`, `tinkoff.org`, `tinkoff.press`, `tinkoff.pro`, `tinkoff.ru`, `tinkoff.shop`, `tinkoff.support`, `tinkoff.tech`, `tinkoff.travel`, `tinkoff.tv`, `tinkoff.ua`, `tinkoff.video`, `tinkoffbank.ru`, `tinkoffbroker.ru`, `tinkoffcredit.ru`, `tinkoffjournal.ru`, `tinkofffund.ru`, `tinkoffinsurance.ru`, `tinkoffmobile.ru` |
| 5 | **VK** | 26 | `vk-cdn.net`, `vk.biz`, `vk.cc`, `vk.com`, `vk.dev`, `vk.link`, `vk.me`, `vk.online`, `vk.photo`, `vk.press`, `vk.ru`, `vk.sh`, `vk.store`, `vk.team`, `vkactuality.ru`, `vkcdn.net`, `vkconnect.ru`, `vkfaces.com`, `vkid.ru`, `vkoauth.net`, `vkontakte.com`, `vkontakte.ru`, `vkontaktes.com`, `vksearch.ru`, `vkstatic.com`, `vkstatic.net` |
| 6 | **Wildberries** | 33 | `wbcdn.net`, `wbext.ru`, `wbforever.ru`, `wbmedia.ru`, `wbnet.ru`, `wbplus.ru`, `wbrbl.ru`, `wbsafe.ru`, `wbselfy.ru`, `wbsmb.ru`, `wildberries.ai`, `wildberries.am`, `wildberries.app`, `wildberries.asia`, `wildberries.az`, `wildberries.by`, `wildberries.club`, `wildberries.com`, `wildberries.dev`, `wildberries.ge`, `wildberries.id`, `wildberries.in`, `wildberries.io`, `wildberries.kg`, `wildberries.kz`, `wildberries.md`, `wildberries.nl`, `wildberries.ru`, `wildberries.se`, `wildberries.shop`, `wildberries.store`, `wildberries.tech`, `wildberries.uz` |
| 7 | **Yandex** | 59 | `yandex.aero`, `yandex.az`, `yandex.bg`, `yandex.by`, `yandex.ca`, `yandex.cloud`, `yandex.co.il`, `yandex.co.th`, `yandex.co.uk`, `yandex.com`, `yandex.com.am`, `yandex.com.br`, `yandex.com.ge`, `yandex.com.ru`, `yandex.com.tr`, `yandex.cz`, `yandex.de`, `yandex.ee`, `yandex.es`, `yandex.eu`, `yandex.fi`, `yandex.fr`, `yandex.gr`, `yandex.hr`, `yandex.hu`, `yandex.id`, `yandex.in`, `yandex.io`, `yandex.it`, `yandex.jobs`, `yandex.kg`, `yandex.kz`, `yandex.lt`, `yandex.lv`, `yandex.md`, `yandex.net`, `yandex.org`, `yandex.pl`, `yandex.pt`, `yandex.ro`, `yandex.rs`, `yandex.ru`, `yandex.se`, `yandex.sk`, `yandex.st`, `yandex.sx`, `yandex.tj`, `yandex.tm`, `yandex.tr`, `yandex.ua`, `yandex.uz`, `yandex.vn`, `yandexadexchange.net`, `yandexcloud.net`, `yandexcom.net`, `yandexdisk.com`, `yandexmetrica.com`, `yandexwebcache.net`, `yandexwebcache.org` |

</details>

<div align = center>
created by UnRKN, Grok & Claude with ❤️