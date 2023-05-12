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
- "Розрізнити описові та неописові повідомлення комітів."
keypoints:
- "`git status` показує стан сховища."
- "Файли можуть зберігатися в робочій директорії проекту (яку бачать користувачі), зоні стейджингу (де будується наступний коміт) і локальний репозиторій (де постійно записуються коміти)."
- "`git add` ставить файли в області додавання."
- "`git commit` зберігає поетапний вміст як новий коміт у локальному сховищі."
- "Написання повідомлення коміту, яке точно описує ваші зміни."
---

Спочатку переконайтеся, що ми все ще в правильній директорії.
Ви повинні знаходитися у директорії `planets`.

~~~
$ cd ~/Desktop/planets
~~~
{: .language-bash}

Давайте створимо файл під назвою "mars.txt', який містить деякі примітки про придатність Червоної планети як бази.
Ми використаємо `nano`, щоб відредагувати файл;
ви можете використовувати будь-який редактор, який вам подобається.
Зокрема, це не повинен бути «core.editor», який ви встановили глобально раніше. Але пам'ятайте, що команда bash для створення або редагування нового файлу буде залежати від редактора, який ви оберете (це може бути не  `nano`). Для оновлення текстових редакторів, перевірте ["Which Editor?"](https://swcarpentry.github.io/shell-novice/03-create/) в уроці [The Unix Shell](https://swcarpentry.github.io/shell-novice/).

~~~
$ nano mars.txt
~~~
{: .language-bash}

Надрукуйте нижче наведений текст у файл "mars.txt':

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

Давайте спочатку перевіримо, що файл був правильно створений, запустивши команду (`ls`):


~~~
$ ls
~~~
{: .language-bash}

~~~
mars.txt
~~~
{: .output}


`mars.txt` містить єдиний рядок, який ми можемо побачити, запустивши:

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
Ми можемо відстежити файл в Git за допомогою `git add`:

~~~
$ git add mars.txt
~~~
{: .language-bash}

згодом переконайтеся, що все правильно:

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

Git тепер знає, що він повинен стежити за "mars.txt',. але він не зафіксував ці зміни.
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
Git приймає все, що ми сказали, щоб зберегти, використовуючи `git add`
і зберігає копію назавжди всередині спеціального каталогу `.git`.
Ця постійна копія називається [commit]({{ page.root }}{% link reference.md %}#commit)
(або [revision]({{ page.root }}{% link reference.md %}#revision)) і його короткий ідентифікатор `f22b25e`. Ваш коміт може мати інший ідентифікатор.

Ми використовуємо команду `-m` (для "message")
записати короткий, описовий і конкретний коментар, який допоможе нам згадати пізніше про те, що ми зробили
і чому.
Якщо ми просто запустимо `git commit` без опції `-m`,
Git запустить `nano` (або будь-який інший редактор, який ми налаштували як `core.editor`)
щоб ми могли написати довше повідомлення.

[Good commit messages][commit-messages] почніть з короткого (< 50 символів) твердження про зміни, внесені в коміт. Загалом, повідомлення має завершити речення "If applied, this commit will" <commit message here>.
Якщо ви хочете вдатися більше в деталі, додайте порожній рядок між підсумковим рядком та вашими додатковими нотатками. Використовуйте цей додатковий простір, щоб пояснити, чому ви внесли зміни та/або яким буде їх вплив.

Якщо ми зараз запустимо `git status`:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
nothing to commit, working tree clean
~~~
{: .output}

Git говорить нам, що все актуально.
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

`git log` перераховує всі коміти, внесені до сховища у зворотному хронологічному порядку.
Список для кожного коміту включає повний ідентифікатор коміту
(який починається з тих же символів, що і
короткий ідентифікатор, попередньо надрукований командою `git commit`),
автор коміту,
коли він був створений,
і повідомлення Git, яке було додано при створенні коміту.

> ## Де мої зміни?
>
> Якщо ми запустимо `ls` в цей момент, ми все одно побачимо лише один файл, який називається `mars.txt`.
> Це відбувається тому, що Git зберігає інформацію про історію файлів
> у спеціальній директорії `.git`, згаданій раніше,
> щоб наша файлова система не засмічувалася
> (і щоб ми випадково не могли відредагувати або видалити стару версію).
{: .callout}

Зараз уявімо Dracula додає більше інформації у файл.
(Знову ж таки, ми будемо редагувати з `nano` і потім `cat` файл, щоб показати його вміст;
ви можете користуватися іншим редактором, та не потребуєте `cat`.)

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

Коли ми зараз запускаємо `git status`,
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

Останній рядок - ключова фраза:
"no changes added to commit".
Ми змінили цей файл,
але ми не сказали Git, що ми хочемо зберегти ці зміни
(ми робимо це за допомогою `git add`)
або, що ми хочемо зберегти їх (це ми робимо за допомогою `git commit`).
Давайте зробимо це зараз. Це хороша практика, щоб завжди переглядати
наші зміни перед їх збереженням. Ми робимо це за допомогою `git diff`.
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

Результат є важким для зрозуміння, тому що 
це насправді серія команд для таких інструментів, як редактори або `patch`,
які вказують як реконструювати один файл, враховуючи інший.
Якщо розділити цей результат на фрагменти:

1.  Перший рядок, вказує нам на те, що Git створює результат, подібний до Unix команди `diff`
    порівнюючи стару та нову версії файлу.
2.  Другий рядок повідомляє які саме версії файлу
    Git порівнює;
    `df0654a` та `315bf3a` унікальні комп'ютерні мітки для цих версій.
3.  Третій та четвертий рядки ще раз показують назву файлу, що змінюється.
4.  Решта рядків найцікавіші, вони показують нам фактичні відмінності
    і рядки, на яких вони відбуваються.
    Зокрема,
    значок `+` в першрму стовпці вказує на те, де ми додали рядок.

Переглянувши наші зміни, прийшов час додати їх:

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

Ой:
Git не буде додавати зміни, тому що ми не використали «git add» спочатку.
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

Git наполягає, щоб ми додали файли до коміту, який ми хочемо комітнути
перед тим як ми фактично робимо коміт щось. Це дозволяє нам комітнути наші зміни поступово та 
охоплювати зімни логічними пропорціями, аніж тільки 
великими партіями.
Наприклад,
припустимо, ми робимо коміт кількох цитат до відповідних досліджень до нашої дисертації.
Можливо, ми захочемо комітнути ці зміни,
та відповідні записи в бібліографії,
але * не * комітити деякі зміни в нашій роботі
(наприклад, висновок, яки ми ще не закінчили).

Щоб це було можливо зробити,
Git має спеціальний *staging area*
де він відстежує речі, які були додані
до поточного [changeset]({{ page.root }}{% link reference.md %}#changeset)
проте, не були комітнуті.

> ## Зона стейджингу
>
> Якщо ви думаєте, що Git робить знімки змін протягом життя проєкту,
> `git add` вказує * що * піде в знімку
> (розміщення речей в зоні стейждингу),
> та `git commit` тоді * насправді робить * знімок, і
> робить постійний запис про нього (як коміт).
> Якщо у вас немає нічого у зоні стейджингу, коли ви вводите `git commit`,
> Git запропонує вам використати `git commit -a` або `git commit --all`,
> який ніби збирає * всіх *, щоб зробити групове фото!
> Однак майже завжди краще
> додати речі виключно в зоні стейджингу, тому що ви можете
> комітнути зміни про які ви забули. (Повертаючись до порівняння з груповим фото,
> якщо ви використали команду "-а",
> до вашої фотографії може потрапити зайва людина!)
> Спробуйте додати речі в зону стейджингу саморуч,
> або ви можете шукати "git undo commit" більше, ніж
> вам хотілося б!
{: .callout}

![The Git Staging Area](../fig/git-staging-area.svg)

Давайте подивимося, як наші зміни в файлі переходять від нашого редактора
до зони стейджингу
і в довгострокове зберігання.
Спочатку,
ми додамо новий рядок в наш файл:

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
(показано з `+` у першій колонці).
Тепер давайте поставимо цю зміну в зоні стейджингу
і подивимося що звітує `git diff`:

~~~
$ git add mars.txt
$ git diff
~~~
{: .language-bash}

Результату немає:
наскільки Git може сказати,
немає різниці між тим, що було запропоновано зберегти назавжди
і що зараз в директорії.
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

це показує нам різницю між
останніми доданими змінами
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

і подивимося на історію, яку ми вже створили:

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

> ## Словесне порівняння
>
> Інколи, наприклад, у випадку текстових документів рядок
> diff занадто великий. Саме тут `--color-words` опція
> `git diff` є надзвичайно зручною, бо вона виділяє кольором усі зміни в коді.
{: .callout}

## Пейджинг логу 
Коли розмір результату `git log` перевищує розмір вашого екрану,
 `git` використовує спеціальну програму "пейджер", щоб поділити результат між сторінками.
Після виклику "пейджера", ви помітите, що останній рядок вашого результату закінчується на `:`, замість звичайного закінчення.

Натисніть <kbd>Q</kbd>, щоб вийти з пейджеру.
Натисніть <kbd>Spacebar</kbd>, щоб перейти на наступну сторінку.
Натисніть <kbd>/</kbd> та напишіть бажане вам слово задля пошуку його на усіх сторінках.
Натисніть <kbd>N</kbd>, для навігації між сторінками.
{: .callout}

## Ліміт розміру логу
>
>Щоб запобігти випадку, коли `git log` повністю займає ваш термінал
> ви можете лімітувати кількість комітів які надає вам Git використовуючи команду `-N`, де `N` - кількість
> комітів які ви би бажали бачити на екрані. Наприклад, якщо ви бажаєте побачити лише останній коміт,                   > використовуйте команду:
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
> Ви також можете комбінувати команду `--oneline` з іншими командами. Одна корисна
> комбінація додає `--graph` для відображення історії комітів як текстового
> графіку і для того, щоб вказати які коміти пов`язані з
> поточним `HEAD`, поточним бранчем `main`, або
> [other Git references][git-references]:
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
> 2. Якщо ви створюєте дирикторію у вашому сховищі Git і заповнюєте його файлами,
>    ви можете додати всі файли в директорії відразу:
>
>    ~~~
>    git add <directory-with-files>
>    ~~~
>    {: .language-bash}
>
>    Try it for yourself:
>
>    ~~~
>    $ touch spaceships/apollo-11 spaceships/sputnik-1
>    $ git status
>    $ git add spaceships
>    $ git status
>    ~~~
>    {: .language-bash}
>
>    Перш ніж рухатися далі, ми зробимо наступні зміни.
>
>    ~~~
>    $ git commit -m "Add some initial thoughts on spaceships"
>    ~~~
>    {: .language-bash}
{: .callout}

Для повторення: коли ми хочемо додати зміни до нашого репозиторію,
спочатку нам потрібно додати змінені файли в зону стейджингу
(`git add`) а потім комітнути стейджд зміни до
репозиторію (`git commit`):

![The Git Commit Workflow](../fig/git-committing.svg)

> ## Вибір повідомлення коміту
>
> Які з наступних повідомлень коміту будуть найбільш оптимальними для
> останнього коміту в `mars.txt`?
>
> 1. "Зміни"
> 2. "Додано рядок 'But the Mummy will appreciate the lack of humidity' до mars.txt"
> 3. "Обговорення впливу клімату Mars' на Mummy"
>
> > ## Відповідь
> > Відповідь 1 є недостатньо детальною, а мета коміту неясна;
> > та відповідь 2 є різницею використання команди "git diff", щоб побачити зміни в цьому коміті;
> > але 3 відповідь хороша: коротка, описова, та імперативна.
> {: .solution}
{: .challenge}

> ## Внесення змін до Git
>
> Яка(і) команда(и) наведені нижче збережуть зміни файлу `myfile.txt`
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
> > 1. Стоврить коміт, лише якщо файли вже були стейджд.
> > 2. Намагатиметься створити новий репозиторій.
> > 3. Правильна відповідь: спочатку додайте файл до простору стейджингу, потм зробіть коміт.
> > 4. Спробує закомітити файл "my recent changes" з повідомленням myfile.txt.
> {: .solution}
{: .challenge}

> ## Коміт декількох файлів
>
> Зона стейджингу може зберігати зміни з будь-якої кількості файлів,
> які ви хочете закомітити за один раз.
>
> 1. Додайте текст до `mars.txt` пояснюючи чому 
> Venus -- це база
> 2. Створіть новий файл `venus.txt` з вашими думками
> про Venus, як базу для вас та ваших друзів
> 3. Додайте зміни з обох файлів у зону стейджтнгу,
> та комітніть ці зміни.
>
> > ## Відповідь
> >
> > Результат нижче з файлу `cat mars.txt` відображає тільки контент доданий під час 
> > цієї вправи. Ваш результат може змінюватися.
> > 
> > Спочатку ми робимо зміни у файлі `mars.txt` та `venus.txt` files:
> > ~~~
> > $ nano mars.txt
> > $ cat mars.txt
> > ~~~
> > {: .language-bash}
> > ~~~
> > Можливо, я повинен почати з бази на Venus.
> > ~~~
> > {: .output}
> > ~~~
> > $ nano venus.txt
> > $ cat venus.txt
> > ~~~
> > {: .language-bash}
> > ~~~
> > Venus це хороша планета, і я, безумовно, повинен розглядати її як базу.
> > ~~~
> > {: .output}
> > Тепер ви можете додати обидва файли до зони стейджингу. Ми можемо зробити це в один рядок:
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
> > Тепер файли готові для коміту. Ви можете перевірити це за допомогою `git status`. Якщо ви готові зробити коміт, використайте:
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

> ## `bio` Репозиторій
>
> * Створіть новий репозиторій Git на вашому комп`ютері під назвою `bio`.
> * Напишіть собі трирядкову біографію у файл під назвою `me.txt`,
> комітніть ваші зміни
> * Змініть один рядок, додайте четвертий рядок
> * Покажіть відмінності
> між оновленим файлом та початковим.
>
> > ## Відповідь
> >
> > За необхідністю, вийдіть з папки `planets`:
> >
> > ~~~
> > $ cd ..
> > ~~~
> > {: .language-bash}
> >
> > Створіть нову папку  `bio` та 'move' в неї:
> >
> > ~~~
> > $ mkdir bio
> > $ cd bio
> > ~~~
> > {: .language-bash}
> >
> > Iніціалізуйте git:
> >
> > ~~~
> > $ git init
> > ~~~
> > {: .language-bash}
> >
> > Створіть файл `me.txt` з вашою біографією, використовуючи `nano` або інший текстовий редактор.
> > Перейшовши в правильну директорію, додайте та зробіть коміт в репозиторій:
> >
> > ~~~
> > $ git add me.txt
> > $ git commit -m "Add biography file" 
> > ~~~
> > {: .language-bash}
> >
> > Змініть файл як описано (змініть один рядок, додайте четверту лінію).
> > Для того щоб показати зміни
> > між оновленим файлом та початковим, використайте `git diff`:
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

