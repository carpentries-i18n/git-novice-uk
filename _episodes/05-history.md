---
title: Перегляд історії
teaching: 25
exercises: 0
questions:
- "Як визначити старі версії файлів?"
- "Як переглянути свої зміни?"
- "Як відновити старі версії файлів?"
objectives:
- "Пояснення що таке HEAD репозиторію та як його використовувати."
- "Визначення та використання відносних номерів комітів Git."
- "Порівняння різних версій відслідковуваних файлів."
- "Відновлення старих версій файлів."
keypoints:
- "`git diff` відображає відмінності між файлами."
- "`git checkout` відновлює старі версії файлів."
---

Як ми побачили у попередньому епізоді, ми можемо посилатися на коміти за їх
ідентифікаторами. Ви можете звернутися до _останнього коміту_ робочої
директорії, використовуючи ідентифікатор `HEAD`.

Кожного разу ми додавали тільки один рядок до `mars.txt`, тож нам буде легко відслідкувати досягнутий
прогрес. Отже, давайте зробимо це, використовуючи `HEAD`.  Перш ніж ми почнемо,
давайте внесемо зміни до `mars.txt`, додавши ще один рядок.

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
An ill-considered change
~~~
{: .output}

Тепер давайте подивимося на наш результат.

~~~
$ git diff HEAD mars.txt
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index b36abfd..0848c8d 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,3 +1,4 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
 But the Mummy will appreciate the lack of humidity
+An ill-considered change.
~~~
{: .output}

Він такий ж самий, який би ви отримали без використання `HEAD` (спробуйте).  Справжня
перевага від цього відчувається коли ви можете посилатися на попередні коміти.  Ми можемо
робити це, додаючи `~1` 
(де "~" - це тільда), 
щоб звернутися до коміту зробленому безпосередньо перед `HEAD`.

~~~
$ git diff HEAD~1 mars.txt
~~~
{: .language-bash}

Якщо ми хочемо побачити різницю між старішими комітами, ми можемо знову використати `git diff`,
проте, з нотацією `HEAD~1`, `HEAD~2`, і так далі, щоб звернутися до них:


~~~
$ git diff HEAD~3 mars.txt
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
~~~
{: .output}

Ми також можемо використати команду `git show`, яка показує які зміни ми внесли у будь-якому попередньо зробленому коміті, а також і повідомлення коміту, на відміну від команди `git diff` 
яка покаже _різницю_ між комітом та нашою 
робочою директорією.

~~~
$ git show HEAD~3 mars.txt
~~~
{: .language-bash}

~~~
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base

diff --git a/mars.txt b/mars.txt
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/mars.txt
@@ -0,0 +1 @@
+Cold and dry, but everything is my favorite color
~~~
{: .output}

Таким чином,
ми можемо створити послідовність комітів.
Найпізніший кінець послідовності позначається як `HEAD`;
ми можемо посилатися на попередні коміти, використовуючи позначення `~`,
тож `HEAD~1`
означає "попередній коміт",
а `HEAD~123` повертається на 123 коміти назад від точки, де ми зараз знаходимось.

Ми також можемо вказувати на коміти, використовуючи
ті довгі рядки цифр і букв,
які показує `git log`.
Це унікальні ідентифікатори змін,
де "унікальні" дійсно означає унікальні:
кожна зміна будь-якого набору файлів на будь-якому комп`ютері
має унікальний 40-символьний ідентифікатор.
Наш перший коміт отримав ідентифікатор 
`f22b25e3233b4645dabd0d81e651fe074bd8e73b`,
тож давайте спробуємо наступне:

~~~
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b mars.txt
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index df0654a..93a3e13 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
~~~
{: .output}

Це правильна відповідь,
проте, введення випадкових 40-символьних рядків незручно,
тож Git дозволяє нам використовувати тільки перші кілька символів (як правило, сім для проєктів нормального розміру):

~~~
$ git diff f22b25e mars.txt
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index df0654a..93a3e13 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
~~~
{: .output}

Дуже добре! Тож
ми можемо зберегти зміни в файли і подивитися, що ми змінили. Тепер, як
ми можемо відновити старіші версії речей?
Припустимо, що ми передумали щодо останнього оновлення файлу
`mars.txt` (рядок "ill-considered change").

`git status` тепер каже нам, що файл був змінений,
але ці зміни не були додані до зони стейджингу:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Ми можемо повернути усе як було
за допомогою `git checkout`:

~~~
$ git checkout HEAD mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

Як ви можете здогадатися з назви цієї команди,
`git checkout` знаходить (тобто відновлює) стару версію файлу.
В цьому випадку,
ми кажемо Git що ми бажаємо відновити версію файлу, записану в `HEAD`
(тобто у останньому зробленому коміті).
Якщо ми хочемо повернутися ще раніше,
замість цього ми можемо використовувати ідентифікатор коміту:

~~~
$ git checkout f22b25e mars.txt
~~~
{: .language-bash}

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   mars.txt

~~~
{: .output}

Звернуть увагу, що в даний момент зміни знаходяться в зоні стейджінгу.
Знову ж таки, ми можемо повернути речі назад, так як вони були,
використовуючи `git checkout`:

~~~
$ git checkout HEAD mars.txt
~~~
{: .language-bash}

> ## Не Загубіть Ваш HEAD
>
> Вище ми використовували
>
> ~~~
> $ git checkout f22b25e mars.txt
> ~~~
> {: .language-bash}
>
> щоб повернути `mars.txt`до свого стану після комміту `f22b25e`. Проте, будьте обережні! 
> Команда `checkout` має інші важливі вараінти використання, та Git не зрозуміє
> ваших намірів, якщо ви будете неточно вводити команди. Наприклад, 
> якщо ви забудете `mars.txt` у попередній команді.
>
> ~~~
> $ git checkout f22b25e
> ~~~
> {: .language-bash}
> ~~~
> Note: checking out 'f22b25e'.
>
> You are in 'detached HEAD' state. You can look around, make experimental
> changes and commit them, and you can discard any commits you make in this
> state without impacting any branches by performing another checkout.
>
> If you want to create a new branch to retain commits you create, you may
> do so (now or later) by using -b with the checkout command again. Example:
>
> git checkout -b 1
>
> HEAD is now at f22b25e Start notes on Mars as a base
> ~~~
> {: .error}
>
> Таким чином, стан"detached HEAD" - це наче "дивися, але не чіпай",
> так що ви не маєте робити ніяких змін в цьому стані.
> Після дослідження попереднього стану вашого репозиторію, "прикріпіть" `HEAD` за допомогою команди `git checkout main`.
{: .callout}

Важливо пам`ятати , що
ми повинні використовувати номер коміту, який ідентифікує стан репозиторію
*перед* зміною, яку ми намагаємося скасувати.
Поширена помилка - використовувати номер коміту,
в якому ми зробили зміни, які намагаємося скасувати.
У наведеному нижче прикладі ми хочемо отримати стан перед самим
нещодавнім комітом (`HEAD~1`), тобто коміт `f22b25e`:

![Git Checkout](../fig/git-checkout.svg)

Отже, якщо скласти це все разом,
то Git працює як зображено у цьому коміксі:

![https://figshare.com/articles/How_Git_works_a_cartoon/1328266](../fig/git_staging.svg)

> ## Спрощення типової ситуації
>
> Якщо уважно прочитати результат команди `git status`,
> ви побачите, що він включає в себе цю підказку:
>
> ~~~
> (use "git checkout -- <file>..." to discard changes in working directory)
> ~~~
> {: .output}
>
> Як сказано раніше,
> `git checkout` без ідентифікатора версії відновлює файли до стану, збереженого в `HEAD`.
> Подвійне тире `--` необхідне щоб відділити імена файлів, які відновлюються,
> від самої команди:
> без подвійного тире
> Git буде намагатися використати назву файлу як ідентифікатор коміту.
{: .callout}

Той факт, що файли можна відновлювати окремо,
сприяє змінам в організації роботи.
Якщо все знаходиться в одному величезному документі,
буде важко (але не неможливо) скасувати зміни у вступі
без скасування також змін, внесених пізніше до висновку.
З іншого боку,
якщо вступ і висновок зберігаються в окремих файлах,
рухатися вперед і назад в часі стає набагато легше.

> ## Відновлення старих версій файлу
>
> Дженніфер зробила цього ранку деякі зміни у скрипті Python, над яким вона до цього працювала тижнями, та
>  "зламала" його - він більше на запускається. Вона витратила
> майже одну годину, намагаючись виправити його, але безуспішно...
>
> На щастя, вона використовує Git для відстеження змін у свому проєкті! Які з наведених нижче команд допоможуть
> їй відновити останню збережену у Git версію її скрипту, який називається
> `data_cruncher.py`?
>
> 1. `$ git checkout HEAD`
>
> 2. `$ git checkout HEAD data_cruncher.py`
>
> 3. `$ git checkout HEAD~1 data_cruncher.py`
>
> 4. `$ git checkout <unique ID of last commit> data_cruncher.py`
>
> 5. Обидві відповіді 2 та 4
>
>
> > ## Відповідь
> >
> > Відповідь (5) - Обидві відповіді 2 та 4. 
> > 
> > Команда `checkout` відновлює файли з репозиторію, перезаписуючи файли у вашій робочій 
> > директорії. Відповіді 2 та 4 обидві відновлюють *останню* версію * в репозиторії * файлу
> > `data_cruncher.py`. Відповідь 2 використовує `HEAD` щоб вказати *останній* коміт, тоді як відповідь 4 використовує 
> > унікальний ID останнього коміту, що саме і означає `HEAD`. 
> > 
> > Відповідь 3 замінить `data_cruncher.py` його версією з коміту *перед* `HEAD`, що НЕ є
> > тим, що ми хотіли.
> > 
> > Відповідь 1 може бути небезпечною! Без назви файлу, `git checkout` відновить **всі файли** 
> > у поточній директорії (і її піддиректоріях) до їх стану згідно із вказаним комітом. 
> > Ця команда відновить `data_cruncher.py` до його останньої збереженої версії, але вона також 
> > відновить *всі інші файли, які було змінено* на цю ж версію, стираючи будь-які зміни, які ви могли
> > внести до цих файлів!
> {: .solution}
{: .challenge}

> ## Скасування коміту
>
> Дженніфер співпрацює з колегами над її скриптом Python. Вона
> зрозуміла, що її останній коміт до репозиторію проєкту містив помилку, і 
> хоче його скасувати. Дженніфер хоче скасувати його правильним чином, щоб всі користувачі репозиторію цього проєкту
> отримали правильні зміни. Команда `git revert [erroneous commit ID]` створить 
> новий коміт, який скасує помилковий коміт
>    
> Команда `git revert`
> відрізняється від `git checkout [commit ID]`, оскільки `git checkout` повертає
> файли, зміни у яких ще не ввійшли до нового коміту у локальному репозиторїї, до їх попереднього стану, тоді як `git revert`
> скасовує зміни, які вже внесені до локальних та віддалених репозиторіїв.
>   
> Нижче наведені правильні кроки і пояснення для Дженніфер щодо користування `git revert`.
> Яка команда відсутня?  
> 1. `________ # Подивіться на історію змін, щоб знайти ідентифікатор коміту`
>
> 2. Скопіюйте цей ID (перші кілька символів ID, наприклад 0b1d055).
>
> 3. `git revert [commit ID]`
>
> 4. Введіть нове повідомлення коміту.
>
> 5. Збережіть його та закрийте редактор.
> 
> 
> > ## Відповідь
> > 
> > Команда `git log` відображає історію проєкту з ідентифікаторами комітів.  
> > 
> > Команда `git show HEAD` покаже зміни, зроблені у останньому коміті, і відобразить
> > його ID; однак Дженніфер повинна перевірити, що це правильний коміт, і більше ніхто
> > не вніс змін до репозиторію.
> {: .solution}
{: .challenge}

> ## Розуміння послідовності дій та історії
>
> Який результат останньої команди в цій послідовності
>
> ~~~
> $ cd planets
> $ echo "Venus is beautiful and full of love" > venus.txt
> $ git add venus.txt
> $ echo "Venus is too hot to be suitable as a base" >> venus.txt
> $ git commit -m "Comment on Venus as an unsuitable base"
> $ git checkout HEAD venus.txt
> $ cat venus.txt #this will print the contents of venus.txt to the screen
> ~~~
> {: .language-bash}
>
> 1. ~~~
>    Venus is too hot to be suitable as a base
>    ~~~
>    {: .output}
> 2. ~~~
>    Venus is beautiful and full of love
>    ~~~
>    {: .output}
> 3. ~~~
>    Venus is beautiful and full of love
>    Venus is too hot to be suitable as a base
>    ~~~
>    {: .output}
> 4. ~~~
>    Помилка, тому що ви змінили venus.txt, але після цього не зберегли ці зміни у коміті
>    ~~~
>    {: .output}
>
> > ## Відповідь
> >
> > Правильною є відповідь 2. 
> > 
> > Команда `git add venus.txt` розміщує поточну версію 'venus.txt' в зоні стейджингу. 
> > Зміни до файлу з другої команди `echo` будуть зроблені лише у робочій копії цього файлу, 
> > але не у його версії в зоні стейджингу.
> > 
> > Тож, коли виконується команда `git commit -m "Comment on Venus as an unsuitable base"`, 
> > версія `venus.txt`, яка буде збережена у коміті, буде з зони стейджингу та
> > буде мати тільки один рядок.
> >  
> >  На цей час робоча копія файлу ще має другий рядок (і тому 
> >  `git status` покаже, що файл змінено). Однак `git checkout HEAD venus.txt` 
> >  замінить робочу копію останньою збереженою версією `venus.txt`.
> >  
> >  Тож, `cat venus.txt` покаже 
> >  ~~~
> >  Venus is beautiful and full of love.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## Перевірка розуміння `git diff`
>
> Подивіться на цю команду: `git diff HEAD~9 mars.txt`. Як ви думаєте,
> що робить ця команда? Що відбувається, коли ви запускаєте її? Чому?
>
> Спробуйте іншу команду, `git diff [ID] mars.txt`, де [ID] замінюється на
> унікальний ідентифікатор вашого останнього коміту. Як ви думаєте, що вона зробить?
> Запустіть її та перевіірте, чи це так.
{: .challenge}

> ## Скасування змін у зоні стейджингу
>
> Команда `git checkout` може бути використана для відновлення попереднього коміту, коли зміни
> були зроблені, але ще не додані до зони стейджингу).
> Чи спрацює вона і для змін, які були додані до зони стейджингу, але не ще збережені у коміті?
> Зробіть зміни у `mars.txt`, додайте їх до зони стейджингу з `git add`, та використайте `git checkout` щоб побачити чи
> можете ви скасувати свої зміни.
{: .challenge}

> ## Перегляд історії
>
> Перегляд історії є важливою частиною роботи з Git, але часто нелегко знайти
> правильний ID коміту, особливо якщо коміт був зроблений декілька місяців тому.
>
> Уявіть, що проект `planets` має більш ніж 50 файлів.
> Ви хотіли б знайти коміт, який змінює певний текст у `mars.txt`.
> Коли ви вводите `git log`, з'являється дуже довгий список.
> Як можна звузити пошук?
>
> Нагадаємо, що команда `git diff` може використовуватись для одного конкретного файлу,
> наприклад, `git diff mars.txt`. Подібну ідею ми можемо застосувати тут.
>
> ~~~
> $ git log mars.txt
> ~~~
> {: .language-bash}
>
> На жаль, деякі з цих повідомлень комітів дуже неоднозначні, наприклад, `update files`.
> Як же перевірити усі ці версії файлу?
>
> Обидві `git diff` та `git log` дуже корисні для отримання звітів про різні деталі історії проекту.
> Чи можна об'єднати їх результат у одну команду? Спробуємо наступне:
>
> ~~~
> $ git log --patch mars.txt
> ~~~
> {: .language-bash}
>
> Ви повинні отримати довгий список, у якому ви побачите s повідомлення коміту, і зроблені зміни.
>
> Питання: Що робить наступна команда?
>
> ~~~
> $ git log --patch HEAD~9 *.txt
> ~~~
> {: .language-bash}
{: .challenge}

