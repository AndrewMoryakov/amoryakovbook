Эта информация со второго очного занятия курсов программирования - Dream Place Academy. Спасибо всем кто присутствовал.

Переменные это именованный объект в программе, который содержит либо значение либо ссылку на объект, в котором уже и храниться значение - но это уже отдельная тема.

Поэтому вот лик-без по переменным в языке C#.

```csharp
<span class="hljs-keyword">class</span> <span class="hljs-title">Program</span>
    {
        <span class="hljs-function"><span class="hljs-keyword">static</span> <span class="hljs-keyword">void</span> <span class="hljs-title">Main</span><span class="hljs-params">( <span class="hljs-keyword">string</span>[] args )</span>
        </span>{<span class="hljs-comment">//int, String, double и др. это типы. Каждая переменная ОБЯЗАНА быть какого-либо типа.</span>
            <span class="hljs-comment">//Каждая переменная может иметь произвольное имя. Имя начинается только с символа или с _ но может в составе имеет цифры</span>
            <span class="hljs-comment">//Хорошим тоном считается если переменная начинается с маленькой буквы.</span>
            <span class="hljs-comment">//Лучше переменную называть не просто n, а numberGarage - то есть переменная должна характеризовать реальный объект</span>
            <span class="hljs-comment">//Переменные можно объявлять на ЛЮБОМ естественном языке - любого народа, даже на русском. Но лучше объявляете переменные на английском языке. Но не очень хорошо, когда переменные называют русскими словами, но английскими буквами</span>
 
            <span class="hljs-keyword">int</span> amount = <span class="hljs-number">1432</span>;<span class="hljs-comment">//Переменная должна быть определенного типа - например int. Переменой при объявлении можно сразу присвоить значение</span>
            <span class="hljs-keyword">int</span> amount2; <span class="hljs-comment">//Переменная может иметь в имени цифры.</span>
 
            <span class="hljs-keyword">int</span> i;<span class="hljs-comment">//Переменную можно объявить но не присваивать ей значение а сделать это после</span>
            i = <span class="hljs-number">342453445</span>;
 
            String login = <span class="hljs-string">"Logan"</span>;<span class="hljs-comment">//Переменный могут быть любых типов, например, String то есть строка</span>
            login = <span class="hljs-string">"Fox"</span>;<span class="hljs-comment">//В переменную мы можем записывать значения соответствующего типа</span>
 
 
            login = Console.ReadLine();<span class="hljs-comment">//Переменную мы можем записать значения с клавиатуры например с помощью Console.ReadLine(); этот метод возвращает строку</span>
            <span class="hljs-comment">//То есть когда программа дойдет до Console.ReadLine(); она остановиться и если пользователь введет какие либо символы с клавиатуры то они запишутся в переменную login</span>
 
            <span class="hljs-comment">//Если мы хотим записать цифру в переменную типа int, которая принимает только цифры, то мы должны преобразовать результат который возвращает метод Console.ReadLine(); в числовой формат. так как этот метод возвращает значения в строковом формате</span>
            <span class="hljs-comment">//поэтому делаем преобразования с помощью</span>
 
            i = Convert.ToInt32( Console.ReadLine() );<span class="hljs-comment">//читаем ввод с клавиатуры с помощью Console.ReadLine() и сразу передаем считанное значение в Convert.ToInt32() который преобразует строку к числу</span>
 
            <span class="hljs-comment">//или</span>
            String numberString = Console.ReadLine();<span class="hljs-comment">//Считываем ввод  с клавиатуры  в переменную numberString типа строка</span>
            i = Convert.ToInt32( numberString );<span class="hljs-comment">//А затем numberString записываем в переменную типа int предварительно преобразовав numberString к числу</span>
            <span class="hljs-comment">//Int32 полностью аналогичен int можно писать, что больше нравиться</span>
 
            <span class="hljs-keyword">int</span> multiTabs = <span class="hljs-number">72334245</span>;
            <span class="hljs-keyword">long</span> big_Multi_Multi_Tabs = multiTabs;<span class="hljs-comment">//multiTabs и big_Multi_Multi_Tabs являются разными типами, но так как маленькое число в multiTabs помещается в переменной big_Multi_Multi_Tabs, которая может хранить и большие числа нежели big_Multi_Multi_Tabs - происходит автоматическое преобразование типов (неявное преобразование)</span>
 
            <span class="hljs-keyword">int</span> cutDouble;<span class="hljs-comment">//Переменная целочисленного типа</span>
            <span class="hljs-keyword">double</span> trabl = <span class="hljs-number">342.233243524</span>;<span class="hljs-comment">//Десятичное число(вещественное)</span>
            cutDouble = (<span class="hljs-keyword">int</span>)trabl;<span class="hljs-comment">//так как в cutDouble нельзя хранить дробные числа, то мы делаем явное приведение с помощью (int). Но в результате десятичная часть отбрасывается =(</span>
 
            login = Convert.ToString( cutDouble );<span class="hljs-comment">//Так как login строковая переменная, а cutDouble вещественная то делаем преобразование с помощью метода ToString() класса Convert</span>
 
            <span class="hljs-comment">// А можно и так</span>
            login = Convert.ToString( cutDouble+<span class="hljs-number">324</span> );
 
            <span class="hljs-comment">//Так же есть и другие методы для преобразования например</span>
            i = Convert.ToInt32(<span class="hljs-string">"23342"</span>);
            i = Convert.ToInt32( <span class="hljs-string">"342"</span>+<span class="hljs-string">"234"</span> );
 
            numberString = <span class="hljs-string">"234"</span>;<span class="hljs-comment">//записываем в строковую переменную строку</span>
            i = Convert.ToInt32( numberString + <span class="hljs-string">"342"</span> );<span class="hljs-comment">//сначала две numberString + "342" строки скрепляются, а результат "234342" преобразуется в число</span>
 
            <span class="hljs-comment">//Ещё можно вспомнить математику</span>
            i = <span class="hljs-number">324</span> / <span class="hljs-number">234</span> * <span class="hljs-number">24</span> / i + <span class="hljs-number">324</span> + cutDouble + (<span class="hljs-keyword">int</span>)trabl;<span class="hljs-comment">//как то так.  (int)trabl - по тому, что trabl вещественного типа</span>
 
            <span class="hljs-comment">//Существует метод который содержит математические функции</span>
            trabl = Math.Pow( <span class="hljs-number">12</span>, i );<span class="hljs-comment">//Возводит 12 в степень i. А в i храниться и обязательно ДОЛЖНО храниться какое либо число! Math.Pow( 12, i ) возвращает вещественное число типа double</span>
            <span class="hljs-comment">//поэтому если хотим записать результат в целочисленную переменную, то делаем так</span>
            i = (<span class="hljs-keyword">int</span>)Math.Pow( <span class="hljs-number">12</span>, i );<span class="hljs-comment">//Явно приводим к типу int</span>
        }
    }

```

> А это некоторые слайды из презентации

![](http://amoryakov.ru/Images/variabletypes.png)![](http://amoryakov.ru/Images/variableserrors.png)