<template>
  <ul id="sunyunzhu" ref="sun" class="hello">
    <li v-for="img in imgs" v-bind:class="img.css"><img v-bind:src="img.src" alt=""></li>
  </ul>
</template>

<script>
var imgs = [
        {css:'1X1',src:require('@/module/index/img/sunyunzhu.png')},

    ];

export default {
  name: 'sunyunzhu',
  data () {
    return {
      imgs: []
    }
  },
  mounted (){
    this.imgs = imgs;    
  }
}


window.onload=function(){
  fs();
  window.onresize = function(){
    fs();
  }

  function fs(){
    var size    = 120,    // 格子的大小
        boxW    = document.getElementById('sunyunzhu').clientWidth,  //box 的宽 高 
        boxH    = document.getElementById('sunyunzhu').clientHeight,
        numW    = Math.floor(boxW/size),    // 每行格子的数量                       
        newSize = boxW/numW,                //  实际格子的大小 
        numH    = Math.floor(boxW/newSize), // 每例格子的数量
        x       = 0,                        //记录格子
        y       = 0;                  
    var len     = imgs.length;
    var  count  = {}  //  记录格子是否占用 
    var li      = document.getElementsByTagName('li');
    for(var i=0;i<len;i++){
        li[i].style.display="block";      
        var imgSize = li[i].getAttributeNode('class').nodeValue.split('X'),
            imgX    = parseInt(imgSize[0]),
            imgY    = parseInt(imgSize[1]);  // 图片所占的格子 

        if(0 == x%numW && x != 0){  // 换行
            x = 0;
            y++;
        }

        if(count[x+'X'+y]){      //  判断格子是否占用  
          x++;
          i--;
          continue;
        }

        if(imgX == 2){
           if(count[x+'X'+y] || count[(x+1)+'X'+y]){   // 判断格子是否能放下 2 
              li[i].style.display="none";
              continue; 
           }         
        }

        if(imgX == 3){
           if(count[x+'X'+y] || count[(x+1)+'X'+y] || count[(x+2)+'X'+y]){   // 判断格子是否能放下 3 
              li[i].style.display="none";
              continue;
           }          
        }      

        switch(imgSize){
          case 'Normal':
              break;
          default:
              if(x+imgX > numW || y+imgY >numH){
                 li[i].style.display="none";
                 continue; 
              }else{
                for(var n = imgY -1;n>=0;n--){
                    for(var m=imgX -1;m>=0;m--){
                        count[(m+x)+"X"+(n+y)] = imgX + "X" + imgY; // 对象添加属性 表示 图片所占的位置
                    } 
                }
              }
              li[i].style.width  = imgX * newSize + 'px';
              li[i].style.height = imgY * newSize + 'px'; 

        }

        li[i].style.left  = newSize * x + 'px';
        li[i].style.top   = newSize * y + 'px';
        x++;
    }      
  }     
}
</script>

<style scoped>
  #sunyunzhu{
    width: 100%;
    height: 960px;
    position: relative;    
  }
  #sunyunzhu li{
    position: absolute;
    top: 0;
    left: 0;
    width: 60px;
    height: 60px;
  }
  #sunyunzhu li img{
    width: 100%;
    height: 100%;
    display: block;
  }
</style>

