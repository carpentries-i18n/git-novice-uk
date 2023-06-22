---
layout: page
title: Обговорення
---

## Часті запитання

У людей часто виникають питання про Git, що виходять за рамки основного матеріалу.
Студенти, які завершили решту уроків, можуть знайти цінність у перегляді наступних тем.

Зауважте, що оскільки цей матеріал не є важливим для базового використання Git, він не буде покритий інструктором.

## Додаткові налаштування Git

В [Налаштування Git]({{ page.root }}/02-setup/),
ми використовували `git config --global`, щоб встановити деякі параметри за замовчуванням для Git.
Виявляється, ці параметри конфігурації зберігаються у вашій домашній директорії 
у звичайному текстовому файлі під назвою  `.gitconfig`.

~~~
$ cat ~/.gitconfig
~~~
{: .language-bash}

~~~
[user]
\tname = Vlad Dracula
\temail = vlad@tran.sylvan.ia
[color]
\tui = true
[core]
\teditor = nano
~~~
{: .output}

Цей файл можна відкрити у бажаному текстовому редакторі.
(Зауважте, що рекомендується продовжити використання команди `git config`,
оскільки це допомагає уникнути введення синтаксичних помилок.)

Зрештою, ви захочете почати налаштовувати поведінку Git.
Це можна зробити, додавши більше записів до вашого `.gitconfig`.
Доступні параметри описані в посібнику:

~~~
$ git config --help
~~~
{: .language-bash}

Зокрема, вам може знадобитися додати псевдоніми.
Це як ярлики для довших команд Git.
Наприклад, якщо вам набридло вводити `git checkout` весь час,
ви можете запустити команду:

~~~
$ git config --global alias.co checkout
~~~
{: .language-bash}

Тепер, якщо ми повернемося до прикладу з [Досліджуючи історію]({{ page.root }}/05-history/), де ми запустили:

~~~
$ git checkout f22b25e mars.txt
~~~
{: .language-bash}

ми могли б тепер надрукувати:

~~~
$ git co f22b25e mars.txt
~~~
{: .language-bash}

## Стиль Git's Log

Гарною метою для налаштування є результат log.
Типовий log досить багатослівний, але не дає графічних підказок, 
таких як інформація про те, які коміти були зроблені локально
і які були витягнуті з віддалених.

Ви можете використовувати `git log --help` та `git config --help` для пошуку різних способів зміни результату
log.
Спробуйте наступні команди і подивіться, який ефект вони мають:

~~~
$ git config --global alias.lg "log --graph"
$ git config --global log.abbrevCommit true
$ git config --global format.pretty oneline
$ git lg
~~~
{: .language-bash}

Якщо вам не подобаються ефекти,
ви можете скасувати їх за допомогою:

~~~
$ git config --global --unset alias.lg
$ git config --global --unset log.abbrevCommit
$ git config --global --unset format.pretty
~~~
{: .language-bash}

> ## Скасування змін конфігурації Git
>
> Ви можете використовувати прапорець `--unset` для видалення небажаних параметрів з `.gitconfig`.
> Інщий спосіб повернути зміни - зберігати ваш `.gitconfig` за допомогою Git.
>
> Щоб отримати підказки щодо того, що ви можете налаштувати,
> перейдіть до GitHub та знайдіть "gitconfig".
> Ви знайдете сотні репозиторіїв, в яких люди зберегли
> свої власні файли конфігурації Git.
> Сортувати їх за кількістю зірок і подивитися на кілька найкращих.
> Якщо ви знайдете ті, які вам подобаються,
> будь ласка, перевірте, що вони охоплені ліцензією з відкритим вихідним кодом, перш ніж клонувати їх.
{: .callout}

## Нетекстові файли

Пригадайте, коли ми обговорювали [Конфлікти]({{ page.root }}/09-conflict/)
була задача, яка запитувала,
"Що робить Git
коли виникає конфлікт у зображенні або якомусь інщому нетекстовому файлі,
що зберігається у контролі версій?"

Тепер ми переглянемо це більш детально.

Багато людей хочуть керувати версіями нетекстових файлів, таких як зображення, PDF-файли та документи Microsoft Office або LibreOffice.
Це правда, що Git може обробляти ці типи файлів (які потрапляють під банер «бінарних» типів файлів).
Однак тільки тому, що це *можна* зробити, не означає, що це *слід* зробити.

Велика частина магії Git походить від можливості робити порівняння по черзі («diffs») між файлами.
Це, як правило, легко для програмування вихідного коду і розміченого тексту.
Для нетекстових файлів різниця зазвичай може виявляти лише те, що файли змінилися
але не можуть сказати, як і де.

Це має різний вплив на продуктивність Git і ускладнить
порівняння різних версій вашого проєкту.

Для базового прикладу, щоб показати різницю, яку він робить,
ми підемо подивитися, що б сталося, якби Dracula had спробував
використання результатів з текстового процесора замість звичайного тексту.

Створіть нову директорію і перейдіть в неї:

~~~
$ mkdir planets-nontext
$ cd planets-nontext
~~~
{: .language-bash}

Для створення нового документа використовуйте програму, таку як Microsoft Word або LibreOffice Writer.
Введіть той самий текст, з якого ми починали раніше:

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

Збережіть документ у директорії `planets-nontext` зщ назвою `mars.doc`.
Знову в терміналі запустіть звичайні команди для налаштування нового репозиторію Git:

~~~
$ git init
$ git add mars.doc
$ git commit -m "Starting to think about Mars"
~~~
{: .language-bash}

Потім внесіть ті ж зміни в `mars.doc`, які ми (або Vlad) раніше зробили в `mars.txt`.

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
~~~
{: .output}

Збережіть і закрийте текстовий процесор.
Тепер подивіться, що Git думає про ваші зміни:

~~~
$ git diff
~~~
{: .language-bash}

~~~
diff --git a/mars.doc b/mars.doc
index 53a66fd..6e988e9 100644
Binary files a/mars.doc and b/mars.doc differ
~~~
{: .output}

Порівняйте це з попереднім `git diff`, отриманим при використанні текстових файлів:

~~~
diff --git a/mars.txt b/mars.txt
index df0654a..315bf3a 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,2 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
~~~
{: .output}

Зверніть увагу, як звичайні файли дають набагато більш інофрмативну різницю (diff).
Ви можете побачити, які саме лінії змінилися і які були зміни.

Неінформативний `git diff` не є єдиним наслідком використання Git на бінарних файлах.
Однак більшість інших проблем зводяться до того, чи можлива хороша різниця (diff).

Це не означає, що ви повинні *ніколи* використовувати Git на бінарних файлах.
Великим правилом є те, що це нормально, якщо бінарний файл не змінюється дуже часто,
і якщо він змінюється, вам не важливо об'єднуватися в невеликі відмінності між версіями.

Ми вже бачили, як опрацьований словесний репорт провалить цей тест.
Прикладом, який проходить тест, є логотип для вашої організації або проєкту.
Незважаючи на те, що логотип буде зберігатися в бінарному форматі, такому як `jpg` або `png`,
ви можете очікувати, що він залишиться досить статичним протягом усього життя вашого репозиторію.
У рідкісних випадках, коли брендинг змінюється,
ви, ймовірно, просто хочете замінити логотип повністю, а не об'єднати невеликі відмінності.

## Видалення файлу

Додавання та зміна файлів - це не єдині дії, які можна виконати
під час роботи над проєктом. Можливо, буде потрібно видалити файл
зі сховища.

Створіть новий файл для планети Nibiru:

~~~
$ echo "This is another name for fake planet X" > nibiru.txt
~~~
{: .language-bash}

Тепер додайте в репозиторій, як ви навчилися раніше:

~~~
$ git add nibiru.txt
$ git commit -m 'adding info on nibiru'
$ git status
~~~
{: .language-bash}

~~~
On branch main
nothing to commit, working tree clean
~~~
{: .output}

Nibiru - не справжня планета. Це була дурна ідея. Давайте видалимо 
її з диска і нехай Git знає про це:

~~~
$ git rm nibiru.txt
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)

   deleted:    nibiru.txt

~~~
{: .output}

Зміна була перенесена у зону стейджингу. Тепер закомітьте видалення та видаліть
файл з самого репозиторію.  Зауважте, що файл буде вилучено
у новому коміті.  Попередній коміт все одно
матиме файл, якщо ви хочете отримати цей конкретний коміт.

~~~
$ git commit -m 'Removing info on Nibiru.  It is not a real planet!'
~~~
{: .language-bash}

## Видалення файлу за допомогою Unix

Іноді ми можемо забути видалити файл через Git. Якщо ви видалили
файл за допомогою Unix `rm` замість використання `git rm`, не хвилюйтеся,
Git досить розумний, щоб помітити відсутній файл. Давайте відтворимо файл і
закомітимо його знову.

~~~
$ echo "This is another name for fake planet X" > nibiru.txt
$ git add nibiru.txt
$ git commit -m 'adding nibiru again'
~~~
{: .language-bash}

Тепер ми видаляємо файл за допомогою Unix `rm`:

~~~
$ rm nibiru.txt
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes not staged for commit:
   (use "git add/rm <file>..." to update what will be committed)
   (use "git checkout -- <file>..." to discard changes in working directory)

    deleted:    nibiru.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Дивіться, як Git помітив, що файл `nibiru.txt` був видалений
з диска. Наступним кроком є "стейджинг" видалення файлу
з репозиторію.  Це робиться за допомогою команди `git rm` так само, як і
раніше.

~~~
$ git rm nibiru.txt
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)

   deleted:    nibiru.txt

~~~
{: .output}

Зміна, яка була зроблена в Unix, тепер була перенесена в зону стейджингу і повинна бути
закомічена.

~~~
$ git commit -m 'Removing info on Nibiru, again!'
~~~
{: .language-bash}

## Видалення файлу

Іншою поширеною зміною під час роботи над проєктом є перейменування файлу.

Створіть файл для планети Krypton:

~~~
$ echo "Superman's home planet" > krypton.txt
~~~
{: .language-bash}

Додайте його до репозиторію:

~~~
$ git add krypton.txt
$ git commit -m 'Adding planet Krypton'
~~~
{: .language-bash}

Всі ми знаємо, що Superman переїхав на Earth. Не те, щоб у нього було багато
вибору. Тепер його рідна планета - Earth.

Переіменуйте файл `krypton.txt` на `earth.txt` за допомогою Git:

~~~
$ git mv krypton.txt earth.txt
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

\trenamed:    krypton.txt -> earth.txt
~~~
{: .output}
Останнім кроком є внесення змін до репозиторію:

~~~
$ git commit -m 'Superman's home is now Earth'
~~~
{: .language-bash}

## Переіменування файлу за допомогою Unix

Якщо ви забули використовувати Git і використовували Unix `mv` замість
`git mv`, у вас буде більше роботи, але Git зможе
впоратися з цим. Давайте спробуємо ще раз перейменувати файл,
цього разу за допомогою Unix `mv`. По-перше, нам потрібно відтворити
файл `krypton.txt`:

~~~
$ echo "Superman's home planet" > krypton.txt
$ git add krypton.txt
$ git commit -m 'Adding planet Krypton again.'
~~~
{: .language-bash}

Давайте перейменуємо файл і подивимося, що Git може з'ясувати сам по собі:

~~~
$ mv krypton.txt earth.txt
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    krypton.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    earth.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Git помітив, що файл `krypton.txt` зник з
файлової системи і з`явився новий файл `earth.txt`.

Додайте ці зміни в зону стейджингу:

~~~
$ git add krypton.txt earth.txt
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    krypton.txt -> earth.txt

~~~
{: .output}

Зверніть увагу, як Git тепер зрозумів, що `krypton.txt` не
зник - його просто перейменували.

Останнім кроком, як і раніше, є внесення змін до репозиторію:

~~~
$ git commit -m 'Superman's home is Earth, told you before.'
~~~
{: .language-bash}

## Більше концептів .gitignore

Для додаткової документації на .gitignore, будь ласка, посилання
[офіційна документація git](https://git-scm.com/docs/gitignore).

У вправі на ігнорування, учням були прелставлені дві варіації ігнорування
вкладених файлів. Залежно від організації вашого репозиторію може задовольнити ваші потреби
над іншим. Майте на увазі, що шлях, яким Git подорожує по
шляхах директорій може бути заплутаним. 

Іноді `**` шаблон стає теж в нагоді, який відповідає кратному
рівню директорій. Наприклад, `**/results/plots/*` змусить git ігнорувати
директорію `results/plots` у будь-якій кореневій директорії.

> ## Ігнорування вкладених файлів: проблема задачі
>
> Враховуючи структуру директорій, яка виглядає як:
>
> ~~~
> results/data
> results/plots
> results/run001.log
> results/run002.log
> ~~~
> {: .language-bash}
> 
> Та .gitignore, яка виглядає так:
>
> ~~~
> *.dat
> ~~~
> {: .output}
>
> HЯк би ви відстежували весь вміст `results/data/`, включаючи файли `*.dat`,
> але ігноруючи інші `results/`?
>
> > ## Відповідь
> >
> > Для цього ваш .gitignore буде виглядати так:
> >
> > ~~~
> > *.dat                 # ігноруйте файли .dat
> > results/*             # ігноруйте файли в директорії
> > !results/data/        # не ігноруйте файли в results/data
> > !results/data/*       # не ігноруйте файли .dat в reults/data
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

