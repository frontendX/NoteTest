# fs 模块
####导语
> fs是filesystem的缩写，该模块提供本地文件的读写能力，基本上是POSIX文件操作命令的简单包装。但是，这个模块几乎对所有操作提供异步和同步两种操作方式，供开发者选择。

##目录
1. readFileSync()
2. writeFileSync()
3. exists(path, callback)
4. mkdir()，writeFile()，readfile()
5. mkdirSync()，writeFileSync()，readFileSync()
6. readdir()
7. stat()
8. watchfile()，unwatchfile()
9. createReadStream()
10. createWriteStream()

##1、readFileSync()
readFileSync方法用于同步读取文件，返回一个字符串。

	var text = fs.readFileSync(fileName, "utf8");

	// 将文件按行拆成数组
	text.split(/\r?\n/).forEach(function (line) {
  		// ...
	});		

该方法的第一个参数是文件路径，第二个参数是文本文件编码，默认为utf8。

不同系统的行结尾字符不同，可以用下面的方法判断。


	// 方法一，查询现有的行结尾字符
	var EOL = fileContents.indexOf("\r\n") >= 0 ? "\r\n" : "\n";

	// 方法二，根据当前系统处理
	var EOL = (process.platform === 'win32' ? '\r\n' : '\n')

##2、writeFileSync()
语法：
代码如下:

	fs.writeFileSync(filename, data, [options])
	
接收参数：

filename      (String)            文件名称

data        (String | Buffer)    将要写入的内容，可以使字符串 或 buffer数据。

options        (Object)           option数组对象，包含：

· encoding   (string)            可选值，默认 ‘utf8′，当data使buffer时，该值应该为 ignored。

· mode         (Number)        文件读写权限，默认值 438

· flag            (String)            默认值 ‘w'

例子：

代码如下:

	fs.writeFileSync('message.txt', 'Hello Node');
	
源码：

代码如下:

	fs.writeFileSync = function(path, data, options) {
		if (!options) {
    		options = { encoding: 'utf8', mode: 438 /*=0666*/, flag: 'w' };
    	} else if (util.isString(options)) {
    		options = { encoding: options, mode: 438, flag: 'w' };
    	} else if (!util.isObject(options)) {
    throw new TypeError('Bad arguments');
    
    	}
    
    	assertEncoding(options.encoding);
    	var flag = options.flag || 'w';
    	var fd = fs.openSync(path, flag, options.mode);
    	if (!util.isBuffer(data)) {
    		data = new Buffer('' + data, options.encoding || 'utf8');
    	}
    	var written = 0;
    	var length = data.length;
    	var position = /a/.test(flag) ? null : 0;
    	try {
    		while (written < length) {
    			written += fs.writeSync(fd, data, written, length - written, position);
      			position += written;
    		}
    	} finally {
    		fs.closeSync(fd);
    	}
    };

