---
title: To the world you may be one person, but to one person you may be the world.
date: 2019-06-15
---
<style type="text/css">
	/* 去除顶部距离 */
	.main-inner{
		margin-top: unset;
	}

	.box{
		visibility: visible;
		overflow: auto; 
	}

	.box li{
		float: left;
		width: 25%;
		position: relative;
		overflow: hidden;
		text-align: center;
		list-style: none;
		margin: 0;
		display: inline;
		padding: 0;
		height: 300px;
	}

	.imgbox{
	 width: 100%;
	 overflow: hidden;
	 height: 250px;
	}

	img.imgitem{
		padding: unset;
		padding: unset;
		border: unset;
		position: relative;
		padding: 0px;
		height: 100%;
		width: 100%;
		object-fit: cover;
	}

.title{
	text-align: center;
	font-size: 26px;
	position: relative;
	overflow-wrap: break-word;
    font-family: 'Lato', "PingFang SC", "Microsoft YaHei", sans-serif;
	padding: 0px !important;
	margin: 0px !important;
}

/* 除去图片边框颜色 */
.posts-expand .post-body img{
	padding: 0px !important;
	border: 1px solid transparent;
}
.posts-expand .post-title {
        display: auto;
}
/* 加载更多的容器 */
.btn-more-posts{
    text-align: center;
    border: unset;
    height: 100px;
	width: 100%;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
}

/* 加载更多框 */
.btn-more-posts span {
	border: 1px solid #000;
	border-radius: 5px;
	padding: 10px 20px 10px 20px;
}

@media (max-width: 767px){
	.box li {
    width: 100%;
}

.box span {
    min-height: 80px;
    border-right: unset;
    font-size: 17px;
}
.box p{
    border-right: unset;
    font-size: 12px;
  
}
.posts-expand {
    margin: unset;
}
div#comments.comments.v {
    width: 96%;
    padding-top: 50px;
}

}

@media (min-width: 1600px){
	.container .main-inner{
		width: 100%;
	}
}
</style>

<div id="box" class="box">

</div>

<script type="text/javascript">

function loadXMLDoc(xmlUrl) 
{
	try //Internet Explorer
	{
		xmlDoc=new ActiveXObject("Microsoft.XMLDOM");
	}
	catch(e)
	{
	  try //Firefox, Mozilla, Opera, etc.
	    {
		  xmlDoc=document.implementation.createDocument("","",null);
	    }
	  catch(e) {
		  alert(e.message)
		  }
	}
	
	try 
	{
		  xmlDoc.async=false;
		  xmlDoc.load(xmlUrl);
	}
	catch(e) {
		try //Google Chrome  
		  {  
			var chromeXml = new XMLHttpRequest();
			chromeXml.open("GET", xmlUrl, false);
			chromeXml.send(null);
			xmlDoc = chromeXml.responseXML.documentElement; 				
			//alert(xmlDoc.childNodes[0].nodeName);
			//return xmlDoc;    
		  }  
		  catch(e)  
		  {  
			  alert(e.message)  
		  }  		  	
	}
	return xmlDoc; 
}

//存储桶的访问域名（基础配置-访问域名）
var xmllink="https://image-1252731913.cos.ap-guangzhou.myqcloud.com"

xmlDoc=loadXMLDoc(xmllink);
var urls=xmlDoc.getElementsByTagName('Key');
var date=xmlDoc.getElementsByTagName('LastModified');
var wid=350;
var showNum=12; //每个相册一次展示多少照片
if ((window.innerWidth)>1200) {wid=(window.innerWidth*3)/18;}
var box=document.getElementById('box');
var i=0;

var content=new Array();
var tmp=0;
var kkk=-1;
for (var t = 0; t < urls.length ; t++) {
	var bucket=urls[t].innerHTML;
	var length=bucket.indexOf('/');
	if(length===bucket.length-1){
		kkk++;
		content[kkk]=new Array();
		content[kkk][0]={'url':bucket,'date':date[t].innerHTML.substring(0,10)};
		tmp=1;
	}
	else {
		content[kkk][tmp++]={'url':bucket.substring(length+1),'date':date[t].innerHTML.substring(0,10)};
	}
}

for (var i = 0; i < content.length; i++) {
	var conBox=document.createElement("div");
	conBox.id='conBox'+i;
	box.appendChild(conBox);
	var title=content[i][0].url;
	for (var j = 1; j < content[i].length && j < showNum+1; j++) {
		var con=content[i][j].url;
		var item=document.createElement("li");
		item.innerHTML="<div class=imgbox id=imgbox style=height:"+wid+"px;><img class=imgitem src="+xmllink+'/'+title+con+" alt="+con+"></div>";
		conBox.appendChild(item);
	}
	if(content[i].length > showNum){
		var moreItem=document.createElement("button");
		moreItem.className="btn-more-posts";
		moreItem.id="more"+i;
		moreItem.value=showNum+1;
		let cur=i;
		moreItem.onclick= function (){
			moreClick(this,cur,content[cur],content[cur][0].url);
		}
		moreItem.innerHTML="<span style=display:inline;>加载更多</span>";
		conBox.appendChild(moreItem);
	}
}

function moreClick(obj,cur,cont,title){
	var parent=obj.parentNode;
	parent.removeChild(obj);
	var j=obj.value;
	var begin=j;
	for ( ; j < cont.length && j < Number(showNum) + Number(begin); j++) {
		console.log( Number(showNum) + Number(begin));
		var con=cont[j].url;
		var item=document.createElement("li");
		item.innerHTML="<div class=imgbox id=imgbox style=height:"+wid+"px;><img class=imgitem src="+xmllink+'/'+title+con+" alt="+con+"></div>";
		parent.appendChild(item);
	}
	if(cont.length > j){
		obj.value=j;
		parent.appendChild(obj);
	}
}

</script>
