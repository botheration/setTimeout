# setTimeout
setTimeout第三个参数
setTimeout可以传入第三个参数、第四个参数….，它们表示神马呢？其实是用来表示第一个参数（回调函数）传入的参数。
setTimeout(function(a, b){ 

  console.log(a);   // 3
  
  console.log(b);   // 4
  
},0, 3, 4)

变异的一：
for (var i = 1; i < 4; i++) {
        var t = setTimeout(function(i) {
            console.log(i);  // 2. 1  4.2
            console.log(t);  // 3. 3  5.3
            clearTimeout(t);
        }, 10, i);
    }
    因为t定义在闭包外面，所以这里的t指的是最后一个循环留下来的t，所以最后一个3被清除了，没有输出。
    在没有clearTimeout的情况下i=1,t=3;i=2,t=3;i=3,t=3
    在有clearTimeout的情况下会清除最后一次循环
  
变异的二：
for (var i = 1; i < 4; i++) {
        var t = setTimeout(function(i, t) {
            console.log(i);  // 2. 2  4. 2
            console.log(t);  // 3. 3  5. 3
            clearTimeout(t);
        }, 10, 2, 3);  
    }
   此时的t定义在闭包里，循环两次结果都是传入的值
    for (var i = 1; i < 4; i++) {
        var t = setTimeout(function(i, t) {
            console.log(i);  // 2. 2  4. 2  6. 2
            console.log(t);  // 3. 4  5. 4  7. 4
            clearTimeout(t);
        }, 10, 2, 4);
    }
    注意此处传入4，会循环3次，
    总结：当传入的参数大于循环的次数的时候会把整个循环进行完；当传入的参数等于循环次数的时候会清除最后一次循环；当传入的参数小于循环数且大于初始化i的值的时候会清除最后一次循环，当传入的参数小于循环数且小于或者等于初始化i的值的时候会把整个循环进行完。
