## path模块使用指南

#### 引用

```javascript
var path = reqire('path')
```

1. 路径解析，得到规范化的路径格式

   ```javascript
   var myPath = path.normalize(__dirname + '/test/a//b//../c/utilyou.mp3');
   console.log(myPath); //windows: E:\workspace\NodeJS\app\fs\test\a\c\utilyou.mp3
   ```

2. 路径结合、合并，路径最后不会带目录分隔符

   ```javascript
   var path1 = 'path1',
       path2 = 'path2//pp\\',
       path3 = '../path3';
   var myPath = path.join(path1, path2, path3);
   console.log(myPath); //path1\path2\path3
   ```

3. 获取绝对路径

   ```javascript
   var myPath = path.resolve('path1', 'path2', 'a/b\\c/');
   console.log(myPath);//E:\workspace\NodeJS\path1\path2\a\b\c
   ```

   ***以应用程序为起点，根据参数字符串解析出一个绝对路径***

4. 获取相对路径

   ```javascript
   //path.relative(from, to);
   //获取两路径之间的相对关系
   var from = 'c:\\from\\a\\',
       to = 'c:/test/b';
   var _path = path.relative(from, to);
   console.log(_path); //..\..\test\b; 表示从from到to的相对路径
   ```

5. 获取路径中目录名

   ```javascript
   var myPath = path.dirname(__dirname + '/test/util.you.mp3');
   console.log(myPath);
   // E:\readText\node\path/test
   ```

6.  获取路径中文件名,后缀是可选的，如果加，请使用'.ext'方式来匹配，则返回值中不包括后缀名；

   ```javascript
   var myPath = path.basename(__dirname + '/test/util you.mp3');
   // util you.mp3
   var myPath = path.basename(__dirname + '/test/util you.mp3','.mp3');
   // util you
   ```

7. 获取路径中的扩展名，如果没有'.'，则返回空

   ```javascript
   console.log(path.extname('a.txt'));
   // .txt
   ```

8. 返回操作系统中文件分隔符

   ```javascript
   path.sep
   // window '\'   linux '/'
   ```

9. 返回操作系统中目录分隔符

   ```javascript
   path.delimiter
   // window是';', Unix中是':'
   ```

   

