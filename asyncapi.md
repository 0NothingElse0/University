# Decimal SDK 1.0.0 documentation

Документация по используемым методам для работы с Decimal SDK (dsc-js-sdk версии 1.1.43")

## Table of Contents

* [Method](#method)
  * [decimal.connect](#dicaml-connect)
  * [PUB lobby.join](#pub-lobbyjoin-operation)
  * [PUB lobby.leave](#pub-lobbyleave-operation)
  * [PUB lobby.message.send](#pub-lobbymessagesend-operation)
  * [PUB lobby.get.all](#pub-lobbygetall-operation)
  * [PUB lobby.get.user](#pub-lobbygetuser-operation)
  * [PUB lobby.game.dice](#pub-lobbygamedice-operation)
  * [PUB lobby.game.buy](#pub-lobbygamebuy-operation)
  * [PUB lobby.game.skip](#pub-lobbygameskip-operation)
  * [PUB lobby.game.pay](#pub-lobbygamepay-operation)
  * [SUB lobby.message.get](#sub-lobbymessageget-operation)
  * [SUB lobby.get.user.result](#sub-lobbygetuserresult-operation)
  * [SUB lobby.refresh](#sub-lobbyrefresh-operation)
  * [SUB lobby.error](#sub-lobbyerror-operation)
  * [SUB lobby.message](#sub-lobbymessage-operation)
  * [SUB lobby.user.join](#sub-lobbyuserjoin-operation)
  * [SUB lobby.user.leave](#sub-lobbyuserleave-operation)
  * [SUB lobby.game.start](#sub-lobbygamestart-operation)
  * [SUB lobby.game.dice.result](#sub-lobbygamediceresult-operation)
  * [SUB lobby.game.player.next](#sub-lobbygameplayernext-operation)
  * [SUB lobby.game.player.move](#sub-lobbygameplayermove-operation)
  * [SUB lobby.game.tile.message](#sub-lobbygametilemessage-operation)
  * [SUB lobby.game.lose](#sub-lobbygamelose-operation)
  * [SUB lobby.game.win](#sub-lobbygamewin-operation)


## Method

### `decimal.connect` Operation

*Подключение к сети Decimal*

<h1 id="decimal.connect"></h1>

##### Mode

| Name | Type | Description |
|---|---|---|
| mainnet | Основная среда | Основной шлюз, для работы Decimal. Используется для запуска приложения в прод и тд. Проводятся реальные транзакции |
| testnet | Тестовая среда | Тестовый шлюз, для финальный проверки работы со всеми транзакциями перед запуском приложения в прод и переведения на mainnet| 
| devnet | Среда разработки | Шлюз для разработки. В целом вся разработка должна вестись в нем, но на момент написания данного руководства devnet не поддерживал выполнения любых транзакций, по этому разработка велась в testnet  |



### PUB `lobby.join` Operation

*Подключение к лобби.*

<h1 id="lobby.join"></h1>

#### Message `joinLobbyData`

*Тело эвента для подключения к лобби*

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are NOT allowed** |
| lobbyId | number | Айди лобби к которому необходимо подключиться | - | - | **required** |

> Examples of payload _(generated)_

```json
{
  "lobbyId": 0
}
```



### PUB `lobby.leave` Operation

*Выход из лобби.*

Тело эвента отсутсвует, необходимо просто вызвать эвент и клиент выйдет из лобби <h1 id="lobby.leave"></h1>


### PUB `lobby.message.send` Operation

*Отправка payload-а клиентам в лобби*

Временный эвент, отправляет любой переданный payload подключенным к лобби клиентам <h1 id="lobby.message.send"></h1>

#### Message `anyData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | Payload может быть любым | - | - | **additional properties are allowed** |

> Examples of payload _(generated)_

```json
{}
```



### PUB `lobby.get.all` Operation

*Получение списка всех лобби, доступна сортировка по полю isFull(заполненное лобби)*

Для получения списка всех лобби необходимо заэмитить этот эвент, сервер отправит ответ на эвент <a href="#lobby.message.get">lobby.message.get</a>

#### Message `getAllData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | Массив со всеми лобби | - | - | **additional properties are allowed** |
| lobbies | allOf | - | - | - | **additional properties are allowed** |
| lobbies.0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| lobbies.0.id | number | Идентификатор лобби | - | - | - |
| lobbies.0.name | string | Название лобби | - | - | - |
| lobbies.0.fieldType | string | Тип поля, временно доступно только стандартное поле (standard) | allowed (`"standard"`, `"big"`, `"huge"`) | - | - |
| lobbies.0.gameType | string | - | allowed (`"1vs1vs1vs1vs1"`, `"1vs2"`, `"2vs2"`, `"2vs2vs2"`, `"2vs2vs2vs2"`) | - | - |
| lobbies.0.bet | number | Ставка в золоте (кг) | - | - | - |
| lobbies.0.password | string | Пароль для лобби | - | - | - |
| lobbies.0.deals | boolean | "Без сделок" | - | - | - |
| lobbies.0.vip | boolean | "Без VIP-карт" | - | - | - |
| lobbies.0.started | boolean | Флаг который отвечает за то запущена ли игра в лобби или нет | - | - | - |
| lobbies.0.creator | object | Создатель лобби | - | - | **additional properties are NOT allowed** |
| lobbies.0.creator.userId | number | Идентификатор игрока | - | - | - |
| lobbies.0.creator.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobbies.0.viewers | array<any> | Массив со зрителями | - | - | - |
| lobbies.0.viewers.userId | number | Идентификатор игрока | - | - | - |
| lobbies.0.viewers.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobbies.0.players | array<any> allOf | Массив с игроками | - | - | - |
| lobbies.0.players.0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| lobbies.0.players.0.userId | number | Идентификатор игрока | - | - | - |
| lobbies.0.players.0.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobbies.0.players.1 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| lobbies.0.players.1.currentCell | number | Клетка на которой сейчас находится игрок | - | - | - |
| lobbies.0.currentPlayerIndex | number | Индекс (из переданого массива) игрока который ходит в данный момент | - | - | - |
| lobbies.0.totalTurns | number | Количество ходов за всю игру | - | - | - |
| lobbies.0.startGameTime | string | Дата когда была начата игра | - | format (`date`) | - |
| lobbies.1 (allOf item) | object | - | - | - | **additional properties are allowed** |
| lobbies.1.isFull | number | Флаг который показывает полное лобби или нет | - | - | - |

> Examples of payload _(generated)_

```json
{
  "lobbies": {
    "id": 0,
    "name": "string",
    "fieldType": "standard",
    "gameType": "1vs1vs1vs1vs1",
    "bet": 0,
    "password": "string",
    "deals": true,
    "vip": true,
    "started": true,
    "creator": {
      "userId": 0,
      "userSocketId": 0
    },
    "viewers": [],
    "players": {
      "userId": 0,
      "userSocketId": 0,
      "currentCell": 0
    },
    "currentPlayerIndex": 0,
    "totalTurns": 0,
    "startGameTime": "2019-08-24",
    "isFull": 0
  }
}
```



### PUB `lobby.get.user` Operation

*Получение лобби текущего юзера*

Получение лобби текущего юзера (ответ приходит на <a href="#lobby.get.user.result">lobby.get.user.result</a>)


### PUB `lobby.game.dice` Operation

*Бросок игральных костей*

Имитация броска игральных костей, необходимо заэмитить этот эвент во время хода соответствующего игрока <br> Результат броска костей будет передан всем находящимся в лобби на эвент <a href="#lobby.game.dice.result">lobby.game.dice.result</a>


### PUB `lobby.game.buy` Operation

*Покупка собственности (Кнопка купить)*

Покупка собственности (если это возможно на текущий ход)


### PUB `lobby.game.skip` Operation

*Отказ (Кпопка отказать)*

Отказ от покупки собственности


### PUB `lobby.game.pay` Operation

*Оплата (Кнопка оплатить)*

Оплата штрафа/таможни/налога






