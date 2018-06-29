# 开发遇到的js问题

## 在遍历中对象属性值要以obj[item]形式，obj.item为underfined

![1528945177212](C:\Users\何超凡\AppData\Local\Temp\1528945177212.png)

```javascript
var searchParam={proId:'100072236-8',proName:'甘油'}
  for (i in searchParam) {
    console.log(searchParam.i);
  } //undefined 	undefined
  for (i in searchParam) {
    console.log(searchParam[i]);
  }// 100072236-8 	甘油
```

