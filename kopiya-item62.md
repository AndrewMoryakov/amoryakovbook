Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent volutpat, felis id ultrices tincidunt, eros urna semper justo, vel dictum est ligula eu diam. Sed nec ligula fringilla, condimentum risus eu, venenatis diam. Phasellus tincidunt nisl in ex dictum sodales eget a leo. Quisque porttitor enim dignissim tortor pretium, quis sagittis metus laoreet. Etiam eu dolor in lectus fermentum hendrerit. Nunc vitae posuere lectus, non elementum risus. Ut pellentesque, tellus vel luctus semper, neque diam tincidunt mi, sed elementum felis orci et lectus. Sed euismod diam eu purus pharetra, in rhoncus est efficitur.

Ut ut vulputate nulla. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Quisque non nisi odio. Vivamus vel turpis tellus. Duis volutpat imperdiet neque a vulputate. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Nulla imperdiet magna non massa blandit, at scelerisque mauris porta. Mauris nisi eros, tempus quis commodo gravida, lacinia nec nisl. Praesent posuere egestas erat, a pretium tortor interdum nec. Quisque viverra ligula eget leo sodales varius. Phasellus urna mi, fringilla nec lacus at, tincidunt aliquam mauris. Maecenas nec semper turpis, vel sagittis lorem. Ut vitae nisl auctor enim placerat tempor sagittis fermentum lectus.

```
<pre class="brush:php"><?php return array (
 'search_string' => '16 сентября',
 'start_date' => '16.09.2017',
 'num_days' => '2',
 'run' => 1504778978,
);
```

```
<pre class="brush: jscript">$(document).ready(function(){
    $('.modal').modal();
	$('.ui.embed').embed();
	$('#video1').embed();
	$('#video2').embed();
	$('.tabular.menu .item').tab();
	
	$("img").click(function (event) {
			$("#modalimg").attr("src", $(this).data("src"));
			$('#modal').modal('show')
    });
    
     $("#morePosts").click(function(){
         var data = $(this).data();
         var $this = $(this);
         $.post('/ajax.html', data, function(response) {
             if(response == ""){
                 $this.hide();
                 return;
             }
             $this.data("offset", data.offset+data.limit);
             $(".results_items").append(response);
         })
     });
   
});
```

```
<pre class="brush:js"> $("#morePosts").click(function(){
         var data = $(this).data();
         var $this = $(this);
         $.post('/ajax.html', data, function(response) {
             if(response == ""){
                 $this.hide();
                 return;
             }
             $this.data("offset", data.offset+data.limit);
             $(".results_items").append(response);
         })
     });
```

Fusce accumsan fringilla ex. Vivamus efficitur faucibus odio et malesuada. Duis ultrices diam eu euismod tincidunt. Morbi bibendum pharetra eleifend. Vivamus tempus nunc eu pulvinar sagittis. Praesent ullamcorper nunc ac nisi volutpat, nec vestibulum enim ornare. Curabitur quis ultricies ante. Donec ultricies pretium condimentum. Fusce malesuada erat eget sem commodo vulputate. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Ut dictum sapien a venenatis volutpat. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque semper eros ligula, ut facilisis ex porttitor mattis. Morbi fermentum eget magna hendrerit laoreet.

Curabitur pretium ante quis efficitur varius. Etiam ex felis, varius vel consequat sed, faucibus a enim. Vestibulum ac nisl et urna mattis convallis. Praesent lacinia ex nec vestibulum euismod. Aliquam commodo eget sapien vel rhoncus. Cras sed leo pretium, fringilla ex at, iaculis urna. Maecenas vitae mollis mi, ac commodo ante. Maecenas finibus ipsum pharetra magna mollis, sed egestas est porta. Morbi finibus semper massa, vel placerat ex feugiat id. Mauris iaculis egestas lorem at tincidunt.

Suspendisse elit ipsum, aliquam ut tristique id, aliquet ac mi. Etiam lacus metus, finibus quis fermentum in, facilisis nec turpis. Suspendisse congue vestibulum blandit. Maecenas porta tincidunt risus, eu congue ex fringilla pellentesque. In nec est mattis, sodales dui nec, suscipit ex. Duis eleifend eu libero et laoreet. Vestibulum eu accumsan enim. Praesent hendrerit quis neque id feugiat. Sed tempus enim sapien, hendrerit vehicula augue tincidunt quis. In hac habitasse platea dictumst. Curabitur nunc dui, tincidunt et vulputate varius, porta at est. Donec quis sagittis sem. Ut ut sapien vitae urna suscipit mattis. Morbi ultricies, metus id elementum feugiat, tellus lorem molestie quam, nec hendrerit velit tortor vitae magna.