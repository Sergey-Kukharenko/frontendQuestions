# javascript tasks

#### **Реализуйте функцию `like`, которая должна принимать входной массив, содержащий имена людей, которым нравится элемент. Он должен возвращать отображаемый текст, как показано в примерах:**

#### **Пример**

```js
/**
likes [] -- must be "no one likes this"
likes ["Peter"] -- must be "Peter likes this"
likes ["Jacob", "Alex"] -- must be "Jacob and Alex like this"
likes ["Max", "John", "Mark"] -- must be "Max, John and Mark like this"
likes ["Alex", "Jacob", "Mark", "Max"] -- must be "Alex, Jacob and 2 others like this"
**/
```

#### **Решение**

```js
const arr = ["Alex", "Jacob", "Mark", "Max"]

function likes(names) {
    switch (names.length) {
        case 0:
            return 'no one likes this';
            break;
        case 1:
            return names[0] + ' likes this';
            break;
        case 2:
            return names[0] + ' and ' + names[1] + ' like this';
            break;
        case 3:
            return names[0] + ', ' + names[1] + ' and ' + names[2] + ' like this';
            break;
        default:
            return names[0] + ', ' + names[1] + ' and ' + (names.length - 2) + ' others like this';
    }
}

likes(arr)
```

#### **Удалить все значения из массива `a`, которые присутствуют в массива `b`**

#### **Пример**

```js
arrayDiff([1,2,2,2,3],[2]) == [1,3]
```

#### **Решение**

```js
const arrayDiff = (a, b) => {
    const setB = new Set(b);
    return a.filter(n => !setB.has(n));
}

arrayDiff([1, 2, 2, 2, 3], [1, 2])
```

**или**

```js
const arrayDiff = (a, b) => a.filter(x => !b.includes(x))

arrayDiff([1, 2, 2, 2, 3], [1, 2])
```

**или**

```js
const arrayDiff = (a, b) => {
    return a.concat(b).filter(function (val, index, arr) {
        return arr.indexOf(val) === arr.lastIndexOf(val);
    });
}

console.log(arrayDiff([1, 2, 2, 2, 3], [1, 2]));
```

**или**

```js
const arrayDiff = (a, b) => a.filter(function (i) { return b.indexOf(i) < 0; })

console.log(arrayDiff([1, 2, 2, 2, 3], [1, 2]));
```

**или**

```js
const arrayDiff = (a, b) => a.filter(function (i) { return b.indexOf(i) < 0; })

console.log(arrayDiff([1, 2, 2, 2, 3], [1, 2]));
```

#### Реализуйте метод, который принимает 3 целочисленных значения `a`, `b`, `c`. Метод должен возвращать `true`, если треугольник можно построить со сторонами заданной длины, и `false` в любом другом случае.

#### (В этом случае для принятия все треугольники должны иметь поверхность больше 0).

#### **Решение**

```js
const isTriangle = (a, b, c) => {
    const sides = [a, b, c];
    const max = Math.max(...sides);
    const sum = sides.reduce((a, b) => a + b, 0);
    return sum - max > max;
}

console.log(isTriangle(1, 1, 1)) // => true
console.log(isTriangle(1, 2, 1)) // = false
console.log(isTriangle(1, 2, 2)) // => true
```

**или**

```js
const arrayDiff = (a, b) => a.filter(function (i) { return b.indexOf(i) < 0; })

console.log(arrayDiff([1, 2, 2, 2, 3], [1, 2]));
```

#### На этот раз ни рассказа, ни теории. В приведенных ниже примерах показано, как написать функцию накопителя:


```js
accum("abcd") -> "A-Bb-Ccc-Dddd"
accum("RqaEzty") -> "R-Qq-Aaa-Eeee-Zzzzz-Tttttt-Yyyyyyy"
accum("cwAt") -> "C-Ww-Aaa-Tttt"
```

#### **Решение**

```js
const accum = str => {
    const newArr = []
    str.split('').forEach(function (letter, idx) {
        let idxChanged = idx + 1
        for (let i = 0; i < idxChanged; i++) {
            (i === 0) ? newArr.push(letter.toUpperCase()) : newArr.push(letter.toLowerCase())
        }
        if (idx < str.length - 1) newArr.push('-')
    });
    return newArr.join('')
}

console.log(accum('ZpglnRxqenU'))
```

**или**

```js
const accum = str => str
    .split('')
    .map((ch, i) => 
    (ch = ch.toLowerCase().repeat(i + 1)) && ch.charAt(0).toUpperCase()+ ch.slice(1))
    .join('-');

console.log(accum('cwAt'))
```

#### Итак, если мы должны начать нашу последовательность Трибоначчи с [1, 1, 1] в качестве начального ввода (сигнатура AKA), у нас есть такая последовательность:

```js
[1, 1 ,1, 3, 5, 9, 17, 31, ...]
```

#### **Решение**

```js
function tribonacci(signature, n) {
    var trib = signature;
    for (i = 3; i < n; i++) {
        trib.push((trib[i - 1] + trib[i - 2] + trib[i - 3]));
    }
    return trib.slice(0, n);
}
console.log(tribonacci([1, 1, 1], 10)); // [1,1,1,3,5,9,17,31,57,105]
```

#### Отсортировать заданную строку. Каждое слово в строке будет содержать одно число. Это число - позиция, которую слово должно занимать в результате.

```js
"is2 Thi1s T4est 3a"  -->  "Thi1s is2 3a T4est"
"4of Fo1r pe6ople g3ood th5e the2"  -->  "Fo1r the2 g3ood 4of th5e pe6ople"
```

#### **Решение**

```js
const order = words => {
    const regex = /\d+/g;
    const wordArray = words.split(' ')
    const newArray = new Array(wordArray.length);

    wordArray.map(word => {
        const m = parseInt(word.match(regex)) - 1
        newArray.splice(m, 1, word)
    })

    return newArray.join(' ')
}

console.log(order('4of Fo1r pe6ople g3ood th5e the2"), "Fo1r the2 g3ood 4of th5e pe6ople'))
console.log(order(''))
```

**или**

```js
const order = words => {
    return words.split(' ').sort(function (a, b) {
        return a.match(/\d/) - b.match(/\d/);
    }).join(' ');
}

console.log(order('is2 Thi1s T4est 3a'))
```

**или**

```js
const order = words => {
    const reg = /\d/;
    const comparator = (word, nextWord) => +word.match(reg) - +nextWord.match(reg)
    return words.split(' ').sort(comparator).join(' ');
}

console.log(order('is2 Thi1s T4est 3a'))
```

#### Учитывая треугольник последовательных нечетных чисел:
```js
             1
          3     5
       7     9    11
   13    15    17    19
21    23    25    27    29
...
```
#### Вычислите суммы строк этого треугольника из индекса строки (начиная с индекса 1), например:

```js
rowSumOddNumbers(1); // 1
rowSumOddNumbers(2); // 3 + 5 = 8
```

#### **Решение**

```js
const rowSumOddNumbers = n => n * n * n

console.log(rowSumOddNumbers(2))
```

#### **Решение**

#### преобразовать строку в новую строку, где каждый символ в новой строке - «(», если этот символ появляется только один раз в исходной строке, или «)», если этот символ появляется более одного раза в исходной строке. Игнорируйте использование заглавных букв при определении того, является ли символ дубликатом.

```js
const duplicateEncode = word => {
    let letterCount = {};
    let letters = word.toLowerCase().split('');

    letters.forEach(function (letter) {
        letterCount[letter] = (letterCount[letter] || 0) + 1;
    });

    return letters.map(function (letter) {
        return letterCount[letter] === 1 ? '(' : ')';
    }).join('');
}

console.log(duplicateEncode('din'));
console.log(duplicateEncode('recede'));
console.log(duplicateEncode('Success'));
```

**или**

```js
const duplicateEncode = word => {
    const letters = word.toLowerCase().split('');
    const counts = letters.reduce((ct, ltr) => ((ct[ltr] = (ct[ltr] || 0) + 1), ct), {});
    return letters.map(letter => counts[letter] === 1 ? '(' : ')').join('');
}
```

**или**

```js
const duplicateEncode = word => {
    return [...word].map(letter =>
        word.match(new RegExp(letter, "ig")).length === 1 ? "(" : ")"
    ).join("");
}
```

**или**

```js
const duplicateEncode = word => {
    return word.toLowerCase()
        .split('')
        .reduce((acc, char, i, arr) => {
            const symbol = arr.filter(letter => letter === char).length < 2 ? '(' : ')'
            return acc + symbol
        }, '')
}
```

**или**

```js
const duplicateEncode = word => {
    var repeat = [];
    var result = [];
    var letters = word.split('');
    for (i = 0; i < letters.length; i++) {
        repeat.push(letters[0]);
        if (repeat.indexOf(letters[i]) > -1) {
            result.push(")");
        } else {
            result.push("(");
        }
        repeat.push(letters[i]);
    }
    return result.join("");
}
```

**или**

```js
const duplicateEncode = word => {
    let w = word.toLowerCase();
    return Array.from(w).map(x => w.replace(new RegExp(`[^${x}]`, 'g'), "").length > 1 ? ')' : '(').join('');
}
```

#### Создайте функцию с именем divisors / Divisors, которая принимает целое число n> 1 и возвращает массив со всеми делителями целого числа (кроме 1 и самого числа), от наименьшего до наибольшего. Если число простое, верните строку '(целое число) простое'

#### **Решение**

```js
function divisors(integer) {
    const arr = [];

    for (let index = integer; index >= 1; index--) {
        let result = integer / index
        if (Number.isInteger(result)) {
            if ((result != 1))
                arr.push(result)

        }

    }

    arr.splice(arr.length - 1, 1)

    return (arr.length === 0) ? `${integer} is prime` : arr
};
```

**или**

```js
function divisors(x) {
    arr = [];
    for (var i = 2; i < x; i++) {
        if (x % i === 0) {
            arr.push(i);
        }
    } if (arr.length === 0) {
        return `${x} is prime`;
    } else {
        return arr;
    }
}
```

#### Азбука морзе

#### **Решение**

```js
function decodeMorse(morseCode) {
    var ref = {
        '.-': 'a',
        '-...': 'b',
        '-.-.': 'c',
        '-..': 'd',
        '.': 'e',
        '..-.': 'f',
        '--.': 'g',
        '....': 'h',
        '..': 'i',
        '.---': 'j',
        '-.-': 'k',
        '.-..': 'l',
        '--': 'm',
        '-.': 'n',
        '---': 'o',
        '.--.': 'p',
        '--.-': 'q',
        '.-.': 'r',
        '...': 's',
        '-': 't',
        '..-': 'u',
        '...-': 'v',
        '.--': 'w',
        '-..-': 'x',
        '-.--': 'y',
        '--..': 'z',
        '.----': '1',
        '..---': '2',
        '...--': '3',
        '....-': '4',
        '.....': '5',
        '-....': '6',
        '--...': '7',
        '---..': '8',
        '----.': '9',
        '-----': '0',
    };

    return morseCode
        .split('   ')
        .map(
            a => a
                .split(' ')
                .map(
                    b => ref[b]
                ).join('')
        ).join(' ').toUpperCase()
}


decodeMorse(".... . -.--   .--- ..- -.. .")
```

**или**

```js
const decodeMorse = morseCode => {

    const MORSE_CODE = {
        '.-': 'A',
        '-…': 'B',
        '-.-.': 'C',
        '-..': 'D',
        '.': 'E',
        '..-.': 'F',
        '--.': 'G',
        '....': 'H',
        '..': 'I',
        '.---': 'J',
        '-.-': 'K',
        '.-..': 'L',
        '--': 'M',
        '-.': 'N',
        '---': 'O',
        '.--.': 'P',
        '--.-': 'Q',
        '.-.': 'R',
        '…': 'S',
        '-': 'T',
        '..-': 'U',
        '…-': 'V',
        '.--': 'W',
        '-..-': 'X',
        '-.--': 'Y',
        '--..': 'Z',
        '-----': '0',
        '.----': '1',
        '..---': '2',
        '…--': '3',
        '….-': '4',
        '…..': '5',
        '-….': '6',
        '--…': '7',
        '---..': '8',
        '----.': '9',
        '': 'SOS',
        '0': 'SOS',
        ' 0 ': 'S O S',
    }

    const decodeLetter = letter => {
        return MORSE_CODE[letter];
    }

    const decodeWord = word => {
        return word.split(' ').map(decodeLetter).join('');
    }

    return morseCode.trim().split('   ').map(decodeWord).join(' ');
}

console.log(decodeMorse('.... . -.--   .--- ..- -.. .'));
```