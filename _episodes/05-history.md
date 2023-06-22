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
> Дженніфер внесла зміни в Python скрипт, над яким вона працювала тижнями, та
> зміни, які вона внесла цього ранку "зламали" скрипт і він більше на запускається. Вона витратила
> ~ 1 годину, намагаючись виправити все, без удачі...
>
> На щастя, вона відстежує версії свого проєкту за допомогою Git! Які команди нижче допоможуть
> їй відновити останню закомічену її Python скрипту, який називається
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
> 5. Обидві 2 та 4
>
>
> > ## Відповідь
> >
> > Відповідь (5)-Обидві 2 та 4. 
> > 
> > Команда `checkout` command відновлює файли зі сховища, перезаписуючи файли у вашій робочій 
> > директорії. Відровіді 2 та 4 обидві відновлюють *останню* версію * в репозиторії * файлу
> > `data_cruncher.py`. Відповідь 2 використовує `HEAD` щоб вказати *останній*, тоді як відповідь 4 використовує 
> > унікальний ID останнього коміту, що означає `HEAD`. 
> > 
> > Відповідь 3 отримує версію `data_cruncher.py` з коміту *перед* `HEAD`, що є НЕ 
> > тим, що ми хотіли.
> > 
> > Відповідь 1 може бути небезпечною! Без назви файлу, `git checkout` відновить **всі файли** 
> > у поточній директорії (і всі попередні директорії) до їх стану, вказаному в коміті. 
> > Ця команда відновить `data_cruncher.py` до останньої версії коміту, але вона також 
> > відновить *будь-які інші файли, які змінено* на цю версію, стираючи будь-які зміни, які ви могли
> > внести до цих файлів!
> > Як вже говорилося вище, ви залишилися в *відстороненому* стані `HEAD`, і ви не хочете бути там.
> {: .solution}
{: .challenge}

> ## Повернення Коміту
>
> Дженніфер співпрацює з колегами над її Python скриптом. Вона
> усвідомлює, що її останній коміт до репозиторію проєкту містив помилку, і 
> хоче його скасувати. Дженніфер хоче скасувати правильно, так що всі в репозиторії проєкту
> отримають правильні зміни. Команда `git revert [erroneous commit ID]` створить 
> новий коміт, який повертає помилковий коміт
>    
> Команда `git revert`
> відрізняється від `git checkout [commit ID]`, оскільки `git checkout` повертає
> файли ще не передані в локальному репозиторії до попереднього стану, тоді як `git revert`
> повертає зміни, внесені до локальних та проєктних репозиторій.   
>   
> Нижче наведені правильні кроки і пояснення для Дженніфер користування `git revert`,
> яка відсутня команда?  
> 1. `________ # Подивіться на історію git проєкту, щоб знайти ідентифікатор коміту`
>
> 2. Скопіювати ID (перші кілька символів ID, наприклад 0b1d055).
>
> 3. `git revert [commit ID]`
>
> 4. Введіть нове повідомлення коміту.
>
> 5. зберегти та закрити.
> 
> 
> > ## Відповідь
> > 
> > Команда `git log` перераховує історію проєкту з ідентифікаторами комітів.  
> > 
> > Команда `git show HEAD` показує зміни, внесені в останній коміт, і перераховує
> > ID комітів; однак Дженніфер повинна перевірити, що це правильний коміт, і більше ніхто
> > не вніс змін до репозиторію.
> {: .solution}
{: .challenge}

> ## Розуміння Робочого Процесу та Історії
>
> Який результат останньої команди в
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
>    Помилка, тому що ви змінили venus.txt без коміту змін
>    ~~~
>    {: .output}
>
> > ## Відповідь
> >
> > Відповідь 2. 
> > 
> > Команда `git add venus.txt` розміщує поточну версію 'venus.txt' в зоні стейджингу. 
> > Зміни до файлу з другої команди «echo» застосовуються лише до робочої копії, 
> > не версії в зоні стейджингу.
> > 
> > Тож, коли виконується `git commit -m "Comment on Venus as an unsuitable base"`, 
> > версія `venus.txt` комітнута до репозиторію, яка є із зони стейджингу та
> > має тільки один рядок.
> >  
> >  На цей час робочий екземпляр ще має другий рядок (та 
> >  `git status` покаже, що файл змінено). Однак `git checkout HEAD venus.txt` 
> >  використовує робочу копію з найновішою версією `venus.txt`.
> >  
> >  Тож, `cat venus.txt` покаже 
> >  ~~~
> >  Venus is beautiful and full of love.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## Перевірка Розуміння "git diff'
>
> Розгляньте цю команду: `git diff HEAD~9 mars.txt`. Як ви думаєте,
> що зробить ця команда, якщо її запустити? Що відбувається, коли ви запускаєте її? Чому?
>
> Спробуйте іншу команду, `git diff [ID] mars.txt`, де [ID] замінюється на
> унікальний ідентифікатор вашого останнього коміту. Як ви думаєте, що станеться,
> і що відбувається?
{: .challenge}

> ## Позбавлення від Змін у Зоні Стейджингу
>
> `git checkout` може бути використаний для відновлення попереднього коміту, коли зміни (ще не в зоні стейджингу) були внесені,
> але чи спрацює вона і для змін, які були занесені до зони стейджингу, але не комітнуті?
> Внесіть зміни до `mars.txt`, додайте зміну, та використайте `git checkout`, щоб побачити чи
> можете ви повернути свою зміну.
{: .challenge}

> ## Досліджуйте та Узагальнюйте Історії
>
> Вивчення історії є важливою частиною Git, і часто це виклик знайти
> правильний ID коміту, особливо якщо коміт був зроблений декілька місяців тому.
>
> Уявіть `planets` проєкт має біль 50 файлів.
> Ви хотіли б знайти коміт, який змінює певний текст у `mars.txt`.
> Коли ви вводите `git log`, з'явився дуже довгий список.
> Як можна звузити пошук?
>
> RНагадаємо, що команда `git diff` дозволяє відсліжувати один конкретний файл,
> наприклад, `git diff mars.txt`. Подібну ідею ми можемо застосувати тут.
>
> ~~~
> $ git log mars.txt
> ~~~
> {: .language-bash}
>
> На жаль, деякі з цих повідомлень комітів дуже неоднозначні, наприклад, `update files`.
> Як ви можете шукати через ці файли?
>
> Обидві `git diff` та `git log` дуже корисні та вони підсумовують іншу частину історії 
> для вас.
> Чи можна об'єднати обох? Спробуємо наступне:
>
> ~~~
> $ git log --patch mars.txt
> ~~~
> {: .language-bash}
>
> Ви повинні отримати довгий список, і ви повинні мати можливість бачити обидва повідомлення коміту і 
> tрізницю між кожним комітом.
>
> Питання: Що робить наступна команда?
>
> ~~~
> $ git log --patch HEAD~9 *.txt
> ~~~
> {: .language-bash}
{: .challenge}

