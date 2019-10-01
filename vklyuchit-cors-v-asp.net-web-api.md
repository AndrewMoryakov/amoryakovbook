CORS - это "совместное использование ресурсов между разными источниками". Выполняя AJAX запрос к серверу из другого домена сервер не отдаст вам данные, точнее отдаст, но браузер их не примет. Выполняя такой запрос браузер отправляет запрос с определенным заголовком, сервер же так-же должен вернуть ответ с нужным заголовком. Если сервер не настроен или ваш домен не находится в списке разрешенных, то вы получите вот такую ошибку:

#### XMLHttpRequest cannot load http://yoursite.com/api/GetCategories.

#### No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://localhost:53000' is therefore not allowed access.

- - - - - -

 Задача/challenge

Заставить сервер отдавать данные таким образом, чтобы браузер выполнял прием данных из другого домена. Коротко говоря включить CORS в ASP.NET Web API  Решение/solution

Сейчас мы настроим сервер так, что он будет отдавать данные всем, клиентам с любых доменов. Но можно настроить так, что правильные он будет отдавать лишь избранным доменам. 1. Устанавливаем nuget пакет `Microsoft.AspNet.WebApi.Cors`
2. Включаем CORS. В файле WebApiConfig.cs пишем строчку `config.EnableCors();`
3. Добавляем к классу web api контроллера атрибут

```csharp
 [EnableCors(origins: <span class="hljs-string">"*"</span>, headers: <span class="hljs-string">"*"</span>, methods: <span class="hljs-string">"*"</span>)]
    <span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">AdminApiController</span> : <span class="hljs-title">ApiController</span>
    {
       ...

```

Теперь Смело запускаем настроенный сервер и Actions нашего Controller будут отдавать правильные данные при кроссдоменный запросе, а браузер их без проблем примет.