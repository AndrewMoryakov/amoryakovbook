Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent volutpat, felis id ultrices tincidunt, eros urna semper justo, vel dictum est ligula eu diam. Sed nec ligula fringilla, condimentum risus eu, venenatis diam. Phasellus tincidunt nisl in ex dictum sodales eget a leo. Quisque porttitor enim dignissim tortor pretium, quis sagittis metus laoreet. Etiam eu dolor in lectus fermentum hendrerit. Nunc vitae posuere lectus, non elementum risus. Ut pellentesque, tellus vel luctus semper, neque diam tincidunt mi, sed elementum felis orci et lectus. Sed euismod diam eu purus pharetra, in rhoncus est efficitur.

Ut ut vulputate nulla. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Quisque non nisi odio. Vivamus vel turpis tellus. Duis volutpat imperdiet neque a vulputate. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Nulla imperdiet magna non massa blandit, at scelerisque mauris porta. Mauris nisi eros, tempus quis commodo gravida, lacinia nec nisl. Praesent posuere egestas erat, a pretium tortor interdum nec. Quisque viverra ligula eget leo sodales varius. Phasellus urna mi, fringilla nec lacus at, tincidunt aliquam mauris. Maecenas nec semper turpis, vel sagittis lorem. Ut vitae nisl auctor enim placerat tempor sagittis fermentum lectus.

```
<span class="hljs-keyword">if</span> (isAwesome){
  <span class="hljs-keyword">return</span> <span class="hljs-literal">true</span>
}

```

And if you'd like to use syntax highlighting, include the language:

```javascript
$(<span class="hljs-built_in">document</span>).ready(<span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">()</span></span>{
    $(<span class="hljs-string">'.modal'</span>).modal();
    $(<span class="hljs-string">'.ui.embed'</span>).embed();
    $(<span class="hljs-string">'#video1'</span>).embed();
    $(<span class="hljs-string">'#video2'</span>).embed();
    $(<span class="hljs-string">'.tabular.menu .item'</span>).tab();
    
    $(<span class="hljs-string">"img"</span>).click(<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-params">(event)</span> </span>{
            $(<span class="hljs-string">"#modalimg"</span>).attr(<span class="hljs-string">"src"</span>, $(<span class="hljs-keyword">this</span>).data(<span class="hljs-string">"src"</span>));
            $(<span class="hljs-string">'#modal'</span>).modal(<span class="hljs-string">'show'</span>)
    });
    
    $(<span class="hljs-string">"#morePosts"</span>).click(<span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">()</span></span>{
         <span class="hljs-keyword">var</span> data = $(<span class="hljs-keyword">this</span>).data();
         <span class="hljs-keyword">var</span> $<span class="hljs-keyword">this</span> = $(<span class="hljs-keyword">this</span>);
         $.post(<span class="hljs-string">'/ajax.html'</span>, data, <span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">(response)</span> </span>{
             <span class="hljs-keyword">if</span>(response == <span class="hljs-string">"<p></p>"</span>){
                 $<span class="hljs-keyword">this</span>.hide();
                 <span class="hljs-keyword">return</span>;
             }
             $<span class="hljs-keyword">this</span>.data(<span class="hljs-string">"offset"</span>, data.offset+data.limit);
             $(<span class="hljs-string">".results_items"</span>).append(response);
         })
     });
   
});

```

```php
<span class="hljs-preprocessor"><?php</span>

<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">saveConfig</span><span class="hljs-params">(<span class="hljs-variable">$cfg</span>)</span>
</span>{
    <span class="hljs-variable">$txt</span> = <span class="hljs-string">"<?php return "</span>.var_export(<span class="hljs-variable">$cfg</span>,<span class="hljs-keyword">true</span>).<span class="hljs-string">";"</span>;
    file_put_contents(<span class="hljs-string">"config.php"</span>,<span class="hljs-variable">$txt</span>);
}

<span class="hljs-keyword">include</span> <span class="hljs-string">"../config.php"</span>;
<span class="hljs-variable">$hours47inseconds</span> = <span class="hljs-number">169200</span>;

<span class="hljs-variable">$cfg</span> = <span class="hljs-keyword">include</span>(<span class="hljs-string">"config.php"</span>);


<span class="hljs-variable">$now</span> = time();

<span class="hljs-keyword">if</span>(<span class="hljs-variable">$now</span> - <span class="hljs-variable">$cfg</span>[<span class="hljs-string">"run"</span>] < <span class="hljs-variable">$hours47inseconds</span>)
{
    <span class="hljs-variable">$been</span> = (<span class="hljs-variable">$now</span> - <span class="hljs-variable">$cfg</span>[<span class="hljs-string">"run"</span>])/<span class="hljs-number">60</span>/<span class="hljs-number">60</span>;
    <span class="hljs-keyword">echo</span> <span class="hljs-string">"Script changer Not Running "</span>.<span class="hljs-string">"Last start = "</span>.date(<span class="hljs-string">"d.m.Y H:i:s"</span>,<span class="hljs-variable">$cfg</span>[<span class="hljs-string">"run"</span>]).<span class="hljs-string">" its been "</span>.number_format(<span class="hljs-variable">$been</span>,<span class="hljs-number">2</span>).<span class="hljs-string">" hours. Next running after "</span>.date(<span class="hljs-string">"d.m.Y H:i:s"</span>,<span class="hljs-variable">$cfg</span>[<span class="hljs-string">"run"</span>]+<span class="hljs-variable">$hours47inseconds</span>);
    <span class="hljs-keyword">return</span>;
}

<span class="hljs-variable">$cfg</span>[<span class="hljs-string">"run"</span>] = <span class="hljs-variable">$now</span>;
saveConfig(<span class="hljs-variable">$cfg</span>);

<span class="hljs-variable">$month</span> = [
    <span class="hljs-string">"января"</span>,
    <span class="hljs-string">"февраля"</span>,
    <span class="hljs-string">"марта"</span>,
    <span class="hljs-string">"апреля"</span>,
    <span class="hljs-string">"мая"</span>,
    <span class="hljs-string">"июня"</span>,
    <span class="hljs-string">"июля"</span>,
    <span class="hljs-string">"августа"</span>,
    <span class="hljs-string">"сентября"</span>,
    <span class="hljs-string">"октября"</span>,
    <span class="hljs-string">"ноября"</span>,
    <span class="hljs-string">"декабря"</span>
];


<span class="hljs-variable">$date_start</span> = <span class="hljs-keyword">new</span> DateTime(<span class="hljs-variable">$cfg</span>[<span class="hljs-string">"start_date"</span>]);
<span class="hljs-variable">$date_start</span>->modify(<span class="hljs-string">"+"</span>.<span class="hljs-variable">$cfg</span>[<span class="hljs-string">"num_days2"</span>].<span class="hljs-string">" day"</span>);
<span class="hljs-variable">$new_date_str</span> = <span class="hljs-variable">$date_start</span>->format(<span class="hljs-string">'j'</span>).<span class="hljs-string">" "</span>.<span class="hljs-variable">$month</span>[<span class="hljs-variable">$date_start</span>->format(<span class="hljs-string">'n'</span>)-<span class="hljs-number">1</span>];


<span class="hljs-variable">$mysqli</span> = <span class="hljs-keyword">new</span> mysqli(DB_HOSTNAME, DB_USERNAME, DB_PASSWORD, DB_DATABASE);
<span class="hljs-keyword">if</span> (<span class="hljs-variable">$mysqli</span>->connect_errno) {
    <span class="hljs-keyword">echo</span> <span class="hljs-string">"Не удалось подключиться к MySQL: ("</span> . <span class="hljs-variable">$mysqli</span>->connect_errno . <span class="hljs-string">") "</span> . <span class="hljs-variable">$mysqli</span>->connect_error;
}
<span class="hljs-keyword">echo</span> <span class="hljs-variable">$mysqli</span>->host_info . <span class="hljs-string">"<br>"</span>;
<span class="hljs-keyword">if</span> (!<span class="hljs-variable">$mysqli</span>->set_charset(<span class="hljs-string">"utf8"</span>)) {
    printf(<span class="hljs-string">"Ошибка при загрузке набора символов utf8: %s\n"</span>, <span class="hljs-variable">$mysqli</span>->error);
    <span class="hljs-keyword">exit</span>();
}

<span class="hljs-variable">$res</span> = <span class="hljs-variable">$mysqli</span>->query(<span class="hljs-string">"SELECT category_id,description FROM "</span>.DB_PREFIX.<span class="hljs-string">"category_description"</span>);

<span class="hljs-keyword">while</span> (<span class="hljs-variable">$row</span> = <span class="hljs-variable">$res</span>->fetch_assoc()) {
    <span class="hljs-variable">$text</span> = <span class="hljs-variable">$row</span>[<span class="hljs-string">"description"</span>];
    <span class="hljs-keyword">if</span> (strpos(<span class="hljs-variable">$text</span>, <span class="hljs-variable">$cfg</span>[<span class="hljs-string">"search_string"</span>] ) === <span class="hljs-keyword">false</span>){
        <span class="hljs-keyword">echo</span> <span class="hljs-string">"Not Found in "</span>.<span class="hljs-variable">$row</span>[<span class="hljs-string">"category_id"</span>].<span class="hljs-string">"<br>\n"</span>;
        <span class="hljs-keyword">continue</span>;
    }

    <span class="hljs-variable">$new_text</span> = str_replace(<span class="hljs-variable">$cfg</span>[<span class="hljs-string">"search_string"</span>], <span class="hljs-variable">$new_date_str</span>, <span class="hljs-variable">$text</span>);
    
    <span class="hljs-keyword">if</span>(<span class="hljs-variable">$mysqli</span>->query(<span class="hljs-string">"UPDATE "</span>.DB_PREFIX.<span class="hljs-string">"category_description SET description = '"</span>.<span class="hljs-variable">$new_text</span>.<span class="hljs-string">"' WHERE category_id="</span>.<span class="hljs-variable">$row</span>[<span class="hljs-string">"category_id"</span>]))
    {
        <span class="hljs-keyword">echo</span> <span class="hljs-string">"GOOD UPDATE "</span>.<span class="hljs-variable">$row</span>[<span class="hljs-string">"category_id"</span>] . <span class="hljs-string">" <br>\n"</span>;
    }
    <span class="hljs-keyword">else</span>
    {
        <span class="hljs-keyword">echo</span> <span class="hljs-variable">$mysqli</span>->error;
    }
}

<span class="hljs-variable">$cfg</span>[<span class="hljs-string">"search_string"</span>] = <span class="hljs-variable">$new_date_str</span>;
<span class="hljs-variable">$cfg</span>[<span class="hljs-string">"start_date"</span>] = <span class="hljs-variable">$date_start</span>->format(<span class="hljs-string">"d.m.Y"</span>);
saveConfig(<span class="hljs-variable">$cfg</span>);




```