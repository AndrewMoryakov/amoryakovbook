Часто настраивая игнорирование файлов в git, это не получается с первого раза. В связи с этим на форумах часто можно встретить такие вопросы как:

Проблема/issue

> #### .gitignore Gitignore not working
> 
> #### .gitignore не работает
> 
> #### не могу создать .gitignore
> 
> #### Can't create .gitignore

Решение/solution

Действительно в windows могут быть проблемы с созданием этого файла. Я по шагам распишу как настроить игнорирование. - - - - - -

1. Создайте файл .gitignore. Это можно сделать командой:

```
<span class="hljs-title">powershell</span> -c <span class="hljs-string">"[io.file]::WriteAllText('.gitignore','',[System.Text.Encoding]::ASCII)"</span>

```

2. Если вы создали файл .gitignore после того, как создали репозиторий и добавили файлы выполните следующие команды:

```
git rm -rf <span class="hljs-comment">--cached .</span>
git add .

```

Если вы создаете .gitignore для проекта в visual studio то [тут](http://amoryakov.ru/blog/git/gi.html) можно найти типичный .gitignore