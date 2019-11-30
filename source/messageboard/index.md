---
title: 留言板
date: 2018-10-11 06:30:17
---

<iframe class="bvideo" src="//player.bilibili.com/player.html?aid=2345583&cid=3662647&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

<style>
.bvideo{width:100%}
</style>

<script>
    //v0.1
    function resizeVideo(){
        var bvideos = document.getElementsByClassName("bvideo");
        for(var i =0;i<bvideos.length;i++){
            var crt = bvideos[i];
            var w = crt.clientWidth;
            var newH = w * 0.66;
            crt.width = w;
            crt.height = newH;
        }
    } 
    window.addEventListener("resize",()=>{
        resizeVideo();
    });
    resizeVideo();
</script>




# 在这里留下你想说的话吧...
