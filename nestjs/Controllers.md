>[!info]
>Контроллеры - принимают запросы и отдают ответы клиенту

* У каждого ендпоинта есть свои контроллер, который выполняет определенные действия(мапит данные, обращается в сервисы и тд и тп)
* Как правило для создания контроллера в nest используются классы и декораторы к ним
```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('cats') //представим,что рутовый контроллер /api
export class CatsController {
  @Get() // endpoint для получения всех котов https://yoururl:port/api/cats 
  findAll(): string {
    return 'This action returns all cats';
  }
  
@Get(':id') // endpoint для получения конкретного кота https://yoururl:port/api/cats/denni 
findOne(@Param() params: any): string {
  console.log(params.id);
  return `This action returns a #${params.id} cat`;
}

  @Post()
  create(): string {
    return 'This action adds a new cat';
  }
}
```

* @Controller('path') - декоратор для контроллера и его пути
* @Get('path') - декоратор для гет запроса (так же может указывать путь, но тут обращение к пути контроллера get /cats) (есть декораторы всех http методов (post,get,put и тд))
* @Req() - декоратор для получения данных запроса
* @Body(key?: string) - декоратор для получения body
![[../imgs/request decorators.png]]
>[!warning]
> __То есть когда мы используем Res, то мы сами обязываем ответить res.send и тд, а так это делает за на библиотека__
> For compatibility with typings across underlying HTTP platforms (e.g., Express and Fastify), Nest provides `@Res()` and `@Response()` decorators. `@Res()` is simply an alias for `@Response()`. Both directly expose the underlying native platform `response` object interface. When using them, you should also import the typings for the underlying library (e.g., `@types/express`) to take full advantage. Note that when you inject either `@Res()` or `@Response()` in a method handler, you put Nest into **Library-specific mode** for that handler, and you become responsible for managing the response. When doing so, you must issue some kind of response by making a call on the `response` object (e.g., `res.json(...)` or `res.send(...)`), or the HTTP server will hang.


* Также поддерживаются маршруты, основанные на шаблонах. Например, звездочка используется в качестве подстановочного знака и будет соответствовать любой комбинации символов.
```typescript
@Get('ab*cd')
findAll() {
  return 'This route uses a wildcard';
}
```

>[!warning]
>wildcard in the middle of the route is only supported by express.

#### Status code

As mentioned, the response **status code** is always **200** by default, except for POST requests which are **201**. We can easily change this behavior by adding the `@HttpCode(...)` decorator at a handler level.
```typescript
@Post()
@HttpCode(204) //Изменяем деволтный ответ на тот, который нам нужен
create() {
  return 'This action adds a new cat';
}
```

#### Headers[#](https://docs.nestjs.com/controllers#headers)

To specify a custom response header, you can either use a `@Header()` decorator or a library-specific response object (and call `res.header()` directly).

```typescript

@Post()
@Header('Cache-Control', 'none')
create() {
  return 'This action adds a new cat';
}
```

#### Redirection[#](https://docs.nestjs.com/controllers#redirection)

To redirect a response to a specific URL, you can either use a `@Redirect()` decorator or a library-specific response object (and call `res.redirect()` directly).

`@Redirect()` takes two arguments, `url` and `statusCode`, both are optional. The default value of `statusCode` is `302` (`Found`) if omitted.

```typescript

@Get()
@Redirect('https://nestjs.com', 301)
```

>[!info] 
>Далее записываем наш контроллер в необходимый нам модуль, чтобы nest смог его определить

```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';

@Module({
  controllers: [CatsController],
})
export class AppModule {}
```