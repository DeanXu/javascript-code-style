Airbnb 的javascript规范指南
=====================
*常用的一些javascript规范*

## <a name='TOC'>目录</a>

  1. [数据类型](#types)
  1. [对象](#objects)
  1. [数组](#arrays)
  1. [string类型](#strings)
  1. [函数](#functions)
  1. [属性](#properties)
  1. [变量](#variables)
  1. [Hoisting](#hoisting)
  1. [Conditional Expressions & Equality](#conditionals)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [ES5 Compatibility](#es5)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [The JavaScript Style Guide Guide](#guide-guide)
  1. [Contributors](#contributors)
  1. [License](#license)


## <a name='types'>数据类型</a>

- **原始类型(Primitives)**：当你给一个原始类型赋值时，返回的是这个值的本身。

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

- **对象类型**:当你给一个对象类型赋值时，返回的是这个值的引用。

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

## <a name='objects'>对象</a>

- 新建一个对象的语法

    ```javascript
    //不推荐
    var item = new Object();

    //推荐
    var item = {};
    ```

- 不要使用[保留字](http://es5.github.io/#x7.6.1)作为键值，否则在IE8下面会出现问题([详情](https://github.com/airbnb/javascript/issues/61))。

    ```javascript
    //不推荐
    var superman = {
      default: { clark: 'kent'},
      private: true
    };

    //推荐
    var superman ={
      defaults: { clark: 'kent'},
      hidden: true
    };
    ```

- 使用可读性强的同义词代替保留字

    ```javascript
    //不推荐
    var superman = {
      class: 'alien'
    };

    //不推荐
    var superman = {
      klass: 'alien'
    };

    //推荐
    var superman = {
      type: 'alien'
    };
    ```

## <a name='arrays'>数组</a>

- 新建一个数组的语法

    ```javascript
    //不推荐
    var items = new Array();

    //推荐
    var items = [];
    ```

- 如果你不知道数组的长度可以使用push将元素加入。

    ```javascript
    var someStack = [];

    //不推荐
    someStack[someStack.length] = 'something';

    //推荐
    someStack.push('something');
    ```

- 当你需要复制一个数组的时候使用slice。[jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length,
      itemsCopy = [],
      i;

    //不推荐
    for (i = 0; i < len; i++){
      itemsCopy[i] = items[i];
    }

    //推荐
    itemsCopy = items.slice();
    ```

- 用slice转换伪数组对象到数组

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

## <a name='strings'>String类型</a>

- 使用单引号`''`

    ```javascript
    //不推荐
    var name = "Bob Parr";

    //推荐
    var name = 'Bob Parr';

    //不推荐
    var fullName - "Bob " + this.lastName;

    //推荐
    var fullName = 'Bob ' + this.lastName;
    ```

- 当字符串长度超过80个时，应该通过字符串连接多行显示。

- 注意：过度使用字符串连接将会印象性能。[jsPerf](http://jsperf.com/ya-string-concat)&[Discussion](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    //不推荐
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    //不推荐
    var errorMessage = 'This is a super long error that \
    was thrown because of Batman. \
    When you stop to think about \
    how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    //推荐
    var errorMessage = 'This is a super long error that ' +
      'was thrown because of Batman.' +
      'When you stop to think about ' +
      'how Batman had anything to do ' +
      'with this, you would get nowhere ' +
      'fast.';
    ```

- 当程序建立一个字符串时， 使用join代替字符串连接。特别是在IE下：[jsPerf](http://jsperf.com/string-vs-array-concat/2)

    ```javascript
    var items,
        messages,
        length, i;

    messages = [{
        state: 'success',
        message: 'This one worked.'
    },{
        state: 'success',
        message: 'This one worked as well.'
    },{
        state: 'error',
        message: 'This one did not work.'
    }];

    length = messages.length;

    // 不推荐
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // 推荐
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

## <a name='functions'>函数</a>

- 函数表达式：

    ```javascript
    // 匿名函数表达式
    var anonymous = function(){
      return true;
    }

    // 命名函数表达式
    var named = function named() {
      return true;
    };

    // 立即执行的函数表达式（IIFE）
    (function(){
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

- 不要将函数声明放在如if/while循环或其他任何语句中。但可以用函数表达式来替代函数声明这么做。一些浏览器可能的确可以在语句中使用函数声明。但是在解析方面的处理各不相同，各种浏览器下兼容性很不好。

- **注意:** ECMA-262定义了一系列的语句，但是函数声明并没有被归类为真正的语句。[关于这点可查看ECMA-262的文档](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)

    ```javascript
    //不推荐
    if (currentUser){
      function test() {
        console.log('Nope.');
      }
    }

    //推荐
    if (currentUser){
      var test = function test() {
        console.log('Yup.');
      }
    }
    ```

- `arguments` 不能作为一个参数的名字， 因为这会覆盖每一个函数内的`arguments`对象。

    ```javascript
    //不推荐
    function nope(name, options, arguments) {
      // ...stuff...
    }

    //推荐
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

## <a name='properties'>属性</a>
- 访问一个属性时，使用点的形式取值。
    ```javascript
    var luke = {
          jedi: true,
            age: 28
    };

    // 不推荐
    var isJedi = luke['jedi'];

    // 推荐
    var isJedi = luke.jedi;
    ```

- 需要一个变量访问一个属性时，试用“[]”来取值。

    ```javascript
    var luke = {
          jedi: true,
            age: 28
    };

    function getProp(prop) {
          return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

## <a name='variables'>变量</a>
- 总是使用 `var` 来定义变量。如果不这么做将定义一个全局变量出来。我们希望避免全局命名空间的污染。
    ```javascript
    // 不推荐
    superPower = new SuperPower();

    // 推荐
    var superPower = new SuperPower();
    ```

- 使用一个`var` 声明多个变量，并且每声明一个变量就换一行。

    ```javascript
    // 不推荐
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';

    // 推荐
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    ```

- 声明多个变量时，把不赋值的变量放在后面。  (This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.)

    ```javascript
    // 不推荐
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // 不推荐
    var i, items = getItems(),
        dragonball,
        goSportsTeam = true,
        len;

    // 推荐
    var items = getItems(),
        goSportsTeam = true,
        dragonball,
        length,
        i;

    ```

- 在一个作用域的顶部给一个变量赋值。


