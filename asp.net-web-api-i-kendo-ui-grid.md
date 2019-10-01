Если вы хотите делать современный Web applications, то уже пора посмотреть в сторону Web API . По Web API много статей в Интернете например - [на metanit](https://metanit.com/sharp/aspnet_webapi/1.1.php). По всему по этому, я не буду рассказывать разжёвывать работу Web API, а просто покажу как сделать ваше приложение, уже клиентское, приложение ещё лучше с помощью [Kendo UI Grid](https://demos.telerik.com/kendo-ui/grid/index). Сегодня мы увидим как вообще использовать Kendo UI Grid.

- - - - - -

Go

Нам нужен контроллер, который способен: - возвращать список записей из базы данных (или откуда угодно)- удалять записи
- обновлять
- добавлять запись.

Я приведу пример метода контроллера, который возвращает список сущностей. К этому методу будет обращаться наша Kendo UI таблица. Точнее она будет обращаться к контролеру, а контроллер сам поймет, что нам нужно вызвать метод GetUsers.

```csharp
<span class="hljs-keyword">public</span> <span class="hljs-keyword">async</span> Task<List<dynamic>> GetUsers()
        {
       <span class="hljs-keyword">using</span>(<span class="hljs-keyword">var</span> db = <span class="hljs-keyword">new</span> ApplicationDbContext())
                                      {
                                          <span class="hljs-keyword">var</span> users = db.Users.ToList();
                                          <span class="hljs-keyword">var</span> modelView = <span class="hljs-keyword">new</span> List<dynamic>();
                                          users.ForEach(el=>modelView.Add(<span class="hljs-keyword">new</span>
                                          {
                                              Id = el.Id,
                                              Name = el.Name,
                                              SurName = el.SurName,
                                              Adress = el.Adress,
                                              Email = el.Email
                                          }));
                                          <span class="hljs-keyword">return</span> modelView;
                                      }
 
        }

```

Начиная с конструкции `csharp using(ApplicationDbContext db = new ApplicationDbContext())` мы извлекаем нужные нам записи из базы данных. Метод GetUsers возвращает список пользователей из базы данных, так же нам нужны другие методы: обновления, добавления, удаления. Поэтому ниже я приведу полный контроллер:

```csharp
<span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">UserController</span> : <span class="hljs-title">ApiController</span>
    {
        <span class="hljs-keyword">private</span> ApplicationDbContext db = <span class="hljs-keyword">new</span> ApplicationDbContext();
        <span class="hljs-comment">// GET: api/User/5</span>
        <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">async</span> Task<IHttpActionResult> <span class="hljs-title">GetUser</span><span class="hljs-params">(<span class="hljs-keyword">string</span> id)</span>
        </span>{
            ApplicationUser applicationUser = db.Users.Find(id);
            <span class="hljs-keyword">if</span> (applicationUser == <span class="hljs-keyword">null</span>)
            {
                <span class="hljs-function"><span class="hljs-keyword">return</span> <span class="hljs-title">NotFound</span><span class="hljs-params">()</span></span>;
            }
            <span class="hljs-function"><span class="hljs-keyword">return</span> <span class="hljs-title">Ok</span><span class="hljs-params">(applicationUser)</span></span>;
        }
        
        [HttpPut]
        <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">async</span> Task<IHttpActionResult> <span class="hljs-title">UpdateUser</span><span class="hljs-params">([FromUri]ApplicationUser applicationUser)</span>
        </span>{
            db.Entry(applicationUser).State = EntityState.Modified;
            <span class="hljs-keyword">await</span> db.SaveChangesAsync();
            <span class="hljs-function"><span class="hljs-keyword">return</span> <span class="hljs-title">StatusCode</span><span class="hljs-params">(HttpStatusCode.NoContent)</span></span>;
        }
 
       [HttpPost]
        <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">async</span> Task<IHttpActionResult> <span class="hljs-title">CreateUser</span><span class="hljs-params">(ApplicationUser applicationUser)</span>
        </span>{
            db.Users.Add(applicationUser);
            <span class="hljs-function"><span class="hljs-keyword">return</span> <span class="hljs-title">CreatedAtRoute</span><span class="hljs-params">(<span class="hljs-string">"DefaultApi"</span>, <span class="hljs-keyword">new</span> { id = applicationUser.Id }, applicationUser)</span></span>;
        }
        
        [ResponseType(<span class="hljs-keyword">typeof</span>(ApplicationUser))]
        [HttpDelete]
        <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">async</span> Task<IHttpActionResult> <span class="hljs-title">DeleteUser</span><span class="hljs-params">(<span class="hljs-keyword">string</span> Id)</span>
        </span>{
            db.Users.Remove(applicationUser);
            <span class="hljs-keyword">await</span> db.SaveChangesAsync();
            <span class="hljs-function"><span class="hljs-keyword">return</span> <span class="hljs-title">Ok</span><span class="hljs-params">(applicationUser)</span></span>;
        }
    }

```

> В коде я оставил только бизнес логику, чтобы сконцентрировать внимание на главном. Каждый метод класса UserController соответствует методам HTTP.

- GET (получить),
- PUT (Изменить),
- POST (добавить),
- DELETE (удалить). Именно поэтому мы пометили каждое действие контроллера аттрибутом котоырй определяет какой HTTP запрос он обрабатывает, например `[HttpPost]`Поэтому давайте перейдем к разработке клиентской части с использованием Kend UI Grid.

Разработка и настройка клиентской части (FrontEnd)

Сейчас мы будем работать с HTML но модуль Kendo UI Grid есть даже для WPF, поэтому FrontEnd может быть даже обычным приложением разработанном на языке C#. И так подключите нужные, для Kend UI Grid, скрипты и стили на страницу:

```html
 <span class="hljs-comment"><!--Kendo UI start--></span>
    <span class="hljs-tag"><<span class="hljs-title">link</span> <span class="hljs-attribute">rel</span>=<span class="hljs-value">"stylesheet"</span> <span class="hljs-attribute">href</span>=<span class="hljs-value">"//kendo.cdn.telerik.com/2015.3.1111/styles/kendo.common-bootstrap.min.css"</span> /></span>
    <span class="hljs-tag"><<span class="hljs-title">link</span> <span class="hljs-attribute">rel</span>=<span class="hljs-value">"stylesheet"</span> <span class="hljs-attribute">href</span>=<span class="hljs-value">"//kendo.cdn.telerik.com/2015.3.1111/styles/kendo.bootstrap.min.css"</span> /></span>
    <span class="hljs-tag"><<span class="hljs-title">script</span> <span class="hljs-attribute">src</span>=<span class="hljs-value">"//kendo.cdn.telerik.com/2015.3.1111/js/kendo.all.min.js"</span>></span><span class="javascript"></span><span class="hljs-tag"></<span class="hljs-title">script</span>></span>
    <span class="hljs-comment"><!--Kendo UI end--></span>

```

На странице объявите элемент, который будет нашей таблицей

```html
<span class="hljs-tag"><<span class="hljs-title">div</span> <span class="hljs-attribute">id</span>=<span class="hljs-value">"grid"</span>></span><span class="hljs-tag"></<span class="hljs-title">div</span>></span>

```

Теперь создайте файл \*.js в котором будет следующий javascript код. Он будет настраивать поведение таблицы, а так-же загружать данные с сервера и отображать

```javascript
<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">obj_to_query</span><span class="hljs-params">(data)</span> </span>{<span class="hljs-comment">//Функция преобразующая объект в строку запроса</span>
    <span class="hljs-keyword">var</span> url = <span class="hljs-string">''</span>;
    <span class="hljs-keyword">for</span> (<span class="hljs-keyword">var</span> prop <span class="hljs-keyword">in</span> data) {
        url += <span class="hljs-built_in">encodeURIComponent</span>(prop) + <span class="hljs-string">'='</span> +
            <span class="hljs-built_in">encodeURIComponent</span>(data[prop]) + <span class="hljs-string">'&'</span>;
    }
    <span class="hljs-keyword">return</span> url.substring(<span class="hljs-number">0</span>, url.length - <span class="hljs-number">1</span>);
}
 
$(<span class="hljs-built_in">document</span>).ready(<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-params">()</span> </span>{
    <span class="hljs-keyword">var</span> crudServiceBaseUrl = <span class="hljs-string">"/api/User/"</span>, <span class="hljs-comment">//Имя нашего контроллера (в соответствие с маршрутом настроенным на сервере</span>
        dataSource<span class="hljs-comment">//Переменная, с помощью которой будут производиться все манипуляции с данными</span>
        = <span class="hljs-keyword">new</span> kendo.data.DataSource({
            transport: {
                read: {<span class="hljs-comment">//Показываем как загружать данные</span>
                    url: crudServiceBaseUrl,<span class="hljs-comment">//Ссылка откуда загружать данные. Мы лишь обращаемся к контроллеру</span>
                    <span class="hljs-comment">//так как в типе запроса мы указали GET то контроллер сам поймет в какой метод передать запрос,</span>
                     <span class="hljs-comment">//поэтому явно указывать его не нужно,</span>
                    <span class="hljs-comment">//Да мы и не знаем как он называется на сервере, нам это не важно.</span>
                    <span class="hljs-comment">//В контроллере UserController будет вызыван метод GetUsers</span>
                    dataType: <span class="hljs-string">"json"</span>,
                    type: <span class="hljs-string">"GET"</span>
                },
                update: {
                    url: <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-params">(options)</span> </span>{
                        <span class="hljs-keyword">return</span> crudServiceBaseUrl + <span class="hljs-string">"?"</span> + obj_to_query(options.models[<span class="hljs-number">0</span>]); <span class="hljs-comment">//формируем запрос для обновления сущности,</span>
                        <span class="hljs-comment">//в options.models[0] находится сущность, которую редактирует пользователь</span>
                        <span class="hljs-comment">//options.models[0] это javascript объект,</span>
                        <span class="hljs-comment">//его мы предварительно преобразуем в параметры которые можно передать в строке URL</span>
                        <span class="hljs-comment">//Так же для этого нам нужно правильно настроить метод контроллера и маршруты</span>
                    },
                    dataType: <span class="hljs-string">"json"</span>,
                    type: <span class="hljs-string">"PUT"</span>
                },
                destroy: {
                    url: <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-params">(options)</span> </span>{
                        <span class="hljs-keyword">return</span> crudServiceBaseUrl + <span class="hljs-string">"?"</span> + obj_to_query(options.models[<span class="hljs-number">0</span>]);<span class="hljs-comment">//Формируем запрос для удаления</span>
                    },
                    dataType: <span class="hljs-string">"json"</span>,
                    type: <span class="hljs-string">"DELETE"</span>
                },
                create: {
                    url: crudServiceBaseUrl,<span class="hljs-comment">//Запрос для создания новой сущности, но я не использую его в своем проекте</span>
                    dataType: <span class="hljs-string">"json"</span>,
                    type: <span class="hljs-string">"POST"</span>,
                    processData: <span class="hljs-literal">false</span>
                },
                parameterMap: <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-params">(options, operation)</span> </span>{<span class="hljs-comment">//Когда мы удаляем или редактируем некую сущность выполняется эта функция</span>
                    <span class="hljs-keyword">if</span> (operation !== <span class="hljs-string">"read"</span> && options.models) {
                        <span class="hljs-comment">//В options находится объект который мы изменили или хотим удалить</span>
                        <span class="hljs-keyword">return</span> options;
                    }
                }
            },
            batch: <span class="hljs-literal">true</span>,<span class="hljs-comment">//Ниже уже дополнительные настройки</span>
            pageSize: <span class="hljs-number">20</span>,
            schema: {
                model: {
                    id: <span class="hljs-string">"Id"</span>,
                    fields: {
                        Id: {
                            editable: <span class="hljs-literal">false</span>,
                            nullable: <span class="hljs-literal">true</span>
                        },
                        Name: {<span class="hljs-comment">//Тут мы перечисляем какие поля присутствуют в объекте, который передаст сервер</span>
                            type: <span class="hljs-string">"string"</span>
                        },
                        SurName: {
                            type: <span class="hljs-string">"string"</span>
                        },
                        Adress: {
                            type: <span class="hljs-string">"string"</span>
                        },
                        Email: {
                            type: <span class="hljs-string">"string"</span>
                        }
                    }
                }
            }
        });
 
    $(<span class="hljs-string">"#grid"</span>).kendoGrid({
        dataSource: dataSource,
        pageable: <span class="hljs-literal">true</span>,
        groupable: <span class="hljs-literal">true</span>,
        sortable: <span class="hljs-literal">true</span>,
        <span class="hljs-comment">//height: 550,</span>
        <span class="hljs-comment">//toolbar: ["create"], //Кнопка для создания сущностей</span>
        columns: [<span class="hljs-comment">//Производим настройку полей, например можем установить валидацию</span>
            <span class="hljs-comment">//"Id",</span>
            {
                field: <span class="hljs-string">"Name"</span>,
                title: <span class="hljs-string">"Имя"</span>,
                width: <span class="hljs-string">"120px"</span>
            }, {
                field: <span class="hljs-string">"SurName"</span>,
                title: <span class="hljs-string">"Фамлия"</span>,
                width: <span class="hljs-string">"120px"</span>
            }, {
                field: <span class="hljs-string">"Adress"</span>,
                title: <span class="hljs-string">"Адрес"</span>,
                width: <span class="hljs-string">"120px"</span>
            }, {
                field: <span class="hljs-string">"Email"</span>,
                title: <span class="hljs-string">"Почта"</span>,
                width: <span class="hljs-string">"120px"</span>
            }, {
                command: [<span class="hljs-string">"edit"</span>, <span class="hljs-string">"destroy"</span>],<span class="hljs-comment">//Добавляем кнопки удаления и редактирования</span>
                title: <span class="hljs-string">"&nbsp;"</span>,
                width: <span class="hljs-string">"250px"</span>
            }
        ],
        editable: <span class="hljs-string">"inline"</span><span class="hljs-comment">//режим редактирования</span>
    });
});

```

csHTML страница

```html

@{
    Layout = null;
}
<span class="hljs-doctype"></span>
<span class="hljs-tag"><<span class="hljs-title">html</span>></span>
<span class="hljs-tag"><<span class="hljs-title">head</span>></span>
    <span class="hljs-tag"><<span class="hljs-title">title</span>></span>Представьтесь<span class="hljs-tag"></<span class="hljs-title">title</span>></span>
    <span class="hljs-tag"><<span class="hljs-title">meta</span> <span class="hljs-attribute">charset</span>=<span class="hljs-value">"utf-8"</span> /></span>
 
    <span class="hljs-comment"><!--Kendo UI start--></span>
    <span class="hljs-tag"><<span class="hljs-title">style</span>></span><span class="css">
        <span class="hljs-tag">html</span> <span class="hljs-rules">{
            <span class="hljs-rule"><span class="hljs-attribute">font-size</span>:<span class="hljs-value"> <span class="hljs-number">14px</span></span></span>;
            <span class="hljs-rule"><span class="hljs-attribute">font-family</span>:<span class="hljs-value"> Arial, Helvetica, sans-serif</span></span>;
        <span class="hljs-rule">}</span></span>
    </span><span class="hljs-tag"></<span class="hljs-title">style</span>></span>
    <span class="hljs-tag"><<span class="hljs-title">link</span> <span class="hljs-attribute">rel</span>=<span class="hljs-value">"stylesheet"</span> <span class="hljs-attribute">href</span>=<span class="hljs-value">"//kendo.cdn.telerik.com/2015.3.1111/styles/kendo.common-bootstrap.min.css"</span> /></span>
    <span class="hljs-tag"><<span class="hljs-title">link</span> <span class="hljs-attribute">rel</span>=<span class="hljs-value">"stylesheet"</span> <span class="hljs-attribute">href</span>=<span class="hljs-value">"//kendo.cdn.telerik.com/2015.3.1111/styles/kendo.bootstrap.min.css"</span> /></span>
    <span class="hljs-tag"><<span class="hljs-title">script</span> <span class="hljs-attribute">src</span>=<span class="hljs-value">"//kendo.cdn.telerik.com/2015.3.1111/js/kendo.all.min.js"</span>></span><span class="javascript"></span><span class="hljs-tag"></<span class="hljs-title">script</span>></span>
    <span class="hljs-comment"><!--Kendo UI end--></span>
 
   
<span class="hljs-tag"></<span class="hljs-title">head</span>></span>
<span class="hljs-tag"><<span class="hljs-title">body</span> <span class="hljs-attribute">style</span>=<span class="hljs-value">"background-color: #fff;"</span>></span>
        <span class="hljs-tag"><<span class="hljs-title">div</span> <span class="hljs-attribute">id</span>=<span class="hljs-value">"grid"</span>></span><span class="hljs-tag"></<span class="hljs-title">div</span>></span>
       <span class="hljs-tag"><<span class="hljs-title">script</span> <span class="hljs-attribute">src</span>=<span class="hljs-value">"/Scripts/Custom/userGrid.js"</span>></span><span class="javascript"></span><span class="hljs-tag"></<span class="hljs-title">script</span>></span>
<span class="hljs-tag"></<span class="hljs-title">body</span>></span>
<span class="hljs-tag"></<span class="hljs-title">html</span>></span>

```

Вернемся к нашему серверу (BackEnd)

Обратите внимание на метод

```csharp
<span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">async</span> Task<IHttpActionResult> <span class="hljs-title">UpdateUser</span><span class="hljs-params">([FromUri]ApplicationUser applicationUser)</span>
        </span>{ ... }

```

Перед параметром стоит атрибут `[FromUri]` он нужен чтобы связать параметры из запроса с объектом типа ApplicationUser, ведь свойства этого типа совпадают с параметрами в строке запроса. А это обеспеченно соответствующей настройкой маршрута. Перейдем в файл WebApiConfig.cs в вашем проекте. Нам нужно добавить маршрут который соответствует тем параметрам которые мы хотим передавать контроллеру, например при обновлении сущности

У меня маршрут выглядит так

```csharp
config.Routes.MapHttpRoute(name: <span class="hljs-string">"userRoute"</span>,
                    routeTemplate: <span class="hljs-string">"api/{controller}/{Id}/{Name}/{SurName}/{Adress}/{Email}"</span>);

```

Заметьте, что мы указали `api/{controller}`, далее параметры. Соответствующие параметры должны быть в любом запросе, который хочет соответствовать этому маршруту и свойствам типа (в моем случае) `ApplicationUser`.

В итоге мы получаем вот такой вот результат: ![](http://amoryakov.ru/Images/kendouigrid.png)

Мне в Kendo UI нравится то, что мы просто указали метод API а он уже знает что мы обновили, что удалили, что добавляем и просто передает эту информацию на сервер, а сервер уже выполняет свои серверные дела.