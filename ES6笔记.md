#   ES6笔记
##  一、ES6简介
	Babel转码器、Traceur转码器

##	二、let和const
1.  let  
	代码块内有效，不存在变量提升，不允许重复声明
2.  块级作用域  
	块级作用域用函数表达式声明，必须有大括号；块级作用域的出现导致匿名自执行函数不再是必要的
3.  const  
	声明一个只读的常量，一旦声明不能改变；代码块内有效，不存在变量提升  
	冻结对象Object.freeze（添加新属性）
>   6种声明方法：var funtion（前两个是顶层变量） let const class（后三个是全局变量） import
4.  顶层对象的属性
5.  globalThis对象  
		全局环境中：this返回顶层对象，ES6返回当前模块  
		函数内的this：函数运行this指向顶层对象  
		new Function('return this')() 返回全局对象
	
##	三、解构赋值
1.  数组解构  
	模式匹配：  
		let [a, b, c] = [1, 4, 6]等号两边模式相同  
		let {foo, bar} = {foo: 'aaa', bar: 'bbb'}  
		解构失败值为undefined  
		变量名与属性名不一致： let{foo: baz} = {foo: 'aaa', bar: 'bbb'}调用第二个变量baz  
		const node = {
		  loc: {
			  start: {
				  line: 1,
				  column: 5
			  }
		  }
		};
		let{loc, loc: {start}, loc: {start: {line}}} = node;
		line //1
		loc //Object {start: Object}
		start //Object {line: 1, colimn: 5}

	嵌套：  
	
		let obj = {};
		let arr = [];
		({foo: obj.prop, bar: arr[0]} = {foo: 123, bar: true})
		obj //{prop:123}
		arr //[true]

	继承：  
	
		const obj1 = {}
		const obj2 = {foo: 'bar'}
		Object.setProptotypeOf(obj1, obj2);
		const {foo} = obj1;
		foo //"bar"
	
	默认值生效的条件是：对象的属性值严格等于undefined  
	
		var {x: y = 3} = {x: 5};
		y //5
  
2.  对象解构、字符串解构、数值、布尔值解构、函数参数解构  
	
		function move({x, y} = {}){
			return [x, y];
		}
		move({x: 3, y: 8}); //[3, 8]
		move({x: 3}); //[3, 0]
		move({}); //[0, 0]
		move(); //[0, 0]
		
		function move({x, y} = {x = 0, y = 0}){
			return [x, y];
		}
		move({x: 3, y: 8}); //[3, 8]
		move({x: 3}); //[3, undefined]
		move({}); //[undefined, undefined]
		move(); //[0, 0]
3.  用途  
	交换值，从函数中返回多个值，遍历  
	
		for(let [key, value] of map){}  
		for(let [, value] of map){}

##	七、拓展
1.  模板字符串(\`${}`)；反引号内含有反引号用反斜线转义  
repeat()返回一个新的重复n次的字符串  
padStart()头部补全/提示字符串格式'09-11'.padStart(10, "YYYY-MM-DD") //"YYYY-09-12"  
Math.trunc()去除小数部分
	
##	八、函数拓展
1.  函数参数使用默认值时函数不能有同名函数  
		
	解构：  
		function foo({x, y = 5}){
			console.log(x, y);
		}
		foo({x: 1, y: 2}) //1 2
		foo({x: 1}) //1 5
		foo({}) //undefined 5
		foo() //undefined undefined
	
	参数默认值：  
		function foo({x, y = 5} = {}){
			console.log(x, y);
		}
		foo({x: 1, y: 2}); //1 2
		foo({x: 1}); //1 5
		foo({}); //undefined 5
		foo(); //undefined 5
2.  函数的length等于默认值前参数个数（不包括默认值参数）

3.  rest参数及是拓展运算符：...变量名  
			
		funtion sortNumbers(){
			return Array.prototype.slice.call(arguments).sort();
		}
		
		const sortNumbers = (...numbers) => numbers.sort();
	函数的length不包括rest参数，且rest参数只能放于最后
	
4.  name属性  
	ES6更正：函数表达式的name属性返回函数名
	
5.  箭头函数  
			
		var f = function(v){
			return v;
		}
	
		var f = v => v;
	函数体内的this对象就是定义时所在的对象  
	不可以当做构造函数，不能new；不可以使用arguments对象；不可以使用yield，箭头函数不能用作Generator函数  
		function foo(){
			setTimeout(() => {
				console.log('id', this.id);
			}, 100);
		}
		
		var id = 21;
		foo.call({id: 42}); //id: 42
	定义对象的方法时不能使用箭头函数（因为对象不构成单独的作用域）；  
	需要动态this
	
6.  尾调用优化：函数的最后一步是调用另一个函数。只在严格模式下生效！  
			
		function f(x){
			return g(x);
		}
	尾递归：  
	
		function Fibonacci(n, ac1 = 1, ac2 = 1){
			if(n <= 1){return arc2};
			return Fibonacci(n - 1, ac2, ac1 + ac2);
		}
		Fibonacci(100);
		Fibonacci(1000);
		fibonacci(10000);
		
		function factorial(n, total = 1){
			if(n === 1) return total;
			return factorial(n - 1, n * total);
		}
		factorial(5);
	
	蹦床函数：  
	
		function trampoline(f){
			while (f && f instanceof Function){
				f = f();
			}
			return f;
		}
	
	真正的尾递归优化  
	
		function tco(f){
			var value, active = false, accumulated = [];
			return function accumulator(){
				accumulated.push(arguments);
				if(!active){
					active = true;
					while(accumulated.length){
						value = f.apply(this, accumulated.shift());
					}
					active = false;
					return value;
				}
			}
		}
		var sum = tco(function(x, y){
			if(y > 0){
				return sum(x + 1, y - 1)
			}else{
				return x;
			}
		})
		sum(1, 100000); //100001
	
##	九、数组拓展
1.  扩展运算符：...数组，不再需要apply方法，与Math.max，push方法的结合使用，  
  	复制数组：  
		const a1 = [1, 2];
		const a2 = [...a1];//const [...a2] = a1;
		合并数组：
		const arr1 = ['a', 'b'];
		const arr2 = ['c', 'd', "f"]
		[...arr1, ...arr2]  //解构赋值是浅拷贝！
	默认调用遍历器接口（Symbol.iterator），有该接口将自动转换为数组
	
2.  Array.from可以将类似数组的对象和可遍历的对象转换为真正的数组、接收第二个参数（用于处理元素后返回新数组）、第三个参数用来绑定this；  
	可用来将字符串转换为数组
	
3.  Array.of()将一组值转换为数组
4.  copyWithin(target(必须，该位置开始替换), start(可选，该位置开始读取), end(可选))
5.  find(回调函数接收三个参数：当前值，当前位置，原数组)查找符合条件的数组成员，findIndex()接收的第二个参数用来绑定回调函数的this对象
6.  fill(， 开始(可选)，结束(可选))给定一个值填充数组
7.  entries(键值对的遍历)、keys(对键遍历)、values(对值遍历)用于遍历数组
8.  includes()是否包含某值
>   Map的has用来查找键名  
>   Set的has用来查找值  
9.  flat(n)将n维数组拉平成一维数组：Infinity任意层、flatMap(参数1：当前数组成员，该成员位置，原数组)对每个成员执行一个函数，只能展开1层；参数2：绑定遍历函数this
10. 空位undefined，避免出现

##	十、对象拓展
1.  属性简介表示  
		function f(x, y){						|	function f(x, y){
			return {x, y}						|		return {x: x, y: y}
		}										|	}
		f(1, 2) //Object {x: 1, y: 2}
		
		const o = {							  |	const o = {
			method(){							|		method: function(){
				return "Hello!";				 |			return "Hello!";
			}									|		}
		}										|	}
		
		let user = {
			name: 'test'
		};
		let foo = {
			bar: 'baz'
		}
		console.log(user, foo) //{name: "test"}{bar: "baz"}
		console.log({user, foo}) //{user: {name: "test"}, foo: {bar: "baz"}}
	
2.  属性的可枚举和遍历  
>   for...in：遍历自身和继承的可枚举属性  
>   Object.keys()：返回自身的可枚举的键名  
>   JSON.stringify()：字符串化对象自身的可枚举属性  
>   Object.assign()：拷贝对象自身的可枚举属性  
>   Object.getOwnPropertySymbols：返回一个包含对象自身的所有Symbol属性键名的数组  
>   Reflect.ownKeys：返回一个数组，不管自身是否可枚举不管键名是Symbol、字符串的所有键名
3.  super指向当前对象的原型对象，只能用在对象的方法中！
4.  扩展运算符...对象
	
##	十一、对象新增方法
1.  Object.is()比较两个值相等，不转换数据类型；+0不等于-0，NaN等于自身
2.  Object.assign()第一个对象是目标对象，后面的对象是源对象，只能拷贝源对象的自身属性，不拷贝继承属性，也不拷贝不可枚举属性；浅拷贝！  
    同名属性会被替换；取值函数复制的是值
3.  Object.getOwnPropertyDescriptors()返回指定对象所有自身属性的描述对象  
	const shallowMerge = (target, source) => Object.defineProperties(target, Object.getOwnPropertyDescriptors(source));
4.  \__proto__，Object.setPeototypeOf(写)，Object.getPrototypeOf(读)，Object.create(创建)
5.  Object.keys()，Object.values()，Object.entries()
6.  Object.fromEntries()将键值对数组转换为对象。可用来处理URL查询字符串
	
##	十二、symbol
1.  第七种数据类型：防止属性名冲突
2.  sym.description 直接返回symbol的描述
3.  属性名  
		let mySymbol = Symbol();
		let a = {};
		a[mySymbol] = 'Hello!'; //只能用方括号指定属性名
		增强的对象写法：let obj = {[s](arg){...}}
	
4.  Object.getOwnPropertySymbols 可以用来定义非私有的内部方法！
5.  Symbol.for()全局的 相同的参数生成同一个值，Symbol.keyFor()返回第一个，其它相同的参数返回undefined
6.  内置值  
	hasInstance判断其他对象是否是该对象的实例、  
	isConcatSpreadable是否可以展开、  
	species创造衍生对象时指向species“绑定”的构造函数、  
	match属性（即函数）存在就调用它然后返回方法的返回值、  
	replace、search、split、iterator返回对象默认遍历器方法、  
	toPrimitive返回对象对应的原始类型值：Default可以是字符串也可以是数值、  
	toStringTag返回toString方法的对象类型第二个字符串[object Array]、  
	unscopables
	
##	十三、set和map数据结构
1.  Set  
	元素的值是唯一的，数组去重[...new Set(array)]；字符串去重[...new Set('ababbc')].join('')  
	属性：size；  
	方法：add，delete，has是否有该成员，clear清除所有；  
	遍历：keys（键）、values（值）、entries（键值对）、forEach（使用回调函数遍历每个成员）参数一：处理函数，参数二：处理函数内部的this对象
2.  WeakSet  
	成员只能是对象，不能有其他类型的值；没有size属性，无法遍历；弱引用，用来储存DOM节点
3.  Map  
	值-值  拓展运算符...可以将Map转为数组
4.  WeakMap 没有clear()，另一用处是部署所有属性
	
##	十四、Proxy代理
1.  Proxy构造函数  
		var proxy = new Proxy(target拦截的目标对象, handler定制拦截行为);
		
		var handler = {
			get: function(target, name){
				if(name === "prototype"){
					return Object.prototype;
				}
				return 'Hello, ' + name;
			},
			apply: function(target, thisBinding, args){
				return args[0];
			},
			construct: function(target, args){
				return {value: args[1]}
			}
		}
		var fproxy = new Proxy(function(x, y){
			return x + y;
		}, handler);
		console.log(fproxy(1, 2));
		console.log(new fproxy(1, 2));
		console.log(fproxy.prototype === Object.prototype);
		console.log(fproxy.foo === "Hello, foo");
		
		支持拦截的操作有13种
2.  实例方法
	+		get(target目标对象，args属性名，receiver选)拦截某个属性的读取操作；可实现链式操作，生成各种DOM节点的通用函数。如果一个属性不可配置且不可写，不能用Proxy修改属性
	-		set(目标对象，属性名，属性值，选)拦截某个属性的赋值操作；防止内部属性被外部读写。如果一个属性不可配置且不可写，set方法不起作用
	*		apply(目标对象，this，参数数组)拦截函数调用、call、apply操作
	+		has(目标对象，查询的属性)拦截HasProperty操作不被in运算符发现即隐藏属性。如果一个属性不可配置且不可写，has方法会报错。对for...in无效
	-		construct(target，args构造函数的参数对象，newTarget实例对象)拦截new
	*		deleteProperty()拦截删除操作。目标对象不可配置属性 deleteProperty会报错
	+		defineproperty()拦截Object.defineProperty操作。目标对象不可拓展，不能增加对象不存在的属性；目标对象属性不可写或不可配置，不能改变这两个设置
	-		getOwnPropertyDescriptor()拦截Object.getOwnPropertyDescriptor()，返回属性描述对象或者undefined
	*		getProtorypeOf()拦截获取对象原型，返回对象或者null。如果目标对象不可拓展，返回目标对象的原型对象
	+		isExtensible()拦截Object.isExtensible。只能返回布尔值且与目标对象的isExtensible属性保持一致
	-		ownKeys()拦截对象自身的读取属性操作
	*		preventExtensions()拦截Object.preventExtensions()，返回一个布尔值。只有目标对象不可拓展才能返回true
	+		setPrototypeOf()拦截Object.setPrototypeOf()，返回布尔值。如果目标对象不可拓展，该方法不得改变目标对象的原型
3.  Proxy.revocable()返回一个可取消的Proxy实例。通过代理访问，访问结束后回收代理权。`Web服务的客服端`
	
##	十五、Reflect
1.  静态方法  
	与proxy方法一一对应  `实现观察者模式`
	
##	十六、promise对象
1.  生成Promise实例  
		const promise = new Promise(function(resolve, reject){
			if(){
				return resolve(value);
			}else{
				return reject(error);//可选
			}
		})
		promise.then(function(value){
			//success
		}, function(error){
			//error可选，一般不用这个参数
		}).catch.finally一定会执行
		
		promise对象是微任务
	
2.  all()只要有一个rejected，就会返回reject实例；全部为resolve则返回resolve实例  
	race()谁先改变完成就返回它的实例  
	Promise.resolve()参数分4种情况  
	Promise.reject()参数会变成后续方法的参数
	
3. try()  
	async()同步函数同步执行，异步函数异步执行
  
##	十七、Iterator和for...of循环
1.  四种集合：Array、Object（非原生具备）、Map、Set  
	数据结构只要部署了Iterator接口，就可以遍历
	
2.  默认调用Symbol.iterator  
	for...of、结构赋值、拓展运算符...、yield
	
3.  遍历器对象的方法：  
	next()、return()、throw()
	
4.  for...of  
	数组（只返回具有数字索引的属性）、Set、Map、类似对象的数组（arguments对象、DOM NodeList对象）、Generator对象  
	for...in遍历数组的键名key：缺点键名如果是数字循环后以字符串作为键名；还会遍历手动添加的其他键、原型链上的键。主要用来遍历对象
	
##	十八、Generator语法
1.  异步编程解决方案：封装内部状态，返回遍历器对象  
		function* generators(){
			yield "hello";
			yield 'world';
			return 'ending';
		}
		var hw = generators();
	
2.  next()  
		function* foo(x) {
			var y = 2 * (yield (x + 1));
			var z = yield(y / 3);
			return (x + y + z);
		}
		var a = foo(5);
		console.log(a.next());//Object{value: 6, done:false}
		console.log(a.next());//Object{value: NaN, done:false}
		console.log(a.next());//Object{value: NaN, done:true}
		
		var b = foo(5);
		console.log(b.next());//Object{value: 6, done:false}
		console.log(b.next(12));//Object{value: 8, done:false}
		console.log(b.next(13));//Object{value: 42, done:true}
	next的参数表示yield表达式的值，第二次使用时才有效
	
3.  for...of  
		function* fibonacci(){
			let [prev, curr] = [0, 1];
			for(;;){
				yield curr;
				[prev, curr] = [curr, prev + curr];
			}
		}
		for (let n of fibonacci()){
			if (n > 1000) break;
			console.log(n);
		}
4.  throw、return；yield*表达式返回该遍历器对象的内部值（yield返回一个遍历器对象）例子。。。  
	yield* 后面的Generator函数（没有return）等同于在Generator函数内部部署了一个for...of循环   `yield遍历`  
		function Tree(left, label, right){
			this.left = left;
			this.label = label;
			this.right = right;
		}
		function* inorder(t){
			if(t){
				yield* inorder(t.left);
				yield t.label;
				yield* inorder(t.right);
			}
		}
		function make(array){
			if(array.length == 1) return new Tree(null, array[0], null);
			return new Tree(make(array[0]), array[1], make(array[2]));
		}
		let tree = make([[['a'], 'b', ['c']], 'd', [['e'], 'f', ['g']]]);
		var result = [];
		for (let node of inorder(tree)){
			result.push(node);
		}
		console.log(result); //['a', 'b', 'c', 'd', 'e', 'f', 'g']
		
		let obj = {
			* myGeneratorMethod(){...}
		}
		等同于
		let obj = {
			myGeneratorMethod: function* (){...}
		}
		
5.  this  
		function* gen(){
			this.a = 1;
			yield this.b = 2;
			yield this.c = 3;
		}
		function F(){
			return gen.call(gen.prototype);
		}
		var f = new F();
		f.next();
		f.next();
		f.next();
		f.a;
		f.b;
		f.c;
6. 状态机  
		var clock = function* (){
			while (true){
				console.log('Tick!');
				yield;
				console.log("Tock!")
				yield;
			}
		}
7. 应用：  
	1、异步操作的同步化表达，改写回调函数`如Ajax`  
		function* loadUI(){
			showLoadingScreen();
			yield loadUIDataAsynchronously();
			hideLoadingScreen();
		}
		var loader = loadUI();
		loader.next()
		loader.next()
	2、控制流管理  
		Promise.resolve(step1)
			.then(step2)
			.then(step3)
			.then(function (value4){
				//
			}, function(error){
				//
			}).done();
			
		function* longRunningTask(value1){
			try{
				var value2 = yield step1(value1);
				var value3 = yield step2(value2);
				var value4 = yield step3(value3);
			}catch(e){
				//
			}
		}
	3、部署Iterator接口  
	4、作为数组结构
	
##	十九、Generator异步应用
1.  特性：暂停恢复执行，数据交换和错误处理
2.  Thunk函数：将多参数函数替换成一个只接受回调函数作为参数的单参数函数。thunkify  
	自动流程管理
3.  co模块 `处理Stream`:data事件、end事件、error事件
	
##	二十、async
1.  Generator函数的语法糖async替换*，await替换yield  
		async function chainAnimationsAsync(elem, animations){
			let ret = null;
			try {
				for(let anim of anumetions){
					ret = await anim(elem);
				}
			}catch(e){
				//
			}
			return ret;
		}
		
	并发读取URL：  
		async function logInOrder(urls){
			const textPromises = urls.map(async url =>{
				const response await fetch(url);
				return response.text();
			});
			for (const textPromise of textPromises){
				console.log(await textPromise);
			}
		}
2.  顶层await可以单独使用，多个await同步执行
	
##	廿一、Class语法
1.  Object.assign可以向类添加多个方法；类的内部定义的方法不可枚举！类必须通过new来调用，类不存在变量提升  
		class Point {
			constructor(x, y){
				this.x = x;
				this.y = y;
			}
			toString(){
				return '(' + this.x + ',' + this.y + ")";
			}
		}
	
2.  静态方法static，通过类名来调用，this指向类，不是类的实例！静态方法可以被继承
3.  new.target确定构造函数是怎么调用的，子类继承父类时new.target返回子类 可用于实现必须继承才能使用的类
	
##  廿二、extends继承
1.  继承原生的构造函数；继承类：Object.getPrototypeOf()判断一个类是否继承另一个类
2.  super作为函数调用，只能在子类的构造函数中使用；作为对象在普通方法中指向父类的原型对象/通过super调用父类方法时this指向当前子类实例，
	在静态方法中指向父类/通过super调用父类方法时指向当前子类
3.  类的两条继承链  
		class A {}  
		class B entends A{}  
		B.__proto__ === A; //true
		B.prototype.__proto__ === A.prototype; //true
		作为对象，子类B的原型（__proto__属性）是父类A；
		作为构造函数，子类B的原型对象（prototype属性）是父类原型对象（prototype属性）的实例
4.  多个对象合成...，对个类合成先mixin后继承
	
##  廿三、Module语法
1.  export模块输出（as为变量重命名）；import加载模块输入，会提升  
	export default function(){alert('foo')} 可以指定任意名字且不使用大括号  
	import {foo} from 'module1';  
	import "lodash";  
	`import()`提案
	
##  廿四、Module加载实现
1.  浏览器加载 type="module"等同于defer属性
2.  CommonJS模块输出的是一个值的拷贝、运行时加载，ES6模块输出的是值的引用、编译时输出接口；  
	CommonJS加载的是一个对象、脚本运行完才生成；ES6模块是一种静态定义
3.  Node CommonJS模块的this指向当前模块；ES6指向undefined
4.  `ES6与CommonJS相互加载`、`循环加载`

##  廿五、编码风格
1.  let 声明代替var 声明；全局常量和多线程用const声明
2.  静态字符串用单引号 const a = 'foobar'
3.  优先使用解构赋值：  
		使用数组成员对变量赋值，const [first, second] = arr;
		函数的参数如果是对象的成员，function getFullName({firstName, lastName}){}
		函数返回多个值，使用对象的解构赋值，  
			function processInput(input){
				return {left, right, top, bottom};
			} 
			const{left, right} = processInput(input);
		
4.  对象  
		const a = {x: null};
		a.x = 3;
		console.log(a);
		
		const obj = {
			id: 5,
			name: 'fangqi',
			[getKey('enabled')]: true,
		}
		
		const atom = {
			ref, value: 1,
			addValue(value){
				return atom.value + value;
			}
		}
		
5.  数组  
		使用拓展运算符拷贝数组
		const itemsCopy = [...items];
		const foo = Array.from(document.querySelectorAll('.foo'));
		
6.  函数  
		匿名函数做参数：[1, 3, 5].map(x => X * x);
		箭头函数取代bind：const boundMethod = (...params) => method.apply(this, params);
		所有配置项集中在一个对象，布尔值不能直接做对象：function divide(a, b, {option = false} = {}){}
		函数体内用...代替arguments：  
			function concatenateAll(...args){
				return args.join('');
			}
		使用默认值语法设置函数参数的默认值：function handleThings(opts = {}){}
		
7.  Map  
		只需key: value数据结构使用Map
8.  Class  
		class Queue {
			constructor(contents = []){
				this._queue = [...contents];
			}
			pop(){
				const value = this._queue[0];
				this._queue.splice(0, 1);
				return value;
			}
		}
		
		class PeekableQueue extends Queue{
			peek(){
				return this._queue[0];
			}
		}
		
9.  模块  
		使用import取代require：import {func1, func2} from 'moduleA'
		使用export取代module.exports：
		
10. ESLint

##	廿六、读懂规格
1.  术语  
	F.[[Call]](v, argumentsList)  
	F：函数对象，[[Call]]：内部方法，F.[[Call]]：运行该函数，v：[[Call]]：运行时的this值，argumentsList：调用时传入函数的参数
	
##	廿七、异步遍历器
1.  asyncIterator异步遍历器：返回的是一个Promise对象；next可以连续调用（放在Promise.all方法里/await最后一步操作）
2.  for await...of遍历异步接口  
		async function main(inputFilePath){
			const readStream = fs.createReadStream(inputFilePath, {encoding: 'utf8', highWaterMark: 1024});
			for await(const chunk of readStream){
				console.log('>>> ' + chunk);
			}
			console.log('### DONE ###');
		}
		
3.  异步Generator函数  
		//同步  
		function* map(iterable, func){
			const iter = iterable[Symbol.iterator]();
			while(true){
				const{value, done} = iter.next();
				if(done) break;
				yield func(value);
			}
		}
		//异步  
		async function* map(iterable, func){
			const iter = iterable[Symbol.asyncIterator]();
			while(true){
				const{value, done} = awiat iter.next();
				if(done) break;
				yield func(value);
			}
		}
		
		await用于将外部操作产生的值输入函数内部，yield命令用于将函数内部的值输出  
		function fetchRandom(){
			const url = 'https://www.random.org/decimal-fractions/' + '?num=1&col=1&format=plain&rnd=new';
			return fetch(url);
		}
		async function* asyncGenerator(){
			console.log('Start');
			const result = await fetchRandom();
			yield 'Result: ' + await result.text();
			console.log('Done');
		}
		const ag = asyncGenerator();
		ag.next().then(({value, done}) => {
			console.log(value);
		})
		
	四种函数：普通函数、async函数、Generator函数、异步Generator函数
		
##  廿八、ArrayBuffer
1.  ArrayBuffer原始的二进制数据
2.  TypedArray用来读写简单类型的二进制数据
3.  DataView用来读写复杂类型的二进制数据
4.  SharedArrayBuffer
5.  Atomics
	
##  廿九、最新提案
1.  do表达式：可以返回值
2.  throw表达式
3.  链判断运算符?.  obj?.prop对象属性  func?.(...args)函数或对象方法的调用  
	构造函数、模板字符串、左侧是super、链运算符用于赋值运算符左侧、右侧为十进制数值 写法禁止！
4.  Null判断运算符 ??类似||，只有运算符左侧的值为null或者undefined时，才返回右侧的值
5.  函数部分执行 ?单个参数的占位符，只能出现在函数调用中；...只会采集一次，多个...每个都相同
6.  管道运算符 |>  
	exclaim(capitalize(doubleSay('hello')))  
	'hello' |> doubleSay |> capitalize |> exclaim只能是单参函数；多参数函数必须进行柯里化 
7.  数值分割符 _
8.  BigInt数据类型 必须添加后缀n
9.  Math.signbit(判断有没有负号)；Math.sign(判断值的正负)
10. 双冒号运算符 ::自动将左边的对象作为上下文环境绑定到右边的函数上  
	左边为空、右边是对象的方法，等于将方法绑定到对象上  
	运算结果还是一个对象，可以采用链式写法
11. Realm API 类似'\<iframe>'防止被隔离的代码拿到全局，Realm顶层对象与原始顶层对象是两个对象
12. #！命令 Unix命令行执行脚本
13. import.meta 在脚本内部获取元信息
	
##  三十、Decorator
	待定...  	

参考：
[很炫酷的网页](https://diygod.me/2073/)
### 后记[TypeScript](http://127.0.0.1:8848/dcloudmdpaser?filename=D%3A%2F%E8%B5%84%E6%96%99%2FTypeScript.md&theme=github-markdown.css&projectname=&repath=D%3A%2F%E8%B5%84%E6%96%99%2F&charset=UTF-8)整理中...尽情期待！