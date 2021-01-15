# Обзор базовых возможностей ES6


#### Block scope variables

**ES6** мы перешли от var к let/const.

Проблема `var` в том, что переменная "протекает" в другие блоки кода, такие как циклы `for` или блоки условий `if`:

**ES5**

```js
var x = 'outer';
function test(inner) {
  if (inner) {
    var x = 'inner'; // scope whole function
    return x;
  }
  return x; // gets redefined on line 4
}

test(false); // undefined 
test(true); // inner
```
В строке `test(false)` можно ожидать возврат `outer`, но нет, мы получаем `undefined`. Почему?


Потому что даже не смотря на то, что блок `if` не выполняется, на 4й строке происходит переопределение `var x` как `undefined`.


**ES6**

```js
let x = 'outer';
function test(inner) {
  if (inner) {
    let x = 'inner';
    return x;
  }
  return x; // gets result from line 1 as expected
}

test(false); // outer
test(true); // inner
```

Изменив `var` на `let` мы откорректировали поведение. Если блок `if` не вызывается, то переменная `x` не переопределяется.

#### IIFE (immediately invoked function expression)

**ES5**

```js
{
  var private = 1;
}

console.log(private); // 1
```

Как видите, `private` протекает наружу. Нужно использовать `IIFE(immediately-invoked function expression)`:

**ES5**

```js
(function(){
  var private2 = 1;
})();

console.log(private2); // Uncaught ReferenceError
```

Если взглянуть на **jQuery/lodash** или любые другие проекты с открытым исходным кодом, то можно заметить, что там `IIFE` используется для содержания глобальной среды в чистоте. А глобальные штуки определяются со специальными символами вроде **_**, **$** или **jQuery**.

В **ES6** не нужно использовать `IIFE`, достаточно использовать блоки и `let`:

**ES6**

```js
{
  let private3 = 1;
}

console.log(private3); // Uncaught ReferenceError
```

Можно также использовать `const` если переменная не должна изменяться.

Итог:

- забудьте `var`, используйте `let` и `const`.
- Используйте `const` для всех референсов; не используйте `var`.
- Если референсы нужно переопределять, используйте `let` вместо `const`.

**Template Literals**

Не нужно больше делать вложенную конкатенацию, можно использовать шаблоны. Посмотрите:

**ES5**

```js
var first = 'Adrian';
var last = 'Mejia';
console.log('Your name is ' + first + ' ' + last + '.');
```

С помощью бэктика () и интерполяции строк `${}`можно сделать так:

**ES6**

```js
const first = 'Adrian';
const last = 'Mejia';
console.log(`Your name is ${first} ${last}.`);
```

#### Multi-line strings

Не нужно больше конкатенировать строки `с + \n`:

**ES5**

```js
var template = '<li *ngFor="let todo of todos" [ngClass]="{completed: todo.isDone}" >\n' +
'  <div class="view">\n' +
'    <input class="toggle" type="checkbox" [checked]="todo.isDone">\n' +
'    <label></label>\n' +
'    <button class="destroy"></button>\n' +
'  </div>\n' +
'  <input class="edit" value="">\n' +
'</li>';
console.log(template);
```

В **ES6** можно снова использовать бэктики:

**ES6**

```js
const template = `<li *ngFor="let todo of todos" [ngClass]="{completed: todo.isDone}" >
  <div class="view">
    <input class="toggle" type="checkbox" [checked]="todo.isDone">
    <label></label>
    <button class="destroy"></button>
  </div>
  <input class="edit" value="">
</li>`;
console.log(template);
```

#### Destructuring Assignment

`ES6 destructing` – полезная и лаконичная штука. Посмотрите на примеры:

Получение элемента из массива

**ES5**

```js
var array = [1, 2, 3, 4];

var first = array[0];
var third = array[2];

console.log(first, third); // 1 3
```

То же самое:

**ES6**

```js
const array = [1, 2, 3, 4];

const [first, ,third] = array;

console.log(first, third); // 1 3
```

#### Обмен значениями

**ES5**

```js
var a = 1;
var b = 2;

var tmp = a;
a = b;
b = tmp;

console.log(a, b); // 2 1
```
То же самое:

**ES6**

```js
let a = 1;
let b = 2;

[a, b] = [b, a];

console.log(a, b); // 2 1
```

#### Деструктуризация нескольких возвращаемых значений

**ES5**

```js
function margin() {
  var left=1, right=2, top=3, bottom=4;
  return { left: left, right: right, top: top, bottom: bottom };
}

var data = margin();
var left = data.left;
var bottom = data.bottom;

console.log(left, bottom); // 1 4
```

В строке 3 можно вернуть в виде массива:

```js
return [left, right, top, bottom];
```

но вызывающему коду придется знать о порядке данных.

```js
var left = data[0];
var bottom = data[3];
```

С **ES6** вызывающий выбирает только нужные данные (строка 6):

**ES6**

```js
function margin() {
  const left=1, right=2, top=3, bottom=4;
  return { left, right, top, bottom };
}

const { left, bottom } = margin();

console.log(left, bottom); // 1 4
```

*Заметка*: В строке 3 содержатся другие возможности **ES6**. Можно сократить `{ left: left }` до  `{ left }`. Смотрите, насколько это лаконичнее по сравнению с версией **ES5**.

#### Деструктуризация и сопоставление параметров

**ES5**

```js
var user = {firstName: 'Adrian', lastName: 'Mejia'};

function getFullName(user) {
  var firstName = user.firstName;
  var lastName = user.lastName;

  return firstName + ' ' + lastName;
}

console.log(getFullName(user)); // Adrian Mejia
```

То же самое (но короче):

**ES6**

```js
const user = {firstName: 'Adrian', lastName: 'Mejia'};

function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`;
}

console.log(getFullName(user)); // Adrian Mejia
```

#### Глубокое сопоставление

**ES5**

```js
function settings() {
  return { display: { color: 'red' }, keyboard: { layout: 'querty'} };
}

var tmp = settings();
var displayColor = tmp.display.color;
var keyboardLayout = tmp.keyboard.layout;

console.log(displayColor, keyboardLayout); // red querty
```

То же самое (но короче):

**ES6**

```js
function settings() {
  return { display: { color: 'red' }, keyboard: { layout: 'querty'} };
}

const { display: { color: displayColor }, keyboard: { layout: keyboardLayout }} = settings();

console.log(displayColor, keyboardLayout); // red querty
```

Это также называют деструктуризацией объекта `(object destructing)`.

Как видите, деструктуризация может быть очень полезной и может подталкивать к улучшению стиля кодирования.

Советы:

- Используйте деструктуризацию для получения элементов из массива и для обмена значениями. Не нужно делать временные референсы – сэкономите время.

- Не используйте деструктуризацию массива для нескольких возвращаемых значений, вместо этого используйте деструктуризацию объекта.

#### Классы и объекты

В **ECMAScript 6** мы перешли от `“функций-конструкторов”` к `“классам”` .

```js
Каждый объект в JavaScript имеет прототип, который является другим объектом. Все объекты в JavaScript наследуют методы и свойства от своего прототипа.
```
В `ES5` объектно-ориентированное программирование достигалось с помощью функций-конструкторов. Они создавали объекты следующим образом:

**ES5**

```js
var Animal = (function () {
  function MyConstructor(name) {
    this.name = name;
  }
  MyConstructor.prototype.speak = function speak() {
    console.log(this.name + ' makes a noise.');
  };
  return MyConstructor;
})();

var animal = new Animal('animal');
animal.speak(); // animal makes a noise.
```

В **ES6** есть новый синтаксический сахар. Можно сделать то же самое с меньшим кодом и с использованием ключевых слов `class` и `construсtor`. Также заметьте, как четко определяются методы: `construсtor.prototype.speak = function () vs speak()`:

**ES6**

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

const animal = new Animal('animal');
animal.speak(); // animal makes a noise.
```

Оба стиля (ES5/6) дают одинаковый результат.

Советы:

- Всегда используйте синтаксис class и не изменяйте prototype напрямую. Код будет лаконичнее и его будет легче понять.

- Избегайте создания пустого конструктора. У классов есть конструктор по умолчанию если не задать собственный.

#### Наследование

Давайте продолжим предыдущий пример с классом `Animal`. Допустим, нам нужен новый класс `Lion`.

В **ES5** придется немного поработать с прототипным наследованием.

**ES5**

```js
var Lion = (function () {
  function MyConstructor(name){
    Animal.call(this, name);
  }

  // prototypal inheritance
  MyConstructor.prototype = Object.create(Animal.prototype);
  MyConstructor.prototype.constructor = Animal;

  MyConstructor.prototype.speak = function speak() {
    Animal.prototype.speak.call(this);
    console.log(this.name + ' roars ');
  };
  return MyConstructor;
})();

var lion = new Lion('Simba');
lion.speak(); // Simba makes a noise.
// Simba roars.
```

Не будем вдаваться в детали, но заметьте несколько деталей:

- Строка 3, напрямую вызываем конструкторAnimal с параметрами.
- Строки 7-8, назначаем прототип `Lion` прототипом класса `Animal`.
- Строка 11, вызываем метод `speak` из родительского класса `Animal`.

В **ES6** есть новые ключевые слова `extends` и `super`.

**ES6**

```js
class Lion extends Animal {
  speak() {
    super.speak();
    console.log(this.name + ' roars ');
  }
}

const lion = new Lion('Simba');
lion.speak(); // Simba makes a noise.
// Simba roars.
```

Посмотрите, насколько лучше выглядит код на **ES6** по сравнению с **ES5**. И они делают одно и то же! Win!

Совет:


Используйте встроенный способ наследования – `extends`.

#### Нативные промисы

Переходим от `callback hell` к промисам `(promises)`

**ES5**

```js
function printAfterTimeout(string, timeout, done){
  setTimeout(function(){
    done(string);
  }, timeout);
}

printAfterTimeout('Hello ', 2e3, function(result){
  console.log(result);

  // nested callback
  printAfterTimeout(result + 'Reader', 2e3, function(result){
    console.log(result);
  });
});
```

Одна функция принимает `callback` чтобы запустить его после завершения. Нам нужно запустить ее дважды, одну за другой. Поэтому приходится вызывать `printAfterTimeout` во второй раз в коллбеке.

Все становится совсем плохо когда нужно добавить третий или четвертый коллбек. Давайте посмотрим, что можно сделать с промисами:

**ES6**

```js
function printAfterTimeout(string, timeout){
  return new Promise((resolve, reject) => {
    setTimeout(function(){
      resolve(string);
    }, timeout);
  });
}

printAfterTimeout('Hello ', 2e3).then((result) => {
  console.log(result);
  return printAfterTimeout(result + 'Reader', 2e3);

}).then((result) => {
  console.log(result);
});
```

С помощью `then` можно обойтись без вложенных функций.

#### Стрелочные функции

В **ES5** обычные определения функций не исчезли, но был добавлен новый формат – стрелочные функции.

В **ES5** есть проблемы с `this`:

**ES5**

```js
var _this = this; // need to hold a reference

$('.btn').click(function(event){
  _this.sendData(); // reference outer this
});

$('.input').on('change',function(event){
  this.sendData(); // reference outer this
}.bind(this)); // bind to outer this
```

Нужно использовать временный `this` чтобы ссылаться на него внутри функции или использовать `bind`. В **ES6** можно просто использовать стрелочную функцию!

**ES6**

```js
// this will reference the outer one
$('.btn').click((event) =>  this.sendData());

// implicit returns
const ids = [291, 288, 984];
const messages = ids.map(value => `ID is ${value}`);
```

#### For…of

От `for` переходим к `forEach` а потом к `for...of`:

**ES5**
```js
// for
var array = ['a', 'b', 'c', 'd'];
for (var i = 0; i < array.length; i++) {
  var element = array[i];
  console.log(element);
}

// forEach
array.forEach(function (element) {
  console.log(element);
});
```

**ES6** `for…of` позволяет использовать итераторы

**ES6**

```js
// for ...of
const array = ['a', 'b', 'c', 'd'];
for (const element of array) {
    console.log(element);
}
```

#### Параметры по умолчанию

От проверки параметров переходим к параметрам по умолчанию. Вы делали что-нибудь такое раньше?

**ES5**

```js
function point(x, y, isFlag){
  x = x || 0;
  y = y || -1;
  isFlag = isFlag || true;
  console.log(x,y, isFlag);
}

point(0, 0) // 0 -1 true 
point(0, 0, false) // 0 -1 true 
point(1) // 1 -1 true
point() // 0 -1 true
```

Скорее всего да. Это распространенный паттерн проверки наличия значения переменной. Но тут есть некоторые проблемы:

- Строка 8, передаем `0`, `0` получаем `0`, `-1`
- Строка 9, передаем `false`, но получаем `true`.

Если параметр по умолчанию это булева переменная или если задать значение `0`, то ничего не получится. Почему? Расскажу после этого примера с **ES6** ;)


В **ES6** все получается лучше с меньшим количеством кода:

**ES6**

```js
function point(x = 0, y = -1, isFlag = true){
  console.log(x,y, isFlag);
}

point(0, 0) // 0 0 true
point(0, 0, false) // 0 0 false
point(1) // 1 -1 true
point() // 0 -1 true
```
Получаем ожидаемый результат. Пример в **ES5** не работал. Нужно проверять на `undefined` так как `false`, `null`, `undefined` и `0 `– это все `falsy`-значения. С числами можно так:

**ES5**

```js
function point(x, y, isFlag){
  x = x || 0;
  y = typeof(y) === 'undefined' ? -1 : y;
  isFlag = typeof(isFlag) === 'undefined' ? true : isFlag;
  console.log(x,y, isFlag);
}

point(0, 0) // 0 0 true
point(0, 0, false) // 0 0 false
point(1) // 1 -1 true
point() // 0 -1 true
```

С проверкой на `undefined` все работает как нужно.

**Rest-параметры**

От аргументов к `rest`-параметрам и операции `spread`.

В **ES5** работать с переменным количеством аргументов неудобно.

**ES5**

```js
function printf(format) {
  var params = [].slice.call(arguments, 1);
  console.log('params: ', params);
  console.log('format: ', format);
}

printf('%s %d %.2f', 'adrian', 321, Math.PI);
```

С `rest  ...` все намного проще.

**ES6**

```js
function printf(format, ...params) {
  console.log('params: ', params);
  console.log('format: ', format);
}

printf('%s %d %.2f', 'adrian', 321, Math.PI);
```

#### Операция Spread

Переходим от `apply()` к `spread`. Опять же, ... спешит на помощь:

```js
Помните: мы используем `apply()` чтобы превратить массив в список аргументов. например, `Math.max()` принимает список параметров, но если у нас есть массив, то можно использовать `apply`.
```

**ES5**

```js
Math.max.apply(Math, [2,100,1,6,43]) // 100
```

В **ES6** используем `spread`:

**ES6**

```js
Math.max(...[2,100,1,6,43]) // 100
```

Мы также перешли от `concat` к `spread'у`:

**ES5**

```js
var array1 = [2,100,1,6,43];
var array2 = ['a', 'b', 'c', 'd'];
var array3 = [false, true, null, undefined];

console.log(array1.concat(array2, array3));
```

В **ES6**:

**ES6**

```js
const array1 = [2,100,1,6,43];
const array2 = ['a', 'b', 'c', 'd'];
const array3 = [false, true, null, undefined];

console.log([...array1, ...array2, ...array3]);
```

