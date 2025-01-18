https://keyv.org/docs/

>[!info]  Для чего нужен Keyv
Keyv предоставляет согласованный интерфейс для хранения значений ключей в нескольких серверных системах с помощью адаптеров хранения. Он поддерживает истечение срока действия на основе TTL, что делает его подходящим в качестве кэша или постоянного хранилища значений ключей.

##  Install keyv

```bash
npm install --save keyv
```

По дефолту keyv все хранит в памяти, но он имеет адаптары для различных хранилищ 
```bash
npm install --save @keyv/redis
npm install --save @keyv/valkey
npm install --save @keyv/memcache
npm install --save @keyv/mongo
npm install --save @keyv/sqlite
npm install --save @keyv/postgres
npm install --save @keyv/mysql
npm install --save @keyv/etcd
```

## Create a New Keyv Instance

Нет необходимости явно указывать к каком хранилищу идет подключение, Keyv сам определяет нужное и подключается

```ts
// example Keyv instance that uses sqlite storage adapter
const keyv = new Keyv('sqlite://path/to/database.sqlite');
```

`Keyv` Parameters

|Parameter|Type|Required|Description|
|---|---|---|---|
|uri|String|N|The connection string URI. Merged into the options object as options.uri. Default value: undefined|
|options|Object|N|The options object is also passed through to the storage adapter. See the table below for a list of available options.|

`options` Parameters

| Parameter   | Type                     | Required | Description                                                                                     |
| ----------- | ------------------------ | -------- | ----------------------------------------------------------------------------------------------- |
| namespace   | String                   | N        | Namespace for the current instance. Default: 'keyv'                                             |
| ttl         | Number                   | N        | This is the default TTL, it can be overridden by specifying a TTL on .set(). Default: undefined |
| compression | @keyv/compress-          | N        | Compression package to use. See Compression for more details. Default: undefined.               |
| serialize   | Function                 | N        | A custom serialization function. Default: JSONB.stringify                                       |
| deserialize | Function                 | N        | A custom deserialization function. Default: JSONB.parse                                         |
| store       | Storage adapter instance | N        | The storage adapter instance to be used by Keyv. Default: new Map()                             |
| adapter     | String                   | N        | Specify an adapter to use. e.g 'redis' or 'mongodb'. Default: undefined                         |

## Create Some Key Value Pairs

Method: `set(key, value, [ttl])` - Set a value for a specified key.

|Parameter|Type|Required|Description|
|---|---|---|---|
|key|String|Y|Unique identifier which is used to look up the value. Keys are persistent by default.|
|value|Any|Y|Data value associated with the key|
|ttl|Number|N|Expiry time in milliseconds|

```ts
const keyv = new Keyv('redis://user:pass@localhost:6379'); 

// set a key value pair that expires in 1000 milliseconds
await keyv.set('foo', 'expires in 1 second', 1000); // true 

// set a key value pair that never expires
await keyv.set('bar', 'never expires'); // true
```

Method: `delete(key)` - Deletes an entry.

|Parameter|Type|Required|Description|
|---|---|---|---|
|key|String|Y|Unique identifier which is used to look up the value. Returns `true` if the key existed, `false` if not.|

```ts
// Delete the key value pair for the 'foo' key 
await keyv.delete('foo'); // true
```

## Advanced - Use Namespaces to Avoid Key Collisions


```ts
const users = new Keyv('redis://user:pass@localhost:6379', { namespace: 'users' }); 
const cache = new Keyv('redis://user:pass@localhost:6379', { namespace: 'cache' }); 

// Set a key-value pair using the key 'foo' in both namespaces 
await users.set('foo', 'users'); // returns true 
await cache.set('foo', 'cache'); // returns true 

// Retrieve a Value 
await users.get('foo'); // returns 'users' 
await cache.get('foo'); // returns 'cache' 

// Delete all values for the specified namespace 
await users.clear();
```

Также есть способность сжатия данных и файлов, все подробности на сайте
