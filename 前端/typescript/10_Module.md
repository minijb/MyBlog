### 1. typescript的namespace

我们可以使用namespace，在同一个namespace中可以使用上下文，如果分割到多个文件，我们可以使用export来延展，使用`/// <reference path="Validation.ts" />`来扩展namespace 的空间

```typescript
//drag.ts
namespace DDInterfaces {
    export interface Draggable {
        dragStarHandler( event: DragEvent):void;
        dragEndHandler( event: DragEvent):void;
    };
    // class function
    export class Namexx{
        name: string;
        constructor(){
            this.name = "hello";
        }
    }
}

//app.ts
/// <reference path="drag.ts"/>
namespace DDInterfaces{
    class nn implements Draggable{
        dragStarHandler(event: DragEvent): void {
            throw new Error("Method not implemented.");
        }
        dragEndHandler(event: DragEvent): void {
            throw new Error("Method not implemented.");
        }

    }
}
```

但是这种方法只适用于一些ts类型，我们可以设置`outFile`让编译后的代码集中到一个文件中，那么此时就可以new对象，

使用这种方法，我们的文件可以放到不同的文件下只要我们指定`/// <reference path="drag.ts"/>`并使用export输出我们需要共用的代码就可以了

## use ES import

和js中差不多

