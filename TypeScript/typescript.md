### typescript中的数据类型
typescript为了使代码编写更加规范，更有利于维护，增加了类型校验，在typescript中主要给我们提供了如下数据类型：

 - boolean 布尔型
 - number 数值型
 - string 字符串型
 - array 数组型
 - tuple 元组类型
 - enum 枚举类型
 - any 任意类型
 - null
 - undefined
 - void类型
 - never型：是其他类型（null 和 undefined）的子类型，代表从不会出现的值。这代表声明never的变量只能被never类型赋值
### 函数的写法
1. 三点运算符 接受新传过来的值
    ```
    // 写法一
    function sum(...result:number[]):number {
    
        for (let i = 0; i < result.length; i++) {
    
           sum+=result[i];
    
        }
    
        return sum;
    
    }
    
    sum(1,2,3,4,);  // 输出10
    
    // 写法二
    function sum(a:number, b:number, ...result:number[]):number {
    
       let sum = a +b;
    
        for (let i = 0; i < result.length; i++) {
    
           sum+=result[i];
    
        }
    
        return sum;
    
    }
    
    sum(1,2,3,4); // 输出10
    ```
### 重载
1.函数重载
- java中方法的重载：指两个或两个以上的同名函数，由于参数不一样，出出现函数重载的情况
- ts中的重载：通过为同一个函数提供多个不同类型的参数，来实现多种功能目的
- ES5中出现同名函数，则写在执行顺序后面的函数会替换之前的函数 

2.ts函数重载

- ES5中出现同名函数，则写在执行顺序后面的函数会替换之前的函数 ,但ts则不会
    ```
    function getResult(str:string):string;
    
    function getResult(str:number):number;
    
    function getResult(str:any):any {
    
        if (typeof str === 'string') {
    
            console.log('传参为string型');
    
        } else {
    
            console.log('传参为number型')
    
        }
    
    }
    
    getResult('这是string型');
    
    getResult(123);
    
    getResult(true); //错误写法，因为在方法重载中没有找到可以匹配boolean型的函数
    ```
说明：上面例子中function getResult(str:any):any {}并不是重载的一部分



### typescript类说明
1. typescript 定义类时，给我们提供了三种修饰符

    - publich ：公有。   在当前类、子类、外面都可以访问
    - protected： 保护。 在当前类、子类可以范围，外面不可以访问
    - private ： 私有。 在当前类可以范围，子类、外面不可以访问
    
    > 属性如果不加修饰符，那默认为：publich

2. static

    不需要实例化，可直接调用

3. 多态

    定义：父类定义了一个方案，但不去实现，让继承它的子类去实现，每一个子类有不同的表现

4. 抽象类

    - 抽象类时提供其他类继承的基类，不能直接被实例化
    - 用abstract关键字定义抽象类和抽象方法，抽象类中的抽象方法不包含具体实现，并且必须在继承类中实现
    - abstract抽象方法只能在抽象类中，否则报错
    - 抽象类和抽象方法用来定义标准（供继承类实现的标准）
### 接口
    待更新

### 命名空间
1. 命名空间

    - 在代码量较大的情况下，为了避免各种变量命名相冲突，可将相似功能的类、函数、接口等放置到命名空间内
    - 同java的包、.Net的命名空间一样，Typescropt的命名空间可以将代码包裹起来，只对外暴露需要在外部访问的对象，命名空间内的对象通过export向外暴露
2. 命名空间和模块的区别

    - 命名空间：内部模块，主要用于组织代码，避免命名冲突
    - 模块：侧重代码服用，一个模块里可能包含多个命名空间
    
### 装饰器
修饰器是js过去几年中js最大的成就之一，现已是ES7的标准特性

1. 装饰器

    装饰器是一种特殊类型的声明，他能被附加到类、方法、属性或参数上，可以修改类的行为。

    通俗的讲：装饰器就是一个方法，可以注入到类、方法、属性和参数上来扩展类、方法、属性、参数的功能

2. 常见装饰器

    - 类装饰器
        - 普通装饰器无法传参
        - 类装饰器，在类声明之前被声明（紧靠着类声明）

            ```
            function addClass(params: any) {
                console.log(params); // params 为当前类
                params.prototype.name = '我是重定义的name';
                params.prototype.getClass = () => {
                    return this.name;
                }
            }
            
            @addClass
            class Animal {
                public name: string = '我是固有的name';
                constractor() {
                    
                }
            }
            
            let animal = new Animal();
            console.log(animal.name); // 输出：我是重定义的name
            animal.getClass(); // 输出：我是重定义的name
            ```
            
    - 装饰器工厂
        - 带参数的装饰器
        
            ```
            // 例子1（重写属性）
            function addClass(params: any) {
                console.log(params); // params 为传入的参数
                
                return (target: any) => {
                    console.log(params); // params 为传入的参数
                    console.log(target); // target 列的原型
                    target.name = '我是重定义的name';
                }
            }
            
            @addClass('我是重定义的name')
            class Animal {
                public name: string = '我是固有的name';
                constractor() {
                    
                }
            }
            
            let animal = new Animal();
            console.log(animal.name); // 输出：我是重定义的name
            animal.getClass(); // 输出：我是重定义的name
            
            // 例子2（重载方法）
            function addClass(params: any) {
                console.log(params); // params 为传入的参数
                
                return class extends Animal {
                    console.log(params); // params 为传入的参数
                    console.log(target); // target 列的原型
                    eat() {
                        this.name = '狗';
                        console.log(this.name + '喜欢吃鱼')
                    }
                }
            }
            
            @addClass('我是重定义的name')
            class Animal {
                public name: string = '猫';
                constractor() {
                    
                }
                
                eat() {
                    console.log(this.name + '喜欢吃鱼')
                }
            }
            
            let animal = new Animal();
            console.log(animal.name); // 输出：狗
            animal.eat(); // 输出：狗喜欢吃鱼
            ```
        - 属性装饰器
        接收两个参数
            - 类的原型（静态成员来说是构造函数，对实例成员来说是类的原型）
            - 类的属性
            ```
            function addProperty(params: any) {
                return function (target:any, attr:any) {
                    console.log(target); // 输出：类的原型
                    console.log(attr); // 输出：类的属性，本例输出：'name'
                    target[attr] = params;
                }
            }
            
            class Animal {
                @addProperty('狗')
                public name: string = '猫';
                constractor() {
                    
                }
                
                eat() {
                    console.log(this.name + '喜欢吃鱼')
                }
            }
            
            let animal = new Animal();
            console.log(animal.name); // 输出：狗
            animal.eat(); // 输出：狗喜欢吃鱼
            ```
        - 方法装饰器
        他会被应用到方法的属性描述上，可以用来监听、修改、替换方法定义，接收三个参数：
            - 类的原型（静态成员来说是构造函数，对实例成员来说是类的原型）
            - 方法名称
            - 方法的属性的描述
            ```
            function addMethod(params: any) {
                return function (target:any, methodName:any, methodDesc) {
                    console.log(target); // 输出：类的原型
                    console.log(methodName); // 输出：方法名称，本例输出：'eat'
                    console.log(methodDesc); // 输出：方法的属性的描述，本例输出：eat方法的描述
                    target['name'] = params;
                    
                    // 修改装饰器的方法，如修改传入的参数类型都改为string型
                    let oldMethod = methodDesc.value; // 保存原class类中的方法
                    
                    // 修改当前方法
                    methodDesc.value = function(...arg:any) {
                        console.log(arg); // [111, 'abc']
                        arg.map((val) => {
                            return String(val);
                        })
                        console.log(arg); // ['111', 'abc']

                        // 使用老方法
                        oldMethod.apply(this, arg); // this:把当前方法传进去，arg把参数传进去。 表示在本方法中调用老方法
                    }
                    
                    
                    
                }
            }
            
            class Animal {
                public name: string = '猫';
                constractor() {
                    
                }
                
                @addMethod('狗')
                eat() {
                    console.log(this.name + '喜欢吃鱼')
                }
            }
            
            let animal:any = new Animal();
            animal.eat(); // 输出：[111, 'abc'] ['111', 'abc'] 狗喜欢吃鱼
            ```
        - 方法参数装饰器
        使用时可以为类的原型添加一些元素数据，接收三个参数：
            - 类的原型（静态成员来说是构造函数，对实例成员来说是类的原型）
            - 方法名称
            - 参数在方法参数中的索引
            ```
            function methodParam(params: any) {
                return function (target:any, methodName:any, paramIndex:number) {
                    console.log(target); // 输出：类的原型
                    console.log(methodName); // 输出：方法名称，本例输出：'eat'
                    console.log(paramIndex); // 输出：参数在方法参数中的索引，本例输出：0
                    target['name'] = params;
                    
                }
            }
            
            class Animal {
                public name: string = '猫';
                constractor() {
                    
                }
                
                eat(@methodParam('狗') name) {
                    console.log((name || this.name) + '喜欢吃鱼')
                }
            }
            
            let animal:any = new Animal();
            animal.eat(); // 输出：类的原型 eat 0 狗喜欢吃鱼
            ```
    - 装饰器执行顺序
    
        - 属性装饰器 > 方法装饰器 > 方法参数装饰器 > 类装饰器
        - 如果有多个同类型装饰器，则先执行后面的
        ```
        @addClass1
        @addClass2
        class Animal {
            @addProperty1()
            @addProperty2()
            public name: string | undefined
            
            constructor() {}
            
            @addMethod1()
            @addMethod2()
            eat(@methodParam1() name:string, @methodParams2() color:string) {
            
            }
        }
        
        /**
         * 上例执行顺序为：@addProperty2 > @addProperty1 > @addMethod2 > @addMethod1 > @methodParams2 > @methodParams1 > @addClass2 > @addClass1 
         * /
        ```
