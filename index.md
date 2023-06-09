---
layout: lesson
root: .  # Це єдина сторінка, яка не відповідає шаблону /:path/index.html
permalink: index.html  # Це єдина сторінка, яка не відповідає шаблону /:path/index.html
---

Вольфман і Дракула були найняті Universal Missions (спінофф
космічних служб з Euphoric State University), щоб дослідити, чи 
можна відправити свій наступний планетарний посадковий модуль на Марс. Вони хочуть мати
можливість працювати над планами одночасно, але вони зіткнулися з проблемами, що роблять
це в минулому. Якщо вони будуть робити по черзі, кожен буде
витрачати багато часу, чекаючи, поки інший закінчить, але якщо вони працюють над
власними копіями та електронною поштою, зміни туди-сюди будуть втрачені, перезаписані
або продубльовані.

Колега пропонує використовувати [контроль версій]({{ page.root }}{% link reference.md %}#version-control), щоб 
керувати своєю роботою. Контроль версій краще, ніж розсилка файлів туди-сюди:

*   Ніщо, що прагне до контролю версій, ніколи не втрачається, якщо
    ви не працюєте дійсно, дуже важко. Оскільки всі старі версії файлів
    зберігаються, завжди можна повернутися в часі, щоб побачити, хто
    саме написав що в певний день, або яка версія програми була використана
    для створення певного набору результатів.

*   Оскільки ми маємо цей запис про те, хто зробив які зміни коли, ми знаємо, кого запитати,
    якщо у нас є питання пізніше, і, якщо потрібно, повернутися 
    vдо попередньої версії, так само, як функція "undo" в редакторі.

*   Коли кілька людей співпрацюють в одному проєкті, можна випадково
    пропустити або перезаписати чиїсь зміни. Система контролю версій
    автоматично повідомляє користувачів, коли виникає конфлікт між роботою
    однієї людини та іншою..

Команди не єдині, хто отримує вигоду від контролю версій: самотні
дослідники можуть отримати величезну користь. Ведення обліку
того, що було змінено, коли і чому надзвичайно корисно для всіх дослідників, якщо їм
коли-небудь потрібно повернутися до проєкту пізніше (наприклад, через рік,
коли пам'ять зникла).

Контроль версій - це лабораторний ноутбук цифрового світу: це те, що
професіонали використовують, щоб стежити за тим, що вони зробили і щоб
співпрацювати з іншими людьми. Кожен великий проєкт розробки
програмного забезпечення покладається на нього, і більшість програмістів використовують його для
своїх невеликих робочих місць. І це не тільки для програмного забезпечення: книги, статті, невеликі набори даних, і 
все, що змінюється з плином часу або потребує спільного використання,
може і має зберігатися в системі контролю версій.

> ## Передумови
>
> У цьому уроці ми використовуємо Git з Unix Shell.
> SОчікується деякий попередній досвід роботи з shell,
> *але це не є обов'язковим*.
{: .prereq}

