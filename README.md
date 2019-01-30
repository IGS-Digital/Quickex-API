# Документация по API Quickex
###### Communication protocol: REST

### Здравствуйте!

Это документация по API Quickex, которое позволяет:
-  Создать свой криптообменник без покупки скриптов и с минимальными усилиями.
-  Расширить предложение уже существующего обменника электронных или цифровых валют.
- Подключить оплату криптовалютой в интернет магазине
- Расширить возможности криптокошелька подключив на нем обмен имеющихся валют.

Ниже вы можете найти описания каждого доступного запроса / метода.



------------





### АВТОРИЗАЦИЯ

------------


|POST`/login` |
| ------------ |
| Метод позволяет пользователям отправлять запросы в API Quickex. Чтобы зарегистрироваться в качестве клиента, пожалуйста, свяжитесь с нашей   [службой технической поддержки](mailto:support@quickex.io "технической поддержки").  Подключение бесплатное, коммиссия Quickex составляет 0,5% и уже включена в курс. Дополнительную коммиссию Вы можете установить самостоятельно|




Для доступа к вашей учетной записи используйте логин и пароль, предоставленные службой. После входа в систему вам будет предоставлен личный токен, необходимый для любого метода, используемого в API Quickex.

Маркер используется для каждого запроса в заголовках (Authorization: Bearer {token}).
Токен меняется каждые 5 минут.
После истечения 5-минутного сеанса повторно примените метод / логин.

#### **Пример URI**

**POST** `https://api.quickex.io / login`



**Ответ** `201`

Headers


    Content-Type: application/json

Body


       Bearer token



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


------------


#### Marketinfo

|GET` /marketinfo/{currencyPair}`|
| ------------ |
|Этот запрос получает текущие данные рынка.|


#### **Пример URI**
**GET** `https://api.quickex.io/marketinfo/eth_btc`

#### **Параметры URI**

**currencyPair**         `string` (обязательно) Пример: `eth_btc`
    Валютная пара

**Ответ**  `200`

Headers



    Content-Type: application/json

Body



    {
      "rate": 0.03101315,
      "limit": 0.0410287605,
      "pair": "eth_btc",
      "min": 0.001
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "rate": {
          "type": "number",
          "description": "Rate for currency pair"
        },
        "limit": {
          "type": "number",
          "description": "Max limit"
        },
        "pair": {
          "type": "string",
          "description": "Currency pair"
        },
        "min": {
          "type": "number",
          "description": "Min limit"
        }
      },
      "required": [
        "rate",
        "limit",
        "pair",
        "min"
      ]
    }

**Ответ**  `200`

Headers



    Content-Type: application/json

Body



    [
      {
        "rate": 0.03101315,
        "limit": 0.0410287605,
        "pair": "eth_btc",
        "min": 0.001
      },
      {
        "rate": 31.74313635,
        "limit": 0.00003699,
        "pair": "btc_eth",
        "min": 0.000001
      }
    ]

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "array"
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


------------
#### Информация о запросе на обмен

|GET`/txStat/{depositAddress}/{tag}`|
| ------------ |
|Этот запрос получает детали транзакции по адресу депозита, указанному в транзакции  обмена|


#### **Пример URI**
**GET** `https://api.quickex.io/txStat/0x4d6DDdAF0baFe591C1E4Fba5F13350fD909d978F/123321`

#### **Параметры URI**

**depositAddress**   ` string` (обязательно) Пример: `0x4d6DDdAF0baFe591C1E4Fba5F13350fD909d978F` 
Адрес депозита 


**tag**    `string` (необязательно) Пример: `123321`
    Тег назначения 

**Ответ**  `200`

Headers

    Content-Type: application/json

Body



    {
      "status": "success",
      "address": "0x4d6DDdAF0baFe591C1E4Fba5F13350fD909d978F",
      "withdraw": "19npLWanndix2Kif2eRz4W9JyHQTQ6fdX",
      "incomingCoin": 0.001,
      "incomingType": "ETH",
      "outgoingCoin": 0.00006126,
      "outgoingType": "BTC",
      "transaction": "04c20dd8-740b-4254-9a2a-7720a8a20dbf",
      "destinationTag": "destination tag",
      "withdrawalTag": "withdrawal tag"
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "status": {
          "type": "string",
          "description": "Success exchange status"
        },
        "address": {
          "type": "string",
          "description": "Deposit address"
        },
        "withdraw": {
          "type": "string",
          "description": "Withdraw address"
        },
        "incomingCoin": {
          "type": "number",
          "description": "Deposit amount, amount you send to exchange"
        },
        "incomingType": {
          "type": "string",
          "description": "Deposit currency"
        },
        "outgoingCoin": {
          "type": "number",
          "description": "Withdrawal amount, amount you get after exchange"
        },
        "outgoingType": {
          "type": "string",
          "description": "Withdrawal currency"
        },
        "transaction": {
          "type": "string",
          "description": "Transaction id"
        },
        "destinationTag": {
          "type": "string",
          "description": "destination tag"
        },
        "withdrawalTag": {
          "type": "string",
          "description": "withdrawal tag"
        }
      },
      "required": [
        "status",
        "address",
        "withdraw",
        "incomingCoin",
        "incomingType",
        "outgoingCoin",
        "outgoingType",
        "transaction"
      ]
    }

**Ответ**  `200`

Headers



    Content-Type: application/json

Body



    {
      "status": "waiting_exchange",
      "address": "0x4d6DDdAF0baFe591C1E4Fba5F13350fD909d978F",
      "withdraw": "19npLWanndix2Kif2eRz4W9JyHQTQ6fdX",
      "incomingCoin": 0.001,
      "incomingType": "ETH"
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "status": {
          "type": "string",
          "description": "Waiting exchange status"
        },
        "address": {
          "type": "string",
          "description": "Deposit address"
        },
        "withdraw": {
          "type": "string",
          "description": "Withdrawal address"
        },
        "incomingCoin": {
          "type": "number",
          "description": "Deposit amount, amount you send to exchange"
        },
        "incomingType": {
          "type": "string",
          "description": "Deposit currency"
        }
      },
      "required": [
        "status",
        "address",
        "withdraw",
        "incomingCoin",
        "incomingType"
      ]
    }
**Ответ**  `200`

Headers



    Content-Type: application/json

Body



    {
      "status": "canceled_by_customer",
      "address": "0x4d6DDdAF0baFe591C1E4Fba5F13350fD909d978F"
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "status": {
          "type": "string",
          "description": "Exchange transaction status (available statuses: `pending_deposit`, `too_low_deposit`, `time_expired`, `canceled_by_customer`)"
        },
        "address": {
          "type": "string",
          "description": "Deposit address"
        }
      },
      "required": [
        "status",
        "address"
      ]
    }



**Ответ**  `400`


Headers



    Content-Type: application/json

Body



    {
      "status": "error",
      "address": "0x2CEb271B86DD201D1fE54Ab3BE350D0Ecf759279",
      "error": "This address is NOT a Quickex deposit address. Do not send anything to it."
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "status": {
          "type": "string",
          "description": "Error status"
        },
        "address": {
          "type": "string",
          "description": "Deposit address"
        },
        "error": {
          "type": "string",
          "description": "Error message"
        }
      },
      "required": [
        "status",
        "address",
        "error"
      ]
    }


------------

#### Монеты

|GET`/getcoins`|
| ------------ |
|Этот запрос позволяет получить список всех валют, которые в настоящее время поддерживает Quickex.|


#### **Пример URI**
**GET** `https://api.quickex.io/getcoins`

#### **Параметры URI**

**Ответ**  `200`


Headers



    Content-Type: application/json

Body



    {
      "BTC": {
        "name": "BTC",
        "symbol": "Bitcoin",
        "image": "https://quickex.io/images/coins/btc.png",
        "imageSmall": "https://quickex.io/images/coins-sm/btc.png",
        "status": "available"
      },
      "ETH": {
        "name": "ETH",
        "symbol": "Etherium",
        "image": "https://quickex.io/images/coins/eth.png",
        "imageSmall": "https://quickex.io/images/coins-sm/eth.png",
        "status": "available"
      },
      "USD": {
        "name": "USD",
        "symbol": "United States dollar",
        "image": "https://quickex.io/images/coins/usd.png",
        "imageSmall": "https://quickex.io/images/coins-sm/usd.png",
        "status": "unavailable"
      },
      "EUR": {
        "name": "EUR",
        "symbol": "Euro",
        "image": "https://quickex.io/images/coins/eur.png",
        "imageSmall": "https://quickex.io/images/coins-sm/eur.png",
        "status": "unavailable"
      },
      "GBP": {
        "name": "GBP",
        "symbol": "Pound sterling",
        "image": "https://quickex.io/images/coins/gbp.png",
        "imageSmall": "https://quickex.io/images/coins-sm/gbp.png",
        "status": "unavailable"
      },
      "LTC": {
        "name": "LTC",
        "symbol": "Litecoin",
        "image": "https://quickex.io/images/coins/ltc.png",
        "imageSmall": "https://quickex.io/images/coins-sm/ltc.png",
        "status": "unavailable"
      },
      "BCH": {
        "name": "BCH",
        "symbol": "Bitcoin Cash",
        "image": "https://quickex.io/images/coins/bch.png",
        "imageSmall": "https://quickex.io/images/coins-sm/bch.png",
        "status": "available"
      },
      "ETC": {
        "name": "ETC",
        "symbol": "Ethereum Classic",
        "image": "https://quickex.io/images/coins/etc.png",
        "imageSmall": "https://quickex.io/images/coins-sm/etc.png",
        "status": "unavailable"
      },
      "EOS": {
        "name": "EOS",
        "symbol": "EOS",
        "image": "https://quickex.io/images/coins/eos.png",
        "imageSmall": "https://quickex.io/images/coins-sm/eos.png",
        "status": "unavailable"
      },
      "DASH": {
        "name": "DASH",
        "symbol": "DASH",
        "image": "https://quickex.io/images/coins/dash.png",
        "imageSmall": "https://quickex.io/images/coins-sm/dash.png",
        "status": "unavailable"
      }
    }

Schema



    {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string",
            "description": "Currency name"
          },
          "symbol": {
            "type": "string",
            "description": "Currency symbol"
          },
          "image": {
            "type": "string",
            "description": "Currency image"
          },
          "imageSmall": {
            "type": "string",
            "description": "Currency small image"
          },
          "status": {
            "type": "string",
            "description": "Currency status"
          }
        }
      }
    }

**Ответ**  `400`

Headers



    Content-Type: application/json

Body



    {
      "error": "Coins is currently unavailable."
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




