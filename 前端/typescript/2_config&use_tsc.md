### Watch mode

如果文件更改，自动执行命令，`tsc`命令

```shell
tsc xxx -w
```

### 使用json管理项目文件

- 使用init来创建配置文件

```sh
tsc --init
```

此时我们使用`tsc`命令的时候，我们会直接编译整个文件夹内的内容！！！！

- 此时使用watch mode会监视整个工程文件的内容

### 多文件编程

再ts中我们可以通过配置`tsconfig.json`文件对ts的编译进行控制(此文件是是通过`tsc --init`命令自动生成的)

此处我们会介绍一些常用的配置，用来控制tsc的编译流程

```json
"exclude": [files that you dont want to complie]
// example ， 我们也可以使用文件通配符*
"exclude" : ["a1.ts","a1*.ts"]
```

此时我们使用`tsc`我们就不会去编译`a1.ts`

> `node_modules`是默认选项
>
> 如果我们没使用exclude，那这个就是默认选项，
>
> 如果我们使用了exclude，那么我们需要添加此选项加速编译！！！

`include`键同理，如果我们设置了inlclude 键，那么我们只会编译include的键！！！

**`files`**

用来指定需要编译的文件列表（注意，只能是文件，不能是文件夹）

```json
 "files": [
    "core.ts",
    "sys.ts",
    "types.ts",
    "scanner.ts",
    "parser.ts",
    "utilities.ts",
    "binder.ts",
    "checker.ts",
    "tsc.ts"
  ]
```

### compilerOptions

#### target--选择编译成js的版本

#### lib

ts所用到的库,默认会加载对应target的库，但是如果我们不使用默认值，那么我们需要导入默认的库比如`"dom","es6","dom.iterable","scripthost"`

#### sourceMap

帮助我们debug and developing。如果设置为ture，再编译的过程中，我们会编一个`.js.map`文件此时我们再浏览器的source标签下，我们就可以看到ts文件，并可以通过ts文件进行debug

#### outDir rootDir

我们一般不会再跟文件中写代码，一般我们会在`src`文件中写代码，打包文件一般放在`dist`文件夹内

此时我们修改`outDir`，我们就不会再ts同级目录下生成文件，而是再指定的目录下生成文件，同时dist文件下的目录回合src下的目录结构一致。

如果我们除src文件外还有其他文件下有需要编译的ts文件，那么rootDir最好不要设置，此时dist文件目录结构就是根文件的目录结构

#### removeXComments

#### noEmitOnErroe

默认为ture，false的时候，就算tsc编译出错也会输出js文件

- noImplicitAny：将不规范的any类型视为error
- strictNullChecks

当然我们可以通过使用if包围来解决这个错误









