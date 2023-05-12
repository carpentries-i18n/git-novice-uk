---
title: Цитування
teaching: 2
exercises: 0
questions:
- "Як полегшити мою роботу для цитування?"
objectives:
- "Зробити вашу роботу легко цитуємою"
keypoints:
- "Додайте файл CITATION до репозиторію, щоб пояснити, як ви хочете цитувати свою роботу."
---

Ви можете додати файл під назвою `CITATION` або `CITATION.txt`,
який описує, як цитувати ваш проєкт;
[один для Software
Carpentry](https://github.com/swcarpentry/website/blob/gh-pages/CITATION)
states:

~~~
To reference Software Carpentry in publications, please cite both of the following:

Greg Wilson: "Software Carpentry: Getting Scientists to Write Better
Code by Making Them More Productive".  Computing in Science &
Engineering, Nov-Dec 2006.

Greg Wilson: "Software Carpentry: Lessons Learned". arXiv:1307.5448,
July 2013.

@article{wilson-software-carpentry-2006,
    author =  {Greg Wilson},
    title =   {Software Carpentry: Getting Scientists to Write Better Code by Making Them More Productive},
    journal = {Computing in Science \& Engineering},
    month =   {November--December},
    year =    {2006},
}

@online{wilson-software-carpentry-2013,
  author      = {Greg Wilson},
  title       = {Software Carpentry: Lessons Learned},
  version     = {1},
  date        = {2013-07-20},
  eprinttype  = {arxiv},
  eprint      = {1307.5448}
}
~~~
{: .source}

Більш детальні поради та інші способи зробити ваш код цитованим можна знайти
[в блозі Software Sustainability Institute](https://www.software.ac.uk/how-cite-and-describe-software) and in:

~~~
Smith AM, Katz DS, Niemeyer KE, FORCE11 Software Citation Working Group. (2016) Software citation
principles. [PeerJ Computer Science 2:e86](https://peerj.com/articles/cs-86/)
https://doi.org/10.7717/peerj-cs.8
~~~
{: .source}


Також є [`@software{...`](https://www.google.com/search?q=git+citation+%22%40software%7B%22)
[BibTeX](https://www.ctan.org/pkg/bibtex) тип запису, якщо немає 
«парасольки» цитування, як стаття або книга для проєкту, який ви хочете
процитувати.

