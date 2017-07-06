# 为什么JS不能像CSS、image一样并行下载了，而会产生阻塞？

## 为了回答这个问题需要了解一下浏览器构造页面的原理。

当浏览器从服务器接收到了HTML文档，并把HTML在内存中转换成DOM树，在转换的过程中如果发现某个节点(node)上引用了CSS或者IMAGE，就会再发1个request去请求CSS或image,然后继续执行下面的转换，而不需要等待request的返回，当request返回后，只需要把返回的内容放入到DOM树中对应的位置就OK。但当引用了JS的时候，浏览器发送1个js request就会一直等待该request的返回。因为浏览器需要1个稳定的DOM树结构，而JS中很有可能有代码直接改变了DOM树结构，比如使用document.write 或 appendChild,甚至是直接使用的location.href进行跳转，浏览器为了防止出现JS修改DOM树，需要重新构建DOM树的情况，所以就会阻塞其他的下载和呈现.

## 嵌入JS应该放在什么位置
- 放在底部，虽然放在底部照样会阻塞所有呈现，但不会阻塞资源下载。
   
- 如果嵌入JS放在head中，请把嵌入JS放在CSS头部。
   
- 使用defer:<script src='xx.js' defer='defer'></script> （注意：defer只能在ie中执行）
   
- 不要在嵌入的JS中调用运行时间较长的函数，如果一定要用，可以用setTimeout来调用
   
