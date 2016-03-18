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