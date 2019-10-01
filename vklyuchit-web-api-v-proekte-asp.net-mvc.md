 Задача/challenge

Подключить контроллер WEB API к проекту ASP.NET MVC и заставить всё это работать.

 Решение/solution

1. Изменить Global.asax.cs
2. Изменить стандартный маршрут для web api
3. Опубликоваь проект в своей файловой системе, удалить из рабочего проекта файлы из папки bin, скопировал из опубликованного проекта файлы \*.bin в в папку bin, рабочего проекта Странно, но одно без другого не работает

```csharp
<span class="hljs-function"><span class="hljs-keyword">protected</span> <span class="hljs-keyword">void</span> <span class="hljs-title">Application_Start</span><span class="hljs-params">()</span>
        </span>{
            AreaRegistration.RegisterAllAreas();
            GlobalConfiguration.Configure(WebApiConfig.Register); <span class="hljs-comment">//Эта строка, чтобы работал Web Api в MVC</span>
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
        }

```

```csharp
       <span class="hljs-keyword">public</span> <span class="hljs-keyword">static</span> <span class="hljs-keyword">class</span> <span class="hljs-title">WebApiConfig</span>
{
       <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">static</span> <span class="hljs-keyword">void</span> <span class="hljs-title">Register</span><span class="hljs-params">(HttpConfiguration config)</span>
       </span>{
           config.MapHttpAttributeRoutes();
       config.Routes.MapHttpRoute(
           name: <span class="hljs-string">"DefaultApi"</span>,
           routeTemplate: <span class="hljs-string">"api/{controller}/{action}/{id}"</span>,
           defaults: <span class="hljs-keyword">new</span> { id = RouteParameter.Optional }
       );
   }

<p>}
</p>
```