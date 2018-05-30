# Документація по API для замовлення відомостей з ДЗК
За допомогою цього API ви можете реалізувати у себе на ресурсі замовлення відомостей з ДЗК. Для авторизації використовується протокол [OAuth2](https://oauth.net/2/). **client_id** та **client_secret** ви можете отримати зареєструвавшись на ресурсі [Електроні сервіси ДЗК](https://e.land.gov.ua) та перейшовши в розділ **Налаштування API** 

## Опис методів API

**Отримання авторизаційного токену виконується за адресою https://e.land.gov.ua/oauth/v2/token** **[POST]**

**Данні, які потрібно передати:**
| Ключ | Значення |
| ------ | ------ |
| client_id | 1_2dgdf8ueinokw00sog0wweg404c4k8vfwkswc345sgg8sckkkc |
| client_secret | 2igsc1ggiykg84www088kos4g84w8sws44oow4k0koc04kksk4 |
| grant_type | client_credentials |

Після отримання авторизаційного токену його потрібно додавати в заголовки при запитах

| Ключ | Значення |
| ------ | ------ |
| Authorization | Bearer `{{access_token}}` |

**Отримання списку регіонів**
**https://e.land.gov.ua/api/get/regions/json** **[GET]** 

```javascript
[
    {
        "id": 1,
        "name": "Івано-Франківська"
    }
]
```
**Отримання списку ЦНАП**
**https://e.land.gov.ua/api/get/cnaps/{regionId}/json** **[GET]** 

```javascript
[
    {
        "id": 243,
        "name": "Центр надання адміністративних послуг Івано-Франківської міської ради"
    },
    {
        "id": 244,
        "name": "Центр надання адміністративних послуг Болехівської міської ради"
    },
]
```

**Отримання детальної інформації про ЦНАП**
**https://e.land.gov.ua/api/get/cnap/detail/{cnapId}/json** **[GET]** 

```javascript
[
    {
        "id": 1,
        "name": "Центр надання адміністративних послуг Оратівської районної державної адміністрації",
        "vid": "районний",
        "dateOpen": "2030-12-20T00:00:00+02:00",
        "forma": "структурний підрозділ районної державної адміністрації",
        "address": "22600, Вінницька обл., Оратівський район, смт Оратів, вул. Героїв Майдану, 82",
        "website": "http://orrda.gov.ua/centr-nadannya-administrativnih-poslug/adresa-centru/",
        "scheduleOfWork": "понеділок, середа, п'ятниця: 08.00-17.00, вівторок, четвер: 08.00-20.00, субота: 08.00-14.00, неділя: вихідний день",
        "officeHours": "понеділок, середа, п'ятниця: 08.00-17.00, вівторок, четвер: 08.00-20.00, субота: 08.00-14.00, неділя: вихідний день",
        "servicesAdmin": true,
        "servicesAgent": true,
        "workersHead": "Янковий Микола Григорович",
        "workersCount": 1,
        "koatuu": "05",
        "enabled": true,
        "removed": false,
        "lat": "49.18955540",
        "lng": "29.53422110",
        "googlePlaceId": "ChIJy6iaOgTr0kARip4fkV-PfXE"
    }
]
```
**Відправка заяви на отримання данних**
**https://e.land.gov.ua/api/excerpt/json** **[POST]** 



**Базовий опис даних для замовлення як фізичною так і юридичною особою. Додаткові поля для кожного типу заявника будуть описані нижче.**

| Ключ | Опис | Обов'язковість |
| ------ | ------ | ------ |
| requestType | Тип заяви. Можливі варінтаи: 9 => Витяг з ДЗК про земельну ділянку; 12 => Викопіювання з картографічної основи ДЗК, кадастрової карти (плану) | Так |
| cadNum | Кадастровий номер земельної ділянки | Так |
| transmit_as | Форма видачі відомостей. Можливі варінти: 1 => у паперовій формі; 2 => у електронній та паперовій формі | Так |
| procuratorySeries | Серія документу, що посвідчує повноваження діяти від імені особи (довіреність) | Так, якщо procuratoryNumber та procuratoryOwner присутні |
| procuratoryNumber | Номер документу, що посвідчує повноваження діяти від імені особи (довіреність) | Так, якщо procuratorySeries та procuratoryOwner присутні  |
| procuratoryOwner | П.І.Б. Уповноваженої особи на отримання Витягу | Так, якщо procuratorySeries та procuratoryNumber присутні |
| documentOwnerType | Тип документу на право власності. Можливі варінти: "" => 'Немає'; "Державний акт" => Державний акт; "Свідоцтво про право власності" => Свідоцтво про право власності  | Так |
| documentOwner | Серія та номер документу на право власності | Так,якщо documentOwnerType == "Державний акт" та documentOwnerType == "Свідоцтво про право власності" |
| region | Регіон. Список регіонів можна отримати в API | Так |
| cnap | Центр надання адміністративних послуг, в якому Ви бажаєте отримати відповідні документи. Список можна отримати в API | Так |

**Додатковий набір даних для фізичної особи**

| Ключ | Опис | Обов'язковість |
| ------ | ------ | ------ |
| type_user[email] | Електронна адреса | Так |
| type_user[fname] | Ім'я | Так |
| type_user[mname] | По батькові | Опційно |
| type_user[lname] | Прізвище | Так |
| type_user[inn] | Ідентифікаційний (податковий) номер | Опційно |
| type_user[address] | Місце проживання фізичної особи | Так |
| type_user[tel] | Номер телефону | Опційно |
| type_user[type_document] | Тип документу, що посвідчує особу. Можливі варінти: "passport_ukr" => Паспорт громадянина України; "passport_ukr_id" => Паспорт громадянина України у формі картки. Опис данних для кожного із типу буде наведений нижче. | Так |
| type_user[accept] | Надаю дозвіл на обробку моїх персональних даних з метою отримання відповідних послуг та використання цих даних для ведення Державного земельного кадастру згідно з вимогами законодавства. | Так |

**Додатковий набір даних для Паспорт громадянина України**

| Ключ | Опис | Обов'язковість |
| ------ | ------ | ------ |
| type_user[document][series] | Серія паспорта | Так |
| type_user[document][number] | Номер паспорта | Так |
| type_user[document][dateIssue] | Дата видачі паспорта у форматі **22-04-2001** | Так |

**Додатковий набір даних для Паспорт громадянина України у формі картки. Опис данних для кожного із типу буде наведений нижче.**

| Ключ | Опис | Обов'язковість |
| ------ | ------ | ------ |
| type_user[document][number] | Номер паспорта | Так |
| type_user[document][dateIssue] | Дата видачі паспорта у форматі **22-04-2001** | Так |

**Додатковий набір даних для юридичної особи**

| Ключ | Опис | Обов'язковість |
| ------ | ------ | ------ |
| type_user[email] | Електронна адреса | Так |
| type_user[orgName] | Назва організації | Так |
| type_user[edrpou] | Код ЕДРПОУ | Так |
| type_user[address] | Місце проживання юридичної особи | Так |
| type_user[tel] | Номер телефону | Опційно |
| type_user[accept] | Надаю дозвіл на обробку моїх персональних даних з метою отримання відповідних послуг та використання цих даних для ведення Державного земельного кадастру згідно з вимогами законодавства. | Так |

**Приклад відповіді з помилками**

```javascript
{
    "status": "error",
    "errors": {
        "type_user": {
            "inn": [
                "Значення повиино бути рівним 10 символам."
            ],
            "document": {
                "number": [
                    "Значення повиино бути рівним 9 символам."
                ]
            }
        }
    }
}
```
**Приклад відповіді успішної операції**

```javascript
{
    "status": "success",
    "request_num": "9900001262018",
    "date": "2018-05-30T12:49:03+03:00",
    "redirect_url": "https://www.liqpay.ua/api/checkout?data=eyJ2ZXJzaW9uIjozLCJsYW5ndWFnZSI6InVrIiwiYW1vdW50IjoiODkuNzEiLCJjdXJyZW5jeSI6IlVBSCIsInR5cGUiOiJidXkiLCJkZXNjcmlwdGlvbiI6Ilx1MDQxMlx1MDQzOFx1MDQ0Mlx1MDQ0Zlx1MDQzMyBcdTA0MzcgXHUwNDE0XHUwNDE3XHUwNDFhIFx1MDQzZlx1MDQ0MFx1MDQzZSBcdTA0MzdcdTA0MzVcdTA0M2NcdTA0MzVcdTA0M2JcdTA0NGNcdTA0M2RcdTA0NDMgXHUwNDM0XHUwNDU2XHUwNDNiXHUwNDRmXHUwNDNkXHUwNDNhXHUwNDQzIiwib3JkZXJfaWQiOiJcdTA0MjAtMTcxLlx1MDQxMlx1MDQzOFx1MDQ0Mlx1MDQ0Zlx1MDQzMyBcdTA0MzcgXHUwNDE0XHUwNDE3XHUwNDFhIFx1MDQzZlx1MDQ0MFx1MDQzZSBcdTA0MzdcdTA0MzVcdTA0M2NcdTA0MzRcdTA0NTZcdTA0M2IgXHUwNDM3XHUwNDMwIDMwXC8wNVwvMjAxOCBcdTA0MWZcdTA0M2VcdTA0M2ZcdTA0NDNcdTA0MzRcdTA0NDBcdTA0MzVcdTA0M2RcdTA0M2FcdTA0M2UgXHUwNDA2LiIsInNlcnZlcl91cmwiOiJodHRwczpcL1wvMTkyLjE2OC4zMy4yMTFcL2FwcF9kZXYucGhwXC9saXFwYXlcL2NhbGxiYWNrXC8iLCJwdWJsaWNfa2V5IjoiaTYyOTY2NzI4NzAyIn0=&signature=a7KfEhOj43w4w2LYO5HeR9MV3AM=",
    "request_type_text": "земельну ділянку",
    "request_type_price_base": 88.1,
    "request_type_price_fee": 1.61
}
```
**Опис тіла відповіді**

| Ключ | Опис |
| ------ | ------ |
| request_num | Номер створеної заяви |
| date | Дата формування заяви |
| redirect_url | Посилання для сплати послуги |
| request_type_text | Короткий опис послуги |
| request_type_price_base | Сума до сплати |
| request_type_price_fee | Комісія банку |
