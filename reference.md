---
layout: reference
---

## Шпаргалки по Git'у для швидкого ознайомлення

*   Шпаргалки з Git'у кількома мовами [доступні тут](https://github.github.com/training-kit/) ([Українська версія](https://training.github.com/downloads/ua/github-git-cheat-sheet/)). Більше матеріалів можна знайти за посиланням [Навчальний сайт GitHub](http://try.github.io/).
*   [Інтерактивна односторінкова візуалізація](http://ndpsoftware.com/git-cheatsheet.html)
    про взаємозв'язки між робочим простором, зоною підготовки, локальним сховищем, вищим сховищем та командами, пов'язаними з кожним з них (з поясненнями).
*   Обидва ресурси також доступні іншими мовами (наприклад, іспанською, французькою та іншими).
* "[Happy Git and GitHub for the useR](http://happygitwithr.com)" - доступна, безкоштовна онлайн книга Дженні Брайан про те, як налаштувати та використовувати Git і GitHub, з конкретними посиланнями на інтеграцію Git з RStudio та роботу з Git в R.
* [Open Scientific Code using Git and GitHub](https://open-source-for-researchers.github.io/open-source-workshop/) - - збірка пояснень та коротких практичних вправ, які допоможуть дослідникам дізнатися більше про контроль версій та програмне забезпечення з відкритим кодом.

## Словник

{:auto_ids}
changeset
:   Група змін до одного або декількох файлів, які є або будуть додані до одного
    [коміту](#commit) в [контролі версій](#version-control)
    [репозиторію](#repository).

коміт
:   Для запису поточного стану набору файлів ([набір змін](#changeset))
    у [контролі версій](#version-control) [репозиторію](#repository). Як іменник,
    результат коміту, тобто записаний набір змін у репозиторії.
    Якщо коміт містить зміни до декількох файлів,
    всі зміни записуються разом.

конфлікт
:   Зміна, зроблена одним користувачем [система управління версіями](#version-control),
    несумісна зі змінами, внесеними іншими користувачами.
    Допомога користувачам [вирішити](#resolve) конфлікти
    є одним з основних завдань управління версіями.

HTTP
:   Hypertext Transfer [Протокол](#protocol), що використовується для обміну веб-сторінками та іншими даними
    у всесвітній павутині.

об'єднати
:   (репозиторій): Для узгодження двох наборів змін з
    [репозиторіїв](#repository).

протокол
:   Набір правил, які визначають, як один комп'ютер спілкується з іншим.
    Загальні протоколи в інтернеті включають [HTTP](#http) та [SSH](#ssh).

віддалений
:   (репозиторію) Контроль версій [репозиторій](#repository), підключений до іншого,
    таким чином, що обидва можуть бути збережені в синхронному обміні [комітів](#commit).

репозиторій
:   Область зберігання, де [конроль версій](#version-control)  система
    зберігає повну історію [комітів](#commit) проєкту та інформацію
    про те, хто що змінив і коли.

вирішити
:   Для усунення [конфліктів](#conflict) між двома або більше несумісними змінами до файлу або набору файлів
    управляється системою [контроль версій](#version-control).

ревізія
:   Синонім [коміту](#commit).

SHA-1
:   [SHA-1 hashes](https://en.wikipedia.org/wiki/SHA-1) - це те, що Git використовує для обчислення ідентифікаторів, у тому числі для комітів.
    Для їх обчислення Git використовує не тільки фактичну зміну коміту, але і його метадані (такі як дата, автор,
    повідомлення), включаючи ідентифікатори всіх комітів попередніх змін. Це робить ідентифікатори комітів Git практично унікальними.
    Тобто ймовірність того, що два коміти, зроблені незалежно один від одного, навіть однієї і тієї ж зміни, отримують один і той же ідентифікатор, надзвичайно
    мала.

SSH
:   Secure Shell [протокол](#protocol) використовується для безпечного зв'язку між комп'ютерами.

часова мітка
:   Запис про те, коли сталася певна подія.

контроль версій
:   Інструмент для керування змінами набору файлів.
    Кожен набір змін створює новий [коміт](#commit) файлів;
    система контролю версій дозволяє користувачам надійно відновлювати старі коміти,
    і допомагає керувати суперечливими змінами, внесеними різними користувачами.

