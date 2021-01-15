# JavaScript

**Ссылки на материал**

**`https://www.cat-in-web.ru/45-js-questions/`**

**`https://www.cat-in-web.ru/rabota-s-massivami-v-javascript/`**

#### Чем отличается **fetch** от **axios**

**Fetch** — нативный низкоуровневый **JavaScript** интерфейс для выполнения HTTP-запросов с использованием обещаний через глобальный метод `fetch()`

**Axios** — **JavaScript** - библиотека, основанная на обещаниях, для выполнения HTTP-запросов.

В современных браузерах `fetch()` работает из коробки, для старых браузеров написаны полифилы, а библиотека это всегда дополнительные килобайты нагрузки.

**GET запросы JSON данных**

Получение JSON данных с сервера.

**Axios** - 1 действие

```js
axios.get('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => console.log(response));
```

**Fetch** - 2 действия: запрос + вызов метода `.json()` над результатом запроса.

```js
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => response.json())
  .then(json => console.log(json))
```

**Обработка ошибок**

**Axios** обрабатывает ошибки логично. Если сервер вернул ответ с HTTP статусом ошибки (например 404 или 500), то обещание будет отвергнуто.

```js
axios.get(url)
  .then(result => console.log('success:', result))
  .catch(error => console.log('error:', error));
```

**Fetch** отвергнет обещание только если произошла сетевая ошибка (DNS не разрешил адрес, сервер недоступен, CORS не разрешен). В отстальных случаях ошибки не будет (включая HTTP статусы 404 или 500), поэтому для получения более интуитивного поведения такие ситуации придётся обрабатывать вручную.

```js
fetch(url)
  .then(response => {
    return response.json().then(data => {
      if (response.ok) {
        return data;
      } else {
        return Promise.reject({status: response.status, data});
      }
    });
  })
  .then(result => console.log('success:', result))
  .catch(error => console.log('error:', error));
  ```

**POST запросы**

С **axios** всё просто, а с **fetch** уже не так: JSON обязан быть преобразован в строку, а заголовок **Content-Type** должен указывать, что отправляются JSON данные, иначе сервер будет рассматривать их как строку.

**Axios**

```js
axios.post('/user', {
  firstName: 'Fred',
  lastName: 'Flintstone'
});
```

**Fetch**

```js
fetch('/user', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
});
```

**Базовые значения для запросов**

Как вы могли заметить, **fetch** это явный API, вы ничего не получаете, если об этом не просите. Если используется аутентификация, основанная на сохранении сессии пользователя, то надо явно указывать куку. Если сервер расположен на поддомене, то надо явно прописывать CORS. Эти опции надо прописывать для всех вызовов сервера и у **fetch** нет механизма для установки значений по-умолчанию, а у **axios** есть.

```js
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Accept'] = 'application/json';
axios.defaults.headers.post['Content-Type'] = 'application/json';
```

**Заключение**

Эквивалентный код

**Axios**

```js
function addUser(details) {
  return axios.post('https://api.example.com/user', details);
}
```

**Fetch**

```js
function addUser(details) {
  return fetch('https://api.example.com/user', {
    mode: 'cors',
    method: 'POST',
    credentials: 'include',
    body: JSON.stringify(details),
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
      'X-XSRF-TOKEN': getCookieValue('XSRF-TOKEN')
    }
  }).then(response => {
    return response.json().then(data => {
      if (response.ok) {
        return data;
      } else {
        return Promise.reject({status: response.status, data});
      }
    });
  });
}
```

#### Чем отличается Vue cli от webpack

**webpack** - универсальная система сборки приложения

**vue-cli** - набор тулз для работы конкретно c vue, внутри использующий в том числе webpack со множеством преднастроек и плагинов

#### Приведение типов

- `undefined` привести к числу будет `null`

- `null` привести к числу будет `0`

- `none` сравнить с `0` будет `false`

#### Лексическая область видимости

Лексическая область видимости это статическая область в **JavaScript**, имеющая прямое отношение к доступу к переменным, функциям и объектам, основываясь на их расположении в коде. Вот пример:

```js
let a = 'global';
function outer() {
    let b = 'outer';
function inner() {
      let c = 'inner'
      console.log(c);   // выдаст 'inner'
      console.log(b);   // выдаст 'outer'
      console.log(a);   // выдаст 'global'
    }
    console.log(a);     // выдаст 'global'
    console.log(b);     // выдаст 'outer'
    inner();
  }
outer();
console.log(a);         // выдаст 'global'
```

Тут функция `inner` имеет доступ к переменным в своей области видимости, в области видимости функции `outer` и глобальной области видимости. Функция `outer` имеет доступ к переменным, объявленным в собственной области видимости и глобальной области видимости.

В общем, цепочка области видимости выше будет такой:

```js
Global {
  outer {
    inner
  }
}
```

Обратите внимание, что функция `inner` окружена лексической областью видимости функции `outer`, которая, в свою очередь, окружена глобальной областью видимости. Поэтому функция `inner` имеет доступ к переменным, определенным в функции `outer` и глобальной области видимости.

В `Node.js` `global` является глобальной областью видимости

#### Замыкание

**Замыкание** — это функция, созданная и возвращенная из другой функции, которая имеет доступ к родительскому скоупу.

**Замыкание** — это функция у которой есть доступ к своей внешней функции по области видимости, даже после того, как внешняя функция прекратилась. Это говорит о том, что замыкание может запоминать и получать доступ к переменным, и аргументам своей внешней функции, даже после того, как та прекратит выполнение.

> Замыкание — это когда функция умеет запоминать и имеет доступ к лексической области видимости даже тогда, когда эта функция выполняется вне своей лексической области видимости.

Давайте рассмотрим небольшой код, чтобы проиллюстрировать это определение.

```js
function foo() {
	var a = 2;

	function bar() {
		console.log( a ); // 2
	}

	bar();
}

foo();
```

**№1**

```js
function person() {
  let name = 'Peter';
  
  return function displayName() {
    console.log(name);
  };
}
let peter = person();
peter(); // выведет 'Peter'
```

Когда выполняется функция `person`, **JavaScript** создаёт новый контекст выполнения и лексическое окружение для функции. После того, как эта функция завершится, она вернёт `displayName` функцию и назначится на переменную `peter`.

Таким образом, её лексическое окружение будет выглядеть так:

```js
personLexicalEnvironment = {
  environmentRecord: {
    name : 'Peter',
    displayName: < displayName function reference>
  }
  outer: <globalLexicalEnvironment>
}
```

Когда функция `person` завершится, её контекст выполнения выкинется из стека. Но её лексическое окружение всё ещё останется в памяти, так как на него ссылается лексическое окружение его внутренней функции `displayName`. Таким образом, её переменные всё ещё будут доступны в памяти.
При выполнении функции peter (которая на самом деле является отсылкой к функции `displayName`), **JavaScript** создаёт новый контекст выполнения и лексическое окружение для этой функции.
Так что его лексическое окружение будет выглядеть таким образом:

```js
displayNameLexicalEnvironment = {
  environmentRecord: {
    
  }
  outer: <personLexicalEnvironment>
}
```

В функции `displayName` нет переменной, её запись окружения будет пуста. Во время выполнения этой функции, **JavaScript** будет пытаться найти переменную name в лексическом окружении функции.

Так как там нет переменных в лексическом окружении функции `displayName`, она будет искать во внешнем лексическом окружении, то есть, лексическом окружении функции `person`, которое до сих пор в памяти. **JavaScript** найдёт эту переменную и name выводится в консоль.

**№2**

```js
function getCounter() {
  let counter = 0;
  return function() {
    return counter++;
  }
}
let count = getCounter();
console.log(count());  // 0
console.log(count());  // 1
console.log(count());  // 2
```

Снова, лексическое окружение для функции `getCounter` будет выглядеть таким образом:

```js
getCounterLexicalEnvironment = {
  environmentRecord: {
    counter: 0,
    <anonymous function> : < reference to function>
  }
  outer: <globalLexicalEnvironment>
}
```

Эта функция возвращает анонимную функцию и назначает её на переменную `count`.

Когда функция `count` выполняется, её лексическое окружение будет выглядеть таким образом:

```js
countLexicalEnvironment = {
  environmentRecord: {
  
  }
  outer: <getCountLexicalEnvironment>
}
```

Когда функция count вызывается, **JavaScript** начнет поиск в лексическом окружении этой функции на наличие переменной `counter`. Снова, если ее запись окружения пуста, то движок пойдёт искать во внешнем лексическом окружении функции.

Движок находит переменную, выводит её в консоль и увеличивает переменную `counter` в лексическом окружении `getCounter` функции.

Таким образом, лексическое окружение для функции `getCounter` после первого вызова функции count будет выглядеть таким образом:

```js
getCounterLexicalEnvironment = {
  environmentRecord: {
    counter: 1,
    <anonymous function> : < reference to function>
  }
  outer: <globalLexicalEnvironment>
}
```

На каждом вызове функции `count`, **JavaScript** создаёт новое лексическое окружение для функции `count`, увеличивает переменную count и обновляет лексическое окружения функции `getCounter`, чтобы соответствовать изменениям.

#### for in и for of отличия

`for in` проходится по ключам, `for of` проходится по значениям и он асинхронный

**for...of**

Оператор `for...of` выполняет обход по элементам коллекций (иначе говоря, итерируемых объектов, например, `Array`, `Map`, `Set`, `String`, `arguments`, `DOM` коллекций и т.д.), вызывая на каждом шаге итерации значение, а не ключ.

Оператор `for...of` для массивов работает аналогично `forEach`, но имеет преимущества: возможность использовать `continue` и `break` для контроля итераций, что невозможно в `forEach`.

**Обход массива**

```js
let arr = [10, 20, 30];

for (let value of arr) {
  value += 1;
  console.log(value);
}
// 11 21 31
```

**Обход строки**

```js
let str = 'cold';

for (let value of str) {
  console.log(value);
}
// "c" "o" "l" "d"
// 11 21 31
```

**Обход Map**

```js
let iterable = new Map([['a', 1], ['b', 2], ['c', 3]]);

for (let entry of iterable) {
    console.log(entry);
}
// ['a', 1] ['b', 2] ['c', 3]

for (let [key, value] of iterable) {
    console.log(value);
}
// 1 2 3
```

**for...in**

Цикл `for...in` проходит через перечисляемые свойства объекта. Он пройдёт по каждому отдельному элементу, вызывая на каждом шаге ключ. Цикл `for...in` проходит по свойствам в произвольном порядке, поэтому его не следует использовать для `Array`.

```js
var obj = {a:1, b:2, c:3};

for (var prop in obj) {
  console.log(obj[prop]);
}

// 1 2 3
// 1 2 3
```

#### Рекурсия

**Рекурсия** - функция, которая вызывает саму себя.

```js
function look_for_key(box) {
  for (item in box) {
    if (item.is_a_box()) {
      look_for_key(item);
    } else if (item.is_a_key()) {
      console.log("found the key!")
    }
  }
}
```

**Хвостовая Рекурсия** - рекурсивный вызов происходит в самом конце. Это позволяет не держать стековые кадры в стеке.

**Стек вызовов** - Когда программа вызывает функцию, функция отправляется на верх стека вызовов. Это похоже на стопку книг, вы добавляете одну вещь за одни раз. Затем, когда вы готовы снять что-то обратно, вы всегда снимаете верхний элемент.

Пример подсчета факториала. Factorial(5) пишется как 5! и рассчитывается как 5! = 5*4*3*2*1. Вот рекурсивная функция для подсчета факториала числа:

```js
function fact(x) {
  if (x == 1) {  
    return 1;  
  } else {      
    return x * fact(x-1);
  }
}
```

Каждое обращение к функции `fact` содержит свою собственную копию `x`. Это очень важное условие для работы рекурсии. Вы не можете получить доступ к другой копии функции от `x`.
«Стопка коробок» сохраняется в стеке. Формируется стек из наполовину выполненных обращений к функции, каждое из которых содержит свой наполовину выполненный список из коробок для просмотра. Стек следит за стопкой коробок для Вас!

#### Оператор typeof

Оператор `typeof` получает некоторое значение и определяет его тип:

```js
let a;
typeof a; // "undefined"

a = "hello world";
typeof a; // "string"

a = 42;
typeof a; // "number"

a = true;
typeof a; // "boolean"

a = null;
typeof a; // "object" - известный баг JS

a = undefined;
typeof a;// "undefined"

a = { b: "c" };
typeof a; // "object"
```

#### Приведение типов (coercion)

Конвертация встроенных типов данных друг в друга в **JavaScript** называется приведением типов (coercion). Оно бывает явное (explicit) и неявное (implicit).

Пример явного приведения:

```js
let a = "42"; // строка
let b = Number(a);

a; // "42" - строка
b; // 42 - число
```
Пример неявного приведения:

```js
let a = "42"; // строка
let b = a * 1;	// строка "42" неявно приводится к числу

a; // "42" - строка
b; // 42  - число
```
Особенно важно понимать, как происходит приведение к логическому типу, так как оно часто происходит в условиях.

Следующие значения приводятся к `false`:

- «» (пустая строка)
- 0, -0, NaN
- null, undefined
- false

А эти — к `true`:

- «hello»  (любая непустая строка, даже » » — строка с пробелом)
- 42 (число, отличное от нуля)
- true
- [ ], [ 1, «2»] (любой массив, даже пустой)
- { }, { a: 42 } (любой объект, даже пустой)
- function foo() { } (любая функция)

#### Равенство (equality)

В **JavaScript** есть два вида сравнения:

- строгое (strict) без приведения типов (===);
- абстрактное с приведением типов.

```js
var a = "42";
var b = 42;

a == b; // true - абстрактное, строка привелась к числу
a === b; // false - строгое
```

Сравнение осуществляется по некоторым простым правилам:

- Если значения (стороны сравнения) могут принимать значения true или false, избегайте нестрогого сравнения и используйте ===;
- Если в сравнении участвуют специфические значения (0, "", [] — пустой массив), избегайте нестрогого сравнения.
- В остальных случаях можно использовать ==. Это безопасно и во многих случаях делает код более читаемым.

#### null и undefined

**JavaScript** (и **TypeScript**) имеют два специальных типа данных — null и undefined. Они предназначены для обозначения разных вещей (концепций):

- undefined — значение не инициализировано
- null — значение в данный момент недоступно

#### Ключевое слово let

Переменные, объявленные с помощью ключевого слова `let`, имеют блочную область видимости. Это значит, что они недоступны снаружи блока `{...}`

#### Полифиллы и шимы (shim)

**Шим** — это любой фрагмент кода, который перехватывает обращение к API и добавляет уровень абстракции в приложение. Шимы существуют не только в вебе.

**Полифилл** — это шим для веба и браузерных API — специальный код (или плагин), который позволяет добавить некоторую функциональность в среду, которая эту функциональность по умолчанию не поддерживает (например, добавить новые функции JS в старые браузеры). Полифиллы не являются частью стандарта HTML5.

#### Разница между ES5 и ES6

- ECMAScript 5 (ES5) — 5-е издание ECMAScript, стандартизированное в 2009 году. Поддерживается современными браузерами практически полностью.
- ECMAScript 6 (ECMAScript 2016, ES6) — 6-е издание ECMAScript, стандартизированное в 2015 году. Частично поддерживается большинством современных браузеров.


Несколько ключевых отличий двух стандартов:

- Стрелочные функции и интерполяция в строках.

```js
const greetings = (name) => {
      return `hello ${name}`;
}


const greetings = name => `hello ${name}`;
```

- Ключевое слово `const`.Константы в **JavaScript** отличаются от констант в других языках программирования. Они сохраняют неизменной только ссылку на значение. Таким образом, вы можете добавлять, удалять и изменять свойства объявленного константным объекта, но не можете перезаписать текущую переменную, в которой лежит этот объект.

```js
const NAMES = [];
NAMES.push("Jim");
console.log(NAMES.length === 1); // true
NAMES = ["Steve", "John"]; // error
```

- Блочная видимость.Переменные, объявленные с помощью новых ключевых слов let и const имеют блочную область видимости, то есть недоступны за пределами {}-блоков. Кроме того, они не поднимаются, как var-переменные.
- Параметры по умолчанию.Теперь функцию можно инициализировать с дефолтным значением параметров. Оно будет использовано, если параметр не будет передан при вызове.

```js
// Basic syntax
function multiply (a, b = 2) {
     return a * b;
}
multiply(5); // 10
```

- Классы и наследование. Новый стандарт ввел в язык поддержку привычного синтаксиса классов `(class)`, конструкторы `(constructor)` и ключевое слово `extend` для оформления наследования.
- Оператор `for-of` для перебора итерируемых объектов в цикле.
- `spread`-оператор, который удобно использовать для слияния объектов и еще во многих случаях.

```js
const obj1 = { a: 1, b: 2 }
const obj2 = { a: 2, c: 3, d: 4}
const obj3 = {...obj1, ...obj2}
```

- Обещания `(Promises)`.Механизм для обработки результатов и ошибок асинхронных операций. По сути, это то же самое, что и коллбэки, но гораздо удобнее. Например, промисы можно чейнить (объединять в цепочки).

```js
const isGreater = (a, b) => {
  return new Promise ((resolve, reject) => {
    if(a > b) {
      resolve(true)
    } else {
      reject(false)
    }
  })
}
isGreater(1, 2)
  .then(result => {
    console.log('greater')
  })
 .catch(result => {
    console.log('smaller')
 })
 ```

 - Модули. Способ разбития кода на отдельные модули, которые можно импортировать при необходимости.

```js
import myModule from './myModule';
```

Синтаксис экспорта позволяет выделить функциональность модуля:

```js
const myModule = { x: 1, y: () => { console.log('This is ES5') }}
export default myModule;
```

#### Примитивные значения

**Является ли число целым (integer)?**

Очень простой способ проверить, имеет ли число дробную часть — разделить его на единицу и проверить остаток.

```js
function isInt(num) {
  return num % 1 === 0;
}

console.log(isInt(4)); // true
console.log(isInt(12.2)); // false
console.log(isInt(0.3)); // false
```

**Почему 0.1 + 0.2 === 0.3 — это false?**

Действительно, в **JavaScript** 0.1 + 0.2 на самом деле равно 0.30000000000000004. Дело в том, что все числа в языке (даже целые) представлены в формате с плавающей запятой (float). В двоичной системе счисления эти числа — бесконечные дроби. Для их хранения выделяется ограниченный объем памяти, поэтому возникают подобные неточности.

#### Объекты

Объекты — это составные структуры. Для них можно определить именованные свойства, в каждом из которых может содержаться значение любого типа.

```js
let obj = {
  a: "hello world", // свойство a со значением "hello world"
  b: 42,
  c: true
};

obj.a;		// "hello world", получение через точку (doted-notation)
obj.b;		// 42
obj.c;		// true

obj["a"];	// "hello world", получение через скобки (bracket-notation)
obj["b"];	// 42
obj["c"];	// true
```

Скобочная нотация также полезна, если вы хотите получить доступ к свойству, имя которого хранится в переменной:

```js
let obj = {
  a: "hello world",
  b: 42
};

let b = "a";

obj[b];			// "hello world"
obj["b"];		// 42
```

#### Сравнение объектов

Непримитивные значения в **JavaScript** (объекты) передаются по ссылке, а операторы сравнения (`==` и `===`) сравнивают именно ссылки. Поэтому два разных объекта с идентичными свойствами не будут равны друг другу.

```js
let a = [1, 2, 3];
let b = [1, 2, 3];

a == b // false
a === b // false
```

Есть одна маленькая хитрость, которой можно воспользоваться для сравнения простых структур. Если сравнить объект с примитивом (например, строкой), он будет приведет к строковому представлению. Для массива, например, строковое представление — это строка со списком элементов, разделенных запятыми. Благодаря этому можно сделать так:

```js
let a = [1, 2, 3];
let b = [1, 2, 3];
let c = "1,2,3";

a == c // true
a.toString() == b.toString() // true
```

#### Массивы

Массивы — это объекты, которые хранят значения не в именованных свойствах, а в нумерованных.

```js
let arr = [
  "hello world",
  42,
  true
];

arr[0];	 // "hello world"
arr[1];	 // 42
arr[2];	 // true
arr.length; // 3

typeof arr; // "object"
```

#### Как очистить массив?

- Присвоить переменной новый пустой массив:

```js
let arr = [1, 2, 3];
arr = [];
```

Этот способ можно использовать, если ваш код никак не ссылается на оригинальный массив, так как при этом создается совершенно новый объект. Если в какой-то переменной есть ссылка на arr, то она не изменится.

```js
let arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; 
let anotherArrayList = arrayList; 
arrayList = []; 
console.log(anotherArrayList); // ['a', 'b', 'c', 'd', 'e', 'f']
```

- Обнулить длину массива

```js
let arr = [1, 2, 3];
arr.length = 0;
```

Существующий массив очистится, так же как и все ссылки на него.

```js
let arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; 
let anotherArrayList = arrayList; 
arrayList.length = 0; 
console.log(anotherArrayList); // []
```

Существующий массив очистится, так же как и все ссылки на него.

```js
let arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; 
let anotherArrayList = arrayList; 
arrayList.length = 0; 
console.log(anotherArrayList); // []
```

- Вырезать лишние элементы

```js
let arr = [1, 2, 3];
arr.splice(0, arr.length);
```

Работает так же, как предыдущий способ:

```js
let arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; 
let anotherArrayList = arrayList; 
arrayList.splice(0, arrayList.length); 
console.log(anotherArrayList); // []
```

- Удалять элементы по одному

```js
let arr = [1, 2, 3];

while(arr.length) {
  arr.pop();
}
```

#### Является ли объект массивом?

Лучший способ узнать, является ли объект инстансом определенного класса, — использовать метод `Object.prototype.toString()`.

```js
let arr = [1, 2, 3];
let obj = {'a': 1, 'b': 2};
let str = "hello";
let foo = function() {};

console.log(Object.prototype.toString.call(arr)); // [object Array]
console.log(Object.prototype.toString.call(obj)); // [object Object]
console.log(Object.prototype.toString.call(str)); // [object String]
console.log(Object.prototype.toString.call(foo)); // [object Function]
```

#### В массиве чисел найти максимальную разницу между двумя элементами, причем большее число должно обязательно стоять после меньшего

```js
let array = [7, 8, 4, 9, 9, 15, 3, 1, 10];
```

Решение выглядит так:

```js
findLargestDifference(array);

function findLargestDifference(array) {
  // если в массиве всего один элемент, возвращаем -1
  if (array.length <= 1) return -1;

  // в currentMin сохраняем текущее минимальное значение
  let currentMin = array[0];
  let currentMaxDifference = 0;

  // перебираем массив и сохраняем максимальную разницу в currentMaxDifference
  // параллельно отслеживаем минимальный элемент

  for (var i = 1; i < array.length; i++) {
    if (array[i] > currentMin && (array[i] - currentMin > currentMaxDifference)) {
      currentMaxDifference = array[i] - currentMin;
    } else if (array[i] <= currentMin) {
      currentMin = array[i];
    }
  }

  // If negative or 0, there is no largest difference
  if (currentMaxDifference <= 0) return -1;

  return currentMaxDifference;
}
```

#### Удалить из массива повторяющиеся значения, вернуть только уникальные элементы

**ES6**

```js
let array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];
Array.from(new Set(array)); // [1, 2, 3, 5, 9, 8]
```

**ES5**

```js
let array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];

uniqueArray(array); // [1, 2, 3, 5, 9, 8]

function uniqueArray(array) {
  let hashmap = {};
  let unique = [];

  for(var i = 0; i < array.length; i++) {
    // если в объекте еще нет ключа, равного значению элемента, добавляем
    if(!hashmap.hasOwnProperty(array[i])) {
      hashmap[array[i]] = 1;
      unique.push(array[i]);
    }
  }

  return unique;
}
```

#### Реализуйте добавление в очередь и удаление из очереди с помощью двух стеков

```js

var inputStack = []; // первый стек
var outputStack = []; // второй стек

// добавление в очередь
// просто добавляем элемент в первый стек
function enqueue(stackInput, item) {
  return stackInput.push(item);
}

// удаление из очереди
function dequeue(stackInput, stackOutput) {

  // развернем стек входящих элементов (input)
  // в output-стеке все элементы окажутся в обратном порядке
  // первый элемент в input-стеке (первый пришедший) станет последним в output-стеке
  // так его проще будет получить, не нарушая нумерацию элементов 

  if (stackOutput.length <= 0) {
    while(stackInput.length > 0) {
      let elementToOutput = stackInput.pop(); // последний из входящих
      stackOutput.push(elementToOutput);
    }
  }

  return stackOutput.pop(); // вернуть последний элемент из output-стека
}
```

В `stackInput` элементы добавляются как в обычную очередь. Если нужно получить первый элемент, мы переворачиваем исходную очередь, там что первый элемент в `stackInput` становится последним в `stackOutput`. Забрать последний элемент из `stackOutput` (который, на самом деле, первый в очереди), можно с помощью метода `pop`.

Обратите внимание, пока в `stackOutput` есть хоть один элемент, мы ничего не переносим из исходной очереди, чтобы не нарушать порядок. Элементы там могут накапливаться и дальше. Когда output-стек обнулится, мы вновь скопируем в него накопившиеся элементы.

#### Найти пересечение массивов

Пересечение — это набор элементов, которые присутствуют в обоих массивах. При этом необходимо отбирать только уникальные элементы.

```js
let firstArray = [2, 2, 4, 1];
let secondArray = [1, 2, 0, 2];

intersection(firstArray, secondArray); // [2, 1]

function intersection(firstArray, secondArray) {

  let hashmap = {};
  let intersectionArray = [];

  firstArray.forEach(function(element) {
    hashmap[element] = 1;
  });

  secondArray.forEach(function(element) {
    if (hashmap[element] === 1) {
      intersectionArray.push(element);
      hashmap[element]++;
    }
  });

  return intersectionArray;
}
```

Сначала создаем хеш, ключами которого являются значения первого массива. Операция поиска по ключу в хеше имеет сложность O(1).
Затем перебираем элементы второго массива и проверяем, есть ли они в первом.
Общая сложность алгоритма — O(n).

#### В неотсортированном массиве целых чисел найти наибольшее произведение любых трех элементов

Помните, что максимальным необязательно окажется произведение трех самых больших чисел в массиве. Если минимальные элементы отрицательны, но велики по модулю, возможно искомым окажется произведение `min1 * min2 * max1` (двум самых маленьких и одного самого большого).

#### Рекурсивный бинарный поиск

Вкратце: бинарный (двоичный) поиск делит отсортированный массив на половины.

```js
function recursiveBinarySearch(array, value, leftPosition, rightPosition) {
  if (leftPosition > rightPosition) return -1;

  var middlePivot = Math.floor((leftPosition + rightPosition) / 2);
  if (array[middlePivot] === value) {
    return middlePivot;
  } else if (array[middlePivot] > value) {
    return recursiveBinarySearch(array, value, leftPosition, middlePivot - 1);
  } else {
    return recursiveBinarySearch(array, value, middlePivot + 1, rightPosition);
  }
}
```

#### Сбалансированность скобок

```js
let expression = "{{}}{}{}"
let expressionFalse = "{}{{}";

isBalanced(expression); // true
isBalanced(expressionFalse); // false
isBalanced(""); // true

function isBalanced(expression) {
  let checkString = expression;
  let stack = [];

  // если строка пуста, то считаем скобки сбалансированными
  if (checkString.length <= 0) return true;

  for (let i = 0; i < checkString.length; i++) {
    if(checkString[i] === '{') {
      stack.push(checkString[i]);
    } else if (checkString[i] === '}') {
      // Pop on an empty array is undefined
      if (stack.length > 0) {
        stack.pop();
      } else {
        return false;
      }
    }
  }

  // If the array is not empty, it is not balanced
  if (stack.pop()) return false;
  return true;
}
```

#### Функция обратного вызова (callback) с простым примером

**Коллбэк** — это функция, которая передается в другую функцию как аргумент и выполняется после того, как закончатся какие-то другие операции. В примере ниже коллбэк логирует в консоль.

```js
function modifyArray(arr, callback) {
  // некоторые операции с массивом arr
  arr.push(100);
  // после того, как операции закончены, выполняется коллбэк
  callback();
}

let arr = [1, 2, 3, 4, 5];

modifyArray(arr, function() {
  console.log("массив модифицирован", arr);
});
```
#### IIFE (Immediately Invoked Function Expression) и паттерн Модуль

`IIFE`, или немедленно выполняемое функциональное выражение, — это объявление функции с ее немедленным выполнением.

```js
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

Такой синтаксис позволяет избежать загрязнения глобального пространства имен, ведь все необходимые переменные можно спрятать в замыкании.

С помощью `IIFE` в **JavaScript** реализуется паттерн проектирования Модуль — независимый от внешнего кода компонент.

#### Как создать по-настоящему приватный метод класса и в чем недостатки таких методов?

Создать действительно приватный метод в `JS` можно, только поместив его в замыкание конструктора.

```js
var Employee = function (name, company, salary) {
  this.name = name || "";       // Публичное свойство
  this.company = company || ""; // Публичное свойство
  this.salary = salary || 5000; // Публичное свойство

  // Приватный метод
  let increaseSalary = function () {
    this.salary = this.salary + 1000;
  };

  // Публичный метод
  this.dispalyIncreasedSalary = function() {
    increaseSalary();
    console.log(this.salary);
  };
};


let emp1 = new Employee("John","Pluto",3000);
let emp2 = new Employee("Merry","Pluto",2000);
let emp3 = new Employee("Ren","Pluto",2500);
```

Недостатком такого способа является неизбежное дублирование. Функция `increaseSalary` будет создаваться для каждого экземпляра `Employee`, хотя в этом нет необходимости.

#### Паттерн Прототип, прототипное наследование

Паттерн Прототип (Шаблон Свойств) создает новые объекты, которые сразу же инициализируются значениями, скопированными из некоторого образца (прототипа). Это могут быть дефолтные значения из базы данных, например.

Этот Паттерн нечасто используется в классических языках программирования, но в **JavaScript** на нем полностью построена объектная модель. До `ES6` даже привычного синтаксиса классов не было (сейчас это просто синтаксический сахар).

Смысл прототипного наследования заключается в том, что свойство или метод объекта, к которому происходит обращение, ищется интерпретатором сначала в самом объекте, затем в его прототипе, прототипе прототипа и так далее по цепочке.

#### Задача по прототипной модели

Что выведет этот код?

```js
let Employee = {
  company: 'xyz'
}
let emp1 = Object.create(Employee);
delete emp1.company
console.log(emp1.company);
```

Ответ: `‘xyz’`. Оператор `delete` удаляет только собственные свойства объекта, а свойство company определено в прототипе.
Проверить, является ли свойство собственным можно с помощью метода `emp1.hasOwnProperty('company')`.
Можно удалить свойство напрямую из прототипа: `delete emp1.__proto__.company`.

#### Ключевое слово new

`new` создает новый объект с типом `object` и устанавливает его внутреннее свойство [`[prototype]`] (`__proto__` — равно свойству `prototype` функции-конструктора). Указатель `this` при этом указывает на только что созданный объект. После того как конструктор модифицирует этот объект, он возвращается.

Выглядит это примерно так:

```js
function New(func) {
    let res = {};
    if (func.prototype !== null) {
        res.__proto__ = func.prototype;
    }
    let ret = func.apply(res, Array.prototype.slice.call(arguments, 1));
    if ((typeof ret === "object" || typeof ret === "function") && ret !== null) {
        return ret;
    }
    return res;
}
```

#### Ключевое слово this

`this` всегда ссылается на объект, в контексте которого вызван метод. Этот объект определяется динамически, его можно менять. Метод, определенный в одном объекте, вполне может быть вызван в контексте другого.

```js
function foo() {
	console.log( this.bar );
}

let obj1 = {
  bar: "obj1",
  foo: foo
};

let obj2 = {
  bar: "obj2"
};

obj1.foo();	    // "obj1"
foo.call( obj2 );   // "obj2"
new foo();	     // undefined
```

Обратите внимание, при вызове функции с `new` она выступает как конструктор и возвращает пустой объект, у которого не определено свойство `bar`.

Если вызвать функцию без контекста (`foo()`) в строгом режиме, будет ошибка, так как `this` в этом случае не определен.

#### Привязка контекста функции — метод bind

Метод `bind()` создает новую функцию, для которой зафиксирован `this` и последовательность аргументов, предшествующих аргументам при вызове новой функции.

Удобно использовать для привязки контекста или закрепления параметров.

```js
function fullName() {
  return "Hello, this is " + this.first + " " + this.last;
}

console.log(fullName()); // => Hello this is undefined undefined

// привязка контекста
var person = {first: "Foo", last: "Bar"};
console.log(fullName.bind(person)()); // => Hello this is Foo Bar
```

#### Как добавить массивам пользовательский метод?

Так как **JavaScript** основан на прототипах, чтобы добавить новый метод всем массивам, нужно определить его в прототипе массивов (объект `Array.prototype`).

```js
Array.prototype.average = function() {
  let sum = this.reduce(function(prev, cur) { return prev + cur; });
  return sum / this.length;
}

let arr = [1, 2, 3, 4, 5];
let avg = arr.average();
console.log(avg); // => 3
```

#### Всплытие событий и как его остановить

Всплытие событий — это концепция `DOM-модели`. Когда событие генерируется на элементе, все родительские элементы последовательно о нем оповещаются. Поэтому вы можете повесить обработчик клика на контейнер карточки и таким образом узнать о клике по вложенной кнопке.

Остановить всплытие можно с помощью метода объекта события event.stopPropagation(). В IE < 9 у объекта события есть свойство `event.cancelBubble`.