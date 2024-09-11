Nest поставляется со встроенным уровнем исключений, который отвечает за обработку всех необработанных исключений в приложении. Когда исключение не обрабатывается кодом вашего приложения, оно перехватывается этим уровнем, который затем автоматически отправляет соответствующий удобный для пользователя ответ.

Обычно это действие выполняется встроенным глобальным фильтром исключений, который обрабатывает исключения типа HttpException (и его подклассы). Когда исключение не распознано (не является ни HttpException, ни классом, наследуемым от HttpException), встроенный фильтр исключений генерирует следующий JSON-ответ по умолчанию:
```json
{
  "statusCode": 500,
  "message": "Internal server error"
}
```

Стандартные исключения

>[!info]
>Nest обеспечивает класс `HttpException` из коробки, а именно из пакета @nestjs/common. Для HTTP REST/GraphQL API ориентированных приложений это является бест практис отправлять стандартные ответы на определенные исключение.

For example, in the `CatsController`, we have a `findAll()` method (a `GET` route handler). Let's assume that this route handler throws an exception for some reason. To demonstrate this, we'll hard-code it as follows:

```typescript
@Get()
async findAll() {
  throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
```
When the client calls this endpoint, the response looks like this:
```json
{
  "statusCode": 403,
  "message": "Forbidden"
}
```
The `HttpException` constructor takes two required arguments which determine the response:

- The `response` argument defines the JSON response body. It can be a `string` or an `object` as described below.
- The `status` argument defines the [HTTP status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status).

By default, the JSON response body contains two properties:

- `statusCode`: defaults to the HTTP status code provided in the `status` argument
- `message`: a short description of the HTTP error based on the `status`
## Кастомный ответ при исключении

```typescript
@Get()
async findAll() {
  try {
    await this.service.findAll()
  } catch (error) {
    throw new HttpException({ // Custom response
      status: HttpStatus.FORBIDDEN,
      error: 'This is a custom message',
    }, HttpStatus.FORBIDDEN, {
      cause: error
    });
  }
}
```
Чтобы переопределить только часть текста ответа в формате JSON, содержащую сообщение, укажите строку в аргументе response. Чтобы переопределить весь текст ответа в формате JSON, передайте объект в аргументе response. Nest сериализует объект и вернет его как текст ответа в формате JSON.  
  
Второй аргумент конструктора - status - должен быть допустимым кодом состояния HTTP. Рекомендуется использовать перечисление HttpStatus, импортированное из @nestjs/common.
Существует третий аргумент конструктора (необязательный) - options, который можно использовать для указания причины ошибки. Этот объект cause не преобразуется в объект response, но он может быть полезен для ведения журнала, предоставляя ценную информацию о внутренней ошибке, которая вызвала исключение HttpException.  

Вот пример переопределения всего текста ответа и указания причины ошибки:
```typescript
@Get()
async findAll() {
  try {
    await this.service.findAll()
  } catch (error) {
    throw new HttpException({
      status: HttpStatus.FORBIDDEN,
      error: 'This is a custom message',
    }, HttpStatus.FORBIDDEN, {
      cause: error
    });
  }
}
```
Using the above, this is how the response would look:

```json

{
  "status": 403,
  "error": "This is a custom message"
}
```

Кастомное исключение
Если вам все же нужно создать настраиваемые исключения, рекомендуется создать свою собственную иерархию исключений, в которой ваши пользовательские исключения наследуются от базового класса HttpException. При таком подходе Nest распознает ваши исключения и автоматически позаботится об ответах на ошибки. Давайте реализуем такое пользовательское исключение:

```typescript
export class ForbiddenException extends HttpException {
  constructor() {
    super('Forbidden', HttpStatus.FORBIDDEN);
  }
}
```
## Применение 
Поскольку ForbiddenException расширяет базовое HttpException, оно будет легко работать со встроенным обработчиком исключений, и поэтому мы можем использовать его внутри метода findAll().
```typescript
@Get()
async findAll() {
  throw new ForbiddenException();
}
```

![[../imgs/Pasted image 20240911092403.png]]Все встроенные исключения также могут содержать как причину ошибки, так и ее описание с помощью параметра options:
```typescript
throw new BadRequestException('Something bad happened', { cause: new Error(), description: 'Some error description' })
```
```json
{
  "message": "Something bad happened",
  "error": "Some error description",
  "statusCode": 400,
}
```

## Фильтры исключений
Хотя базовый (встроенный) фильтр исключений может автоматически обрабатывать многие случаи, вам может потребоваться полный контроль над уровнем исключений. Например, вы можете захотеть добавить ведение журнала или использовать другую схему JSON, основанную на некоторых динамических факторах. Фильтры исключений предназначены именно для этой цели. Они позволяют вам точно контролировать поток управления и содержание ответа, отправляемого клиенту.
Давайте создадим фильтр исключений, который отвечает за перехват исключений, которые являются экземпляром класса HttpException, и реализацию пользовательской логики ответа для них. Для этого нам понадобится доступ к базовым объектам platform Request и Response. Мы получим доступ к объекту Request, чтобы получить исходный URL-адрес и включить его в информацию журнала. Мы будем использовать объект Response для непосредственного управления отправляемым ответом, используя метод response.json().
```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
      });
  }
}
```
>[!hint]
>All exception filters should implement the generic `ExceptionFilter<T>` interface. This requires you to provide the `catch(exception: T, host: ArgumentsHost)` method with its indicated signature. `T` indicates the type of the exception.

## Использование фильтров
```typescript
@Post()
@UseFilters(new HttpExceptionFilter())
async create(@Body() createCatDto: CreateCatDto) {
  throw new ForbiddenException();
}
```

>[!warning]
>We have used the `@UseFilters()` decorator here. Similar to the `@Catch()` decorator, it can take a single filter instance, or a comma-separated list of filter instances. Here, we created the instance of `HttpExceptionFilter` in place. Alternatively, you may pass the class (instead of an instance), leaving responsibility for instantiation to the framework, and enabling **dependency injection**.

```typescript
@Post()
@UseFilters(HttpExceptionFilter)
async create(@Body() createCatDto: CreateCatDto) {
  throw new ForbiddenException();
}
```

>[!hint]
>Prefer applying filters by using classes instead of instances when possible. It reduces **memory usage** since Nest can easily reuse instances of the same class across your entire module.

##  Использование фильтра для целого контроллера и глобально
In the example above, the `HttpExceptionFilter` is applied only to the single `create()` route handler, making it method-scoped. Exception filters can be scoped at different levels: method-scoped of the controller/resolver/gateway, controller-scoped, or global-scoped. For example, to set up a filter as controller-scoped, you would do the following:
```typescript
@UseFilters(new HttpExceptionFilter())
export class CatsController {}
```

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalFilters(new HttpExceptionFilter());
  await app.listen(3000);
}
bootstrap();
```
