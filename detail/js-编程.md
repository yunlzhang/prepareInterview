1、给出两个字符串，求出他们最大的公共字串,例如输入 '1AB2345CD' ,'12345EF',输出'2345';

```js
function f(str1,str2){
    var result;
    var temp = [];
    var flag = false;
    var len = str1.length;
    var i = len;//截取位数
    while (i > 0){
        for(var j = 0;j < len - i+1 ;j++){
            if(str2.indexOf(str1.substr(j,i)) !== -1){
                result = str1.substr(j,i); 
                flag = true;
            }
        }
        if(flag) break;
        i--;
    }
    return result;
}
```