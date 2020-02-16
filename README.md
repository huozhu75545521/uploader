# Upload
SpringBoot+Web Uploader实现的断点上传功能

`WebUploader是由Baidu WebFE(FEX)团队开发的一个简单的以HTML5为主，FLASH为辅的现代文件上传组件。在现代的浏览器里面能充分发挥HTML5的优势，同时又不摒弃主流IE浏览器，沿用原来的FLASH运行时，兼容IE6+，iOS 6+, android 4+。两套运行时，同样的调用方式，可供用户任意选用。采用大文件分片并发上传，极大的提高了文件上传效率。
`

Uploader下载：http://fex.baidu.com/webuploader/download.html

实现流程：
    首先引入相关依赖、启动器，然后在application.yml中配置文件映射，设置端口号、最大上传限制。
    
引入Js、Css文件
    
    webuploader.css，webuploader.min.js，Uploader.swf直接从官网下载解压获取
    
    bootstrap.min.css
    webuploader.css
    jquery-3.1.1.min.js
    webuploader.min.js
    Uploader.swf


分片断点续传实现要用到对文件md5唯一值，导入了md5.js（可以从网上下载）

                  <script th:src="@{md5.js}"></script>
                  
上传的过程分为以下几步：

1、选择文件后，开始上传前；checkFile先判断文件有没有存在；可以将文件的md5值，存入数据库或者缓存中，来进行判断

2、在上传的过程中，页面会将文件的基本信息发送到服务端，包括文件的大小，名称，分片数，第几个分片等信息，发送到服务器

3、在每次上传分片的时候，checkChunk判断该分片是否上传；已经上传，则该分片不再重复上传。服务器将接收的分片文件放到md5目录下

4、页面分片上传完毕，发送merge合并请求。服务器将分片合成后，放入指定目录，并删除临时md5目录及目录下的临时分片文件



**PS:代码中有两个TODO还没有完成。**
   
        1、将文件的md5值，存入数据库或者缓存中，来进行判断
        2、删除临时md5目录及目录下的临时分片文件
