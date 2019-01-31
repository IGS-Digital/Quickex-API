#  Документация по API Quickex
###### Communication protocol: REST

##  Здравствуйте!

Это документация по API Quickex, которое позволяет:
-  Создать свой криптообменник без покупки скриптов и с минимальными усилиями.
-  Расширить предложение уже существующего обменника электронных или цифровых валют.
- Подключить оплату криптовалютой в интернет магазине
- Расширить возможности криптокошелька подключив на нем обмен имеющихся валют.

Ниже вы можете найти описания каждого доступного запроса / метода.



------------





## АВТОРИЗАЦИЯ

------------


|POST `/login` |
| ------------ |
| Метод позволяет пользователям отправлять запросы в API Quickex. Чтобы зарегистрироваться в качестве клиента, пожалуйста, свяжитесь с нашей   [службой технической поддержки](mailto:support@quickex.io "технической поддержки").  Подключение бесплатное, коммиссия Quickex составляет 0,5% и уже включена в курс. Дополнительную коммиссию Вы можете установить самостоятельно|




Для доступа к вашей учетной записи используйте логин и пароль, предоставленные службой. После входа в систему вам будет предоставлен личный токен, необходимый для любого метода, используемого в API Quickex.

Маркер используется для каждого запроса в заголовках (Authorization: Bearer {token}).
Токен меняется каждые 5 минут.
После истечения 5-минутного сеанса повторно примените метод / логин.

#### **Пример URI**

**POST** `https://api.quickex.io / login`



**Response** `201`

Headers


    Content-Type: application/json

Body


       Bearer token



------------

## GET REQUESTS

------------


### **Курсы**

|GET `/rate/{currencyPair}`|
| ------------ |
| Этот запрос получает текущий курс. Эта ставка не включает комиссию за транзакцию (майнер), взимаемую с каждой транзакции. |

#### **Пример URI**
**GET** `https://api.quickex.io/rate/eth_btc`

#### **Параметры URI**

**currencyPair**    `string` (обязательно) Пример: `eth_btc`
Валютная пара


**Response**  `200`


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

**Response**  `400`

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


### Депозитный лимит

|GET ` /limit/{currencyPair}`|
| ------------ |
|Этот запрос получает ограничение на количество базовой валюты, которое может быть обменено, как установлено Quickex в данный момент. Любая избыточная сумма будет отправлена ​​на адрес возврата. Это значение может быть изменено из-за внезапных колебаний рынка.|

#### **Пример URI**
**GET** `https://api.quickex.io/limit/eth_btc`

#### **Параметры URI**

**currencyPair**         `string` (обязательно) Пример: `eth_btc`
Валютная пара

**Response**  `200`

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
    

**Response**  `404`

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


### Marketinfo

|GET ` /marketinfo/{currencyPair}`|
| ------------ |
|Этот запрос получает текущие данные рынка.|


#### **Пример URI**
**GET** `https://api.quickex.io/marketinfo/eth_btc`

#### **Параметры URI**

**currencyPair**         `string` (обязательно) Пример: `eth_btc`
Валютная пара

**Response**  `200`

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

**Response**  `200`

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

**Response**  `404`


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
### Информация о запросе на обмен

|GET `/txStat/{depositAddress}/{tag}`|
| ------------ |
|Этот запрос получает детали транзакции по адресу депозита, указанному в транзакции  обмена|


#### **Пример URI**
**GET** `https://api.quickex.io/txStat/0x4d6DDdAF0baFe591C1E4Fba5F13350fD909d978F/123321`

#### **Параметры URI**

**depositAddress**   ` string` (обязательно) Пример: `0x4d6DDdAF0baFe591C1E4Fba5F13350fD909d978F` 
Адрес депозита 


**tag**    `string` (необязательно) Пример: `123321`
Тег назначения 

**Response**  `200`

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

**Response**  `200`

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
**Response**  `200`

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



**Response**  `400`


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

### Валюты

|GET `/getcoins`|
| ------------ |
|Этот запрос позволяет получить список всех валют, которые в настоящее время поддерживает Quickex.|


#### **Пример URI**
**GET** `https://api.quickex.io/getcoins`

#### **Параметры URI**

**Response**  `200`


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

**Response**  `400`

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


------------

### Получить список последних транзакций

|GET `/recenttx/{max}`|
| ------------ |
|Этот запрос получает список самых последних транзакций.|


#### **Пример URI**
**GET** `https://api.quickex.io/recenttx/5`

#### **Параметры URI**

**max**    `number` (не обязательно) Пример: 5

  Максимальное количество транзакций для возврата. Если параметр не указан, будет возвращено 5 транзакций. Кроме того, параметр должен быть числом от 1 до 50 (включительно)..



**Response**  `200`
Headers



    Content-Type: application/json

Body



    [
      {
        "txid": "613d76d3-2f72-4003-844e-92b6dd160432",
        "amount": 0.00191317,
        "curIn": "ETH",
        "curOut": "ETC",
        "timestamp": "1536307658"
      }
    ]

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "array"
    }


**Response**  `400`
Headers



    Content-Type: application/json

Body



    {
      "error": "Recent transaction list is currently unavailable."
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

### Получить список транзакций с закрытым ключом API

|GET `/txbyapikey/{apikey}`|
| ------------ |
|Этот запрос получает список транзакций по закрытому ключу API. Это позволяет партнерам получать список всех транзакций, которые когда-либо были созданы с определенным ключом API. Транзакции создаются с открытым партнерским ключом, но их ищет соответствующий закрытый ключ. Использование открытых и закрытых ключей защищает конфиденциальность данных аккаунта наших партнеров.|


#### **Пример URI**
**GET** `https://api.quickex.io/txbyapikey/23efa324efb213a`

#### **Параметры URI**

**apikey**   `string` (обязательно) Пример:` 23efa324efb213a`
Является ли приватный ключ API партнера. 

**Response**  `200`

Headers



    Content-Type: application/json

Body



    [
      {
        "inputTXID": "5306c5ed-38b6-4da0-a1c8-0e78b725ddc9",
        "inputAddress": "0x56Dc35da0Bd2776c024Fbc14eB15CbE105059696",
        "inputCurrency": "ETH",
        "inputAmount": 0.0025,
        "outputTXID": "311c56a08b3444894373476f9576a62987078fb8499878b9ca0264e893622863",
        "outputAddress": "3771GPPSfDixm3cy3dM6GvjzKYYqXRmQCD",
        "outputCurrency": "BTC",
        "outputAmount": 0.00310434,
        "status": "time_expired",
        "destinationTag": "123",
        "withdrawalTag": "1234"
      }
    ]
    
Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "": {
          "type": "object",
          "properties": {
            "inputTXID": {
              "type": "string",
              "description": "Transaction id"
            },
            "inputAddress": {
              "type": "string",
              "description": "Input address"
            },
            "inputCurrency": {
              "type": "string",
              "description": "Input currency"
            },
            "inputAmount": {
              "type": "number",
              "description": "Input amount"
            },
            "outputTXID": {
              "type": "string",
              "description": "Transaction hash"
            },
            "outputAddress": {
              "type": "string",
              "description": "Output address"
            },
            "outputCurrency": {
              "type": "string",
              "description": "Output currency"
            },
            "outputAmount": {
              "type": "number",
              "description": "Output amount"
            },
            "status": {
              "type": "string",
              "description": "Transaction status"
            },
            "destinationTag": {
              "type": "string",
              "description": "Destination tag"
            },
            "withdrawalTag": {
              "type": "string",
              "description": "Withdrawal tag"
            }
          },
          "required": [
            "inputTXID",
            "inputAddress",
            "inputCurrency",
            "inputAmount",
            "outputTXID",
            "outputAddress",
            "outputCurrency",
            "outputAmount",
            "status"
          ]
        }
      }
    }

**Response**  `400`

Headers



    Content-Type: application/json

Body



    {
      "error": "Can't find transactions"
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

### Получить список транзакций с конкретным адресом для вывода средств и приватным ключом API 

|GET `txbyaddress/{withdrawalAddress}/{apikey}/{destinationTag}`|
| ------------ |
|Этот запрос позволяет партнерам получать список всех транзакций, которые когда-либо отправлялись на один из их адресов, в частности. Необходимо предоставить личный ключ партнера. Это позволяет получить список транзакций, которые были отправлены на определенный адрес назначения и были созданы с использованием открытого ключа партнера.|


#### **Пример URI**
**GET** `https://api.quickex.io/txbyaddress/3771GPPSfDixm3cy3dM6GvjzKYYqXRmQCD/23efa324efb213a/123321`

#### **Параметры URI**

**withdrawalAddress**    `string` (обязательно) Пример: `3771GPPSfDixm3cy3dM6GvjzKYYqXRmQCD`
 Снятие адреса 


**apikey**   ` string` (обязательно) Пример`: 23efa324efb213a`
Является ли приватный ключ API партнера. 

**destinationTag**    `string` (необязательно) Пример: `123321`
Тег назначения. 

**Response**  `200`

Headers



    Content-Type: application/json

Body



    [
      {
        "inputTXID": "5306c5ed-38b6-4da0-a1c8-0e78b725ddc9",
        "inputAddress": "0x56Dc35da0Bd2776c024Fbc14eB15CbE105059696",
        "inputCurrency": "ETH",
        "inputAmount": 0.0025,
        "outputTXID": "311c56a08b3444894373476f9576a62987078fb8499878b9ca0264e893622863",
        "outputAddress": "3771GPPSfDixm3cy3dM6GvjzKYYqXRmQCD",
        "outputCurrency": "BTC",
        "outputAmount": 0.00310434,
        "status": "time_expired",
        "destinationTag": "destination tag",
        "withdrawalTag": "withdrawal tag"
      }
    ]

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "array"
    }


**Response**  `400`

Headers



    Content-Type: application/json

Body



    {
      "error": "Can't find transactions"
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

### Подтвердить адрес 

|GET `/validateAddress/{address}/{currency}`|
| ------------ |
|Этот запрос позволяет пользователям проверить, что их адрес назначения действителен в соответствии с данным демоном кошелька. Если запрос возвращает значение «true», этот адрес проверяется демоном монеты, обозначенным символом валюты.|


#### **Пример URI**
**GET** `https://api.quickex.io/validateAddress/3DWFocD1xA4Ws5uREcHbKYqTY2viVMxSV5/btc`

#### **Параметры URI**

**address**  `string` (обязательно) Пример: `3DWFocD1xA4Ws5uREcHbKYqTY2viVMxSV5`
Адрес
	
**currency **   ` string` (обязательно) Пример:` btc`
Валюта


**Response**  `200`

Headers



    Content-Type: application/json

Body



    {
      "isvalid": true
    }
    
Schema


    
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "isvalid": {
          "type": "boolean",
          "description": "Is valid address"
        }
      },
      "required": [
        "isvalid"
      ]
    }


**Response**  `200`

Headers



    Content-Type: application/json

Body



    {
      "isvalid": false,
      "error": "Invalid address."
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "isvalid": {
          "type": "boolean",
          "description": "Is valid address"
        },
        "error": {
          "type": "string",
          "description": "Error message"
        }
      },
      "required": [
        "isvalid",
        "error"
      ]
    }



------------

##  POST REQUESTS

------------

### Создать быструю транзакцию 

|POST `/quick`|
| ------------ |
|Этот запрос создает быструю транзакцию (без указания суммы).|


#### **Пример URI**
**POST** ` https://api.quickex.io / quick`

#### **Параметры URI**

**Request** `with body`


- withdrawal: `12v4rjzyXnRF7dwNb4ukxTpYrugBTy6nct` (обязательно) - Адрес получателя

- pair: `eth_btc` (обязательно) - Валютные пары

- returnAddress: `0xd68CcC74C32BAB4c4c6F289b3b1754f46a8311FE` (обязательно) - Адрес возврата (для сдачи) в случае если клиент перевел сумму выше нашего лимита

- destinationTag: `destination tag` (необязательно) - тег назначения

- withdrawalTag: `withdrawal tag` (необязательно) - withdrawal tag

- apiKey: `234eac2234a423f` (необязательно) - Публичный api ключ

Headers



    Content-Type: application/json

Body


    
    {
      "withdrawal": "12v4rjzyXnRF7dwNb4ukxTpYrugBTy6nct",
      "pair": "eth_btc",
      "returnAddress": "0xd68CcC74C32BAB4c4c6F289b3b1754f46a8311FE",
      "destinationTag": "destination tag",
      "withdrawalTag": "withdrawal tag",
      "apiKey": "234eac2234a423f"
    }
    



**Response**  `200`

Headers



    Content-Type: application/json
    
Body



    {
      "withdrawal": "qpjs462knq4anmpyyf7axypplff5s4dyyql0k9rlkc",
      "withdrawalType": "BCH",
      "deposit": "0x886C0620d8A59DD5D53946644a1D91326862D1e7",
      "depositType": "ETH",
      "orderId": "678492f6-2944-45ba-b031-8ce9f4b14141",
      "apiPubKey": "abe1278ac423349e775",
      "destinationTag": "destination tag",
      "withdrawalTag": "withdrawal tag"
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "withdrawal": {
          "type": "string",
          "description": "Withdrawal address"
        },
        "withdrawalType": {
          "type": "string",
          "description": "Withdrawal currency"
        },
        "deposit": {
          "type": "string",
          "description": "Deposit address"
        },
        "depositType": {
          "type": "string",
          "description": "Deposit currency"
        },
        "orderId": {
          "type": "string",
          "description": "Order id"
        },
        "apiPubKey": {
          "type": "string",
          "description": "Public api key"
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
        "withdrawal",
        "withdrawalType",
        "deposit",
        "depositType",
        "orderId"
      ]
    }

**Response**  `400`

Headers



    Content-Type: application/json

Body



    {
      "error": "Can't create quick transaction"
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

### Создать фиксированную транзакцию  

|POST `/sendamount`|
| ------------ |
|Позволяет пользователям запрашивать фиксированную сумму для отправки на адрес вывода. Вы указываете адрес для вывода средств и сумму, которую хотите отправить на него. Мы возвращаем сумму, которую вы хотите снять, и адрес, на который эта сумма должна быть отправлена, и ждем вашего подтверждения|


#### **Пример URI**
**POST** ` https://api.quickex.io / sendamount`

#### **Параметры URI**

**Request** `with body`


- amount: `0.03101415` (необязательно) - Сумма вывода, сумма, которую вы получите после обмена

> Предупреждение! ¶
Если параметр `amount` пропущен, то  `depositAmount` не используется.

- depositAmount: `1` (optional, number) - Сумма депозита, сумма, которую вы отправляете на обмен


> Предупреждение! ¶
Если параметр `` depositAmount` пропущен, то  `amount` не используется.

- withdrawal: `12v4rjzyXnRF7dwNb4ukxTpYrugBTy6nct` (required, string) - Адрес получателя

- pair: `eth_btc` (required, string) - Пары обмена 

- returnAddress: `0xd68CcC74C32BAB4c4c6F289b3b1754f46a8311FE` (required, string) - Адрес возврата (для сдачи) в случае если клиент перевел сумму выше нашего лимита

- destinationTag: `destination tag` (optional, string) - destination tag

- withdrawalTag: `withdrawal tag` (optional, string) - withdrawal tag
- apiKey: `234eac2234a423f` (optional, string) - Публичный api ключ

**Response**  `201`

Headers



    Content-Type: application/json

Body



    {
      "pair": "eth_btc",
      "withdrawal": "qpjs462knq4anmpyyf7axypplff5s4dyyql0k9rlkc",
      "withdrawalAmount": 0.001,
      "expiration": 1540456968,
      "maxLimit": 10,
      "deposit": "0x886C0620d8A59DD5D53946644a1D91326862D1e7",
      "depositAmount": 0.002,
      "quotedRate": 1,
      "orderId": "678492f6-2944-45ba-b031-8ce9f4b14141",
      "apiPubKey": "abe1278ac423349e775",
      "destinationTag": "destination tag",
      "withdrawalTag": "withdrawal tag"
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
        "withdrawal": {
          "type": "string",
          "description": "Withdrawal address"
        },
        "withdrawalAmount": {
          "type": "number",
          "description": "Withdrawal amount, amount you get after exchange"
        },
        "expiration": {
          "type": "number",
          "description": "Expiration time for deposit"
        },
        "maxLimit": {
          "type": "number",
          "description": "Max limit for deposit"
        },
        "deposit": {
          "type": "string",
          "description": "Deposit address"
        },
        "depositAmount": {
          "type": "number",
          "description": "Deposit amount, amount you send to exchange"
        },
        "quotedRate": {
          "type": "number",
          "description": "Quoted rate"
        },
        "orderId": {
          "type": "string",
          "description": "Order id"
        },
        "apiPubKey": {
          "type": "string",
          "description": "Public api key"
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
        "pair",
        "withdrawal",
        "withdrawalAmount",
        "expiration",
        "maxLimit",
        "deposit",
        "depositAmount",
        "quotedRate",
        "orderId"
      ]
    }
**Response**  `400`

Headers



    Content-Type: application/json

Body



    {
      "error": "Can't create fixed amount transaction"
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

### Запрос котировочной цены

|POST `/sendamount`|
| ------------ |
|Этот запрос вернет только информацию о котировке и не будет генерировать адрес депозита.|


#### **Пример URI**
**POST** ` https://api.quickex.io/sendamount `

#### **Параметры URI**

**Request** `with body`

- amount: `0.03101415` (required, number) - Сумма, которую вы получите после обмена
- pair: `eth_btc` (required, string) - Пара обмена

Headers


    Content-Type: application/json

Body



    {
      "amount": 0.03101415,
      "pair": "eth_btc"
    }

**Response**  `200`

Headers



    Content-Type: application/json

Body



    {
      "quotedRate": 1,
      "maxLimit": 0.041028,
      "pair": "eth_btc",
      "withdrawalAmount": 0.001,
      "depositAmount": 0.002,
      "expiration": 1540456968
    }
    
Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "quotedRate": {
          "type": "number",
          "description": "Quoted rate"
        },
        "maxLimit": {
          "type": "number",
          "description": "Max limit"
        },
        "pair": {
          "type": "string",
          "description": "Currency pair"
        },
        "withdrawalAmount": {
          "type": "number",
          "description": "Withdrawal amount, amount you get after exchange"
        },
        "depositAmount": {
          "type": "number",
          "description": "Deposit amount, amount you send to exchange"
        },
        "expiration": {
          "type": "number",
          "description": "Expiration time for deposit"
        }
      },
      "required": [
        "quotedRate",
        "maxLimit",
        "pair",
        "withdrawalAmount",
        "depositAmount",
        "expiration"
      ]
    }
**Response**  `400`

Headers



    Content-Type: application/json

Body



    {
      "error": "Can't create fixed amount transaction"
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

### Отмена ожидаемой транзакции


|POST `/cancelpending`|
| ------------ |
|Позволяет пользователям запрашивать отмену транзакции, выбранной по адресу депозита.|


#### **Пример URI**
**POST** `https://api.quickex.io/cancelpending`

#### **Параметры URI**

**Request** `with body`



- address: `0xd68CcC74C32BAB4c4c6F289b3b1754f46a8311FE` (required, string) - Адрес депозита, связанный с ожидающей транзакцией

- tag: `destination tag` (optional, string) - destination tag

Headers



    Content-Type: application/json

Body



    {
      "address": "12v4rjzyXnRF7dwNb4ukxTpYrugBTy6nct",
      "tag": "destination tag"
    }



**Response**  `200`


Headers



    Content-Type: application/json

Body



    {
      "success": "Exchange request successfully update",
      "error": "null"
    }

Schema



    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "type": "object",
      "properties": {
        "success": {
          "type": "string",
          "description": "Success message"
        },
        "error": {
          "type": "string",
          "description": "Error message (always null)"
        }
      },
      "required": [
        "success",
        "error"
      ]
    }


**Response**  `400`

Headers



    Content-Type: application/json

Body



    {
      "error": "Can't cancel pending transaction"
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

### Получить результат транзакции по электронной почте


|POST `/cancelpending`|
| ------------ |
|Позволяет пользователям запрашивать отмену транзакции, выбранной по адресу депозита.|


#### **Пример URI**
**POST** `https://api.quickex.io/cancelpending`

#### **Параметры URI**

**Request** `with body`
