---
layout: default
title: Trouble Shooting List For Ruby
---



## puts,p,print的区别 ##

puts 输出内容后，会自动换行(如果内容参数为空，则仅输出一个换行符号)；另外如果内容参数中有转义符，输出时将先处理转义再输出。

p 基本与puts相同，但不会处理参数中的转义符号。

print 基本与puts相同，但输出内容后，不会自动在结尾加上换行符。

    s = "aaaa\nbb\tbb"
     
    p s
    p "****************"
    puts s
    p "****************"
    print s

输出结果为

    "aaaa\nbb\tbb"
    "****************"
    aaaa
    bb bb
    "****************"
    aaaa
    bb bb>Exit code: 0

更多细节请参考，[puts,p,print的区别](http://www.cnblogs.com/yjmyzz/archive/2010/02/22/1671130.html)

## Ruby 1.9 String Class 不再支持each 方法 ##

Ruby 1.9 String类删除了each 方法，取而代之each_line或者lines方法。 

如果希望能够保留each方法，可以执行以下代码： 

if RUBY_VERSION.match('1.9') 

  class String 
    alias each each_line 
  end 
end 


## ruby 中super和super()的区别 ##

我们经常要在子类的initialize方法中调用super和super()。

从语法上说super和super()是有微妙区别的。

super不带括号表示调用父类的同名函数，并将本函数的所有参数传入父类的同名函数；

super()带括号则表示调用父类的同名函数，但是不传入任何参数；

演示代码如下：

    class SParent
    	def initialize *args
    		args.each {|arg| puts arg}
    	end
    end
    
    class SChild < SParent
    	def initialize a, b, c
    		super
    	end
    end
    
    a, b, c = *%W[a b c]
    SChild.new a, b, c # puts a, b, c if super
    SChild.new a, b, c # puts nothing if super()

可以看出当SChild的initialize中调用super()时，代码是不会打印任何信息的。这是因为super()没有向SParent的initialize方法传任何参数。

## Error：invalid multibyte char (US-ASCII) ##

Error：Ruby 1.9 - invalid multibyte char (US-ASCII)

解决办法：


Write # encoding: utf-8 on top of that file. That changes the default encoding of all string/regexp literals in that file utf-8.

更多细节请参考，[Ruby 1.9 - invalid multibyte char (US-ASCII)](http://stackoverflow.com/questions/3678172/ruby-1-9-invalid-multibyte-char-us-ascii)



## Error：unknown regexp options - latay ##


检查是否使用了半角“”


## Ruby 'require' error: cannot load such file ##


问题如果用require加载文件（引用别的文件）

方法为： require ".\\文件名"           //当文件在同一目录下，注意要用“\\”

或者用绝对路径： 但是也需要用“\\”

更多细节请参考，[Ruby 'require' error: cannot load such file](http://stackoverflow.com/questions/9750610/ruby-require-error-cannot-load-such-file)

## top (required)>': uninitialized constant WWW (NameError) ##

remove the WWW:: - that got removed a long time ago.


## Could not find Chrome binary（watir） ##

Please install the chrome to the default directory, the chrome installations would automatically install the app to the default folder:

> %HOMEPATH%\Local Settings\Application Data\Google\Chrome\Application\chrome.exe


## embedded document meets end of file ##

注释中的begin，end不对齐


## protocol.rb Timeout::Error ##

Found this while perusing the web. Hopes it helps somebody! Setting a longer timeout

Here is how you can adjust the timeout in the Net::HTTP API:

    require 'net/http' 
    
    http = Net::HTTP.new(@host, @port)
    http.read_timeout = 500

More Information, pls refer to ['rescue in rbuf_fill': Timeout::Error (Timeout::Error)](http://stackoverflow.com/questions/10011387/rescue-in-rbuf-fill-timeouterror-timeouterror)

## Encoding::UndefinedConversionError: "\xE5" from ASCII-8BIT to UTF-8 ##

将

    str.encode('UTF-8') if str.encoding != 'UTF-8'

改为

    str = str.force_encoding('GBK')
    str = str.encode('UTF-8')