Установка .Net Framework 3.5 на Windows 8.1 или Windows 8 часто является проблемой. В сети много решений как минимум 2. Я покажу одно из них и ещё одно о котором мало кто знает!

Вообще если лень читать можете посмотретьтам я все показываю на живом примере!

<iframe allow="autoplay; encrypted-media" allowfullscreen="" frameborder="0" height="315" src="https://www.youtube.com/embed/2vGP-1dqXAw" width="560"></iframe>Ну а кто любит читать, то let's go...

**Способ N1**

Запускаем командную строку от имени администратора, для этого перемещаем курсор мыши в левый нижний угол и нажимаем правую кнопку мыши и конечно же запускаем командную строку от имени администратора.

После появится командная строка. Сперва нам нужно перейти в `C:\WINDOWS\system32>`

Для этого набираем команду `cd C:\WINDOWS\system32`

Теперь нам нужна копия операционной системы Windows 8 или другой. Открываем файловую систему диска или образа с осью находите папку source и копируем папку в любое удобное место например на диск C:\\

Теперь перейдем непосредственно к установке dot net'a.

В командную строку необходимо записать вот эту строку:

```
dism.exe /<span class="hljs-constant">Online</span> /<span class="hljs-constant">Enable</span>-<span class="hljs-constant">Feature</span> /<span class="hljs-constant">FeatureName</span><span class="hljs-symbol">:NetFx3</span> /<span class="hljs-constant">All</span> /<span class="hljs-constant">Source</span><span class="hljs-symbol">:c</span><span class="hljs-symbol">:source</span>\sxs /<span class="hljs-constant">LimitAccess</span>

```

Важный параметр это Source: сюда пишем путь к папке source, а так же к папке sxs, которая находится в папке source. После нажимаем кнопку Enter.

> Если установка прошла успешно, то в консоли появится сообщение об этом и установка пройдёт до 100%, в таком случае Вы можете закрыть эту статью и рассказывать всем, что Вы смогли установить .Net Framework 3.5 на Windows 8.1.

Если же произойдёт ошибка то скорее всего загрузка достигнет ~36% и остановится, в таком случае переходим к способу 2.

Вообще проблема установки .Net Framework 3.5 на Windows 8.1 происходит из обновления операционной системы `(KB2966826 и KB2966828)`. Короче говоря, чтобы успешно установить то, что нам нужно, эти обновления нужно удалить, ну а потом снова поставить!

И так. Перейдем в Панель управления -> Программы -> Программы и компоненты -> Установленные обновления. И там удаляем эти 2 обновления.

И ставим `.Net Framework 3.5` так как вам нравится