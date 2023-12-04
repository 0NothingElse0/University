# Decimal SDK 1.0.0 documentation

Документация по используемым методам для работы с Decimal SDK (dsc-js-sdk v1.1.43")

## Table of Contents

* [Method](#method)
  * [decimal.connect](#decimal-connect)
  * [PUB create.wallet](#create-wallet)
  * [PUB get.balance.coin](#get-balance-coin)
  * [PUB get.balance.token](#get-balance-token)
  * [PUB verification.address](#verification-address)


## Method

### `decimal.connect`

*Подключение к сети Decimal*

<h1 id="decimal.connect"></h1>

##### Mode

| Name | Type | Description |
|---|---|---|
| mainnet | Основная среда | Основной шлюз, для работы Decimal. Используется для запуска приложения в прод и тд. Проводятся реальные транзакции |
| testnet | Тестовая среда | Тестовый шлюз, для финальный проверки работы со всеми транзакциями перед запуском приложения в прод и переведения на mainnet| 
| devnet | Среда разработки | Шлюз для разработки. В целом вся разработка должна вестись в нем, но на момент написания данного руководства devnet не поддерживал выполнения любых транзакций, по этому разработка велась в testnet  |

##### Example
Подключение к Decimal делается же следующим образом:
const decimal = await Decimal.connect(DecimalNetworks.mainnet);

### `Создание кошелька`

*Как в Decimal создать кошелёк*

<h1 id="create.wallet"></h1>

##### Example
```
const bip39 = require("bip39");
const mnemonic = bip39.generateMnemonic(); - генерирование мнемоника(Тоже само, что и seed-фраза)
const decimalWallet = new Wallet(mnemonic); - Если кошелька для mnemonic ещё не создано, то создаст новый кошелек. Если же кошелёк уже существует, то будет получен этот кошелёк
```

При необходимости можно установить текущий кошелёк для выполнения операций через : 
```
decimal.setWallet(decimalWallet); - тут decimalWallet - экземпляр полученного кошелька
```
### Options
| Name | Type | Description |
|---|---|---|
| address | string | Адрес текущего кошелька для работы с коинами. Например, d01lhl73321tst2x4v5ygmvk2v0l4jez4mwl2yqt6 |
| evmAddress | string | Адрес текущего кошелька для работы с токенами. Например, 0xfdffe8e3225c16341dx42236cb298ffd6591576e| 
| gateUrl | string | Шлюз к которому идёт обращение. Например, 'https://testnet-gate.decimalchain.com/api/  |

### `Получение баланса коинов`

*Получение баланса коинов кошелька.*

<h1 id="get.balance.coin"></h1>

```
decimal.getAddress(address) - возвращает баланс для всех коинов на кошельке с указанным address
```

> Examples of payload _(generated)_

```json
{
  "key": "value"
}
```
<p>key - сокращенное название монеты. Например, Decimal - del</p>
<p>value - сумма содержащаяся на счету. В общем случае, содержит 18 знаков после запятой + n кол-во знаков после запятой(Зависит от баланса).</p>

Пример получения баланса коинов кошелька:
##### Example
```
const decimalWallet = new Wallet(mnemonic);
const decimal = await Decimal.connect(DecimalNetworks.testnet);
decimal.setWallet(decimalWallet);
const decimalBalance = (await decimal.getAddress(address)).balance;
```

###  `Получение баланса токенов` Operation

*Получение баланса токенов кошелька.*

<h1 id="get.balance.token"></h1>

```
decimal.getEvmAccountBalance(address); - возвращает баланс для всех коинов на кошельке с указанным address
```
### Options
| Name | Type | Description |
|---|---|---|
| evmAccountERC20TokenBalances | Токен ERC20 | Содержит информацию о ERC20 токенах на аккаунте(По умолчанию через Decimal создаются именно они) |
| evmAccountERC721TokenBalances | Токен ERC721 | Содержит информацию о ERC721 токенах на аккаунте| 
| evmAccountERC1155TokenBalances | Токен ERC1155 | Содержит информацию о ERC1155 токенах на аккаунте|

Пример получения баланса токенов ERC20 кошелька:
##### Example
```
const decimalWallet = new Wallet(mnemonic);
const decimal = await Decimal.connect(DecimalNetworks.testnet);
decimal.setWallet(decimalWallet);
const evmBalance = await decimal.getEvmAccountBalance(decimalWallet.evmAddress);
const evmTokenBalance = evmBalance.evmTokenAccountBalances.evmAccountERC20TokenBalances;
```
> Examples of payload _(generated)_

```json
{
      "amount": "10000000000000000000",
      "evmTokenId": 69,
      "evmAccountId": 1553,
      "evmToken":  {
          "id": 69,
          "address": "0xa2a6296133c79c08be5cc588b92fcec83f10d767",
          "symbol": "NothingWTSTestToken",
          "title": "NOT",
          "decimals": 18,
          "totalSupply": "10000000000000000000000000",
          "evmContractId": 134,
          "evmTokenTypeName": "ERC20",
          "createdAt": "2023-11-23T17:12:26.088Z",
          "updatedAt": "2023-11-23T17:12:27.978Z"
      }
    }

```
evmAccountERC20TokenBalances
| Name | Type | Description |
|---|---|---|
| amount | string | Кол-во токена на кошельке |
| evmTokenId | integer | Ид токена в EVM(Ethereum Virtual Machine)| 
| evmAccountId | integer | Ид аккаунта создавшего токен в EVM|
| evmToken | object | Объект с информацией об токене |

evmToken
| Name | Type | Description |
|---|---|---|
| id | integer | Ид | 
| address | string | Адрес контракта токена|
| symbol | string | Полное название токена |
| title | string | Краткое название токена| 
| decimals | integer | Кол-во знаков после запятой|
| totalSupply | string | Общее кол-во созданных токенов(Максимальный лимит) |
| evmContractId | integer | Ид контракта| 
| evmTokenTypeName | string | Тип контракта|

### `Верификация адреса кошелька`

*Верификация адреса кошелька для коинов Decimal*

<h1 id="verification.address"></h1>

##### Example
```
decimal.verifyAddress(address)
```