```typescript
interface Person {
	name : string;
	age : number;

	greet(phrase: string): void;
};


let user1: Person;
user1 = {
	name : "hello",
	age : 100,
	greet(phrase: string){
		console.log(phrase);
	}
}
```

实现了接口的对象需要包含对应的结构！！！

同理interface也可以用于对象

```typescript
class PP implements Person{
	constructor(public name:string ,public age:number){

	}
	greet(phrase: string): void {
		return;
	}
}

```

我们也可以直接进行定义，也可以使用简短定义。

我们像java一样如下

```typescript
let one = new PP("hello", 100);
one.greet("hello");
```

同样一个类可以实现多个接口

同样实现readonly关键字

**interface extends**

接口之间是可以继承的

```typescript
interface Named{
	readonly name: string;
}

interface Greetable extends Named{
	greet(phrese:string) :void;
}
//这里可以多继承
interface Greetable extends Named,another{
	greet(phrese:string) :void;
}
```

interface可以用来规范函数

```typescript
interface AddFn{
    (a:number) :number;
}
```

### 可选属性

```typescript
interface Named{
	readonly name: string;
    outputName?: string;
}
```

