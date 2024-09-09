>[!info]
>Модуль - это класс, аннотированный с помощью декоратора @Module(). Декоратор @Module() предоставляет метаданные, которые Nest использует для организации структуры приложения.

![[../imgs/Pasted image 20240909094352.png]]
Используем сервисы и провайдеры там, где это необходимо и логически более правильно
```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

Регистрируем CatsModule в рутовом
```typescript
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```
![[../imgs/Pasted image 20240909094910.png]]
Так же мы можем делится модулем, если вдруг его функционал необходим в другом месте приложения
```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService] // делимся
})
export class CatsModule {}
```
Now any module that imports the `CatsModule` has access to the `CatsService` and will share the same instance with all other modules that import it as well.

Модули также могут импортироваться и экспортироваться
```typescript
@Module({
  imports: [CommonModule],
  exports: [CommonModule],
})
export class CoreModule {}
```
