#   TypeScript笔记
##  一、简介
	JavaScript的超集，提供类型系统和对ES6的支持。文件后缀为.ts

##	二、基础
+   原始数据类型  
	布尔值：
		let isDone: boolean = false;
		使用new Boolean（构造函数）创造的是Boolean对象！直接调用Boolean可以返回Boolean类型
			let createByBoolean: Boolean = new Boolean(1);
			let createByBoolean: boolean = Boolean(1);
	数值：
		let decLiteral: number = 6;
	字符串：
		let myName: string = "tom";
		let sentence = `Hello, my name is ${myName}`;
	空值：void表示没有任何返回值的函数
		function alertName() : void{
			alert ('My name is Tom');
		}
	Null和undefined：是所有类型的子类型
	
+   任意值Any
		let yna: any = "seven";
		yna = 7;
	等价于：
		let something;//let something: any
		something = "seven";
		something = 7;
	
+   类型推论
+   联合类型
		let yna: string | number;
		yna = 'seven';
		yna = 7;
	访问联合类型的属性和方法：
		function getString(something: string | number): string {
			return something.toString();
		}
	
+   接口：对 类的一部分行为进行抽象/对 对象形状进行描述
		interface Person{
			readonly id: number;//只读属性
			name: string;
			age?: number;//可选属性
			[propName: string]: any;//任意属性
		}
		let tom: Person = {
			id: 9527,
			name: 'Tom',
			age: 25
		}
	
+   数组类型
		let fibonacci: number[] = [1, 1, 2, 3, 5];//数组中不允许出现其他类型
		let fibonacci: Array<number> = [1, 1, 2, 3, 5];//数组泛型
		let list: any[] = ['xvat', 25, {baidu: 'http://baidu.com'}];
		function sum(){
			let args: {
				[index: number]: number;
				length: number;
				callee: Function;
			} = arguments;
		}//接口表示类数组
	等价于：
		function sum(){
			let args: IArguments = arguments;
		}
		
+   函数类型  
[函数式编程](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/)  
	声明式：
		function sum(x: number, y: number): number{
			return x + y;
		}
	表达式：
		let mySum = function(x: number, y: number): number{
			return x + y;
		}
	接口定义函数：
		interface SearchFunc{
			(source: string, firstName:string = 'Tom'//参数默认值位置无要求, lastName: string, subString?: string//可选参，位置必须在必选参后面！): boolean;
		}
		let mySearch: SearchFunc;
		mySearch = function(source: string, subString: string){
			return source.search(subString) !== -1;
		}
	剩余参数：
		function push(array: any[], ...items: any[]//必须是最后一个参数){
			items.forEach(function(item){
				array.push(item);
			});
		}
		let a = [];
		push(a, 1, 2, 3);
	重载：
		function reverse(x: number): number;
		function reverse(x: string): string;
		function reverese(x: number | string): numner | string{
			if(typeof x === 'number'){
				return Number(x.toString().split('').reverse().join(''));
			}else if(typeof x === 'string'){
				return x.split('').reverse().join('');
			}
		}
		
+   类型断言  
	<类型>值 / 值as类型，手动指定一个值的类型
		function getLength(something: string | number): number{
			if((<string>something).length){
				return (<string>something).length;
			}else{
				return something.toString().length;
			}
		}
		
+   声明文件
1.  全局变量：通过\<script>标签引入第三方库，注入全局变量  
	1、declare const 声明全局变量
		declare const jQuery: (selector: string) => any;
		jQuery("#foo");
	2、declare function 声明全局方法：@types统一管理第三方库的声明文件
		declare function jQuery(selector: string): any;
		jQuery('#foo');
		重载：declare function jQuery(domReadyCallback: () => any): any;
		jQuery(function(){
			alert("Dom Ready!");
		})
		
	3、declare calss 声明全局类
		declare class Animal{
			name: string;
			constructor(name: string);
			sayHi(): string;
		}
		let cat = new Animal('Tom');
		
	4、declare enum 声明全局枚举
		declare enum Directions{
			Up, Down, Left, Right
		}
		let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
		
	5、declare namespace 声明（含有子属性的）全局对象
		declare namespace jQuery{
			function ajax(url: string, settings?: any): void;
		}
		jQuery.ajax('/api/get_something');
	嵌套命名空间：
		declare namespace jQuery{
			function ajax(url: string, settings?: any): void;
			namespace fn{
				function extend(object: any): void;
			}
		}
		jQuery.ajax('/api/get_something');
		jQuery.fn.extend({
			check: function(){
				return this.each(function(){
					this.checked = true;
				})
			}
		})
		
	6、interface和type 声明全局类型
		interface AjaxSettings{
			method?: 'GET' | 'POST'
			data?: any;
		}
		declare namespace jQuery{
			function ajax(url: string, settings?: AjaxSettings): void;
		}
		
		let settings: AjaxSettings = {
			method: 'POST',
			data: {
				name: 'foo'
			}
		}
		jQuery.ajax('/api/post_something', settings);
		
		<!-- 防止命名冲突
		declare namespace jQuery{
			interface AjaxSettings{
				method?: 'GET' | 'POST'
				data?: any;
			}
			function ajax(url: string, settings?: AjaxSettings): void;
		} 
		let settings: jQuery.AjaxSettings = {
			method: 'POST',
			data: {
				name: "foo"
			}
		}
		jQuery.ajax('/api/post_something', settings);
		-->
		
	声明合并：
		declare function jQuery(selector: string): any;
		declare namespace jQuery{
			function ajax(url: string, settings?: any): void;
		}
		jQuery("#foo");
		jQuery.ajax('/api/get_something');
	......
2.  npm包：通过import foo from 'foo'导入，符合ES6模块规范
	7、export 导出变量
		export const name: string;
		export function getName(): string;
		export class Animal{
			constructor(name: string);
			sayHi(): string;
		}
		export enum Directions{
			Up, Down, Left, Right
		}
		export interface Options{
			data: any;
		}
		
		import {name, getName, Animal, Directions, Options} from 'foo';
		console.log(name);
		let myName = getName();
		let cat = new Animal('Tom');
		let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
		let options: Options = {
			data: {
				name: 'foo'
			}
		}
		
	8、export namespace 导出（含有子属性的）对象
		export namespace foo{
			const name: string;
			namespace bar {
				function baz(): string;
			}
		}
		import {foo} from 'foo';
		foo.bar.baz();
		
	9、export default ES6默认导出
		export default function foo(): string;
		import foo from 'foo';
		foo();
		只有function、class、interface可以直接导出
		
	10、export = commonjs导出模块
		commonjs：module.exports = foo;整体导出；exports.bar = bar;单个导出
		第一种：
		const foo = require('foo');
		const bar = require('foo').bar;
		第二种：
		import * as foo from 'foo';
		import {bar} from 'foo';
		第三种：
		import foo = require('foo');
		import bar = foo.bar;
		第四种：
		export = foo;
		declare function foo(): string;
		declare namespace foo{
			const bar: number;
		}
		
3.  UMD库：既可以通过\<script>标签引入，又可以通过import导入
	11、export as namespace UMD库声明全局变量
		export as namespace foo;
		export default foo;
		declare function foo(): string;
		declare namespace foo{
			const bar: number;
		}
	
4.  直接拓展全局变量：通过\<script>标签引入后，改变一个全局变量的结构
		interface String{
			prependS(): string;
		}
		"foo".prependS();
	还可以：
		declare namespace JQuery{
			interface CustomOptions{
				bar: string;
			}
		}
		interface JQueryStatic{
			foo(options: JQuery.CustomOptions): string;
		}
		jQuery.foo({
			bar: ''
		})
		
5.  在npm包或UMD库中拓展全局变量：引用npm包或UMD库后，改变一个全局变量的结构
	12、declare global 扩展全局变量
		declare global{
			interface String{
				prependS(): string;
			}
		}
		export {};
		'bar'.prependS();
	
6.  模块插件：通过\<script>或import导入后，改变另一个模块的结构
	13、declare module 扩展模块
		import * as moment from 'moment';
		declare module 'moment'{
			export function foo(): moment.CalendarKey;
		}
		import * as moment from 'moment';
		import 'moment-plugin';
		moment.foo();
	一次性声明多个模块：	
		declare module 'foo'{
			export interface Foo{
				foo: string;
			}
		}
		declate module 'bar'{
			export function bar(): string;
		}
		import {Foo} from 'foo';
		import * as bar from 'bar';
		let f: Foo;
		bar.bar();
	14、/// <reference />三斜杆指令
		import * as moment from 'moment';
		declare module 'moment'{
			export function foo(): moment.CalendarKey;
		}
	等同于：
		/// <reference types='jquery' />必须放在文件最顶端
		declare function foo(options: JQuery.AjaxSettings): string;
		foo({});
	依赖一个全局变量的声明文件：
		/// <reference type="node" />
		export function foo(p: NodeJS.Process): string;
		import {foo} from 'node-plugin';
		foo(global.process);
	拆分声明文件：
		/// <reference types="sizzle" />声明对另一个库的依赖
		/// <reference path="JQueryStatic.d.ts" />声明对另一个文件的依赖
		/// <reference path="JQuery.d.ts" />
		/// <reference path="misc.d.ts" />
		/// <reference path="legacy.d.ts" />
		export = jQuery;
>自动生成声明文件 -d "declaration": true
>declatationDir 生成.d.ts文件的目录
>declatationMap 对每个.d.ts文件生成对应的.d.ts.map文件
>emitDeclarationOnly 仅生成.d.ts文件，不生成.js文件
	
+   发布声明文件
	将声明文件和源码放在一起
	手动：
		{
			"name": "foo",
			"version": "1.0.0",
			"main": "lib/index.js",
			"types": "foo.d.ts",//index.d.ts//入口文件
		}
	
+   
[内置对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)  
		let b: Boolean = new Boolean(1);
		let e: Error = new Error('Error occurred');
		let d: Date = new Date();
		let r: RegExp = /[a-z]/;
		Document  
		Event  
		let body: HTMLElement = document.body;
		let allDiv: NodeList = document.querySelectorAll('div');
		document.addEventListener('click', function(e: MouseEvent){
		})
	
##	三、进阶
*   类型别名
		type Name = string;
		type NameResolver = () => string;
		type NameOrResolver = Name | NameResolver;
		
*   字符串字面量类型
		type EventNames = 'click' | "scroll" | 'mousemove';
		function handleEvent(ele: Element, event: EventNames){
		}
		handleEvent(document.getElementById('hello'), 'scroll');
		
*   元组
		let tom: [string, number] = ["Tom", 25];
		tom[1] = 26;
		tom = ['Cat', 18];
		
*   枚举
		enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>'S'};
		enum Color {Red, Green, Blue = 'blue'.length};
1.  常数枚举 const enum；
2.  外部枚举 declare enum；
		
*   类
		属性和方法，继承，存取器，静态方法static；实例属性（构造函数中this.xxx => name = 'jack';constructor(){}），静态属性
		修饰符public、private（修饰构造函数时该类不能被继承或者实例化）、protected（修饰构造函数时该类只允许被继承）
			class Animal {
				public constructor(public name){
					this.name = name;
				}
			}
		readonly：只读属性，只允许出现在属性声明或索引签名中，与修饰符同在时写在修饰符后
			class Animal{
				public constructor(public readonly name){
					this.name = name;
				}
			}
		抽象类：abstract；不允许实例化，抽象方法必须被子类实现
			abstract class Animal{
				public name;
				public constructor(name){
					this.name = name;
				}
				public abstract sayHi();
			}
			
			class Cat extends Animal{
				public sayHi(){
					console.log(`Meow, My name is ${this.name}`);
				}
			}
			let cat = new Cat('Tom');
		类的类型：与接口类似
			class Animal{
				name: string;
				constructor(name: string){
					this.name = name;
				}
				sayHi(): string{
					return `My name is ${this.name}`;
				}
			}
			let a: Animal = new Animal('Jack');
			console.log(a.sayHi());
			
*   类与接口
		interface Alarm{
			alert();//都实现了该接口
		}
		interface Light{
			lightOn();
			lightOff();
		}
		class Door{}
		class SecurityDoor extends Door implements Alarm{
			alert(){
				console.log('SecurityDoor alert');
			}
		}
		class Car implements Alarm{
			alert(){
				console.log("Car alert");
			}
			lightOn(){
				console.log("Car light on");
			}
			lightOff(){
				console.log("Car light off");
			}
		}//实现了多个接口
	混合类型：
		interface Counter{
			(start: number): string;
			interval: number;
			reset(): void;
		}
		function getCounter(): Counter{
			let counter = <Counter>function (start: number){};
			counter.interval = 123;
			counter.reset = function(){};
			return counter;
		}
		let c = getCounter();
		c(10);
		c.reset();
		c.interval = 5.0;
		
*   泛型
			function createArray<T>(length: number, value: T): Array<T>{
				let result: T[] = [];
				for (let i = 0; i < length; i++){
					result[i] = value;
				}
				return result;
			}
			createArray(3, 'd');
		多类型参数：	
			function swap<T, U>(tuple: [T, U]): [U, T]{
				return [tuple[1], tuple[0]];
			}
			swap([7, "seven"]);//["seven", 7]
		泛型约束：
			interface Lengthwise{
				length: number;
			}
			function loggingIdentity<T entends Lengthwise>(arg: T): T{
				console.log(arg.length);
				return arg;
			}
			
			function copyFields<T extends U, U>(target: T, source: U): T{
				for (let id in source){
					target[id] = (<T>source)[id];
				}
				return target;
			}
			let x = {a: 1, b: 2, c: 3, d: 4};
			copyFields(x, {b: 10, d: 20});
		泛型接口：
			interface CreateArrayFunc{
				<T>(length: number, value: T): Array<T>;
			}
			let createArray: CreateArrayFunc;
			createArray = function<T>(length: number, value: T): Array<T>{
				let result: T[] = [];
				for (let i = 0; i < length; i++){
					result[i] = value;
				}
				return result;
			}
			createArray(3, 'd');
		等同于：
			interface CreateArrayFunc<T>{
				(length: number, value: T): Array<T>;
			}
			let createArray: CreateArrayFunc<any>;
			createArray = function<T>(length: number, value: T): Array<T>{
				let result: T[] = [];
				for (let i = 0; i < length; i++){
					result[i] = value;
				}
				return result;
			}
			createArray(3, 'd');
		泛型类：
			class GenericNumber<T>{
				zeroValue: T;
				add: (x: T, y: T) => T;
			}
			let myGenericNumber = new GenericNumber<number>();
			myGenericNumber.zeroValue = 0;
			myGenericNumber.add = function(x, y){return x + y;};
		泛型参数的默认类型：
			function createArray<T = string>(length: number, value: T): Array<T>{
				let result: T[] = [];
				for (let i = 0; i < length; i++){
					result[i] = value;
				}
				return result;
			}
			
*   声明合并
		接口合并：
		interface Alarm{
			price: number;
			weight: number;
			alert(s: string): string;
			alert(s: string, n: number): string;
		}
		
*   扩展阅读
		Never: 永远不存在值的类型，一般用于错误处理函数

##	四、工程
-   代码检测
	TSLint/ESLint + typescript-eslint-parser
