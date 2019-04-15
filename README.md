# 函数防抖与函数节流

防抖与节流这个话题在2018年-2019年在各个社区都非常火，文章也非常多。最初见到“防抖节流”时，确实让我疑惑，这是最近新出的什么技术吗？我看过多文章介绍后，发现这其实是在工作当中一个很常见使用频率也非常高的事件处理方法。

# 偶遇场景

我们在做WEB客户端开发时，会经常使用resize、scroll、input的input事件，它们都是在非常短的时间内会多次触发事件，而且触发频率非常高。这样高频的调用回调函数，会造成浏览器非常大的计算压力，同时也会大量占用内存。使浏览器操作延迟，卡顿。

### 函数防抖

防抖（debounce），多次触发事件，只执行一次回调。这就像坐电梯，电梯在人进入后等待8s关门，在电梯没关门前，又有新的人要进电梯，电梯关门时间是根据最后进入电梯的人重新计算。假如还有人要进入电梯，那么电梯会重置上一个人的进入时间。防抖函数也是一样，触发事件，函数将在一段时间后执行，假如中途用户又一次操作了当前事件对象，那么函数的等待时间会被重置重新开始。

``` html
<input type="text" id="input">
<script>
    window.onload=function () {
      var inputID=document.getElementById("input");

      //处理防抖的程序
      function debounce(fun,delayed) {
        // 使用闭包，让每次调用时点击定时器状态不丢失
        var timeout=null;
        return function () {
          // 如果用户在定时器（上一次操作）执行前再次点击，那么上一次操作将被取消
          clearTimeout(timeout);
          var context=this;
          var args=arguments;
          timeout=setTimeout(function () {
            // 使用事件绑定的当前对象，替换处理程序的当前对象
            // 并将事件绑定的当前对象参数，传递给操作之后的处理程序
            fun.apply(context,args);
          },delayed)
        }
      }

      var Main=debounce(function (e) {
        //处理操作的主程序
        console.log('防抖成功！\n获取到的输入值为：',e.target.value)
      },1000)
      inputID.addEventListener('input',Main);
    }
</script>
```

