.net core позволяет настраивать "переменную среды", чтобы менять поведение приложения. В частности хочу показать как включить расширенный вывод сообщени об ошибке в asp.core.

- - - - - -

`ASPNETCORE_ENVIRONMENT` - из коробки поддерживает 3 значения:

- Development
- Staging
- Production

В процессе разработке обычно устанавливают `Development`, после релиза продукта желательно установить `ASPNETCORE_ENVIRONMENT` в значение `Production`. Оставлять в продакшене(в релизе) Development не безопасно.

Если установить значение Production, то приложениее становится несколько производительнее и безопаснее, но повторюсь, что при обычной разработке это не нужно.

- - - - - -

Всё что выше это краткое вступление.

Задача/challenge

Когда мы публикуем приложение на производственную среду (сайт на хостинг) не смотря на то, что у нас на компьютере приложение работало, в производственной среде оно может не функционировать. (Хотя тут надо использовать Докер, но это уже вообще отдельная история) Если ASPNETCORE\_ENVIRONMENT установлена не как Development или вовсе не установлена, то при возникновении ошибки, мы не увидим подробности, которые помогут нам в её решении. Вместо подробного сообщения мы увидим сообщение с текстом: > #### Swapping to Development environment will display more detailed information about the error that occurred. 
> 
> #### Development environment should not be enabled in deployed applications, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the ASPNETCORE\_ENVIRONMENT environment variable to Development, and restarting the application.

Как раз у меня в сервисе bebebooking такая ситуация: ![](/Images/envermentsvariablesbeb.jpg)

С точки зрения безопасности это хорошо. Но если нам нужно понять проблему, то эта информация нам ничего не даст.  
(Конечно ошибки нужно логировать и смотреть логи, а переменную среды не трогать, но это отдельная тема. Просто не всегда удобно посмотреть логи.)

- - - - - -

Решение/solution

Поэтому иногда на время может быть полезным установить среду как Development.   
В результате мы увидим подробное сообщение об ошибке. В asp.core в web.config мы устанавливаем следущее:

```xml
<span class="hljs-tag"><<span class="hljs-title">aspNetCore</span> <span class="hljs-attribute">processPath</span>=<span class="hljs-value">".\DreamPlace.Web.Beepbooking.exe"</span> <span class="hljs-attribute">stdoutLogEnabled</span>=<span class="hljs-value">"false"</span> <span class="hljs-attribute">stdoutLogFile</span>=<span class="hljs-value">".\logs\stdout"</span> ></span>
       <span class="hljs-tag"><<span class="hljs-title">environmentVariables</span>></span>
          <span class="hljs-tag"><<span class="hljs-title">environmentVariable</span> <span class="hljs-attribute">name</span>=<span class="hljs-value">"ASPNETCORE_ENVIRONMENT"</span> <span class="hljs-attribute">value</span>=<span class="hljs-value">"Development"</span> /></span>
        <span class="hljs-tag"></<span class="hljs-title">environmentVariables</span>></span>
       <span class="hljs-tag"></<span class="hljs-title">aspNetCore</span>></span>
    <span class="hljs-tag"></<span class="hljs-title">system.webServer</span>></span>

```

В результате ![](/Images/errenverbebdevelop.jpg)