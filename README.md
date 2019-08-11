# Не доделано:

- База данных работает отдельно от основного клиента.
- Модели должны автоматически читаться из директории как и команды и ивенты.
- Команда `help` должна быть изначально.
- `Event.unload()`.
- `Event.reload()`.
- `Command.unload()`.
- `Command.reload()`.
- Кулдаун для команд.
- Permissions Level для команд.
- И еще много всего.

## Examples

`index.js` structure:

```js
const { Core } = require('discore.js');
new Core({
  // options
}).login('token');
```

- Alternate login system

```js
const { Core } = require('discore.js');
new Core({
  token: 'token',
});
```

- All options (defaults)

```js
const { Core } = require('discore.js');
new Core({
  typing: false,
  eventsFolder: 'events',
  commandsFolder: 'commands',
  token: null,
  prefix: undefined,
  splitArgs: ' ',
  cmdsIn: ['text'],
  ignoreCase: true,
  cmdLowerCase: true,
});
```

##### Methods:

- uniqid.gen() // Generates unique identificator

#### Events

Events are placed in `.\events\`(**eventsFolder** option).
For instance creating `.\events\Main\ready.js` will be an event `ready` in the `Main` category. Subcategories are also allowed and gonna be a second and more folder levels.

Their structure (options argument defined with default configuration):

```js
const { Event } = require('discore.js');

module.exports = class extends Event {
  get options() {
    return {
      enabled: true,
      key: null, // Same as name but more important.
      name: null, // Key is going to be event name.
      once: false,
    };
    // If key and name are null then they will be defined as file name.
    // For example, ready.js is gonna be 'ready'
  }

  run(...params) {
    // Event code
  }

  init() {
    // Optional method. Runs on 'ready'
    // event so you are able to use discord
    // data via this.client.
  }
};
```

##### Methods:

- `toggle()`
- `enable()`
- `disable()`
- `unload()`
- `reload()`
- `toString()`

##### Included events:

- Command Handler

#### Commands

Commands are placed in `.\commands\` (**commandsFolder** option).
For instance creating `.\events\Main\Command.js` will be a command `Command` in the `Main` category. Subcategories are also allowed and gonna be a second and more folder levels.

Their structure (options argument defined with default configuration):

```js
const { Command } = require('discore.js');

module.exports = class extends Command {
  get options() {
    return {
      enabled: true,
      key: null, // Same as name but more important.
      name: null, // Key is going to be command name.
      cooldown: 0, // In milliseconds
      aliases: [],
      permLevel: 0,
      description: undefined,
    };
    // If key and name are null then they will be defined as file name.
    // For example, test.js is gonna be 'test'
  }

  run(message, args) {
    // Command code
  }

  init() {
    // Optional method. Runs on 'ready'
    // event so you are able to use discord
    // data via this.client.
  }
};
```

##### Methods:

- `toggle()`
- `enable()`
- `disable()`
- `unload()`
- `reload()`
- `toString()`

#### Database

DB's structure (options argument defined with default configuration):

```js
const { Core, DB } = require('discore.js');

const client = new Core({
  /* ... */
});
const db = new DB('url', {
  /* Options */
});

client.db = db;
```

##### Methods:

- addModel()

#### Models ( DB )

Their structure (options argument defined with default configuration):

```js
// Must define all default properties.
// You can leave properties as undefined.
const data = {
  id: { type: String, default: undefined },
  messageCount: { type: Number, default: 0 },
};

// Model name can not contain spaces in it
db.addModel('model_name', data);
```

##### Methods:

- hasOne()
- findOne()
- insertOne()
- updateOne()
- upsertOne()

###### hasOne()

```js
// Working with model from previus example.
// You can use `db['model_name']`

// Searches for document with `id` of '123'.
let res1 = db.model_name.hasOne({ id: '123' });
let res2 = db.model_name.hasOne('id', '123'); // Same.
let res3 = db.model_name.hasOne(val => val.id === '123'); // Same.

console.log(typeof res); // Returns true or false (Boolean).
console.log(typeof res2); // Same.
console.log(typeof res3); // Same.
```

###### findOne()

```js
// Working with model from previus example.
// You can use `db['model_name']`
let res1 = db.model_name.findOne({ id: '123' }); // Searches for document with `id` of '123'.
let res2 = db.model_name.findOne('id', '123'); // Same.
let res3 = db.model_name.findOne(val => val.id === '123'); // Same.

/*
  Returns document. If there is no document
  then you will getdefault settings which
  were defined by yourself.

  That means you can not get undefined or
  null.
*/
console.log(typeof res);
console.log(typeof res2); // Same.
console.log(typeof res3); // Same.
```

###### insertOne()

```js
// **upsertOne() method is recommended to use!**
db.model_name.insertOne({
  id: '3213',
  messageCount: 1, // If not defined, going to be 0.
});
```

###### updateOne()

```js
// **upsertOne() method is recommended to use!**

/*
  All of these examples are going to search
  for `id` of '3213' and update 
*/
db.model_name.updateOne({ id: '3213' }, { messageCount: 2 });
db.model_name.updateOne('id', '3212', { messageCount: 2 });
db.model_name.updateOne(val => val.id === '3212', { messageCount: 2 });
```

###### upsertOne()

```js
/*
  upsertOne() method is trying to update
  a document. If document is not exists then
  is going to insert it.
*/

// All of these examples are going to search
// for `id` of '3213' and update
// messageCount to 2.
db.model_name.updateOne({ id: '3213' }, { messageCount: 2 });
db.model_name.updateOne('id', '3212', { messageCount: 2 });
db.model_name.updateOne(val => val.id === '3212', { messageCount: 2 });
```
