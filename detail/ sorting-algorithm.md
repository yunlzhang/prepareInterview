## 关于排序算法的一些知识点总结

排序方法  |   时间复杂度       |  空间复杂度   | 稳定性     |  排序方式
----    |     -----         |     ---     |   ---     |   ---
冒泡排序  |    O(n^2)        |     O(1)     |   稳定    |  In-place
选择排序  |   O(n^2)         |     O(1)     |   不稳定  |   In-place
插入排序  |    O(n^2)        |    O(1)      |    稳定   |   In-place
快速排序  |    O(n * log n)  |  O(log n)    |    不稳定  |   In-place
<!-- 堆排序    |    O(n * log n)  |  O(1)        |    不稳定  |  In-place -->

In-place: 占用常数内存



具体：

#### 冒泡排序

```js
function sort(arr){
    var temp;
    for(var i = 0,len = arr.length ; i < len - 1 ; i++){
        for(var j = 0; j < len  - i - 1 ; j++){
            if(arr[j] > arr[j + 1]){
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    return arr;
}

```

#### 选择排序

```js
function sort(arr){
    var temp;
    for(var i =0,len = arr.length ; i < len - 1; i++){
        for(var j = i + 1 ; j < len ; j++){
            if(arr[j] < arr[i]){
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        
    }
    return arr;
}
```

#### 插入排序

```js
function sort(arr){
    var temp , j;
    for(var i = 1,len = arr.length;i < len - 1 ;i++){
        temp = arr[i]; //i 之前的数都是排好序的
        j = i - 1;
        while(j >= 0 && arr[j] > temp){
            arr[j+1] = arr[j];
            j--;
        }
        arr[j + 1] = temp;
    }
    return arr;
}
```

#### 快速排序

```js
function sort(arr){
    var len=arr.length;
    if(len<=1){ 　　　　
        return arr; 　　
    } 　　
    var pIndex=Math.floor(len/2);　　
    var pivot=arr.splice(pIndex,1);　　
    var left=[],right = []; 　　
    for(var i=0; i < len; i++){
        if(arr[i] < pivot[0]){
            left.push(arr[i]); 　　　　
        }else{　　　　　　
            right.push(arr[i]); 　　　　
        } 　
    } 　　
    return sort(left).concat(pivot,sort(right));
}
```


<!-- #### 堆排序

```js
function sort(arr){
    

}
``` -->


思考题：

给你两个已经排好序的数据，如何快速的进行归并输出，比如输入[1,3,5,7],[2,4,6,8,10] 输出[1,2,3,4,5,6,7,8,9,10] 







```js
//解答

function concatSort(arr1,arr2){
    var result = [];
    var i = 0, j = 0;
    while(i < arr1.length || j < arr2.length ){
    if(arr1[i] >= arr2[j]){
            result.push(arr2[j]);
            j++;
        }else{
            result.push(arr1[i]);
            i++
        }
    if(i == arr1.length &&  j == arr2.length) break;
    }
    return result;
}
//时间复杂度m+n
```








参考：https://juejin.im/post/57dcd394a22b9d00610c5ec8
