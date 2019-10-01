Написав какой либо сайт c использованием технологии ASP.NET и SQL Server Compact в качестве СУБД он может работать на вашей локальной машине, но это необязательно должно быть верно после публикации проекта на хостинге.

 Задача/challenge

Заставить удаленный сервер работать с базой данных

###### SQL Server Compact SQL Server Compact.

Не обязательно, но могут возникнуть ошибки с разным содержанием например с таким:

####  Unable to load the native components of SQL Server Compact corresponding to the ADO.NET provider of version 8876. Install the correct version of SQL Server Compact. Refer to KB article 974247 for more details. 

Или таким:

####  Cannot open '\[youthostpath\]\\cgid\\App\_Data\\datebase.sdf'. Provider 'System.Data.SqlServerCe.3.5' not installed. 

 Решение/solution

1. Первое, что мы можем попытаться сделать - это добавить необходимые ссылки в папку bin опубликованного сайта - то есть напрямую на хостинг. Когда вы создаете локальную базу данных с расширением sdf, я говорю о SQL Server Compact, Visual Studio автоматически добавляет необходимые ссылки

![](amoryakov.ru/Images/mssqlrefrs.png)

В свойствах этой сборки необходимо установить значения параметра "Копировать локально" в значение true. В итоге после публикации сайта, данная сборка копируется в папку bin нашего опубликованного проекта. При попытке запуска в таком случае может возникнуть ошибка. Ошибка может возникнуть по тому, что серверу требуются дополнительные библиотеки, так как архитектура сервера может иметь свою специфику. Где найти эти библиотеки? Они могут располагаться по следующему пути:

```
C:\Program Files\Microsoft SQL Server Compact Edition\v4<span class="hljs-number">.0</span>\<span class="hljs-keyword">Private</span>\x86
<p>C:\Program Files\Microsoft SQL Server Compact Edition\v4<span class="hljs-number">.0</span><span class="hljs-keyword">Private\amd64
</p>
```

Выбираем в зависимости от того, что нужно вам. копируем от туда все файлы и копируем в папку bin на сервере.

- - - - - -

В идеале, после этого, сайт должен корректно работать, если ошибки остаются то есть ещё один рецепт.

 Шаг 2/Step 2

Добавим следующие теги

```xml
<span class="hljs-tag"><<span class="hljs-title">configuration</span>></span>
<p><span class="hljs-tag"><<span class="hljs-title">system.data</span>></span></p>
<span class="hljs-tag">&lt;<span class="hljs-title">DbProviderFactories</span>&gt;</span>

  <span class="hljs-tag">&lt;<span class="hljs-title">remove</span> <span class="hljs-attribute">invariant</span>=<span class="hljs-value">"System.Data.SqlServerCe.3.5"</span>/&gt;</span>

  <span class="hljs-tag">&lt;<span class="hljs-title">add</span> <span class="hljs-attribute">name</span>=<span class="hljs-value">"Microsoft SQL Server Compact Data Provider"</span> <span class="hljs-attribute">invariant</span>=<span class="hljs-value">"System.Data.SqlServerCe.3.5"</span> <span class="hljs-attribute">description</span>=<span class="hljs-value">".NET            Framework Data Provider for Microsoft SQL Server Compact"</span> <span class="hljs-attribute">type</span>=<span class="hljs-value">"System.Data.SqlServerCe.SqlCeProviderFactory,                System.Data.SqlServerCe, Version=3.5.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"</span>/&gt;</span>

<span class="hljs-tag">&lt;/<span class="hljs-title">DbProviderFactories</span>&gt;</span>

<p><span class="hljs-tag"></<span class="hljs-title">system.data</span>></span></p>

**... и ещё**

```xml
 <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-tag"</span>></span>&lt;<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-title"</span>></span>runtime<span class="hljs-tag"></<span class="hljs-title">span</span>></span>&gt;<span class="hljs-tag"></<span class="hljs-title">span</span>></span>

  <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-tag"</span>></span>&lt;<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-title"</span>></span>assemblyBinding<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-attribute"</span>></span>xmlns<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-value"</span>></span>"urn:schemas-microsoft-com:asm.v1"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>&gt;<span class="hljs-tag"></<span class="hljs-title">span</span>></span>

    <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-tag"</span>></span>&lt;<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-title"</span>></span>dependentAssembly<span class="hljs-tag"></<span class="hljs-title">span</span>></span>&gt;<span class="hljs-tag"></<span class="hljs-title">span</span>></span>

      <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-tag"</span>></span>&lt;<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-title"</span>></span>assemblyIdentity<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-attribute"</span>></span>name<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-value"</span>></span>"System.Data.SqlServerCe"<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-attribute"</span>></span>publicKeyToken<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-value"</span>></span>"89845dcd8080cc91"<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-attribute"</span>></span>culture<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-value"</span>></span>"neutral"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>/&gt;<span class="hljs-tag"></<span class="hljs-title">span</span>></span>

      <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-tag"</span>></span>&lt;<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-title"</span>></span>bindingRedirect<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-attribute"</span>></span>oldVersion<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-value"</span>></span>"0.0.0.0-65535.65535.65535.65535"<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-attribute"</span>></span>newVersion<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-value"</span>></span>"4.0.0.0"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>/&gt;<span class="hljs-tag"></<span class="hljs-title">span</span>></span>

    <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-tag"</span>></span>&lt;/<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-title"</span>></span>dependentAssembly<span class="hljs-tag"></<span class="hljs-title">span</span>></span>&gt;<span class="hljs-tag"></<span class="hljs-title">span</span>></span>

    <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-tag"</span>></span>&lt;<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-title"</span>></span>qualifyAssembly<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-attribute"</span>></span>partialName<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-value"</span>></span>"System.Data.SqlServerCe"<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-attribute"</span>></span>fullName<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-value"</span>></span>"System.Data.SqlServerCe, Version=4.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>/&gt;<span class="hljs-tag"></<span class="hljs-title">span</span>></span>

  <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-tag"</span>></span>&lt;/<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-title"</span>></span>assemblyBinding<span class="hljs-tag"></<span class="hljs-title">span</span>></span>&gt;<span class="hljs-tag"></<span class="hljs-title">span</span>></span>

<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-tag"</span>></span>&lt;/<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-title"</span>></span>runtime<span class="hljs-tag"></<span class="hljs-title">span</span>></span>&gt;<span class="hljs-tag"></<span class="hljs-title">span</span>></span>
<span class="hljs-tag"></<span class="hljs-title">code</span>></span><span class="hljs-tag"></<span class="hljs-title">pre</span>></span>
<span class="hljs-tag"><<span class="hljs-title">p</span> <span class="hljs-attribute">style</span>=<span class="hljs-value">"font-size:16pt; color:green"</span>></span> Шаг 3/Step 3 | Меняем свойства проекта./ Let's to change project properties<span class="hljs-tag"></<span class="hljs-title">p</span>></span>
<span class="hljs-tag"><<span class="hljs-title">p</span>></span>Меняем в свойствах проекта конечную платформу с Any PC на x86. Может помочь. Но 1й способ уже должен решить все проблемы!<span class="hljs-tag"></<span class="hljs-title">p</span>></span>
<span class="hljs-tag"><<span class="hljs-title">blockquote</span>></span>
<span class="hljs-tag"><<span class="hljs-title">p</span>></span>Вывод.
Задача и решение в этой статье, в принципе являются вполне логичными и относящиеся к вещам - &quot;само собой разумеющимся&quot;. Но иногда это может сбить с толку, и подтолкнуть перейти, даже с небольшими проектами,  на полноценный SQL Server или другу СУБД :),<span class="hljs-tag"></<span class="hljs-title">p</span>></span>
<span class="hljs-tag"></<span class="hljs-title">blockquote</span>></span>

```