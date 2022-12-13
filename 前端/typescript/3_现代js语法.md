### 在箭头函数中使用type返回类型

```typescript
let a = "hello";


const printOutput: (a:number | string ) => void = output => {
    console.log(output);
}

printOutput(a);

```

### 函数的默认值

```typescript
function aa(a:number , b:number = 100) :number{
    return a + b;
}
```

### 扩展符号...

将可迭代类型结构并作为函数的参数

```typescript
const hobbies = ['ss','aa'];
const activeHobbies = ['Hiking'];
activeHobbies.push(...hobbies);
```

对象也可以使用...

```typescript
const person = {
    name : 'Max',
    age : 30
}

const anotherPerson = {...person};
```

**...同样可以出现在函数的参数中，表示接收一串参数**

```typescript
const aaa = (...numbers : number[]) => {
    return numbers.reduce((curRsdult , curValue) =>{
        return curRsdult+curValue;
    }, 0);
};

console.log(aaa(1,2,3,4,5));
```

### 解构

```typescript
const [hobby , hobby2] = hobbies;
const {firstname , age} = person;
//我们可以指定我们需要的命名
const {firstname: userName , age} = person;
```

