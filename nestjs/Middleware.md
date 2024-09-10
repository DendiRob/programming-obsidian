![[../imgs/Pasted image 20240910090242.png]]Что может миддлвар: 
* Выполнять любой код
* Изменять объекты request/response
* Прерывать  request-response цикл
* Вызывать функцию next()
* Если текущий мидлвар не заканчивает цикл request-response, то нужно передать функцию next(), иначе цикл не завершится
Чтобы создать мидлвар вы должны использовать @Injectable декоратор и имплементировать класс миддлвара от NestMiddleware

>[!warning]
>`Express` and `fastify` handle middleware differently and provide different method signatures, read more [here](https://docs.nestjs.com/techniques/performance#middleware).

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log('Request...');
    next();
  }
}
```

Используем миддлвар
Так как в модуле у нас нет слота для миддлваров, то мы внедрям в класс модуля, используем configure() метод
```typescript
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('cats');
  }
}
```
В приведенном выше примере мы настроили программное обеспечение LoggerMiddleware для обработчиков маршрутов /cats, которые ранее были определены внутри CatsController. Мы также можем дополнительно ограничить промежуточное программное обеспечение определенным методом запроса, передав объект, содержащий путь маршрута и метод запроса, методу forRoutes() при настройке промежуточного программного обеспечения. В приведенном ниже примере обратите внимание, что мы импортируем перечисление RequestMethod для ссылки на желаемый тип метода запроса.

```typescript
import { Module, NestModule, RequestMethod, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes({ path: 'cats', method: RequestMethod.GET });
  }
}
```

>[!info]
>The `configure()` method can be made asynchronous using `async/await` (e.g., you can `await` completion of an asynchronous operation inside the `configure()` method body).

>[!warning]
>When using the `express` adapter, the NestJS app will register `json` and `urlencoded` from the package `body-parser` by default. This means if you want to customize that middleware via the `MiddlewareConsumer`, you need to turn off the global middleware by setting the `bodyParser` flag to `false` when creating the application with `NestFactory.create()`.

![[../imgs/Pasted image 20240910092108.png]]
#### Middleware consumer
MiddlewareConsumer - это вспомогательный класс. Он предоставляет несколько встроенных методов для управления промежуточным программным обеспечением. Все они могут быть просто объединены в цепочку в стиле fluent. Метод forRoutes() может использовать одну строку, несколько строк, объект RouteInfo, класс контроллера и даже несколько классов контроллеров. В большинстве случаев вы, вероятно, просто передадите список контроллеров, разделенных запятыми. Ниже приведен пример с одним контроллером (но можно больше):
```typescript
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';
import { CatsController } from './cats/cats.controller';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes(CatsController);
  }
}
```
Некоторые пути можно исключить из под воздействия мидлвара:

```typescript
consumer
  .apply(LoggerMiddleware)
  .exclude( //исключаем пути ниже
    { path: 'cats', method: RequestMethod.GET },
    { path: 'cats', method: RequestMethod.POST },
    'cats/(.*)',
  )
  .forRoutes(CatsController);
```

#### Functional middleware
>[!info]
>Использовать вместо классового middleware в любом случае, где у middleware нет зависимостей и тд и тп.

```typescript
import { Request, Response, NextFunction } from 'express';

export function logger(req: Request, res: Response, next: NextFunction) {
  console.log(`Request...`);
  next();
};
```

```typescript
consumer
  .apply(logger)
  .forRoutes(CatsController);
```

#### Multiple middleware
```typescript
consumer.apply(cors(), helmet(), logger).forRoutes(CatsController);
```

#### Global middleware
If we want to bind middleware to every registered route at once, we can use the `use()` method that is supplied by the `INestApplication` instance:
```typescript
const app = await NestFactory.create(AppModule);
app.use(logger); // только функциональный мидлвар
await app.listen(3000);
```

>[!hint]
>Accessing the DI container in a global middleware is not possible. You can use a [functional middleware](https://docs.nestjs.com/middleware#functional-middleware) instead when using `app.use()`. Alternatively, you can use a class middleware and consume it with `.forRoutes('*')` within the `AppModule` (or any other module).


