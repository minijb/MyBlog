### 类修饰器

```typescript
function Logger(constructor: Function){
	console.log("Logger .....");
	console.log(constructor);
}

@Logger
class Person {
	name = "Max";

	constructor(){
		console.log("create a project");
	}
}
// decorate execute when we define the class not instantiate it
// const pers = new Person();

// console.log(pers);

```

### 通过修饰器工程

我们可以返回一个function，这样我们就可以指定参数输入，这样可以更加灵活

```typescript
function Logger(logString: string){
	return function(constructor: Function){
		console.log(logString);
		console.log(constructor);
	}
}

@Logger('Logging - Person')
class Person {
	name = "Max";

	constructor(){
		console.log("create a project");
	}
}
```

### 更多useful decorator

```typescript
function WithTemplate(template: string , hookId: string){
	return function(constructor: Function){
		const hookEl = document.getElementById(hookId);
		if (hookEl){
			hookEl.innerHTML = template;
		}
	}
}

@WithTemplate("<h1>My Person</h1>", "app")
class Person {

```

```typescript
function WithTemplate(template: string , hookId: string){
	return function(constructor: any){
		const hookEl = document.getElementById(hookId);
		const p = new constructor();
		if (hookEl){
			hookEl.innerHTML = template;
			hookEl.querySelector("h1")!.textContent = p.name;
		}
	}
}

@WithTemplate("<h1>My Person</h1>", "app")
class Person {
//此时，新建的h1标签内就是person类的name！！！！
```

### 对于类中属性的修饰器

```typescript
//target 为目标原型或者构造器函数 
function Log(target: any, propertyName: string|Symbol){
	console.log('Property decorate');
	console.log(target , propertyName);
}


class Product {
	@Log
	title: string;
	private _price: number;

	set price(val: number){
		this._price = val;		
	};
	constructor(t: string , p: number){
		this.title = t;
		this._price = p;
	}
}

```

### 其他类属性中添加装饰器

```typescript
function Log2(target: any , name: string , descriptor: PropertyDescriptor){
	console.log('Access decorate');
	console.log(target);
	console.log(name);
	console.log(descriptor);
}

class Product {
	@Log
	title: string;
	private _price: number;
	
	@Log2
	set price(val: number){
		this._price = val;		
	};
	constructor(t: string , p: number){
		this.title = t;
		this._price = p;
	}
}
//我们输出 原型， 名称， 以及属性描述！！！！
//针对method同理！！！
```

如果是方法中的参数也可以加装饰器！！！

```typescript
function Log3( target:any , name:string, postion: number ){
	console.log('parameter decorate');
	console.log(target);
	console.log(name);
	console.log(postion);
}

class Product {
	@Log
	title: string;
	private _price: number;
	
	@Log2
	set price(val: number){
		this._price = val;		
	};
	constructor( t: string , p: number){
		this.title = t;
		this._price = p;
	};

	output(@Log3 t:string ){
		console.log(t);
	}
}
//最后参数就是参数的位置
```

**注意这里的**：现在我们的修饰器仅仅在定义一个类的时候起作用！！！！

### 修饰器的返回值

不同修饰器返回值的作用是不同的

比如我们可以在类修饰器中返回一个新的constroction

```typescript
function WithTemplate(template: string , hookId: string){
	return function<T extends {new (...args: any[]):{name:string}}>(originalConstructor: T){
		return class extends originalConstructor{
			constructor(..._: any[]){
				super();
				const hookEl = document.getElementById(hookId);
				if (hookEl){
					hookEl.innerHTML = template;
					hookEl.querySelector("h1")!.textContent = this.name;
				};
				console.log(this.name);
			}
		};//返回原本的构造函数，这样就可以保留原本的值
	}
}

@WithTemplate("<h1>My Person</h1>", "app")
class Person {
	name = "Max";

```

这里我们需要好好讲讲

- new 关键字

可以看到此处我们使用泛型 我们限定了这个泛型来自于`{}`也就是一个对象，**new**关键字说明我们可以通过new来创建这个对象，同时new method可以跟上一些参数，这些参数来自构造函数！！！(这里表示他的构造函数可以接收任何多的任何类型的值，如果我们指定参数，那么在本次返回的对象的构造函数必须有对应类型的参数)最终它返回一个对象(此对象我们可以指定)。这种方式常用来表示一个工厂函数！！！！！

https://www.typescriptlang.org/docs/handbook/generics.html

#### 其他地方返回值

- 在参数，属性上的修饰器 可以有返回值，但是ts会无视
- 对于accesser和method是可以用返回值来代替原本的值的

此处以method为例

```typescript
class Printer {
	message = 'This works';

	showMessage(){
		console.log(this.message);
	}
}

const p = new Printer();

const button = document.querySelector("button");
button?.addEventListener('click', p.showMessage.bind(p));
```

现在我们需要使用p中的showMessage方法，但是因为this的缘故，log是undefined，我们需要使用bind绑定上下文，我们可以使用修饰器自动的添加bind方法

```typescript
function Autobind(_: any,_1: string, descriptor: PropertyDescriptor){
	const originMethod = descriptor.value;//原本的方法
	const adjDescriptor:PropertyDescriptor = {
		configurable: true,
		enumerable: false,
		get(){
			const boundFn = originMethod.bind(this);
			return boundFn;
		}
	};
	return adjDescriptor;
}

class Printer {
	message = 'This works';
	
	@Autobind
	showMessage() {
		console.log(this.message);
	}
}

const p = new Printer();

const button = document.querySelector("button");
button?.addEventListener('click', p.showMessage);

```

> 其他return的使用https://www.bilibili.com/video/BV1MF411T7rn?p=106&spm_id_from=pageDriver&vd_source=8beb74be6b19124f110600d2ce0f3957
