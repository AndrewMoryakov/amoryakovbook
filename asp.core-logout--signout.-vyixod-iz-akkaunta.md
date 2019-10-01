Если мы создаем в asp.core 2 проект не как Empty, а как готовый шаблон "Web Application", то будет создан контроллер `AccountController` со множеством действий, одно из них это `Logout`.

Но из коробки оно не всегда работает. То есть вы нажимаете на клиентской стороне кнопку "Выход" но результата не будет.

Сейчас я расскажу почему.

 Задача/challenge

> ##### ASP.CORE не работает выход, действие Logout, не работает SignOutAsync
> 
> ##### SignOutAsync doesn't work

 Решение/solution

Я перепишу свой метод Logout так, что явно укажу какую схему использовать:

```csharp
        <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">async</span> Task<IActionResult> <span class="hljs-title">Logout</span><span class="hljs-params">()</span>
        </span>{
            <span class="hljs-keyword">await</span> _signInManager.Context.SignOutAsync(CookieAuthenticationDefaults.AuthenticationScheme);
            _logger.LogInformation(<span class="hljs-string">"User logged out."</span>);
            <span class="hljs-function"><span class="hljs-keyword">return</span> <span class="hljs-title">RedirectToAction</span><span class="hljs-params">(nameof(HomeController.Index)</span>, "Home")</span>;
        }

```

 Объяснение/explanation

Как выглядит автоматически генерируемое действие `Logout` контроллера `AccountController` в asp.core 2 Вот так:

```csharp
        <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">async</span> Task<IActionResult> <span class="hljs-title">Logout</span><span class="hljs-params">()</span>
        </span>{
            <span class="hljs-keyword">await</span> _signInManager.SignOutAsync();
            _logger.LogInformation(<span class="hljs-string">"User logged out."</span>);
            <span class="hljs-function"><span class="hljs-keyword">return</span> <span class="hljs-title">RedirectToAction</span><span class="hljs-params">(nameof(HomeController.Index)</span>, "Home")</span>;
        }

```

Вроде кажется всё хорошо, разработчики Microsoft даже не делают каких-либо дополнительных проверок. Код должен работать однозначно, но не всегда.

Если посмотреть исходники метода `Logout`, то увидим следующее:

```csharp
    <span class="hljs-comment"><span class="hljs-xmlDocTag">///</span> <span class="hljs-xmlDocTag"><summary></span>Signs the current user out of the application.<span class="hljs-xmlDocTag"></summary></span></span>
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">virtual</span> <span class="hljs-keyword">async</span> Task <span class="hljs-title">SignOutAsync</span><span class="hljs-params">()</span>
    </span>{
      SignInManager<TUser> signInManager = <span class="hljs-keyword">this</span>;
      <span class="hljs-keyword">await</span> signInManager.Context.SignOutAsync(IdentityConstants.ApplicationScheme);
      <span class="hljs-keyword">await</span> signInManager.Context.SignOutAsync(IdentityConstants.ExternalScheme);
      <span class="hljs-keyword">await</span> signInManager.Context.SignOutAsync(IdentityConstants.TwoFactorUserIdScheme);
    }

```

Тут инкапсулируются методы которые уже выполняют наш Logout. В отличие от метода Logout в контроллере AccountController, мы видем, что тут передаются дополнительные параметры: например `csharpIdentityConstants.ApplicationScheme`

Если посмотреть исходники этих свойство, то:

```csharp
<span class="hljs-comment"><span class="hljs-xmlDocTag">///</span> <span class="hljs-xmlDocTag"><summary></span></span>
  <span class="hljs-comment"><span class="hljs-xmlDocTag">///</span> Represents all the options you can use to configure the cookies middleware uesd by the identity system.</span>
  <span class="hljs-comment"><span class="hljs-xmlDocTag">///</span> <span class="hljs-xmlDocTag"></summary></span></span>
  <span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">IdentityConstants</span>
  {
    <span class="hljs-keyword">private</span> <span class="hljs-keyword">static</span> <span class="hljs-keyword">readonly</span> <span class="hljs-keyword">string</span> CookiePrefix = <span class="hljs-string">"Identity"</span>;
    <span class="hljs-comment"><span class="hljs-xmlDocTag">///</span> <span class="hljs-xmlDocTag"><summary></span></span>
    <span class="hljs-comment"><span class="hljs-xmlDocTag">///</span> The scheme used to identify application authentication cookies.</span>
    <span class="hljs-comment"><span class="hljs-xmlDocTag">///</span> <span class="hljs-xmlDocTag"></summary></span></span>
    <span class="hljs-keyword">public</span> <span class="hljs-keyword">static</span> <span class="hljs-keyword">readonly</span> <span class="hljs-keyword">string</span> ApplicationScheme = IdentityConstants.CookiePrefix + <span class="hljs-string">".Application"</span>;
  ...

```

А я в своем проекте для определения схемы аутентификации использую схему:

`CookieAuthenticationDefaults.AuthenticationScheme`

Поэтому я должен явно указать какую схему использовать.

Подобная ситуация происходит при авторизации через социальные сети метод: `ExternalLoginCallback`