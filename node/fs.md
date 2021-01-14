# fs使用指南

`fs` 模块可用于与文件系统进行交互

```javascript
const fs = require('fs');
```

所有的文件系统操作都具有同步的、回调的、以及基于 promise 的形式。

#### 文件或者文件夹是否存在

```javascript
fs.access(filpath,(err)=>{
    if(err){
        // 文件不存在
    }else{
        // doSomeThing...
    }
})
```

#### 统计信息

```javascript
fs.stat(filepath,(err,stats)=>{})
// stats 属性和方法
// stats.isFile()
// stats.isDictionary()
// stats.mode  获取权限
// stats.size: 字节长度
// stats.ctime 属性或内容上次被修改的时间 (设置缓存的响应头可能会用到)
// stats.mtime 档案的内容上次被修改的时间 (设置缓存的响应头可能会用到)
// stats.atime 上次被读取的时间
```

#### 读取文件

```javascript
fs.readFile(filepath,(err,data)=>{
    console.log(data.toString())
})
// or
fs.readFile(filepath,{encoding:'utf-8'},(err,data)=>{
    console.log(data)
})
// 同步方法fs.readFileSync，返回字符串
```

#### 写入文件

```javascript
function w(){
    fs.writeFile(filepath,'新内容',{encoding:'utf-8',flag:'w+'},(err,data)=>{
		if(err){
            //如果是权限不足
            fs.chmod(filpath,0755,(err)=>{
                if(err) throw err
                arguments.callee()
            })
        }
    })
}
// mode值说明
0600：所有者可读写，其他的用户不行
0644：所有者可读写，其他的用户只读
0740：所有者可读写，所有者所在的组只读
0755：所有者可读写，其他用户可读可执行
```

#### 目录操作

##### 创建目录

使用fs.mkdir(path,[mode],callback)创建目录，path是需要创建的目录，[mode]是目录的权限（默认是0777），callback是回调函数。

```javascript
// 创建 newdir 目录
fs.mkdir('./newdir', function(err) {
    if (err) {
        throw err;
    }
    console.log('make dir success.');
});
```

##### 读取目录

使用fs.readdir(path,callback)读取文件目录。

```javascript
fs.readdir('./newdir', function(err, files) {
    if (err) {
        throw err;
    }
    // files是一个数组
    // 每个元素是此目录下的文件或文件夹的名称
    console.log(files);
});
```







​	