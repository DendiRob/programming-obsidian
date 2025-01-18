Документация предлагает следующие действия, чтобы внедрить кэширование в NestJS приложение

## 1. Установка зависимостей

```bash
npm install @nestjs/cache-manager cache-manager
```

Изначально (default) все будет храниться в памяти приложения, но после внедрения [[keyv]] можно переключиться на более продвинутые решения, такие как [[../Redis|Redis]]

## 2. Регистрация дефолтно кэширование 

```typescript
import { Module } from '@nestjs/common';
import { CacheModule } from '@nestjs/cache-manager';
import { AppController } from './app.controller';

@Module({
  imports: [CacheModule.register()],
  controllers: [AppController],
})
export class AppModule {}
```

>[!info]
>Такая инициализация позволит запустить кэширование с дефолтными настройками и сразу же использовать его

## Как взаимодействовать с кэшированием

```typescript
constructor(@Inject(CACHE_MANAGER) private cacheManager: Cache) {}
```

>[!info]
>The `Cache` class is imported from the `cache-manager`, while `CACHE_MANAGER` token from the `@nestjs/cache-manager` package.

Применение

```typescript
const value = await this.cacheManager.get('key');
```

```typescript
await this.cacheManager.set('key', 'value');
```

>[!warning]
>Кэширование в памяти приложения(дефолтное решение) может взаимодействовать только с определенными [типами данных](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm#javascript_types)

## Глобальное использование `CacheModule`

```typescript
CacheModule.register({
  isGlobal: true,
});
```

## Использование других хранилищ для кэша (Пример на Redis)

```bash
npm install @keyv/redis
```

Вообще можно регистрировать несколько хранилищ: 

```typescript
import { Module } from '@nestjs/common';
import { CacheModule, CacheStore } from '@nestjs/cache-manager';
import { AppController } from './app.controller';
import KeyvRedis from '@keyv/redis';
import { Keyv } from 'keyv';
import { CacheableMemory } from 'cacheable';

@Module({
  imports: [
    CacheModule.registerAsync({
      useFactory: async () => {
        return {
          stores: [
            new Keyv({
              store: new CacheableMemory({ ttl: 60000, lruSize: 5000 }),
            }),  
            new KeyvRedis('redis://localhost:6379'),
          ],
        };
      },
    }),
  ],
  controllers: [AppController],
})
export class AppModule {}
```

Первый указанный, используется по дефолту

>[!info]
>The `CacheableMemory` store is a simple in-memory store, while `KeyvRedis` is a Redis store. [cacheable](https://cacheable.org/)


