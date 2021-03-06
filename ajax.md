# ajax
```javascript
    // 创建一个ajax对象
    var xhr = new XMLHttpRequest();
    // 打开请求url
	// method: http请求方式：get,post
	// url: 接口地址
    // async: 设置同步或者异步请求，默认是异步，此处暂写同步 false
    xhr.open('GET','data.txt', false)
    //事件监听 一般监听的都是ready-state-change事件（ajax状态改变事件），基于这个事件可以获取服务器返回的响应头相应体的内容
    xhr.onreadystatechange = function() {
        if(xhr.readyState === 4 && xhr.status === 200) {
            var res = xhr.responseText;
            console.log(res)
        }
    }
    // 发送ajax请求
    xhr.send()
  
    // 原生js封装promise
    var  myNewAjax=function(url){
    return new Promise(function(resolve,reject){
        var xhr = new XMLHttpRequest();
        xhr.open('get',url);
        xhr.send(data);
        xhr.onreadystatechange=function(){
             if(xhr.status==200&&readyState==4){
                  var json=JSON.parse(xhr.responseText);
                  resolve(json)
             }else if(xhr.readyState==4&&xhr.status!=200){
                  reject('error');
             }
        }
    })
  }
    
```
### ajax状态和http网络状态码 
#### ajax状态
0：unsent 刚开始创建xhr,还么有发送
1：opened 已经执行了open这个操作
2：headers_received 已经发送ajax请求（ajax任务开始，响应头信息已经被客户端接收了，服务器的时间，返回http状态码） 
3：loading 相应主体内容正在返回
4：done 响应主体被客户端接受

#### http网络状态码
根据状态码能够清楚的反映出当前交互的结果及原因
  200 ok  成功（只能证明返回信息成功了，但不一定是你业务需要的）
  301 moved permanently 永久重定向
  302 临时重定向
  400 请求参数错误
  404 找不到资源
  500 未知服务器错误
  503 服务器超负荷 

### 关于http请求方式
    所有的请求都可以给服务器端传递内容，也都可以从服务端获取内容
    - get： 从服务端获取数据（给的少拿的多）
    - post:  想服务端提交推送数据（给的多拿的少）
    - delete:  删除服务端的某些内容（一般是删除一些文件）
    - put: 想服务器上存放一些内容（一般般也是文件）
    - head: 只想获取服务器返回的响应头信息，不要相应主体中的内容
    - options:  一般使用它向服务器发送一个探测性请求，如果服务器端返回了信息，说明当前客户端和服务器建立了链接，我们可以继续执行其他请求了。（trace是干这件事的但是axios这个ajax类库在基于cross domian进行跨域请求的时候，就是先发送options请求进行探测尝试，然后能连通服务器，才会继续发送其他请求）

#### get vs post 
[传递给服务器信息的方式不一样的]
get是基于url地址“问号传参的方式”把信息传递给服务器，post是基于请求主体把信息传递给服务器
get 一般应用于拿（给服务器的会少一些），而post给服务器的很多，如果post是基于问号传参方式来搞会出现一些问题：url会拼接很长，浏览器对于url的长度有有最大限度（谷歌8kb火狐7kb ie2kb ...）超过的部分浏览器就会把它截取掉
[get不安全，post相对安全]
因为get是基于“问号传参”把信息传递给服务器的，容易被骇客进行url劫持
[get会产生不肯控制的缓存，post不会]
不可控：不是想要就要，想不要就不要的，这是浏览器自主记忆的缓存，我们无法基于js控制，真实项目中我们都会把这个缓存干掉_=Math.random()

### 客户端和服务端交互模型
- 概念
  + 客户端：所有可以向服务端发送请求的一端都是客户端
  + 服务端： 所有可以接受客户端的请求，并且给其相应一些内容的都是服务端 
- 客户端和服务端交互
- step
  1. 浏览器首先向DNS域名解析服务器发送请求
  2. DNS反解析：根据浏览器请求地址中的域名，到DNS服务器中找到对应的服务器外网IP地址
  3. 通过找到的外网IP，向对应的服务器发送请求（首先访问的是服务器的WEB站点管理：准确来说先基于工具在服务器谁给你创建很多服务，当有客户端访问的时候，服务器会匹配出具体是请求哪个服务）
  4. 通过url地址中携带的端口号，找到服务器上对应的服务，以及服务所管理的项目源文件
  5. 服务器根据请求地址中的路径名称，问好或者哈希值，把客户端需要的内容进行转呗和处理
  6. 把准备的内容相应给客户端（如果请求的是HTML或者CSS等这样的资源文件，服务器返回的是资源文件中的源代码）【不是文件本身】
  7. 客户端浏览器接受到服务器返回的源代码，基于自己内部的渲染引擎（内核）开始进行页面的绘制和渲染
    - 首先计算DOM结构生成DOM TREE
    - 自行而下运行代码，加载css等资源内容
    - 根据获取的css生成带样式的redner tree
    - 开始渲染和绘制
   8. 我们把一次完整的 请求+相应 称之为“http事务”事务就是完整的一次操作，请求和相应缺一不可
   9. 一个页面完全加载完成，需要向服务器发起很多次http事务操作。一般来说：首先把HTML源代码拿出来，加载html的时候，遇到link/scrpit/img.iframe/video/audio都会简历http事务交互
      -  特殊情况：如图我们做了资源缓存处理304，而且即将加载的资源在之前加载过了，这样的操作和传统的http事务有所不一样，他们是从服务器和浏览器的缓存中读取数据，比传统的读取块很多
 10. 在客户端想服务器请求，以服务端把内容相应给客户端的时候，中间互相传递了很多内容（客户端吧一些内容传递服务器，服务器把一些内容相应给客户端），我们把传递的内容统称为“http报文”


`优化 减少请求`

#### 客户端和服务端交信息交互方式
[客户端给服务端]
问号传参
设置请求头
设置请求主体

[服务端返回给客户端]
设置响应头信息，例如把服务时间通过响应头返回给客户端，客户端通过获取响应头信息得到这个时间（响应头返回的速度是优先于响应主体的）
设置响应主体，
重要信息都在响应主体中