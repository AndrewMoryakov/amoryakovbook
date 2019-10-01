Создаем модель используя подход CodeFirst. Первая главня таблица *Category* имеет свойство навигации коллекции *Products*. Вторая таблица зависимая таблица *Food*... Вот с ней мы и будем работать далее

Зависимая таблица имеет вторичный ключ **CategoryId** и свойство навигации *Category* именно благодаря вторичному ключу *CategoryId*, который не поддерживает значения null будет происходить каскадное удаление. Удаляя категорию, конечно-же, будет удалена и запись в таблице *Food*, так как она **обязана** ссылаться на что-либо, но то, на что она ссылалась мы удалили. Код ниже демонстрирует как создавать модель таблиц где работает каскадное удаление:

```csharp
  <span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">Food</span>
       {
             [Key]
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">int</span> Id { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">string</span> Name { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">int</span> CategoryId { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             [ForeignKey(<span class="hljs-string">"CategoryId"</span>)]
             <span class="hljs-keyword">public</span> Category Category { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
       }
 
       <span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">Category</span>
       {
             [Key]
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">int</span> Id { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">string</span> Name { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> ICollection<FoodProduct> Products { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-title">Category</span><span class="hljs-params">()</span>
             </span>{
                    Products = <span class="hljs-keyword">new</span> List<FoodProduct>();
             }
       }

```

Но теперь давайте "Отключим" каскадное удаление, другими словами, позволим внешнему ключу *CategoryId* в зависимой таблице *Food* содержать значения null. Верно ведь, теперь мы можем спокойно удалить запись в таблице *Category* на которую ссылается одна из записей в таблице *Food*. Код ниже показывает как это сделать:

```csharp
  <span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">Food</span>
       {
             [Key]
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">int</span> Id { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">string</span> Name { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">int</span>? CategoryId { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             [ForeignKey(<span class="hljs-string">"CategoryId"</span>)]
             <span class="hljs-keyword">public</span> Category Category { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
       }
 
       <span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">Category</span>
       {
             [Key]
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">int</span> Id { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">string</span> Name { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> ICollection<FoodProduct> Products { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-title">Category</span><span class="hljs-params">()</span>
             </span>{
                    Products = <span class="hljs-keyword">new</span> List<FoodProduct>();
             }
       }

```

> Вообще если в C# объявить значимый тип со знаком вопроса сразу после названия типа. То он становится Nullable, то есть разрешает записывать в себя значения null, что является прирегативой ссылочных типов. Но это отдельная история.

Следующая ситуация -- это запретить удаление записей из основной таблицы *Category* если на эту запсь через внешний ключ ссылаются записи из зависимой таблицы, в нашем случеае таблицы *Food*. Чтобы это сделать в EF нужно вообще удалить внешний ключ, оставив только внешний ключ в виде ссылки на основную таблицу. Код ниже:

```csharp
  <span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">Food</span>
       {
             [Key]
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">int</span> Id { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">string</span> Name { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> Category Category { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
       }
 
       <span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">Category</span>
       {
             [Key]
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">int</span> Id { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> <span class="hljs-keyword">string</span> Name { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-keyword">public</span> ICollection<FoodProduct> Products { <span class="hljs-keyword">get</span>; <span class="hljs-keyword">set</span>; }
             <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-title">Category</span><span class="hljs-params">()</span>
             </span>{
                    Products = <span class="hljs-keyword">new</span> List<FoodProduct>();
             }
       }

```