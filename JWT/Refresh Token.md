## Виды токенов
- **Токены доступа (JWT)** — это токены, с помощью которых можно получить доступ к защищенным ресурсам. Они **короткоживущие**, но **многоразовые**. В них может содержаться дополнительная информация, например, время жизни или `IP`-адрес, откуда идет запрос. Все зависит от желания разработчика.
- **Рефреш токен (RT)** — эти токены выполняют только одну специфичную задачу — получение нового токена доступа. И на этот раз без сервера авторизации не обойтись. Они **долгоживущие**, но **одноразовые**.

Основной сценарий использования такой: как только старый `JWT` истекает, то с ним мы уже не можем получить приватные данные, тогда отправляем `RT` и нам приходит новая пара `JWT+RT`. С новым `JWT` мы снова можем обращаться к приватным ресурсам. Конечно, рефреш токен тоже может протухнуть, но случится это не скоро, поскольку живет он намного дольше своего собрата.

Благодаря такому подходу мы уменьшаем задержку по времени обращения к серверу `latency`, да и сама серверная логика становится сильно проще. А с точки зрения безопасности, если у нас всё-таки украли токен доступа, то воспользоваться им смогут только ограниченное время — не больше времени его жизни. Чтобы злоумышленник смог пользоваться дольше — ему потребуется украсть еще и рефреш, но тогда настоящий пользователь узнает, что его взломали, поскольку его выкинет из системы. И стоит такому пользователю снова войти в систему, он получит обновленную пару `JWT+RT`, а украденные превратятся в тыкву.