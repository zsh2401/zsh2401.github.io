---
title: 留言板
date: 2018-10-11 06:30:17
---

<iframe id="headVideo" src="//player.bilibili.com/player.html?aid=2345583&cid=3662647&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
<script>
    window.onresize = resizeVideo;
    function resizeVideo(){
        var newVideoW =  document.getElementById("content").offsetWidth * 1;
        var newVideoH = newVideoW * 0.66;
        console.log(newVideoW);
        document.getElementById("headVideo").width = newVideoW;
        document.getElementById("headVideo").height = newVideoH;
    } 
    resizeVideo();
</script>

# 在这里留下你想说的话吧...
