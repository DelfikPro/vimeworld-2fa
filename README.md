Бот для восстановления аккаунтов с утерянными кодами TOTP

# Лазейка закрыта 4 января 2020 года.

# Как это работало до патча?
Код для двухэтапной авторизации состоит из шести цифр - 1 000 000 вариантов

Новый код появляется раз в 30 секунд, при этом каждый код действует ~3 минуты,

это сделано из-за потенциальных погрешностей в измерении времени между серверами вайма и клиентами, а ещё из-за потенциальных рукожопов, которые не успеют ввести код

Имеем 6 кодов которые действуют одновременно, шанс 1 к 166 666 угадать

Программа получает на вход куки `PHPSESSID`, перебирает коды запросами к `cp.vimeworld.ru/login_2fa`, получая в ответ код 302 (перенаправление)

Если заголовок `location` редиректа ведёт обратно на `login_2fa`, значит код неверный

Если заголовок `location` ведёт на `index`, то вход в аккаунт успешно выполнен, теперь нужно отключить двухэтапку

Двухэтапка отключается запросом к `cp.vimeworld.ru/ajax/security.php`, в котором нужно указать валидный код для подтверждения

Поскольку роботы правят миром, всё делается почти мгновенно, и код, использованный для входа в аккаунт, чаще всего подходит и для отключения двухэтапки

У всех запросов к `cp.vimeworld.ru/ajax/security.php` есть контрольный параметр `crsf-token`, который приходит пользователю вместе с html-страницей `cp.vimeworld.ru/security`.

Неизвестно, принёс ли этот CRSF какую-то безопасность, ведь если захватить куки, то можно захватить и security.html

Во всяком случае, эта проверка не мешает успешно отключать двухэтапку и радовать людей аккаунтами

Вроде как у вайма есть защита от слишком частых запросов (чаще чем 200мс кидать не выходит), этого недостаточно для того чтобы сделать двухэтапку пуленепробиваемой, но бесит, поэтому боты юзают кучу проксей и кидают запросы с 300 адресов одновременно

Взломано более дюжины дорогостоящих аккаунтов, которые группа техподдержки не хочет восстанавливать (нарушения правил сервера)
