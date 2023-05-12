---
title: Налаштування Git
teaching: 5
exercises: 0
questions:
- "Як налаштувати Git для використання?"
objectives:
- "Налаштування `git` під час його першого використання на комп'ютері."
- "Розуміння значення конфігураційного параметра `--global`."
keypoints:
-   "Користування `git config` з параметром ` -- global` для налаштування імені користувача, адреси електронної пошти, редактора та інших параметрів раз на комп'ютер."
---

Коли ми користуємося Git на новому комп'ютері вперше,
нам потрібно налаштувати декілька речей. Нижче наведено кілька прикладів
з налаштувань, які ми встановимо, коли почнемо працювати з Git:

*   ваше ім'я та адреса електронної пошти,
*   ваш основний текстовий редактор,
*   і що ми хочемо використовувати ці параметри глобально (тобто для кожного проєкту).

В командному рядку (command line), команди Git виглядають як `git verb options`,
де `verb` - це те що ми фактично хочемо зробити, та `options` - це додаткова інформація, яка може бути потрібна для `verb`. Ось як
Dracula налаштовує свій новий ноутбук:

~~~
$ git config --global user.name "Vlad Dracula"
$ git config --global user.email "vlad@tran.sylvan.ia"
~~~
{: .language-bash}

Будь ласка, використовуйте своє власне ім'я та електронну пошту замість "Dracula". Ці ім'я користувача та електронна пошта будуть асоційовані з вашою подальшою діяльністю Git,
а це означає, що будь-які зміни надіслані в
[GitHub](https://github.com/),
[BitBucket](https://bitbucket.org/),
[GitLab](https://gitlab.com/) або
інший Git хост-сервер
після цього уроку будуть містити цю інформацію.

В цьому уроці ми будемо працювати з [GitHub](https://github.com/), тож використовувана електронна пошта повинна бути такою ж, як і та, яка використовується для налаштування вашого GitHub акаунту. Якщо вас турбує конфіденційнйсть вашої електронної адреси, будь ласка перегляньте [GitHub's instructions for keeping your email address private][git-privacy]. 

>## Збереження конфіденційності вашої електронної адреси
>
>Якщо ви вирішили приховати власну електронну адресу на GitHub, тоді використовуйте для `user.email` електронну адресу у вигляді `username@users.noreply.github.com` замінивши `username` вашим ім'ям користувача GitHub.
{: .callout}


> ## Закінчення рядків
>
> Як і з іншими ключами, коли ви вводите <kbd>Enter</kbd> або <kbd> </kbd>, або <kbd>Return</kbd> на Macs на вашій клавіатурі,
> ваш комп'ютер кодує це як символ.
> Різні операційні системи використовують різні символ(и) для репрезентації кінця рядку.
> (Ви також можете почути про це як "нові рядки" або "розриви рядків".)
> Тому що Git використовує ці символи для порівняння файлів,
> це може спричинити несподівані проблеми під час редагування файлу на різних машинах.
> Хоча це поза межами цього уроку, ви можете більше прочитати про це питання 
> [у посібнику "Pro Git"](https://www.git-scm.com/book/en/v2/Customizing-Git-Git-Configuration#_core_autocrlf).
>
> Ви можете змінити спосіб розпізнавання закінчень рядків в Git
> використовуючи `core.autocrlf` команду до `git config`.
> Рекомендовані такі параметри:
>
> Для macOS та Linux:
>
> ~~~
> $ git config --global core.autocrlf input
> ~~~
> {: .language-bash}
>
> Для Windows:
>
> ~~~
> $ git config --global core.autocrlf false
> ~~~
> {: .language-bash}
{: .callout}

Dracula також повинен налаштувати його улюблений текстовий редактор, як наведено в таблиці нижче:

| Редактор             | Команда для налаштування                            |
|:-------------------|:-------------------------------------------------|
| Atom | `$ git config --global core.editor "atom --wait"`|
| nano               | `$ git config --global core.editor "nano -w"`    |
| BBEdit (Mac, with command line tools) | `$ git config --global core.editor "bbedit -w"`    |
| Sublime Text (Mac) | `$ git config --global core.editor "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl -n -w"` |
| Sublime Text (Win, 32-bit install) | `$ git config --global core.editor "'c:/program files (x86)/sublime text 3/sublime_text.exe' -w"` |
| Sublime Text (Win, 64-bit install) | `$ git config --global core.editor "'c:/program files/sublime text 3/sublime_text.exe' -w"` |
| Notepad (Win)    | `$ git config --global core.editor "c:/Windows/System32/notepad.exe"`|
| Notepad++ (Win, 32-bit install)    | `$ git config --global core.editor "'c:/program files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`|
| Notepad++ (Win, 64-bit install)    | `$ git config --global core.editor "'c:/program files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`|
| Kate (Linux)       | `$ git config --global core.editor "kate"`       |
| Gedit (Linux)      | `$ git config --global core.editor "gedit --wait --new-window"`   |
| Scratch (Linux)       | `$ git config --global core.editor "scratch-text-editor"`  |
| Emacs              | `$ git config --global core.editor "emacs"`   |
| Vim                | `$ git config --global core.editor "vim"`   |
| VS Code                | `$ git config --global core.editor "code --wait"`   |

Ви можете переналаштувати текстовий редактор для Git у будь-який момент.

> ## Вихід з Vim
>
> Зауважте, що Vim є стандартним редактором для багатьох програм. Якщо ви до цього не користувалися Vim і хочете вийти з сесії не зберігаючи ваших змін,
натисніть <kbd>Esc</kbd> потім надрукуйте `:q!` та натисніть <kbd>Enter</kbd> або <kbd> </kbd>, або <kbd>Return</kbd> на Macs.
> Якщо ви хочете зберегти свої зміни та вийти, натисніть<kbd>Esc</kbd> потім надрукуйте `:wq` та натисніть <kbd>Enter</kbd> або <kbd> </kbd>, або <kbd>Return</kbd> на Macs.
{: .callout}

Git (2.28+) надає вам змогу обрати назву гілки, яку буде створено під час
ініціалізації будь-якої нової репозиторії.  Dracula вирішив використати цю можливість, щоб назвати її `main`, щоб це відповідало хмарному сервісу, який він в кінцевому підсумку використовуватиме.

~~~
$ git config --global init.defaultBranch main
~~~
{: .language-bash}

> ## Найменування гілки Git за замовчуванням
>
> Зміни у змісті репозиторію пов’язані з "гілкою".
> Для початківців у цьому уроці буде достатньо знати, що гілки існують, і що в цьому уроці використовується тільки одна гілка.  
> За замовчуванням, Git створить гілку під назвою `master` 
> коли ви створюєте нову репозиторію за допомогою `git init` (пояснено в наступному Епізоді). Цей термін нагадує про 
> расистську практику людського рабства, тому у 
> [спільноті розробників програмного забезпечення](https://github.com/github/renaming)  вирішили використовувати 
> більш інклюзивну мову. 
> 
> В 2020, більшість сервісів хостингу коду Git перейшли до використання `main` як стандартної 
> гілки. Наприклад, будь-яка нова репозиторія створена у GitHub та GitLab за замовчуванням буде мати назву
> `main`.  Однак в Git такої ж зміни ще не було застосовано. В результаті локальні репозиторії 
> повинні бути налаштовані вручну, мати ту саму назву головної гілки як і більшість хмарних сервісів.  
> 
> Для версій Git до 2.28, зміну можна зробити на індивідуальному рівні репозиторії. 
> Команду для цього можна знайти в наступному епізоді. Зауважте, якщо це значення не встановлено у вашій локальній системі Git, 
> то `init.defaultBranch` за замовчуванням буде `master`.
{: .callout}

П'ять команд, які ми щойно запускаємо вище, потрібно запускати лише один раз: параметр `--global` каже Git
використовувати ці налаштування для кожного проєкту у вашому обліковому записі користувача на цьому комп'ютері.

Ви можете перевірити ваші налаштування у будь-яку мить:

~~~
$ git config --list
~~~
{: .language-bash}

Ви можете змінити ваші налаштування стільки разів, скільки забажаєте: використовуйте
ті ж самі команди, щоб обрати інший текстовий редактор або оновити вашу електронну адресу.

> ## Проксі-сервер
>
> В деяких мережах потрібно використовувати
> [проксі-сервер](https://en.wikipedia.org/wiki/Proxy_server). Якщо це так, 
> вам також може знадобитися повідомити про це Git:
>
> ~~~
> $ git config --global http.proxy proxy-url
> $ git config --global https.proxy proxy-url
> ~~~
> {: .language-bash}
>
> Щоб вимкнути проксі, скористайтеся
>
> ~~~
> $ git config --global --unset http.proxy
> $ git config --global --unset https.proxy
> ~~~
> {: .language-bash}
{: .callout}

> ## Довідка про Git та посібник користувача
>
> Завжди пам'ятайте: якщо ви забули підкоманди або параметри команди `git`, ви можете отримати 
> відповідний список параметрів, надрукувавши `git <command> -h` або подивитись у документації Git, ввівши
> `git <command> --help`, e.g.:
>
> ~~~
> $ git config -h
> $ git config --help
> ~~~
> {: .language-bash}
>
> Під час перегляду документації запам’ ятайте, що `:`  — це підказка, яка очікує на команди, і ви можете натиснути кнопку <kbd>Q</kbd> щоб вийти з посібника.
>
> Більш загально можна отримати список доступних `git` команд і подальші ресурси за допомогою цієї команди:
>
> ~~~
> $ git help
> ~~~
> {: .language-bash}
{: .callout}

[git-privacy]: https://help.github.com/articles/keeping-your-email-address-private/

