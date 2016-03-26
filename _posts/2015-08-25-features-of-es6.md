---
layout: post
title:  "Возможности ES6"
categories: javascript
published: true
---

{% newthought 'ES6 -' %} новый(хотя в целом уже не) стандарт Javascript. Наконец нашел время пройтись по фичам, и выбрать то, что буду юзать уже сейчас.<!--more-->  Выбирал по ссылкам с подборкой этих фич: [один](http://es6-features.org/#RawStringAccess) и [два](https://github.com/lukehoban/es6features).

<!-- странная статья, немного не про то вообще -  Также полезно для прочтения: <http://habrahabr.ru/post/216997/> , -->
Итак:

1. **Let и const**. Let - для области видимости как С\С++(раньше в js все переменные переносились к началу функции). Const - просто неизменяемая константа.

    Достаточно простые вещи, которые только делают поведение js более ожидаемым.

    Например, в этой функции x будет виден только в блоке if(true), и не будет за его пределами

   ```javascript
function f() {
      if (true) {
          let x;
          {
              // okay, block scoped name
              const x = "sneaky";
              // error, const
              x = "foo";
          }
          // error, already declared in block
          let x = "inner";
      }
}
   ```

    также по поводу let из <https://leanpub.com/understandinges6/read/>:

    <blockquote class="smallquote" markdown="1">
    **Using let in loops.**
    The behavior of let inside of loops is slightly different than with other blocks. Instead of creating a variable that is used with each iteration of the loop, each iteration actually gets its own variable to use. This is to solve an old problem with JavaScript loops. Consider the following:


   ```javascript
var funcs = [];
for (var i=0; i < 10; i++) {
    funcs.push(function() { console.log(i); });
}
funcs.forEach(function(func) {
    func();     // outputs the number "10" ten times
});
   ```

     This code will output the number 10 ten times in a row. That’s because the variable i is shared across each iteration of the loop, meaning the closures created inside the loop all hold a reference to the same variable. The variable i has a value of 10 once the loop completes, and so that’s the value each function outputs.

     To fix this problem, developers use immediately-invoked function expressions (IIFEs) inside of loops to force a new copy of the variable to be created:


   ```javascript
var funcs = [];
for (var i=0; i < 10; i++) {
    funcs.push((function(value) {
        return function() {
            console.log(value);
        }
    }(i)));
}
funcs.forEach(function(func) {
    func();     // outputs 0, then 1, then 2, up to 9
});
   ```
    This version of the example uses an IIFE inside of the loop. The i variable is passed to the IIFE, which creates its own copy and stores it as value. This is the value used of the function for that iteration, so calling each function returns the expected value.

    A let declaration does this for you without the IIFE. Each iteration through the loop results in a new variable being created and initialized to the value of the variable with the same name from the previous iteration. That means you can simplify the process by using this code:


   ```javascript
var funcs = [];
for (let i=0; i < 10; i++) {
    funcs.push(function() { console.log(i); });
}
funcs.forEach(function(func) {
    func();     // outputs 0, then 1, then 2, up to 9
})
   ```
    This code works exactly like the code that used var and an IIFE but is, arguably, cleaner.

     </blockquote>

2. **List comprehensions** - это как генераторы в эрланге, удобные и мощные штуки(к слову, это часть стандарта es7 уже)

   ```javascript
let numbers = [ 1, 4, 9 ];
[for (num of numbers) Math.sqrt(num)];
// => [ 1, 2, 3 ]
   ```

3. **Arrows** - просто приятный сокращенный синтаксис для анонимных функций, напоминает такой же синтаксис в  C#.

    было:

   ```javascript
var f = function(x) { return x * 2 }
var f2 = function(x,y) { return x * y }
   ```
   стало:

   ```javascript
let f = x => x * 2 или (x => x * 2)
let f2 = ((x, y) => x * y)
   ```

4. **Template Strings** - строки с интерполяцией + многострочные строки. удивительно, как долго это добиралось до js.

   ```javascript
   var a = 1;
// Multiline strings
var a = `In JavaScript this is not legal.`
// String interpolation
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
   ```

    Однако здесь есть одновременно одна штуковина. Называется в стандарте Raw String Access. Не совсем я втыкаю, что это и как работает, пример:

   ```javascript
function quux (strings, ...values) {
  strings[0] === "foo\n"
  strings[1] === "bar"
  strings.raw[0] === "foo\\n"
  strings.raw[1] === "bar"
  values[0] === 42
}
quux `foo\n${ 42 }bar`
   ```

    Однако если делать quux(`foo\n${ 42 }bar`) получалось совсем иное. WTF? вобщем надо быть осторожным с доступом к строке по id после интерполяции, мало ли.

5. **Destructuring** - в эрланге и прочем мире паттерн матчинг.


   ```javascript
// list matching
var [a, , b] = [1,2,3];
// object matching
var { op: a, lhs: { op: b }, rhs: c } = getASTNode()
// object matching shorthand
// binds `op`, `lhs` and `rhs` in scope
var {op, lhs, rhs} = getASTNode()
// Can be used in parameter position
function g({name: x}) {
  console.log(x);
}
g({name: 5})
// Fail-soft destructuring
var [a] = [];
a === undefined;
// Fail-soft destructuring with defaults
var [a = 1] = [];
a === 1;
   ```

6. **Default + Rest + Spread** - для параметров в функциях - параметры по умолчанию, "лишние" параметры, и типо разворачивания списка в его элементы.


   ```javascript
function f(x, y=12) { // default
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) == 15;
   ```
   ```javascript
function f(x, ...y) { // rest
// y is an Array
  return x * y.length;
}
f(3, "hello", true) == 6
   ```
   ```javascript
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) == 6  // spread
   ```

    Причем spread можно использовать не только при вызове функций, можно например в массивах или строках -

   ```javascript
var params = [ "hello", true, 7 ]
var other = [ 1, 2, ...params ] // [ 1, 2, "hello", true, 7 ]
   ```
   ```javascript
var str = "foo"
var chars = [ … str ] // [ "f", "o", "o" ]
   ```

7. <span id='iterators'></span>**Iterators + For..Of** - итераторы типо CLR IEnumerable или Java Iterable. *for .. of* - используется для итерации навроде *for ... in*.
    Итераторы могут быть ленивыми ([доки](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Iteration_protocols)). Пример:

   ```javascript
let fibonacci = {
  [Symbol.iterator]() {
    let pre = 0, cur = 1;
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        return { done: false, value: cur }
      }
    }
  }
}
for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
   ```

    И:

   ```javascript
var myIterable = {}
myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};
[...myIterable] // [1, 2, 3]
   ```
   ```javascript
function NumberIterator(number){
 var i = 0;
 return {
    next: function(){
    return i < number
       ? {done: false, value: i++}
       : {done: true};
    }
 }
}
var iter = NumberIterator(3);
iter.next(); // => {done: false, value: 0}
iter.next(); // => {done: false, value: 1}
iter.next(); // => {done: false, value: 2}
iter.next(); // => {done: true}
   ```

8. **Generators** -  функция, выполнение которой можно приостановить.
    Вызов функции-генератора не вызывает немедленное исполнение её
    тела; вместо этого в ней возвращается объект итератор. При вызове метода
    итератора *next()*, тело функции-генератора возобновляет свою работу до первого
    встреченного оператора yield, который определяет возвращаемое значение или
    делегирует возврат другому генератору при помощи *yield\*  anotherGenerator()* ([доки](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/function*))
    Пример:

   ```javascript
function* idMaker(){
    var index = 0;
    while(true)
        yield index++;
}
var gen = idMaker();
console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
   ```
   ```javascript
let fibonacci = {
 *[Symbol.iterator]() {
    let pre = 0, cur = 1
    while(true) {
      [ pre, cur ] = [ cur, pre + cur ]
      yield cur
    }
  }
}
for (let n of fibonacci) {
 if (n > 1000)
    break
 console.log(n)
}
   ```
   ```javascript
function* range (start, end, step) {
   while (start < end) {
       yield start
       start += step
   }
}
for (let i of range(0, 10, 2)) {
    console.log(i) // 0, 2, 4, 6, 8
}
   ```

    На самом деле весьма интересная штука, правда сходу её применение не назову(над подумать).
    В статье на хабре написали, что это удобно для избавления от колбеков(also see on Q (<https://github.com/kriskowal/q>) ). Здесь функции fetch выполняются как синхронные, хотя на самом деле асинхронные. Похоже на async\await в C#

   ```javascript
Q.spawn(function*(){
  var user = yield fetchUser();
  var userPosts = yield fetchUserPosts(user.id);
  return userPosts;
});
   ```
9. **Promises** - да, буду использовать(хотя многие и без es6 используют давно), вместе с генераторами и какой-нибудь либой типо Q. Вызываем resolve, когда процесс выполнился успешно, или reject, когда с ошибкой. Пример:
﻿

   ```javascript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}
var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
   ```

Новые функции - в string - `includes`, `startWith`, `endWith`

ещё для взгляда: `Object.assign`, `Object.is`

Спросите, почему нет классов? Потому, что они показались слишком страшными) Здесь я перечислил те вещи, которые буду использовать прямо сейчас, и пока классы в проект впиливать рановато(или уже нет? =)) + сама концепция таких классов часто вызывала нарекания(в эрланге вообще без классов живут, хорошо живут), и мне больше по душе модули + функции без привязки к данным, где это возможно.

По крайней мере - сейчас.


Ещё из того, что не буду юзать:
**Enhanced Object Literals** - позволяет делать типо

```javascript
{ [ 'prop_' + (() => 42)() ]: 42 };
```
надеюсь, до такого у меня не дойдет, ибо страх.

**Modules** - не использую, так как на проекте browserify.

**Map + Set + WeakMap + WeakSet** - не вижу пользы, с учётом, что всё все равно будет эмулироваться.

**Proxy** - пока не нужны, хотя говорят, что штука очень классная. Над поближе ознакомиться будет.

**Symbols** - не нравится как создаются:

```javascript
var key = Symbol("key");
```

возможно буду использовать позже, а пока нет.
хотя в других языках использую с удовольствием. Хотя мб это нечто другое, в статье на хабре:

```javascript
console.log(person[Symbol('name')]); // => undefined,
```
каждый вызов Symbol возвращает уникальный ключ
Вроде классная вещь там, где нужны строго уникальные значения.
