# Websocket API 0.0.1 documentation

При работе с документацией нужно учитывать, что на эвенты с маркировкой SUB необходимо подписываться,
а эвенты с маркировкой PUB необходимо emit-ить.
<hr>
Примерный flow работы с WS:<br>
<ol>
  <li><h4><a href="#lobby.create">lobby.create</a></h4> - клиент создает лобби на выходе получает его <strong>id</strong></li>
  <li><h4><a href="#lobby.join">lobby.join</a></h4> - клиенты могут подключиться к лобби по его <strong>id</strong></li>
  <li><h4><a href="#lobby.message.send">lobby.message.send</a></h4> - клиент может отправить любой payload в свое лобби (при этом все получетели обязаны быть подписаны на эвент <strong><a href="#lobby.message.get">lobby.message.get</a></strong>)</li>
  <li><h4><a href="#lobby.leave">lobby.leave</a></h4> - клиент может выйти из лобби</li>
</ol>
<hr>
<p> Для получения ошибок необходимо подписаться на эвент <strong><a href="#lobby.error">lobby.error</a></strong></P>
<p> Для получения успешных ответов (например после создания лобби) необходимо подписаться на эвент <strong><a href="#lobby.message">lobby.message</a></strong></P>


## Table of Contents

* [Servers](#servers)
  * [development](#development-server)
* [Operations](#operations)
  * [PUB lobby.create](#pub-lobbycreate-operation)
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

## Servers

### `development` Server

* URL: `localhost:port`
* Protocol: `websocket`

При эмите каждого эвента в <strong>headers.authorization</strong> необходимо передавать токен для авторизации - <strong>Bearer {token}</strong>

#### Security

##### Security Requirement 1

* Type: `HTTP`
  * Scheme: bearer
  * Bearer format: JWT







## Operations

### PUB `lobby.create` Operation

*Создание лобби*

<h1 id="lobby.create"></h1>

#### Message `createLobbyData`

*Тело эвента для создания лобби*

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are NOT allowed** |
| name | string | Название лобби, если оно не будет передано ему будет присвоено значение юзернейма. | - | - | - |
| fieldType | string | Тип поля, временно доступно только стандартное поле (standard) | allowed (`"standard"`, `"big"`, `"huge"`) | - | **required** |
| gameType | string | - | allowed (`"1vs1vs1vs1vs1"`, `"1vs2"`, `"2vs2"`, `"2vs2vs2"`, `"2vs2vs2vs2"`) | - | **required** |
| bet | number | Ставка в золоте (кг) | - | - | **required** |
| password | string | Пароль для лобби | - | - | - |
| deals | boolean | "Без сделок" | - | - | - |
| vip | boolean | "Без VIP-карт" | - | - | - |

> Examples of payload _(generated)_

```json
{
  "name": "string",
  "fieldType": "standard",
  "gameType": "1vs1vs1vs1vs1",
  "bet": 0,
  "password": "string",
  "deals": true,
  "vip": true
}
```



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


### SUB `lobby.message.get` Operation

*Получение payload-a от клиента в лобби*

Для получение payload-a необходимо подписаться на эвент. <h1 id="lobby.message.get"></h1>

#### Message `anyData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | Payload может быть любым | - | - | **additional properties are allowed** |

> Examples of payload _(generated)_

```json
{}
```



### SUB `lobby.get.user.result` Operation

*Получение лобби текущего юзера*

Получение лобби текущего юзера <h1 id="lobby.get.user.result"></h1>

#### Message `<anonymous-message-12>`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are allowed** |
| lobby | object | - | - | - | **additional properties are NOT allowed** |
| lobby.id | number | Идентификатор лобби | - | - | - |
| lobby.name | string | Название лобби | - | - | - |
| lobby.fieldType | string | Тип поля, временно доступно только стандартное поле (standard) | allowed (`"standard"`, `"big"`, `"huge"`) | - | - |
| lobby.gameType | string | - | allowed (`"1vs1vs1vs1vs1"`, `"1vs2"`, `"2vs2"`, `"2vs2vs2"`, `"2vs2vs2vs2"`) | - | - |
| lobby.bet | number | Ставка в золоте (кг) | - | - | - |
| lobby.password | string | Пароль для лобби | - | - | - |
| lobby.deals | boolean | "Без сделок" | - | - | - |
| lobby.vip | boolean | "Без VIP-карт" | - | - | - |
| lobby.started | boolean | Флаг который отвечает за то запущена ли игра в лобби или нет | - | - | - |
| lobby.creator | object | Создатель лобби | - | - | **additional properties are NOT allowed** |
| lobby.creator.userId | number | Идентификатор игрока | - | - | - |
| lobby.creator.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobby.viewers | array<any> | Массив со зрителями | - | - | - |
| lobby.viewers.userId | number | Идентификатор игрока | - | - | - |
| lobby.viewers.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobby.players | array<any> allOf | Массив с игроками | - | - | - |
| lobby.players.0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| lobby.players.0.userId | number | Идентификатор игрока | - | - | - |
| lobby.players.0.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobby.players.1 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| lobby.players.1.currentCell | number | Клетка на которой сейчас находится игрок | - | - | - |
| lobby.currentPlayerIndex | number | Индекс (из переданого массива) игрока который ходит в данный момент | - | - | - |
| lobby.totalTurns | number | Количество ходов за всю игру | - | - | - |
| lobby.startGameTime | string | Дата когда была начата игра | - | format (`date`) | - |

> Examples of payload _(generated)_

```json
{
  "lobby": {
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
    "startGameTime": "2019-08-24"
  }
}
```



### SUB `lobby.refresh` Operation

*Получение актуального списка лобби*

Эвент вызывается когда происходит какое либо изменение в лобби, оповещает всех пользователей (которые не лобби)

#### Message `refreshData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | Массив с лобби | - | - | **additional properties are allowed** |
| lobbies | object | - | - | - | **additional properties are NOT allowed** |
| lobbies.id | number | Идентификатор лобби | - | - | - |
| lobbies.name | string | Название лобби | - | - | - |
| lobbies.fieldType | string | Тип поля, временно доступно только стандартное поле (standard) | allowed (`"standard"`, `"big"`, `"huge"`) | - | - |
| lobbies.gameType | string | - | allowed (`"1vs1vs1vs1vs1"`, `"1vs2"`, `"2vs2"`, `"2vs2vs2"`, `"2vs2vs2vs2"`) | - | - |
| lobbies.bet | number | Ставка в золоте (кг) | - | - | - |
| lobbies.password | string | Пароль для лобби | - | - | - |
| lobbies.deals | boolean | "Без сделок" | - | - | - |
| lobbies.vip | boolean | "Без VIP-карт" | - | - | - |
| lobbies.started | boolean | Флаг который отвечает за то запущена ли игра в лобби или нет | - | - | - |
| lobbies.creator | object | Создатель лобби | - | - | **additional properties are NOT allowed** |
| lobbies.creator.userId | number | Идентификатор игрока | - | - | - |
| lobbies.creator.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobbies.viewers | array<any> | Массив со зрителями | - | - | - |
| lobbies.viewers.userId | number | Идентификатор игрока | - | - | - |
| lobbies.viewers.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobbies.players | array<any> allOf | Массив с игроками | - | - | - |
| lobbies.players.0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| lobbies.players.0.userId | number | Идентификатор игрока | - | - | - |
| lobbies.players.0.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobbies.players.1 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| lobbies.players.1.currentCell | number | Клетка на которой сейчас находится игрок | - | - | - |
| lobbies.currentPlayerIndex | number | Индекс (из переданого массива) игрока который ходит в данный момент | - | - | - |
| lobbies.totalTurns | number | Количество ходов за всю игру | - | - | - |
| lobbies.startGameTime | string | Дата когда была начата игра | - | format (`date`) | - |

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
    "startGameTime": "2019-08-24"
  }
}
```



### SUB `lobby.error` Operation

*Получение любой ошибки связанной с лобби*

Для получение ошибки необходимо подписаться на эвент. <h1 id="lobby.error"></h1>

#### Message `errorData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are allowed** |
| error | object | Тело ошибки | - | - | **additional properties are allowed** |

> Examples of payload _(generated)_

```json
{
  "error": {}
}
```



### SUB `lobby.message` Operation

*Получение любой успешной информации связанной с лобби*

Для получение информации необходимо подписаться на эвент. <h1 id="lobby.message"></h1>

#### Message `successData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are allowed** |
| data | object | Тело ответа | - | - | **additional properties are allowed** |

> Examples of payload _(generated)_

```json
{
  "data": {}
}
```



### SUB `lobby.user.join` Operation

*Эвент входжа в лобби*

Эвент вызывается когда юзер вошел в лобби


### SUB `lobby.user.leave` Operation

*Эвент выхода из лобби*

Эвент вызывается когда юзер вышел из лобби


### SUB `lobby.game.start` Operation

*Старт игры*

Этот эвент вызывается при старте игры

#### Message `getLobbyData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are allowed** |
| lobby | object | - | - | - | **additional properties are NOT allowed** |
| lobby.id | number | Идентификатор лобби | - | - | - |
| lobby.name | string | Название лобби | - | - | - |
| lobby.fieldType | string | Тип поля, временно доступно только стандартное поле (standard) | allowed (`"standard"`, `"big"`, `"huge"`) | - | - |
| lobby.gameType | string | - | allowed (`"1vs1vs1vs1vs1"`, `"1vs2"`, `"2vs2"`, `"2vs2vs2"`, `"2vs2vs2vs2"`) | - | - |
| lobby.bet | number | Ставка в золоте (кг) | - | - | - |
| lobby.password | string | Пароль для лобби | - | - | - |
| lobby.deals | boolean | "Без сделок" | - | - | - |
| lobby.vip | boolean | "Без VIP-карт" | - | - | - |
| lobby.started | boolean | Флаг который отвечает за то запущена ли игра в лобби или нет | - | - | - |
| lobby.creator | object | Создатель лобби | - | - | **additional properties are NOT allowed** |
| lobby.creator.userId | number | Идентификатор игрока | - | - | - |
| lobby.creator.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobby.viewers | array<any> | Массив со зрителями | - | - | - |
| lobby.viewers.userId | number | Идентификатор игрока | - | - | - |
| lobby.viewers.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobby.players | array<any> allOf | Массив с игроками | - | - | - |
| lobby.players.0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| lobby.players.0.userId | number | Идентификатор игрока | - | - | - |
| lobby.players.0.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| lobby.players.1 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| lobby.players.1.currentCell | number | Клетка на которой сейчас находится игрок | - | - | - |
| lobby.currentPlayerIndex | number | Индекс (из переданого массива) игрока который ходит в данный момент | - | - | - |
| lobby.totalTurns | number | Количество ходов за всю игру | - | - | - |
| lobby.startGameTime | string | Дата когда была начата игра | - | format (`date`) | - |

> Examples of payload _(generated)_

```json
{
  "lobby": {
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
    "startGameTime": "2019-08-24"
  }
}
```



### SUB `lobby.game.dice.result` Operation

*Результат броска кубиков*

Эвент вызывается когда какой либо из игроков кидает кубики <h1 id="lobby.game.dice.result"></h1>

#### Message `getDiceResult`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are allowed** |
| user | allOf | - | - | - | **additional properties are allowed** |
| user.0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| user.0.userId | number | Идентификатор игрока | - | - | - |
| user.0.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| user.1 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| user.1.currentCell | number | Клетка на которой сейчас находится игрок | - | - | - |
| first | number | Результат броска первого кубика | - | - | - |
| second | number | Результат броска второго кубика | - | - | - |
| total | number | Результат броска костей, число от 1 до 12 | - | - | - |

> Examples of payload _(generated)_

```json
{
  "user": {
    "userId": 0,
    "userSocketId": 0,
    "currentCell": 0
  },
  "first": 0,
  "second": 0,
  "total": 0
}
```



### SUB `lobby.game.player.next` Operation

*Передача хода другому игроку*

Эвент вызывается когда происходит передача ходу другому игроку

#### Message `switchPlayerData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object allOf | - | - | - | **additional properties are allowed** |
| 0 (allOf item) | allOf | - | - | - | **additional properties are allowed** |
| 0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| userId | number | Идентификатор игрока | - | - | - |
| userSocketId | number | Идентификатор сокета игрока | - | - | - |
| 1 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| 1.currentCell | number | Клетка на которой сейчас находится игрок | - | - | - |
| 1 (allOf item) | object | - | - | - | **additional properties are allowed** |
| 1.index | number | Индекс (из переданого массива) игрока | - | - | - |

> Examples of payload _(generated)_

```json
{
  "userId": 0,
  "userSocketId": 0,
  "currentCell": 0,
  "index": 0
}
```



### SUB `lobby.game.player.move` Operation

*Переход игрока на другую клетку*

Эвент вызывается когда игрок переходит на другую клетку

#### Message `nextPlayerData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are allowed** |
| player | object allOf | Игрок который перешел | - | - | **additional properties are allowed** |
| player.0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| player.0.userId | number | Идентификатор игрока | - | - | - |
| player.0.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| player.1 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| player.1.currentCell | number | Клетка на которой сейчас находится игрок | - | - | - |
| count | number | Количество клеток на которое игрок перешел | - | - | - |

> Examples of payload _(generated)_

```json
{
  "player": {
    "userId": 0,
    "userSocketId": 0,
    "currentCell": 0
  },
  "count": 0
}
```



### SUB `lobby.game.tile.message` Operation

*Попадание игрока на какую либо клетку*

Эвент вызывается когда игрок попадает на какую либо клетку

#### Message `tileMessageData`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are allowed** |
| message | string | Сообщение при переходе на клетку | - | - | - |
| allowed | array<any> | Разрешенные действия для игрока | - | - | - |

> Examples of payload _(generated)_

```json
{
  "message": "string",
  "allowed": []
}
```



### SUB `lobby.game.lose` Operation

*Проигрыш игрока*

Эвент вызывается когда какой либо игрок проигрывает (баланс < 0)

#### Message `<anonymous-message-23>`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are allowed** |
| player | allOf | - | - | - | **additional properties are allowed** |
| player.0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| player.0.userId | number | Идентификатор игрока | - | - | - |
| player.0.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| player.1 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| player.1.currentCell | number | Клетка на которой сейчас находится игрок | - | - | - |

> Examples of payload _(generated)_

```json
{
  "player": {
    "userId": 0,
    "userSocketId": 0,
    "currentCell": 0
  }
}
```



### SUB `lobby.game.win` Operation

*Победа игрока*

Эвент вызывается когда игрок выигрывает

#### Message `<anonymous-message-24>`

##### Payload

| Name | Type | Description | Value | Constraints | Notes |
|---|---|---|---|---|---|
| (root) | object | - | - | - | **additional properties are allowed** |
| player | allOf | - | - | - | **additional properties are allowed** |
| player.0 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| player.0.userId | number | Идентификатор игрока | - | - | - |
| player.0.userSocketId | number | Идентификатор сокета игрока | - | - | - |
| player.1 (allOf item) | object | - | - | - | **additional properties are NOT allowed** |
| player.1.currentCell | number | Клетка на которой сейчас находится игрок | - | - | - |

> Examples of payload _(generated)_

```json
{
  "player": {
    "userId": 0,
    "userSocketId": 0,
    "currentCell": 0
  }
}
```



