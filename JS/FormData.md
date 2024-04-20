>[!info]
>The **`FormData`** interface provides a way to construct a set of key/value pairs representing form fields and their values, which can be sent using the [`fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/fetch), [`XMLHttpRequest.send()`](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/send) or [`navigator.sendBeacon()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon) methods. It uses the same format a form would use if the encoding type were set to `"multipart/form-data"`.

Конструктор:

```JS
let formData = new FormData([form]);
```

Если передать в конструктор элемент HTML-формы `form`, то создаваемый объект автоматически прочитает из неё поля.

Его особенность заключается в том, что методы для работы с сетью, например `fetch`, позволяют указать объект `FormData` в свойстве тела запроса `body`.

Он будет соответствующим образом закодирован и отправлен с заголовком `Content-Type: multipart/form-data`.

То есть, для сервера это выглядит как обычная отправка формы.

## Методы объекта FormData

- `formData.append(name, value)` – добавляет к объекту поле с именем `name` и значением `value`,
- `formData.append(name, blob, fileName)` – добавляет поле, как будто в форме имеется элемент `<input type="file">`, третий аргумент `fileName` устанавливает имя файла (не имя поля формы), как будто это имя из файловой системы пользователя,
- `formData.delete(name)` – удаляет поле с заданным именем `name`,
- `formData.get(name)` – получает значение поля с именем `name`,
- `formData.has(name)` – если существует поле с именем `name`, то возвращает `true`, иначе `false`
>[!warning]
>Технически форма может иметь много полей с одним и тем же именем `name`, поэтому несколько вызовов `append` добавят несколько полей с одинаковыми именами.
Ещё существует метод `set`, его синтаксис такой же, как у `append`. Разница в том, что `.set` удаляет все уже имеющиеся поля с именем `name` и только затем добавляет новое.

То есть этот метод гарантирует, что будет существовать только одно поле с именем `name`, в остальном он аналогичен `.append`:
- `formData.set(name, value)`,
- `formData.set(name, blob, fileName)`.

Поля объекта `formData` можно перебирать, используя цикл `for..of`: