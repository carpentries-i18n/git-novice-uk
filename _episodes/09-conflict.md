---
title: Конфлікти
teaching: 15
exercises: 0
questions:
- "Що робити, коли мої зміни конфліктують з чужими?"
objectives:
- "Пояснити, що таке конфлікти і коли вони можуть виникнути."
- "Вирішення конфліктів, що виникають внаслідок об`єднання."
keypoints:
- "Конфлікти виникають, коли двоє або більше людей змінюють однакові рядки одного файлу."
- "Система контролю версій не дозволяє людям перезаписувати зміни один одного наосліп, але виділяє конфлікти, щоб їх можна було вирішити."
---

Як тільки люди можуть працювати паралельно, вони, швидше за все, "наступлять один
на одного".  Це станеться навіть з однією людиною: якщо ми працюємо
над частиною програмного забезпечення як на нашому ноутбуці, так і на сервері в лабораторії, ми можемо внести
різні зміни в кожну копію.  Контроль версій допомагає нам керувати ними
[конфлікти]({{ page.root}}{% link reference.md %}#conflict) надаючи нам інструменти для
[вирішення]({{ page.root }}{% link reference.md %}#resolve) змін, що перекриваються.

Щоб побачити, як ми можемо вирішити конфлікти, ми повинні спочатку створити їх. Файл
`mars.txt` в даний час виглядає так в обох копіях партнерів нашого `planets`
репозиторію:

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

Додамо лише рядок до копії колаборанта:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
This line added to Wolfman's copy
~~~
{: .output}

а потім відправимо наші зміни на GitHub:

~~~
$ git add mars.txt
$ git commit -m "Add a line in our home copy"
~~~
{: .language-bash}

~~~
[main 5ae9631] Add a line in our home copy
 1 file changed, 1 insertion(+)
~~~
{: .output}

~~~
$ git push origin main
~~~
{: .language-bash}

~~~
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 331 bytes | 331.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/vlad/planets.git
   29aba7c..dabb4c8  main -> main
~~~
{: .output}

Тепер давайте власник зробить 
іншу зміну своєї копії *без* 
оновлення з GitHub:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
We added a different line in the other copy
~~~
{: .output}

Ми можемо комітнути наші зміни локально:

~~~
$ git add mars.txt
$ git commit -m "Add a line in my copy"
~~~
{: .language-bash}

~~~
[main 07ebc69] Add a line in my copy
 1 file changed, 1 insertion(+)
~~~
{: .output}

але Git не дозволить нам відправити зміни на GitHub:

~~~
$ git push origin main
~~~
{: .language-bash}

~~~
To https://github.com/vlad/planets.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/vlad/planets.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
~~~
{: .output}

![Суперечливі зміни]({{ site.baseurl }}/fig/conflict.svg)

Git відкидає це відправлення, оскільки виявляє, що віддалений репозиторій має нові оновлення, які не були
включені до локальної гілки.
Що ми повинні зробити, це витягнути зміни з GitHub,
[об`єднати]({{ page.root }}{% link reference.md %}#merge) їх у копію, в якій ми зараз працюємо, а потім відправити їх.
Давайте почнемо з отримання змін:

~~~
$ git pull origin main
~~~
{: .language-bash}

~~~
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 2), reused 3 (delta 2), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/vlad/planets
 * branch            main     -> FETCH_HEAD
    29aba7c..dabb4c8  main     -> origin/main
Auto-merging mars.txt
CONFLICT (content): Merge conflict in mars.txt
Automatic merge failed; fix conflicts and then commit the result.
~~~
{: .output}

Команда `git pull` оновлює локальний репозиторій, щоб включити ті зміни,
які вже включені у віддаленому репозиторії.
Після того, як зміни з віддаленої гілки були отримані, Git виявляє, що зміни, внесені до локальної копії, 
перекриваються з тими, що вносяться до віддаленого сховища, і тому відмовляється об'єднати дві версії, 
щоб зупинити нас від видалення нашої попередньої роботи. Конфлікт позначений у 
файлі, що постраждав:

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
<<<<<<< HEAD
We added a different line in the other copy
=======
This line added to Wolfman's copy
>>>>>>> dabb4c8c450e8475aee9b14b4383acc99f42af1d
~~~
{: .output}

Нашим змінам передує `<<<<<<< HEAD`.
Потім Git вставив `=======` як роздільник між конфліктуючими змінами і
позначив кінець вмісту, завантаженого з GitHub за допомогою `>>>>>>>`.
(Рядок букв і цифр після цього маркера
ідентифікує щойно завантажений коміт.)

Тепер ми маємо відредагувати цей файл, щоб видалити ці маркери
та узгодити зміни.
Ми можемо зробити все, що хочемо: зберегти зміни, зроблені в локальному сховищі, зберегти
зміни, зроблені у віддаленому сховищі, написати щось нове, щоб замінити обидва,
або позбутися від змін повністю.
Давайте замінимо обидва так, щоб файл виглядав наступним чином:

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
We removed the conflict on this line
~~~
{: .output}

Щоб закінчити злиття,
ми додаємо `mars.txt` до змін, що вносяться об`єднанням, а потім
комітимо:

~~~
$ git add mars.txt
$ git status
~~~
{: .language-bash}

~~~
On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

\tmodified:   mars.txt

~~~
{: .output}

~~~
$ git commit -m "Merge changes from GitHub"
~~~
{: .language-bash}

~~~
[main 2abf2b1] Merge changes from GitHub
~~~
{: .output}

Тепер ми можемо відправити наші зміни на GitHub:

~~~
$ git push origin main
~~~
{: .language-bash}

~~~
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 8 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 645 bytes | 645.00 KiB/s, done.
Total 6 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), completed with 2 local objects.
To https://github.com/vlad/planets.git
   dabb4c8..2abf2b1  main -> main
~~~
{: .output}

Git відстежує те, що ми об'єднали з чим, 
тому нам не потрібно знову виправляти речі вручну
коли колаборант, який зробив першу зміну, знову отримує ці зміни:

~~~
$ git pull origin main
~~~
{: .language-bash}

~~~
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 6 (delta 4), reused 6 (delta 4), pack-reused 0
Unpacking objects: 100% (6/6), done.
From https://github.com/vlad/planets
 * branch            main     -> FETCH_HEAD
    dabb4c8..2abf2b1  main     -> origin/main
Updating dabb4c8..2abf2b1
Fast-forward
 mars.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~
{: .output}

Ми отримуємо об'єднаний файл:

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
We removed the conflict on this line
~~~
{: .output}

Нам не потрібно знову об`єднуватися, тому що Git знає, що хтось це вже зробив.

Здатність Git вирішувати конфлікти дуже корисна, але вирішення конфліктів 
вимагає часу і зусиль, і може виводити помилки, якщо конфлікти не вирішуються
правильно. Якщо ви виявите, що вирішуєте багато конфліктів у проєкті,
розгляньте ці технічні підходи до їх зменшення::

- Отримувати зміни частіше, особливо перед початком нової роботи
- Використовуйте тематичні гілки для розділення роботи, об'єднання до main, коли завершили
- Зробити менше величезних комітів
- Там, де логічно доречно, розбити великі файли на менші, щоб було
  менше шансів, що два автори змінять один і той самий файл одночасно

Конфлікти також можна мінімізувати за допомогою стратегій управління проєктами:

- Уточнюйте, хто відповідає за які сфери з вашими колаборантами
- Обговорити, які завдання повинні виконуватися з вашими колаборантами, щоб
  завдання, які очікують зміни одних і тих же ліній, не працювали одночасно
- Якщо конфлікти є стилістичними (наприклад, таби vs. пробіли), втсановіть
  угоду проєкту, яка регулює та використовує інструменти стилю коду (наприклад, 
  `htmltidy`, `perltidy`, `rubocop`, тд.) для виконання, якщо це необхідно

> ## Вирішення Конфліктів, які Ви Створюєте
>
> Клонуйте репозиторій, створений вашим інструктором.
> Додайте до нього новий файл,
> і змініть існуючий файл (ваш інструктор скаже, який з них).
> За вказівкою вашого інструктора,
> отримайте його зміни з репозиторію, щоб створити конфлікт,
> а потім вирішіть його.
{: .challenge}

> ## Конфлікти на Нетекстових файлах
>
> Що робить Git
> коли виникає конфлікт у зображені або іншому нетекстовому файлі,
> що зберігається в контролі версій?
>
> > ## Відповідь
> >
> > Давайте спробуємо. Припустимо, Dracula робить фотографію Марсіанської поверхні та
> > називає її `mars.jpg`.
> >
> > Якщо у вас немає файлу зображення Марса, ви мжете створити
> > фіктивний бінарний файл ось так:
> >
> > ~~~
> > $ head -c 1024 /dev/urandom > mars.jpg
> > $ ls -lh mars.jpg
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > -rw-r--r-- 1 vlad 57095 1.0K Mar  8 20:24 mars.jpg
> > ~~~
> > {: .output}
> >
> > `ls` показує нам, що було створено 1-кілобайт файл. Він повний
> > випадкових байтів, прочитаних з спеціального файлу, `/dev/urandom`.
> >
> > Тепер, припустимо, Dracula додає `mars.jpg` до його репозиторію:
> >
> > ~~~
> > $ git add mars.jpg
> > $ git commit -m "Add picture of Martian surface"
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > [main 8e4115c] Add picture of Martian surface
> >  1 file changed, 0 insertions(+), 0 deletions(-)
> >  create mode 100644 mars.jpg
> > ~~~
> > {: .output}
> >
> > Припустимо, що Wolfman тим часом додав схожу фотографію.
> > Його зображення - марсіанське небо, але воно *також* названо `mars.jpg`.
> > Коли Dracula намагається відправити зміни, він отримує знайоме повідомлення:
> >
> > ~~~
> > $ git push origin main
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > To https://github.com/vlad/planets.git
> >  ! [rejected]        main -> main (fetch first)
> > error: failed to push some refs to 'https://github.com/vlad/planets.git'
> > hint: Updates were rejected because the remote contains work that you do
> > hint: not have locally. This is usually caused by another repository pushing
> > hint: to the same ref. You may want to first integrate the remote changes
> > hint: (e.g., 'git pull ...') before pushing again.
> > hint: See the 'Note about fast-forwards' in 'git push --help' for details.
> > ~~~
> > {: .output}
> >
> > Ми дізналися, що повинні спочатку отримувати зміни і вирішувати будь-які конфлікти:
> >
> > ~~~
> > $ git pull origin main
> > ~~~
> > {: .language-bash}
> >
> > Коли виникає конфлікт на зображенні або іншому двійковому файлі, git друкує
> > таке повідомлення:
> >
> > ~~~
> > $ git pull origin main
> > remote: Counting objects: 3, done.
> > remote: Compressing objects: 100% (3/3), done.
> > remote: Total 3 (delta 0), reused 0 (delta 0)
> > Unpacking objects: 100% (3/3), done.
> > From https://github.com/vlad/planets.git
> >  * branch            main     -> FETCH_HEAD
> >    6a67967..439dc8c  main     -> origin/main
> > warning: Cannot merge binary files: mars.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
> > Auto-merging mars.jpg
> > CONFLICT (add/add): Merge conflict in mars.jpg
> > Automatic merge failed; fix conflicts and then commit the result.
> > ~~~
> > {: .output}
> >
> > Повідомлення про конфлікт тут в основному таке ж, як і для `mars.txt`, але 
> > є один ключовий рядок:
> >
> > ~~~
> > warning: Cannot merge binary files: mars.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
> > ~~~
> > {: .output}
> >
> > Git не може автоматично вставляти конфліктні маркери в зображення, як це робить
> > для текстових файлів. Отже, замість редагування файлу зображення, ми повинні перевірити
> > tверсію, яку ми хочемо зберегти. Потім ми можемо додати і комітнути цю версію.
> >
> > На ключовій лінії вище, Git зручно дав нам ідентифікатори коміту
> > для двох версій `mars.jpg`. Наша версія `HEAD`, а версія Wolfman 
> > `439dc8c0...`. Якщо ми хочемо використовувати нашу версію, ми можемо використати
> > `git checkout`:
> >
> > ~~~
> > $ git checkout HEAD mars.jpg
> > $ git add mars.jpg
> > $ git commit -m "Use image of surface instead of sky"
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > [main 21032c3] Use image of surface instead of sky
> > ~~~
> > {: .output}
> >
> > Якщо замість цього ми хочемо використовувати версію Wolfman, ми можемо використовувати `git checkout`
> > з ідентифікатором коміту Wolfman, `439dc8c0`:
> >
> > ~~~
> > $ git checkout 439dc8c0 mars.jpg
> > $ git add mars.jpg
> > $ git commit -m "Use image of sky instead of surface"
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > [main da21b34] Use image of sky instead of surface
> > ~~~
> > {: .output}
> >
> > Ми також можемо зберігати *обидва* зображення. Загвоздка в тому, що ми не можемо їх залишити
> > під однією й тією ж назвою. Але, ми можемо перевірити кожну версію послідовно
> > і *перейменувати* файл, потім додати перейменовані версії. Спочатку перевірте кожне 
> > зображення і перейменуйте його:
> >
> > ~~~
> > $ git checkout HEAD mars.jpg
> > $ git mv mars.jpg mars-surface.jpg
> > $ git checkout 439dc8c0 mars.jpg
> > $ mv mars.jpg mars-sky.jpg
> > ~~~
> > {: .language-bash}
> >
> > Потім видаліть старий `mars.jpg` і додайте два нових файли:
> >
> > ~~~
> > $ git rm mars.jpg
> > $ git add mars-surface.jpg
> > $ git add mars-sky.jpg
> > $ git commit -m "Use two images: surface and sky"
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > [main 94ae08c] Use two images: surface and sky
> >  2 files changed, 0 insertions(+), 0 deletions(-)
> >  create mode 100644 mars-sky.jpg
> >  rename mars.jpg => mars-surface.jpg (100%)
> > ~~~
> > {: .output}
> >
> > Тепер обидва зображення Марса перевіряються в репозиторії, і `mars.jpg`
> > більше не існує.
> {: .solution}
{: .challenge}

> ## Типова Робоча Сесія
>
> Ви сідаєте за комп'ютер, щоб працювати над спільним проєктом, який відстежується у
> віддаленому репозиторії Git. Під час робочого сеансу ви приймаєте наступні
> дії, але не в цьому порядку:
>
> - *Внесіть зміни *, додавши число '100' до текстового файлуe `numbers.txt`
> - *Оновіть віддалений* репозиторій, щоб відповідати локальному репозиторію
> - *Святкуйте* свій успіх з якимось напоєм(-ями)
> - *Оновіть локальний* репозиторій, щоб відповідати віддаленому репозиторію
> - *Перенесіть зміни в зону стейджингу*, щоб зробити коміт
> - *Комітніть зміни* до локального репозиторію
>
> В якому порядку слід виконувати ці дії, щоб мінімізувати шанси 
> конфліктів? Наведіть команди вище в порядок, у стовпці *action* в таблиці
> нижче. Коли ви розставите все у відповідному порядку, подивіться, чи можете ви написати відповідні команди в стовпці
> *command*. Кілька кроків вже заповнені, щоб допомогти вам 
> розпочати.
>
> |order|action . . . . . . . . . . |command . . . . . . . . . . |
> |-----|---------------------------|----------------------------|
> |1    |                           |                            |
> |2    |                           | `echo 100 >> numbers.txt`  |
> |3    |                           |                            |
> |4    |                           |                            |
> |5    |                           |                            |
> |6    | Celebrate!                | `AFK`                      |
>
> > ## Відповідь
> >
> > |order|action . . . . . . |command . . . . . . . . . . . . . . . . . . . |
> > |-----|-------------------|----------------------------------------------|
> > |1    | Update local      | `git pull origin main`                     |
> > |2    | Make changes      | `echo 100 >> numbers.txt`                    |
> > |3    | Stage changes     | `git add numbers.txt`                        |
> > |4    | Commit changes    | `git commit -m "Add 100 to numbers.txt"`     |
> > |5    | Update remote     | `git push origin main`                     |
> > |6    | Celebrate!        | `AFK`                                        |
> {: .solution}
{: .challenge}

