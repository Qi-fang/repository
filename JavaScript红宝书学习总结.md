#   JavaScript红宝书学习总结
个人学习历程，省去不常用的部分以及最基础的部分。完本请参考《JavaScript高级程序设计》（第三版）非常详细、易懂。  
源码整理中。。。


##  一、JavaScript简介
	核心：ECMAScript（提供核心语法功能）, BOM（与浏览器交互）, DOM（操作页面）

##	二、HTML中的script
1.	script标签位置  
	内联样式：置于"\</body>"标签上一格；  
	外嵌样式：推荐使用。浏览器会缓存js脚本，当引用相同脚本时文件可复用，维护更方便。外嵌"\<script>间的js代码会被忽略\</script>"只会加载外部js代码
2.	参数：src；type；  
	defer延迟脚本，立即加载延迟执行，有序的，在DOMContentload事件前执行；渲染完再执行  
	async异步脚本，立即加载异步执行，无序的；下载完就执行
>   补充：页面加载顺序  
>   1、解析hmtl结构  
>   2、加载css样式表和js脚本  
>   3、解析并执行脚本  
>   4、DOM树构建完成即：DOMContentload事件  
>   5、加载图片等外部文件  
>   6、页面加载完成即：load事件  
3.  noscript 脚本被禁用，浏览器不支持脚本
	
##	三、基本概念
1.  语法  
	严格模式："use strict"  
	const（声明常量）let 声明减少错误，声明多个变量用逗号隔开
	
2.  数据类型  
	基本数据类型：
		Null空对象指针；Undefined；  
		String：   
			toString()、+,
		Nunber：  
			Nunber()、parseInt()、parseFloat();
		Boolean：true/false,
			false: null undefined "" 0 NaN非数值(任何涉及到NaN的操作都返回NaN；NaN与任何都不相等，包括NaN本身);
	复杂数据类型：Object  
		constructor(创建对象的函数)、  
		hasOwnProperty(判断propertyName是否是该对象的属性)、  
		isPrototypeOf(检查是否是传入对象object的原型)、  
		propertyIsEnumerable(检查属性propertyName是否可以用for-in枚举)、  
		toLocaleString()、  
		toStirng()、  
		valueOf()
>   补充：typeof基本类型、instanceof(判断对象是否属于某个引用类型，五种基本数据类型将返回false;result = variable instanceof constructor)、constructor、toString(null和undefined没有该方法，可以用String()来判断)  
[判断数据类型的4种方法的区别](https://www.cnblogs.com/onepixel/p/5126046.html)  
3.	操作符：[运算符优先级](https://blog.csdn.net/weixin_44198965/article/details/94149656)  
			算数运算符 + - * / %（取余） ++ --前置会先求值再执行，后置则是先执行后求值  
			逻辑运算符 ||短路或（或） &&逻辑与（且） ！（非）  
			位运算符 ~按位取反 <<按位左移 >>按位右移 >>>无符号右移 ^异或  
			赋值运算符 += -= *= /= %=  
			比较运算符 > < >= <= == !=（不等于） ===（全等于） !==（全不等）(比较字符串时，比较对应位置的字符串编码值)  
			三目运算符

4.  流控制语句
	if、do_while、while、switch...case...for、(label)
>	break,continue区别：break跳出循环，continue结束当前循环。
5.  函数
		return 后面的语句永远不会执行
		后定义的函数可以覆盖之前定义的函数，但不能重载
	函数声明式：会进行函数提升
		function 函数名(){...}其name属性返回函数名
			function addSomeNumber(arguments){return arguments + 100}
			var result = addSomeNumber(200);
	函数表达式：
		var FunctionName = function(){...}其实就是将匿名函数赋值给变量FunctionName

##	四、变量作用域内存
1.	变量：[外援1号](https://baijiahao.baidu.com/s?id=1617122665184788397&wfr=spider&for=pc)、[外援2号](https://www.cnblogs.com/little-jelly/p/5742745.html)  
		基本数据类型：占据的空间是固定的，保存在栈中，用typeof判断
			访问值；				  复制新值，操作互不影响；		都是按照值进行传递
		引用数据类型：保存在堆中，用instanceof判断
			访问的是内存地址(指针)；	复制的是内存地址(指针)，操作相互影响


2.	作用域：
		局部作用域：；全局作用域：。  
		if语句和for语句结束后创建的变量还能继续使用，不会被销毁  
		内部环境可以通过作用域链访问外部环境，而外部不能访问内部。每个环境只能向上不能向下。  
		延长作用域链：catch和with
3.	垃圾回收：标记清除，引用计数
4.	优化： 为全局变量和全局对象的属性解除引用（将值设为null）
	
##	五、引用类型
>   alert(会自动调用toString())、Object.prototype.toString()可以用来检测对象的类型、.call()来调用
1.	Object
	字面量创建：  
			var person = {
				name: "fangqi",
				age: 26
			}
	
2.	Array
	字面量创建：
			var colors = ["yellow", "red", "blue"];
	属性： length、
	Array.isArray(a)判断a是不是数组
			push(添加到末尾)、pop(删除最后一个)、shift(删除第一个元素)、unshift(添加到第一项)、
			reverse(倒序)、sort(排序：默认按照字符串升序，负数为降序，整数为升序)、
			concat()、slice(截取)、splice(删除 传2个参数；插入 传3个参数且第二个参数为0；替代 传3个参数且第二个参数不为0)、indexOf(查找)、
			every()、some()、filter()、map()、forEach()、reduce()数组归并
	
3.	Date  
	var now = new Date();获取当前日期的时间戳
4.	RegExp
5.	Function
	
6.	包装类String(Number、Boolean)
			length,prototype,apply()call()bind()改变this指向、join(将数组转换为字符串)、toFixed(数值类型指定小数位数)、
			charAt(字符位置)、concat(可以用+号代替)、slice(起始位置，终点位置)、substring()、substr(起始位置，长度)、indexOf()、trim(删除前后空格)、toUpperCase(大写)、toLowerCase(小写)、
			match()
		
7.	内置对象：Global、Math  
		global(全局) = window;	encodeURI(编码)、decodeURI(解码)  
		Math：最小值min()，最大值max()，向上取整ceil()，向下取整floor()，四舍五入round()，0到1之间任意值random()，绝对值abs()
	
##	六、面向对象（重点）
1.  理解对象  
		Object.defineProperty(obj, propertyName, {
			//数据属性
			configurable: true,//可删除
			enumerable: true,//可遍历
			writable: true,//可读
			value: null,//属性值
			只要调用该方法，默认值都是false
			
			//访问器属性
			configurable: true,//可删除
			enumerable: true,//可遍历
			get: function(){}
			set: function(newValue){}
		})  
	定义及读取属性：  
	Object.defineProperty(定义单个属性)、  
	Object.defineProperties(定义多个属性)、  
	Object.getOwnPropertyDescriptor(读取属性)
2.  创建对象  
	工厂模式（无法解决对象识别问题即 怎样知道对象的类型）；  
	构造函数模式（实例化时会创建多次）构造函数以大写字母开头；  
	原型模式（引用类型值的共享）；
>	isPrototypeOf()检测对象是否在另一个对象的原型链上  
>	getPrototypeOf()返回对象的原型  
>	新创建的属性在实例中，不影响原型中的属性！  
>	hasOwnProperty()检测属性存在于实例中还是存在于原型中  
>	in
>	<!-- hasPrototypeProperty() -->
>	for-in返回自身可枚举属性包括继承的属性  
>	keys返回自身可枚举属性  
>	getOwnPropertyNames()返回所有属性包括不可枚举的属性  
		
	构造函数+原型：构造函数定义实例属性、原型模式定义方法和共享属性；  
		function Person(name, age, job){
			this.name = name;
			this.age = age;
			this.job = job;
			this.friends = ["lele", "fangqi"];
		}
		Person.prototype = {
			constructor : Person,
			sayName : function(){
				alert(this.name)
			}
		}
		var person1 = new Person("Tom", 28, "Doctor");
		var person2 = new Person("Cat", 28, "Programmer");
		Person1.friends.push("Vsc");
		console.log(person1.friends);
		console.log(person2.friends)
		console.log(person1.friends === person2.friends);
		console.log(person1.sayName === person2.sayName);
		推荐
		
	动态构造模式（增加一个 原型是否有对应方法的 判断）：  
		function Person(name, age, job){
			this.name = name;
			this.age = age;
			this.job = job;
			if(typeof this.sayName != "function"){
				Person.prototype.sayName = function(){
					console.log(this.name);
				}
			}
		}
		var friend = new Person("fangqi", 26, "dcotor");
		friend.sayName();
		推荐
	
	寄生构造函数模式（不能使用instanceof确定对象类型）；  
	稳妥够着函数模式（不能使用instanceof确定对象类型）  
[创建对象常用方法总结](https://www.cnblogs.com/looyulong/p/10973213.html)	
3.  继承  
	原型链继承；构造函数继承；组合继承；
	call/apply继承；寄生组合继承：  
			function SuperType(name){
				this.name = name;
				this.color = ["red", "yellow", "blue"];
			}
			SuperType.prototype.sayName = function(){
				alert(this.name);
			}
			function SubType(name, age){
				SuperType.call(this, name);
				this.age = age;
			}
			inheritPrototype(SubType, SuperType);
			SubType.prototypr.sayAge = function(){
				alert(this.age)
			}
			推荐

##	七、函数表达式（重点）
1.	匿名函数：其name属性返回值为null；createComparisonFunction()；私有作用域：定义一个自执行匿名函数
2.	递归：  
			var factorial = (function f(num){
				if(num < 1){
					return 1;
				}else{
					return num * f(num - 1)
				}
			})  
3.	闭包：子函数访问父函数的局部变量,因此必报得到的变量值是最终值
4.	this对象：  
	在全局环境中或者类似全局性执行环境（匿名函数）中this指向window；而当函数被对象调用时this指向调用对象  
	函数被调用时会自动获取this、arguments变量，且只会在这个函数内搜索！
>   可以向函数传递任意数量的参数，且可以通过arguments访问  
>   对arguments对象使用Array.proptotype.slice()可以将对象转化为数组！  
>   arguments对象的callee属性 指向拥有arguments对象的函数，递归使用可以解耦
5.	内存泄漏：
6.	特权方法：有权访问私有属性或私有函数的公有方法叫特权方法
7.	静态私有变量：私有作用域中定义私有变量或函数
6.	模仿块级作用域：单例创建私有变量和特权方法
9.	增强模块：增强单例的特权方法
	
##	八、BOM
1.  window(open(打开新窗口)、close(关闭窗口)；top(指向最外层)、parent(指向上层)、self(指向window))frame
2.  间歇调用（setinteval定时器）、超时调用（setTimeout延迟器）
3.  系统对话框（alert(警告框1)，confirm(确认框2)，prompt(输入框)）windowprint(打印)
4.  locaiton(href(跳转到指定页面)，search(返回从问号到URL末尾的所有内容)，reload(重新加载页面))
5.  history.go()可以是数字或者位置的字符串

##	九、客户端检测
	能力检测、怪癖检测、用户代理检测

##	十、DOM
>	DOM操作非常耗性能，尽量避免DOM操作！
1.	节点关系：
2.	操作节点：  
	添加到末尾appendChild()、插入insertBefore()、替换节点replaceChild()、移除节点removeChild()、createElment()、
	克隆cloneChild()接收两个参数：1是否深拷贝，2、创建元素
3.	常用的Document类型：元素节点1、文本节点text2、属性节点3  
+		文档类型：document.domain	根节点文档节点的唯一子节点<html>元素即文档元素  
*		查找元素：getDocumentById如果多个元素ID值相同只返回第一次出现的元素  
+		文档写入：document.write、writeln(换行)，node.data访问文本内容  
*		取得元素属性：getAttribute()、setAttribute()、removeAttribute()  
	Node、parentNode父节点、childNodes子节点们、nextSibling下一个、previousSibling上一个、firstChild第一个、lastChild最后一个、NodeList(等同于arguments，都可以使用Array.prototype.slice())  
	document: url获取完整URL, domain获取域名, referrer获取上一个页面URL  
	ownerDocument指向文档节点  
	DocumentFragment文档碎片，可用来更新DOM树
	
##	十一、DOM扩展
1.	选择元素querySelector()、querySelectorAll()
2.	元素遍历：新增5个属性
3.	getElementsByClassName()、classList.toggle有值删除 无值添加
4.	HTMLDocument  
	1、readyState:loading正在加载，complate加载完成
5.  插入标记  
	innerHTML返回调用元素所有的子节点  
	outHTML返回调用者本身的所有子节点
6.  内存与性能问题： 
	
##	十二、DOM+
1.  样式  
	myDiv.style.cssText = {"width: 20px; hegiht: 10px; background: wihte"}多个值  
	1、元素大小
2.  遍历  
	NodeIterator:nextNode()、previousNode()  
	var iterator = document.createNodeIterator(root, NodeFilter.SHOW_ELEMENT, filter, flase)  
	TreeWalker:parentNode()、firstChild()、lastChild()、nextSibling()、previousSubling()  
	var walker = document.createTreeWalker(div, NodeFileter.SHOW_ELEMENT, filter, flase)
3.  范围
	
##	十三、事件（重点）
1.  事件流
+	事件冒泡 推荐使用；处于目标阶段；事件捕获  三个阶段  
	添加事件：addEventListener("要处理的事件名"，"函数"，true捕获阶段调用/false冒泡阶段调用)  
	移除事件：removeEventListener(参数与addEventListener参数相同)
+	事件对象：event  
	currentTarget(正在处理事件的元素)、target(事件的目标)、type(事件类型)、cancelable()、
	preventDefault(阻止默认事件)、stopPropagation(取消捕获或者冒泡)、eventPhase(事件处于什么阶段)
+	事件类型  
			load(页面加载完成后触发)scroll(滚动事件)resize(浏览器窗口变化是触发)；focus(获得焦点时触发)blur(失焦时触发)  
			a.鼠标事件   
				click(点击事件)、dbclick(双击事件)，
				mousewheel(鼠标滚动事件)、mousedown(鼠标按下时触发)、mouseenter(移入时触发)、mouseleave(移出时触发)、
				mousemove(移动时触发)、mouseout()、mouserover()、mouseup(鼠标松开时触发)  
			b.键盘事件  
				keydown()、keyup()，键码，textInput事件  
			c.contextmenu()、beforunload()、DOMContentLoaded(DOM树创建完成后触发)、hashchange  
			d.设备事件、触摸事件：  
				touchstart(触摸时)、touchmove(触摸移动)、touchend(触摸结束)、touchcancel()
				属性有：identifier获取设备ID、target触摸的DOM节点目标  
			e.手势事件：  
				readystatechange:uninitialized未初始化、loading、loaded、interactive交互、complete完成
2.  内存与性能  
	事件委托
		
3.  模拟事件
	createEvent(创建)->初始化->dispatchEvent(触发)
	
##	十四、表单脚本
1.  表单
	提交按钮submit；防止重复提交：提交后使用禁用按钮；重置表单  
		EventUtil.addHandler(form, "submit", function(event){
			event = EventUtil.getEvent(evnet);
			var target = EventUtil.getTarget(event);
			
			var btn = target.elements["submit-btn"];
			btn.disabled = true;
		})  
	每个表单字段都有focus和blur方法和focus、blur、change事件
2.  文本：选择文本select()，selectionStart、selectionEnd事件  
	setSelectionRange()方法参数类似substring的两个参数  
	selectText()接收三个参数：要操作的文本框，类似substring的两个参数  
	过滤输入；必填字段：required；检测有效性validity()方法，novalidate属性表示不验证；  
	重排序：使用DOM节点操作  
	序列化
3.	富文本编辑：插入iframe；设置contentEditable属性为true
	
##	十五、canvas
1.  填充fillStyle和描边strokeStyle
2.	矩形fillRect()、strokeRect()、clearRect()
3.	弧形、圆
4.	绘制文本： fillText()；strokeText()接收4个参数：文本字符串，x,y坐标，可选参数最大像素密度
	拥有三个属性: font(文本样式、大小、字体)，对齐方式，基线
	
##	十六、HTML5
1.  XDM跨文档消息传送postMessage()：一条消息， 接收方来自哪个域  
	接收XDM会触发message事件:三个参数，data，origin这个参数必须判断域相同，source；可选执行回调event.source.postMessage()
2.  拖放dragstart->drag->draged;dragenter->dragover->dragleave->drop  
	dataTransfer：  
	dropEffect元素放置的位置，该属性必须放置在ondragenter事件中  
	effectAllowed两者相互搭配才有效，必须放置在ondragstart事件中
3.  video、audio
4.  历史状态管理  
	history.pushState()/replaceState()不会创建新状态 hashchange
  
##	十七、DEBUG
1.  错误报告
2.	错误处理：
	try{}catch(error){message保存着错误信息}finally{一定会执行}  
	throw  
3.	常见的错误类型：  
	RangeError:范围错误  
	ReferenceError:找不到对象、变量不存在  
	TypeError:访问不存在的方法、传入类型不符。  
	类型转换错误：即“==”“===”的使用、if等流程控制语句使用非布尔值  
	数据类型错误：
4.	调试：assert(condition, message){}可以代替部分函数的if语句
	
##	十八、JS与XML
	略
	
##	十九、E4X
	略
	
##	二十、JSON
>	是一种数据格式，JSON字符串必须用双引号
1.	JSON.stringify()把JS对象序列化为JSON字符串  
	还可以接受另外两个参数：过滤器（数组，replacer过滤函数function(key, value)）、选项是否保留缩进
2.	JSON.parse()将JSON字符串解析成js对象  
	还可以接受另外一个参数：reviver还原函数function(key, value)
>	深拷贝：var copy = JSON.parse(JSON.stringify(js))
3.  toJSON()返回自身的JSON数据格式
	
##	廿一、Ajax与comet（重点）
>	Ajax核心XMLHttpRequest
1.  var xhr = new XMLHttpRequest()//创建核心对象var xhr = createXHR();
2.  xhr.onreadystatechange = function(){if(xhr.status)}
3.  xhr.open()//请求类型、请求的URL、是否异步
4.  xhr.send()//发送
5.  abort终止、error错误、responseText响应文本、status响应的状态、readyState
6.  GET请求：请求参数会追加到URL末尾
7.  POST请求
8.  FormData；模仿表单提交；progress创建进度  
	append();
9.  跨域：CORS，JSONP，SSE：长轮询、HTTP流
10.  CSRF攻击
	
##  廿二、高级技巧
1.  Object.prototypt.toString.call(value)安全的类型检测
2.  惰性载入：方法二创建一个匿名的自执行函数；  
	函数柯里化：只传递一部分参数给函数调用，让他返回一个函数去处理剩下的参数
3.  防篡改对象：Object.preventExtensions()不能给对象添加属性和方法
	Object.istExtensible()对象是否可拓展  
	密封对象：Object.seal()不能添加、删除属性和方法
	Object.isSealed()确认对象是否封装  
	冻结对象：Object.freeze()不能添加、删除、修改属性和方法
4.  高级定时器：重复定时器setTimeout(){setTimeout(){}}链式调用
5.  节流函数throttle(要执行的函数，哪个作用域执行)自动进行定时器的设置和清除
6.  自定义事件：观察者模式
7.  拖放DragDrop
	
##  廿三、客户端储存
1.  navigator.onLine在线、navigator.offline离线
2.  核心对象applicationCache
3.  数据储存：  
	cookie：有数量和长度的限制  
	WebStorage：sessionStorage会话，localStorage  
	clear()，getItem()，setItem()，removeItem()；
	storage事件
4.  indexedDB.open()添加onerror和onsuccess事件
	
##  廿四、最佳实践
1.  可维护性：可读性（每个函数和方法描述其目的，参数代表什么，算法说明），变量名用名词函数名用动词布尔值is开头，使用常量
2.  性能：避免全局查找；使用局部变量将属性查找替代为值查找：var URL = window.location.href；do-while循环；Duff  
	使用文档碎片构建DOM结构，innerHTML一次现场更新，事件代理，合并文件，最小化访问HTMLCollection：
	调用getElementsByTagName()、获取childNodes、attributes属性、document.forms、document.images
3.  部署

##  廿五、新兴API
1.  requestAnimationFrame()动画循环
2.  document.hidden()表示页面是否隐藏
3.  getCurrentPosition(){成功回调Position参数，可选的失败回调参数，可选的非常精确的定位}  
	watchPosition(成功回调Position参数，可选的失败回调参数)，clearWatch()取消监控
4.  File、FileReader：progress、error、load、abort()中断读取、loadend()读取完文件可能是读完文件或者读取发生了错误
	或者读取中断、slice()读取部分内容、读取拖放的文件、使用XHR和FormData以表单提交的方式上传文件
5.  Web计时：window.performance
6.  Worker  
	var worker = new Worker("stufftodo.js")//实例化Worker  
	worker.postMessage("")  
	onerror事件  
	worker.terminate()//立即停止Worker  
	全局作用域中this、self指向worker本身 调用close()也可以停止，importScript()添加脚本

##  附录
1.  ...、默认参数、生成器
2.  迭代器Iterator,next()调用下一个值；数组领悟；解构赋值：等于号“=”
3.  代理对象Proxy.create(handler, prototype原型对象)；映射Map（get、set、delete）与集合Set（add、has、delete）
4.  class、extends
5.  模块module

加密
工具：校验器、压缩器、单元测试、文档生成器、安全执行环境

>推荐免费视频网址：学堂在线->Web前端工程狮  
参考：
[参考网站1](https://www.jianshu.com/p/a59053f5188d)
[参考网站2](https://segmentfault.com/a/1190000017025825)
[很炫酷的网页](https://diygod.me/2073/)
[参考网站3](https://www.kancloud.cn/crossken/professional_js_web_developers/207402)
[参考网站5](http://www.ayqy.net/blog/《javascript高级程序设计》学习笔记12篇/)
### 后记[ES6](https://github.com/Qi-fang/repository/blob/master/ES6%E7%AC%94%E8%AE%B0.md)、[TypeScript](http://127.0.0.1:8848/dcloudmdpaser?filename=D%3A%2F%E8%B5%84%E6%96%99%2FTypeScript.md&theme=github-markdown.css&projectname=&repath=D%3A%2F%E8%B5%84%E6%96%99%2F&charset=UTF-8)整理中...尽情期待！