После обновления NuGet пакетов в своём проекте MVC у меня появилась ошибка

> #### HTTP Error 500.23 - Internal Server Error
> 
> ##### Запрашиваемая страница не доступна из-за неверной конфигурации данных для этой страницы.

 Решение/solution

Проблема связанна с тем, что IIS 7,x работает в интегрированном режиме. Чтобы решить проблему нужно модифицировать Web.Config. **Способ 1** ( не рекомендуется) Добавьте в раздел `<system.webServer>` элемент `<validation validateIntegratedModeConfiguration="false"/>`Проблема в том, что разрешены обработчики, которые не используются, это может нести угрозу безопасности.

**Способ 2**В разделе `<system.web>` удалите или закомментируйте тег `<httpHandlers>` и всё его содержимое. В результате содержимое тега `<system.web>` будет выглядеть примерно так:

```xml
<span class="hljs-tag"><<span class="hljs-title">system.web</span>></span>
    <span class="hljs-tag"><<span class="hljs-title">identity</span> <span class="hljs-attribute">impersonate</span>=<span class="hljs-value">"false"</span>/></span>
    <span class="hljs-tag"><<span class="hljs-title">authentication</span> <span class="hljs-attribute">mode</span>=<span class="hljs-value">"None"</span>/></span>
    <span class="hljs-tag"><<span class="hljs-title">compilation</span> <span class="hljs-attribute">debug</span>=<span class="hljs-value">"true"</span> <span class="hljs-attribute">targetFramework</span>=<span class="hljs-value">"4.5"</span>/></span>
    <span class="hljs-tag"><<span class="hljs-title">httpRuntime</span> <span class="hljs-attribute">targetFramework</span>=<span class="hljs-value">"4.5"</span>/></span>
    <span class="hljs-comment"><!--<httpHandlers>
      <add path="*.less" verb="GET" type="dotless.Core.LessCssHttpHandler, dotless.Core"/>
    </httpHandlers>--></span>
  <span class="hljs-tag"></<span class="hljs-title">system.web</span>></span>

```

После выполненных действий проблема должна быть решена, если нет, то внимательно читайте рекомендации на странице ошибки.