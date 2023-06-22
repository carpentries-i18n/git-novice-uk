---
title: Відстеження змін
teaching: 20
exercises: 0
questions:
- "Як записати зміни в Git?"
- "Як перевірити стан контролю версій свого репозиторію?"
- "Як записати нотатки про те, які зміни було внесено і чому?"
objectives:
- "Пройти цикл зміни-додавання-коміту для одного або декількох файлів."
- "Пояснити де зберігається інформація на кожному етапі цього циклу."
- "Пояснити різницю між інформативними та неінформативними повідомленнями комітів."
keypoints:
- "`git status` показує стан репозиторію."
- "Файли можуть зберігатися в робочій директорії проекту (яку бачать користувачі), зоні стейджингу (де будується наступний коміт) і локальному репозиторію (де коміти постійно зберігаються)."
- "`git add` додає файли до зони стейджингу."
- "`git commit` зберігає все, що міститься у зоні стейджингу, як новий коміт у локальному репозиторії."
- "Складайте повідомлення коміту так щоб воно акуратно описувало ваші зміни."
---

Спочатку переконайтеся, що ми все ще в правильній директорії.
Ви повинні знаходитися у директорії `planets`.

~~~
$ cd ~/Desktop/planets
~~~
{: .language-bash}

Давайте створимо файл під назвою `mars.txt`, який буде містити деякі нотатки
про придатність Червоної Планети як бази.
Ми будемо використовувати редактор `nano` для редагування файлу;
ви можете використовувати будь-який редактор, який вам подобається.
Зокрема, це не обовʼязково повинен бути `core.editor`, який ви вказали глобально. Але пам'ятайте, що команда bash для створення або редагування нового файлу буде залежати від редактора, який ви оберете (це може бути не  `nano`). Для довідки щодо текстових редакторів, дивіться ["Which Editor?"](https://swcarpentry.github.io/shell-novice/03-create/) в уроці [The Unix Shell](https://swcarpentry.github.io/shell-novice/).

~~~
$ nano mars.txt
~~~
{: .language-bash}

Надрукуйте нижче наведений текст у файлі `mars.txt`:

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

Давайте спочатку перевіримо, що файл був правильно створений, запустивши команду `ls`:


~~~
$ ls
~~~
{: .language-bash}

~~~
mars.txt
~~~
{: .output}


`mars.txt` містить тільки один рядок, який ми можемо побачити, запустивши:

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

Якщо ми знову перевіримо статус нашого проєкту,
Git повідомляє нам, що він помітив новий файл:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main

No commits yet

Untracked files:
   (use "git add <file>..." to include in what will be committed)

\tmars.txt

nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

Повідомлення "untracked files" означає, що в директорії існує файл, який
Git не відстежує.
Ми можемо повідомити Git що цей файл треба відстежувати за допомогою команди `git add`:

~~~
$ git add mars.txt
~~~
{: .language-bash}

та згодом переконатися, що все виглядає правильно:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

\tnew file:   mars.txt

~~~
{: .output}

Git тепер знає, що він повинен стежити за файлом "mars.txt', але він ще не зафіксував зміни у цьому файлі.
Щоб зробити це,
нам потрібно запустити ще одну команду:

~~~
$ git commit -m "Start notes on Mars as a base"
~~~
{: .language-bash}

~~~
[main (root-commit) f22b25e] Start notes on Mars as a base
 1 file changed, 1 insertion(+)
 create mode 100644 mars.txt
~~~
{: .output}

Коли ми запускаємо `git commit`,
Git бере все, що ми раніше запросили його зберегти, використовуючи `git add`,
та зберігає постійну копію цих змін у спеціальній директорії `.git`.
Ця постійна копія називається [коміт]({{ page.root }}{% link reference.md %}#commit) (commit)
(або [revision]({{ page.root }}{% link reference.md %}#revision)). У цьому прикладі коміт має скорочений ідентифікатор `f22b25e`. Ваш коміт може мати інший ідентифікатор.

Ми використовуємо команду `-m` (від "message")
щоб надати короткий, інформативний та конкретний коментар, який допоможе нам згадати пізніше про те, що ми зробили і чому.
Якщо ми просто запустимо `git commit` без опції `-m`,
Git запустить `nano` (або будь-який інший редактор, який ми вказали як `core.editor`)
щоб ми могли написати довше повідомлення.

[Гарні повідомлення комітів][commit-messages] починаються з короткого (< 50 символів) твердження про зміни, внесені в коміт. Загалом, повідомлення має завершити речення "If applied, this commit will" <commit message here>.
Якщо ви хочете вдатися більше в деталі, додайте порожній рядок між першим рядком та вашими додатковими нотатками. Використовуйте додаткові нотатки, щоб пояснити, чому ви внесли зміни та/або яким буде їх вплив.

Якщо ми тепер запустимо `git status`:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
nothing to commit, working tree clean
~~~
{: .output}

Git говорить нам, що поточний стан файлів відповідає їх стану, який збережений у репозиторїї.
Якщо ми хочемо знати, що саме ми зробили нещодавно - 
ми можемо попросити Git показати нам історію проєкту, використовуючи `git log`:

~~~
$ git log
~~~
{: .language-bash}

~~~
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base
~~~
{: .output}

`git log` виводить перелік усіх комітів, які були внесені до репозиторію, у зворотному хронологічному порядку.
Для кожного коміту буде надруковано повний ідентифікатор коміту
(який починається з тих же символів, що і
скорочений ідентифікатор, попередньо надрукований командою `git commit`),
автор коміту,
дата його створення,
і повідомлення Git, яке було додано під час запису коміту.

> ## Де зберігаються мої зміни?
>
> Якщо ми зв цей момент запустимо `ls`, ми все одно побачимо лише один файл, який називається `mars.txt`.
> Це відбувається тому, що Git зберігає інформацію про історію файлів
> у спеціальній директорії `.git`, згаданій раніше,
> щоб наша файлова система не засмічувалася
> (і щоб ми випадково не могли змінити або видалити стару версію).
{: .callout}

Тепер уявімо, що Dracula додає нову інформацію до файлу.
(Знову ж таки, ми будемо редагувати його за допомогою `nano`, і потім перевіряти його зміст за допомогою `cat`;
ви можете користуватися іншим редактором, та можете не використовувати `cat`.)

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
~~~
{: .output}

Тепер, коли ми запускаємо `git status`,
Git повідомляє нам, що файл, про який він вже знає, був змінений:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

\tmodified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Ключова фраза - це останній рядок:
"no changes added to commit".
Ми змінили цей файл,
але ми не сказали Git, що ми маємо намір зберегти ці зміни у майбутньому
(за допомогою `git add`)
або, що ми хочемо зберегти їх зараз (за допомогою `git commit`).
Давайте зробимо це зараз. Хорошою практикою є перегляд
наших змін кожного разу перед їх збереженням. Ми робимо це за допомогою `git diff`.
Це показує нам відмінності між поточним станом
файлу і останною збереженою версією:

~~~
$ git diff
~~~
{: .language-bash}

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

Результат цієї команди важко зрозуміти, тому що 
це насправді серія команд для таких інструментів, як редактори або `patch`,
яка вказує як змінити один файл за допомогою іншого.
Якщо розділити цей результат на фрагменти:

1.  Перший рядок вказує нам на те що результат цієї команди у Git подібен до Unix команди `diff`,
    яка порівнює стару та нову версії файлу.
2.  Другий рядок повідомляє які саме версії файлу
    Git порівнює;
    `df0654a` та `315bf3a` є унікальними ідентифікаторами цих версій.
3.  Третій та четвертий рядки ще раз показують назву файлу, що змінюється.
4.  Решта рядків найцікавіші, вони показують нам фактичні відмінності
    і рядки, у яких вони відбуваються.
    Зокрема,
    значок `+` в першому стовпці вказує де ми додали рядок.

Після того, як ми переглянули наші зміни, прийшов час зберегти їх:

~~~
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
~~~
{: .language-bash}

~~~
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

\tmodified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Але це не спрацює:
Git не буде додавати зміни, тому що ми не використали спочатку `git add`.
Давайте виправимо це:

~~~
$ git add mars.txt
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
~~~
{: .language-bash}

~~~
[main 34961b1] Add concerns about effects of Mars' moons on Wolfman
 1 file changed, 1 insertion(+)
~~~
{: .output}

Git наполягає, щоб ми додали файли до набору змін, які ми хочемо записати,
перед тим як ми зробимо коміт. Це дозволяє зберігати зміни поступово та 
обʼєднувати їх у логічні блоки, аніж у 
великі набори змін.
Наприклад,
припустимо, ми робимо коміт кількох цитат відповідних досліджень у нашій дисертації.
Можливо, ми бажаємо зберегти ці зміни,
та відповідні записи у бібліографії,
але *не* зберігати деякі інші зміни в нашій роботі
(наприклад, висновок, яки ми ще не закінчили).

Щоб це було можливо зробити,
Git має спеціальну *зону стейджингу* *staging area*
де він відстежує речі, які були додані
до поточного [набору змін]({{ page.root }}{% link reference.md %}#changeset)
проте, ще не були збережені.

> ## Зона стейджингу
>
> Якщо ви будете уявляти, ніби Git робить знімки змін протягом життя проєкту, то
> `git add` вказує *що* буде на знімку
> (додаючи речі в зоні стейждингу),
> а `git commit` після того *насправді робить* знімок, та
> назавжди зберігає його (як коміт).
> Якщо у зоні стейджингу нічого немає, то коли ви введете `git commit`,
> Git запропонує вам використати `git commit -a` або `git commit --all`,
> який ніби збирає разом *всіх*, щоб зробити групове фото!
> Однак майже завжди краще
> явним чином додати речі до зони стейджингу, тому без цього ви можете
> випадково зберегти інші зміни, про які ви забули. (Повертаючись до порівняння з груповим фото,
> якщо ви використали команду "-а",
> до вашої фотографії може потрапити зайва людина!)
> Тому додавайте речі в зону стейджингу власноруч -
> у іншому випадку вам може знадобитися шукати допомоги з "git undo commit" частіше, ніж
> вам хотілося б!
{: .callout}

![Зона стейджингу Git]({{ site.baseurl }}/fig/git-staging-area.svg)

Давайте подивимося, як наші зміни у файлі проходять шлях від текстового редактора
до зони стейджингу
і далі у довгострокове зберігання.
Спочатку,
ми додамо новий рядок у наш файл:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

~~~
$ git diff
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{: .output}

Поки що все добре:
ми додали один рядок в кінці файлу
(що зазначає `+` у першій колонці).
Тепер давайте помістимо цю зміну у зону стейджингу
та подивимося що після цього звітує `git diff`:

~~~
$ git add mars.txt
$ git diff
~~~
{: .language-bash}

Результату немає:
це виглядає ніби для Git
немає різниці між тим, що вже було збережено назавжди
і тим, що зараз міститься у робочій директорії.
Проте,
якщо ми зробимо наступне:

~~~
$ git diff --staged
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{: .output}

то ми побачимо різницю між
останніми збереженими змінами
і тими, які знаходяться в зоні стейджингу.
Давайте збережемо наші зміни:

~~~
$ git commit -m "Discuss concerns about Mars' climate for Mummy"
~~~
{: .language-bash}

~~~
[main 005937f] Discuss concerns about Mars' climate for Mummy
 1 file changed, 1 insertion(+)
~~~
{: .output}

перевіримо наш статус:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
nothing to commit, working tree clean
~~~
{: .output}

і подивимося на історію попередніх змін:

~~~
$ git log
~~~
{: .language-bash}

~~~
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5 (HEAD -> main)
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Discuss concerns about Mars' climate for Mummy

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Add concerns about effects of Mars' moons on Wolfman

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base
~~~
{: .output}

> ##  Порівняння з підсвіткою змінених слів у рядках
>
> Інколи, наприклад, у випадку текстових документів, результат
> `diff` дуже важко зрозуміти. Саме тут `--color-words` опція для
> `git diff` є надзвичайно зручною, бо вона виділяє кольором змінені слова.
{: .callout}

## Перегляд історії змін по сторінках 
Коли розмір результату `git log` перевищує розмір вашого екрану,
 `git` використовує спеціальну програму "пейджер", щоб поділити результат між сторінками.
Після виклику "пейджера", ви помітите, що останній рядок вашого результату закінчується на `:`, замість звичайного закінчення.

Натисніть <kbd>Q</kbd>, щоб вийти з пейджеру.
Натисніть <kbd>пробіл</kbd>, щоб перейти на наступну сторінку.
Натисніть <kbd>/</kbd> та напишіть бажане слово задля пошуку його на усіх сторінках.
Натисніть <kbd>N</kbd>, для навігації між результатами пошуку.
{: .callout}

## Обмеження розміру відображеної історії змін
>
>Щоб запобігти випадку, коли `git log` повністю займає ваш термінал,
> ви можете обмежувати кількість комітів які відображує Git, використовуючи опцію `-N`, де `N` - кількість
> комітів які би ви бажали бачити на екрані. Наприклад, якщо ви бажаєте побачити лише останній коміт,
> використовуйте команду
 ~~~
$ git log -1
{: .language-bash}
 ~~~
 commit 005937fbe2a98fb83f0ade869025dc2636b4dad5 (HEAD -> main)
> Author: Vlad Dracula <vlad@tran.sylvan.ia>
> Date: Thu Aug 22 10:14:07 2013 -0400
>
> Discuss concerns about Mars' climate for Mummy
> ~~~
> {: .output}
>
> Ви також можете зменшити кількість інформації, використовуючи
> опцію `--oneline`:
>
> ~~~
> $ git log --oneline
> ~~~
> {: .language-bash}
> ~~~
> 005937f (HEAD -> main) Discuss concerns about Mars' climate for Mummy
> 34961b1 Add concerns about effects of Mars' moons on Wolfman
> f22b25e Start notes on Mars as a base
> ~~~
> {: .output}
>
> Ви також можете комбінувати опцію `--oneline` з іншими опціями. Одна корисна
> комбінація додає `--graph` для графічного відображення історії комітів за допомогою
> псевдографіки, вказуючи при цьому які коміти пов`язані з
> поточним `HEAD`, поточним бранчем `main`, або
> [іншими обʼєктами у Git репозиторії][git-references]:
>
> ~~~
> $ git log --oneline --graph
> ~~~
> {: .language-bash}
> ~~~
> * 005937f (HEAD -> main) Discuss concerns about Mars' climate for Mummy
> * 34961b1 Add concerns about effects of Mars' moons on Wolfman
> * f22b25e Start notes on Mars as a base
> ~~~
> {: .output}
{: .callout}

> ## Директорії
>
> Дві важливі речі, які ви повинні знати про директорії в Git.
>
> 1. Git не відстежує директорії самостійно, тільки файли всередині них.
>    Спробуйте власноруч:
>
>    ~~~
>    $ mkdir spaceships
>    $ git status
>    $ git add spaceships
>    $ git status
>    ~~~
>    {: .language-bash}
>
>    Зауважте, наша новостворена порожня директорія `spaceships` не з`являється в
>   списку невідстежуваних файлів, навіть якщо ми конкретно додамо їх (_через_ `git add`) до нашого
>    репозиторію. Ось чому ви іноді бачите файли `.gitkeep` 
>    в інших порожніх директоріях. На відміну від `.gitignore`, ці файли не є особливими
>   і їх єдиною метою є заповнити директорію, щоб Git додав її до
>    репозиторію. Насправді, ви можете назвати такі файли до вашої довподоби.
>
> 2. Якщо ви створюєте директорію у вашому репозиторії Git і заповнюєте її файлами,
>    ви можете додати всі файли в директорії відразу:
>
>    ~~~
>    git add <directory-with-files>
>    ~~~
>    {: .language-bash}
>
>    Спробуйте власноруч:
>
>    ~~~
>    $ touch spaceships/apollo-11 spaceships/sputnik-1
>    $ git status
>    $ git add spaceships
>    $ git status
>    ~~~
>    {: .language-bash}
>
>    Перш ніж рухатися далі, ми збережемо ці зміни.
>
>    ~~~
>    $ git commit -m "Add some initial thoughts on spaceships"
>    ~~~
>    {: .language-bash}
{: .callout}

Для повторення: коли ми хочемо додати зміни до нашого репозиторію,
спочатку нам потрібно додати змінені файли в зону стейджингу
(`git add`) а потім зберегти заплановані зміни до
репозиторію (`git commit`):

![Процес запису комітів у Git]({{ site.baseurl }}/fig/git-committing.svg)

> ## Вибір повідомлення коміту
>
> Які з наступних повідомлень коміту будуть найбільш оптимальними для
> останнього коміту в `mars.txt`?
>
> 1. "Зміни"
> 2. "Додано рядок 'But the Mummy will appreciate the lack of humidity' до mars.txt"
> 3. "Обговорення впливу клімату на Марсі на Mummy"
>
> > ## Відповідь
> > Відповідь 1 є недостатньо детальною, а мета коміту неясна;
> > відповідь 2 дублює результат команди "git diff" яка відобразить зміни зроблені у цьому коміті;
> > 3 відповідь - оптимальна: коротка, інформативна, та імперативна.
> {: .solution}
{: .challenge}

> ## Збереження змін у Git
>
> Яка(які) з наведених нижче команда збережуть зміни у файлі `myfile.txt`
> до мого локального Git репозиторію?
>
> 1. ~~~
>    $ git commit -m "my recent changes"
>    ~~~
>    {: .language-bash}
> 2. ~~~
>    $ git init myfile.txt
>    $ git commit -m "my recent changes"
>    ~~~
>    {: .language-bash}
> 3. ~~~
>    $ git add myfile.txt
>    $ git commit -m "my recent changes"
>    ~~~
>    {: .language-bash}
> 4. ~~~
>    $ git commit -m myfile.txt "my recent changes"
>    ~~~
>    {: .language-bash}
>
> > ## Відповідь
> >
> > 1. Створить коміт, лише якщо файли вже були у зоні стейджінгу.
> > 2. Намагатиметься створити новий репозиторій.
> > 3. Правильна відповідь: спочатку додайте файл до зони стейджингу, потм зробіть коміт.
> > 4. Спробує записати коміт файлу з назвою "my recent changes" з повідомленням myfile.txt.
> {: .solution}
{: .challenge}

> ## Коміт декількох файлів
>
> Зона стейджингу може зберігати зміни у будь-якій кількісті файлів,
> які ви хочете записати у один коміт.
>
> 1. Додайте текст до `mars.txt` про те, що ви вирішили 
> розглянути побудову бази на Венері
> 2. Створіть новий файл `venus.txt` з вашими думками
> стосовно Венери як бази для вас та ваших друзів
> 3. Додайте зміни у обох файлах до зони стейджтнгу,
> та зробіть коміт цих змін.
>
> > ## Відповідь
> >
> > Результат нижче з файлу `cat mars.txt` відображає тільки контент доданий під час 
> > цієї вправи. Ваш результат може виглядати іншим чином.
> > 
> > Спочатку ми робимо зміни у файлі `mars.txt` та `venus.txt` files:
> > ~~~
> > $ nano mars.txt
> > $ cat mars.txt
> > ~~~
> > {: .language-bash}
> > ~~~
> > Maybe I should start with a base on Venus.
> > ~~~
> > {: .output}
> > ~~~
> > $ nano venus.txt
> > $ cat venus.txt
> > ~~~
> > {: .language-bash}
> > ~~~
> > Venus is a nice planet and I definitely should consider it as a base.
> > ~~~
> > {: .output}
> > Тепер ви можете додати обидва файли до зони стейджингу. Ми можемо зробити це однією командою:
> >
> > ~~~
> > $ git add mars.txt venus.txt
> > ~~~
> > {: .language-bash}
> > Або за допомогою декількох команд:
> > ~~~
> > $ git add mars.txt
> > $ git add venus.txt
> > ~~~
> > {: .language-bash}
> > Тепер файли готові для коміту. Ви можете перевірити це за допомогою `git status`. Якщо ви готові зробити коміт, використовуйте
> > ~~~
> > $ git commit -m "Write plans to start a base on Venus"
> > ~~~
> > {: .language-bash}
> > ~~~
> > [main cc127c2]
> >  Write plans to start a base on Venus
> >  2 files changed, 2 insertions(+)
> >  create mode 100644 venus.txt
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## Репозиторій `bio`
>
> * Створіть новий репозиторій Git на вашому комп`ютері під назвою `bio`.
> * Напишіть три рядки своєї біографії у файлі під назвою `me.txt`,
> та зробіть коміт цих змін
> * Змініть один з рядків, додайте четвертий рядок
> * Покажіть відмінності
> між оновленим файлом та його попередньою версією.
>
> > ## Відповідь
> >
> > Якщо необхідно, вийдіть з папки `planets`:
> >
> > ~~~
> > $ cd ..
> > ~~~
> > {: .language-bash}
> >
> > Створіть нову папку  `bio` та перейдіть до неї:
> >
> > ~~~
> > $ mkdir bio
> > $ cd bio
> > ~~~
> > {: .language-bash}
> >
> > Iніціалізуйте репозиторій Git:
> >
> > ~~~
> > $ git init
> > ~~~
> > {: .language-bash}
> >
> > Створіть файл `me.txt` з вашою біографією, використовуючи `nano` або інший текстовий редактор.
> > Коли будете готові, додайте його до зони стейджингу та запишіть коміт до репозиторію:
> >
> > ~~~
> > $ git add me.txt
> > $ git commit -m "Add biography file" 
> > ~~~
> > {: .language-bash}
> >
> > Змініть файл як описано (змініть один рядок, додайте четвертий рядок).
> > Для того щоб показати зміни
> > між оновленим файлом та його попередньою версією, використайте `git diff`:
> >
> > ~~~
> > $ git diff me.txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

[commit-messages]: https://chris.beams.io/posts/git-commit/
[git-references]: https://git-scm.com/book/en/v2/Git-Internals-Git-References

{% include links.md %}

