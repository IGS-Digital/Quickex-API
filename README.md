# Документация по API Quickex
###### Communication protocol: REST

### Здравствуйте!

Это документация по API Quickex, которое позволяет:
1. Создать свой криптообменник. Легко, своими руками и бесплатно
2. Расширить предложение уже существующего обменника электронных или цифровых валют.
3. Подключить оплату криптовалютой в интернет магазине
4. Расширить возможности криптокошелька подключив на нем обмен имеющихся валют.

Ниже вы можете найти описания каждого доступного запроса / метода.


------------


### АВТОРИЗАЦИЯ

------------


|POST`/login` |
| ------------ |
| Это позволяет пользователям отправлять запросы в API Quickex. Чтобы зарегистрироваться в качестве клиента, пожалуйста, свяжитесь с нашей   [службой технической поддержки](mailto:support@quickex.io "технической поддержки"). |




Для доступа к вашей учетной записи используйте логин и пароль, предоставленные службой. После входа в систему вам будет предоставлен личный токен, необходимый для любого метода, используемого в API Quickex.

Маркер используется для каждого запроса в заголовках (Authorization: Bearer {token}).
Токен меняется каждые 5 минут.
После истечения 5-минутного сеанса повторно примените метод / логин.

#### **Пример URI**

**POST** `https://api.quickex.io / login`



**Ответ** `201`

- Headers

`Content-Type: application/json`

- Body

`Bearer token`

------------


### ПОЛУЧИТЬ КУРСЫ 

------------


#### **Rate**

|GET`/rate/{currencyPair}`|
| ------------ |
| Этот запрос получает текущий курс. Эта ставка не включает комиссию за транзакцию (майнер), взимаемую с каждой транзакции. |

#### **Пример URI**
**GET** `https://api.quickex.io/rate/eth_btc`

#### **Параметры URI**

**currencyPair**    `string` (обязательно) Пример: `eth_btc`
    Валютная пара


**Ответ**  `200`

Headers



    Content-Type: application/json

Body



    {
      "pair": "eth_btc",
      "rate": 0.03101315
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "pair": {
          "type": "string",
          "description": "Currency pair"
        },
        "rate": {
          "type": "number",
          "description": "Rate for currency pair"
        }
      },
      "required": [
        "pair",
        "rate"
      ]
    }

**Ответ**  `400`

Headers



    Content-Type: application/json

Body



    {
      "error": "Pair eth_fake is currently unavailable."
    }
    
Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "error": {
          "type": "string",
          "description": "Error message"
        }
      },
      "required": [
        "error"
      ]
    }


------------


#### Депозитный лимит

|GET` /limit/{currencyPair`|
| ------------ |
|Этот запрос получает ограничение на количество базовой валюты, которое может быть обменено, как установлено Quickex в данный момент. Любая избыточная сумма будет отправлена ​​на адрес возврата. Это значение может быть изменено из-за внезапных колебаний рынка.|

#### **Пример URI**
**GET** `https://api.quickex.io/limit/eth_btc`

#### **Параметры URI**

**currencyPair**         `string` (обязательно) Пример: `eth_btc`
    Валютная пара

**Ответ**  `200`

Headers



    Content-Type: application/json

Body



    {
      "pair": "eth_btc",
      "limit": 0.00003699,
      "min": 0.000001
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "pair": {
          "type": "string",
          "description": "Currency pair"
        },
        "limit": {
          "type": "number",
          "description": "Max limit for currency pair"
        },
        "min": {
          "type": "number",
          "description": "Min limit for currency pair"
        }
      },
      "required": [
        "pair",
        "limit",
        "min"
      ]
    }
    

**Ответ**  `404`

Headers



    Content-Type: application/json

Body



    {
      "error": "Unknown pair"
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "error": {
          "type": "string",
          "description": "Error message"
        }
      },
      "required": [
        "error"
      ]
    }
