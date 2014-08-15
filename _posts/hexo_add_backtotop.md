title: Hexo博客优化：添加返回顶部功能 
date: 2014-08-15 23:21:26
categories: Hexo
tags: [hexo,返回顶部]
---
1.添加HTML代码。新建文件 /themes/light/layout/_partial/totop.ejs ，在文件中加入HTML代码：

     <DIV style="DISPLAY: none" id=goTopBtn title="Scroll Back to Top"><IMG border=0 src="<%- config.root %>images/top.jpg"></DIV>
     <SCRIPT type=text/javascript>goTopEx();</SCRIPT>

2、在文件hemes/light/layout/_partial/head.ejs 添加
 
    <SCRIPT type=text/javascript src="<%- config.root %>js/scrolltop.js"></SCRIPT>
<!--more--> 

3.添加JS代码。新建文件 /themes/light/source/js/scrolltop.js，在文件中添加javascript代码：

     // JavaScript Document
    function goTopEx(){
	var obj=document.getElementById("goTopBtn");
	function getScrollTop(){
		return document.documentElement.scrollTop+document.body.scrollTop;
	}
	function setScrollTop(value){
		if(document.documentElement.scrollTop){
			document.documentElement.scrollTop=value;
		}else{
			document.body.scrollTop=value;
		}
	} 
	window.onscroll=function(){getScrollTop()>0?obj.style.display="":obj.style.display="none";}
	obj.onclick=function(){
		var goTop=setInterval(scrollMove,10);
		function scrollMove(){
			setScrollTop(getScrollTop()/1.1);
			if(getScrollTop()<1)clearInterval(goTop);
		}
	}
    }



4、在目录themes/light/source/css/_partial新建文件scrolltop.styl，内容如下：

     #goTopBtn
      POSITION fixed
      TEXT-ALIGN center
      -HEIGHT 30px
       WIDTH 45px
       BOTTOM 35px
       HEIGHT 47px
       FONT-SIZE 12px
       CURSOR pointer
       RIGHT 10px
       _position absolute
       _right auto
   
5、在文件themes/light/source/css/style.styl添加：

    @import '_partial/scrolltop'

6、添加按钮图片，将图片复制到themes/light/source/images，文件名为top.jpg
