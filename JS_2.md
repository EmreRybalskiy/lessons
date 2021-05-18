# JS Lesson 2

# Преобразование типов

* Строковое преобразование
* Численное преобразование
* Логическое преобразование



# Строковое преобразование
Для того чтобы явно преобразовать значение в строку, можно воспользоваться функцией **String()**. 

Неявное преобразование вызывает использование обычного оператора сложения, +, с двумя операндами, если один из них является строкой:

```

String(123) // явное преобразование
123 + ''    // неявное преобразование

```

Все примитивные типы преобразуются в строки вполне естественным и ожидаемым образом:

```

String(123)    // '123'
String(-12.3)  // '-12.3'
String(null)   // 'null'
String(undefined)  // 'undefined'
String(true)   // 'true'
String(false)  // 'false'

```

В случае с типом **Symbol** дело несколько усложняется, так как значения этого типа можно преобразовать к строковому типу только явно.

```

String(Symbol('my symbol'))   // 'Symbol(my symbol)'
'' + Symbol('my symbol')      // ошибка TypeError

```

Что будет?

```

let a = 1 + ''; // a === ?

let b = 2 + 'hello'; // b === ?

let c = typeof(String(12) + 1); // c === ?

let d = 12 + '1'; // d === ? 

```


# Преобразование к типу Boolean

Для того, чтобы явно преобразовать значение к логическому типу, используют функцию **Boolean()**. 

Неявное преобразование происходит в логическом контексте, или вызывается логическими операторами **(|| && !)**.

```

Boolean(2)          // явное преобразование
if (2) { ... }      // неявное преобразование в логическом контексте
!!2                 // неявное преобразование логическим оператором
2 || 'hello'        // неявное преобразование логическим оператором

```

Обратите внимание на то, что операторы, вроде || и && выполняют преобразование значений к логическому типу для внутренних целей, а возвращают значения исходных операндов, даже если они не являются логическими.

```

// это выражение возвращает число 123, а не true
// 'hello' и 123 неявно преобразуются к логическому типу при работе оператора && для вычисления значения выражения
let x = 'hello' && 123;   // x === 123

```

Так как при приведении значения к логическому типу возможны лишь два результата — **true** или **false**, легче всего освоить этот вид преобразований, запомнив те выражения, которые выдают **false**:

```

Boolean('')           // false
Boolean(0)            // false     
Boolean(-0)           // false
Boolean(NaN)          // false
Boolean(null)         // false
Boolean(undefined)    // false
Boolean(false)        // false

```

# Преобразование к типу Number

Явное преобразование к числовому типу выполняется с помощью функции **Number()** — то есть по тому же принципу, который используется для типов **Boolean** и **String**.

Неявное приведение значения к типу Number выполняют следующие операторы:

- Операторы сравнения **(>, <, <=, >=)**.
- Побитовые операторы **(|, &, ^, ~)**.
- Арифметические операторы **(-, +, *, /, %)**. Обратите внимание на то, что оператор + с двумя операндами не вызывает неявное преобразование к числовому типу, если хотя бы один оператор является строкой.
- Унарный оператор **+**.
- Оператор нестрогого равенства **==** **(а также !=)**. Обратите внимание на то, что оператор == не производит неявного преобразования в число, если оба операнда являются строками.

```

Number('123')   // явное преобразование
+'123'          // неявное преобразование
123 != '456'    // неявное преобразование
4 > '5'         // неявное преобразование
5/null          // неявное преобразование
true | 0        // неявное преобразование

```

Вот как в числа преобразуются примитивные значения:

```
Number(null)         // 0
Number(undefined)    // NaN
Number(true)         // 1
Number(false)        // 0
Number(" 12 ")       // 12
Number("-12.34")     // -12.34
Number("\n")         // 0
Number(" 12s ")      // NaN
Number(123)          // 123

```

Значения типа Symbol не могут быть преобразованы в число ни явно, ни неявно

```
Number(Symbol('my symbol'))    // Ошибка TypeError
+Symbol('123')                 // Ошибка TypeError

```

Вот правила, которые стоит запомнить:

```

null == 0               // false, null не преобразуется в 0
null == null            // true
null === null           // true

undefined == undefined  // true
undefined === undefined  // true

null == undefined       // true
null === undefined      // false

```

# Преобразование типов для объектов

Самое простое — это преобразование в логическое значение: любое значение, не являющееся примитивом, всегда true

```
const person = {
 name: 'Vlad'
};

Boolean(person); // true

const obj = {};
Boolean(obj); // true

```

Но что будет, если я приведу object к строке?

```
let a = {};
console.log(a + ''); // [object Object]

```

А если к числу?

```
const b = {};
console.log(b + 1); // [object Object]1
```

Для этого есть стандартные методы js - **toString()** и **valueOf()** :


```
let person = {
 name: 'Vlad',
 toString: function () {
   return 'Hello World!'
 },
 valueOf: function () {
   return 1
 }
};

String(person); // 'Hello World!'
person + 1; // 2
person + ''; // '1'

```

!!! Обратите внимание на то, что person + '' возвращает ‘1’ в виде строки. Оператор **+** вызывает стандартный режим преобразования. Как уже было сказано, Object рассматривает приведение к числу как преобразование по умолчанию, поэтому использует сначала метод valueOf() а не toString().


# Примеры

```
true + false             // 1
12 / "6"                 // 2
"number" + 15 + 3        // 'number153'
15 + 3 + "number"        // '18number'
[1] > null               // true
"foo" + + "bar"          // 'fooNaN'
'true' == true           // false
false == 'false'         // false
null == ''               // false
!!"false" == !!"true"    // true
['x'] == 'x'             // true 
[] + null + 1            // 'null1'
[1,2,3] == [1,2,3]       // false

```

![help](https://a.d-cd.net/SYAAAgKXiOA-960.jpg)




# Тип BigInt

BigInt – это специальный числовой тип, который предоставляет возможность работать с целыми числами произвольной длины.

Чтобы создать значение типа BigInt, необходимо добавить n в конец числового литерала или вызвать функцию BigInt()

```
const bigint = 1234567890123456789012345678901234567890n;

const sameBigint = BigInt("1234567890123456789012345678901234567890");

const bigintFromNumber = BigInt(10); // то же самое, что и 10n

```


BigInt можно использовать как обычные числа, к примеру:

```
console.log(1n + 2n); // 3

console.log(5n / 2n); // 2

```

Но в математических операциях мы не можем смешивать bigint и обычные числа:

```
alert(1n + 2); // Error: Cannot mix BigInt and other types

```


# Функции (function)

В JavaScript функция является особым типом объекта, который позволяет закладывать и применять определенную логику для обработки данных.

**Синтааксис**

```
function наименование_функции(аргумент_1, аргумент_2, ..., аргумент_N){
  тело_функции
}

```

Вызов функции JavaScript осуществляется по ее имени, за которым следуют круглые скобки с аргументами (если они имеются)

Именно круглые скобки говорят, что функцию необходимо выполнить.

```
function sayHello() {
  console.log('Hello World');
}

sayHello(); // в консоли будет Hello World

```

- Давайте напишем функцию, которая будет выводить сумму чисел 1 и 5 в консоль


Рассмотрим пример c аргументами:

```
function sayHello(name) {
 console.log('Hello, ' + name);
};

sayHello('Vlad'); // в консоли будет Hello, Vlad

```

- name - аргумент функции sayHello

Аргументы передаем в вызов функции. Можно передевать любое кол-во аргументов.

```
function sayHello(name, lastName, age) {
 console.log('Hello, ' + name + ' ' + lastName);
 console.log('Your age - ' + age)
};

sayHello('Oleg', 'Popov', 10);

```

В приведенном примере объявляется функция sayHello(), которая принимает три аргумента - name, lastName, age.

Давайте напишем функция, которая будет выводить в консоль сумму двух аргументов.

```
function calc(a, b) {
 ...
}

```


На объявление функции распространяется действие механизма всплытия. 

Это значит, что в скрипте функция может быть вызвана до ее физического объявления.

```
printMessage('JavaScript Functions');

function printMessage(message){
  console.log(message);
}

```

Функции JavaScript также могут быть анонимными. 

Анонимной называется функция без имени. 

Такие функции используются в качестве callback-ов при обработке событий или присваиваются переменным, которые в дальнейшем передается другим функциями как параметры.

```
let sayHello = function(name) {
    console.log('Name is ' + name)
}

sayHello('Vova');

```

Функция передана в качестве аргумента:

```
let action = function() {
    console.log('action')
}

let doSomething = function(func) {
    func();
}

doSomething(action);

```

Значения для аргументов можно устанавливать по умолчанию:

```
function sayHello(name = 'Vlad') {
 console.log('Hello, ' + name);
};

sayHello(); // в консоли будет Hello, Vlad

```

Если передать аргумент, то он перетрет значение по умолчанию.

Бывают случаи, когда необходимо, чтобы функция принимала заранее неизвестное количество аргументов. 

В JavaScript это возможно сделать используя так называемые REST-аргументы.

```
function sum(...numbers) {
    console.log(numbers);
};

sum(1, 2, 3); // [1,2,3]
sum(1, 2, 3, 4, 5); // [1,2,3,4,5]

```

Обратите внимание на синтаксис описания REST-аргументов при объявлении функции: они описываются как один аргумент, но с многоточием в начале. 

Здесь многоточие является оператором расширения. 

В теле функции значением аргумента является массив со всеми переданными значениями.

Допускается одновременное использование обычных и REST-параметров.

```
function sum(a, b, ...numbers){
    console.log(a, b, numbers);
};

sum(1, 2, 3); // 1, 2, [3]
sum(1, 2, 3, 4, 5); // 1, 2, [3,4,5]

```

Также внутри каждой функции доступна для использования переменная arguments, которая содержит массив всех переданных функции параметров.

```
function someFunc1(a, b, c){
  return arguments;
}

function someFunc2(...params){
  return arguments;
}

someFunc1('Hello', 1, true); // ['Hello', 1, true]
someFunc2(1, 2, 3); // [1, 2, 3]

```

# Ключевое слово return

Ключевое слово return является оператором JavaScript, который возвращает переданное ему значение и завершает выполнение функции.

```
function getValue(a){
  return a;
}

getValue(3); // 3

```

Если не передать значение, то функция вернет undefined.

```
function someFunc(){
  return;
}

someFunc(); // undefined

```

Весь код, следующий после использования оператора return, никогда не будет выполнен.

```
function getValue(a){
  if(a > 0) {
    return a;
  } else { 
    return 0;
  }
  
  console.log('test'); // никогда не выполнится
}

getValue(3); // 3
getValue(-2); // 0

```

Зачем нужен return

```
function getValue() {
 return 1;
}

let value = getValue();
// value === 1

```

```
funtion getValue(a, b) {
 return a + b;
}

let result = getValue(1, 2);
// result === 3
```

# Стрелочные функции

Стрелочные функции JavaScript представляют собой анонимные функции с более удобным синтаксисом.

Синтаксис стрелочной функции JavaScript.

```
(параметр_1, ..., параметр_N) => {...}

```

Пример: 

```
const func = () => {
  console.log('hello');
};

func();

```

Пример с аргументами: 

```
let arrowFunc = (a, b) => {
  return a + b;
};

arrowFunc(3, 7); // 10

```

Если тело функции состоит из одной строки и при этом возвращает значение, то фигурные скобки и оператор return можно опустить.

```
let arrowFunc = (a, b) => a + b;

const result = arrowFunc(2, 6);
// result === 8

```

Тоже самое что и:

```
let arrowFunc = (a, b) => {
  return a + b
};

const result = arrowFunc(2, 6);
// result === 8

```

# Массив

Массив - это упорядоченная коллекция значений. Значения в массиве называются элементами, и каждый элемент характеризуется числовой позицией в массиве, которая называется индексом (**индексы начинаются с нуля**).

Создание массивов:

```
const arr = [];

```

Массив может хранить в себе любые значения:

```
const arr = [1, 2, 3, '4'];

```

Другой способ создания массива состоит в вызове конструктора Array():

```
const arr = new Array(5, 4, 3, 2, 1, "тест");

```

# Чтение и запись элементов массива

Доступ к элементам массива осуществляется с помощью оператора []. 

```
массив[индекс]

```

Слева от скобок должна присутствовать ссылка на массив. 

**Внутри скобок должно находиться произвольное выражение, возвращающее неотрицательное целое значение**. 

Этот синтаксис пригоден как для чтения, так и для записи значения элемента массива. 

Следовательно, допустимы все приведенные далее JavaScript-инструкции:

```
// Создать массив с одним элементом
const arr = [1, 2, 3];

// Получаем первый элемент массива
console.log(arr[0]); // 1

// Записываем новый элемент в массив
arr[3] = 'test';
console.log(arr); [1, 2, 3, 'test']

//Запись нового элемента через выражение
arr[1 + 3] = 'hello';
console.log(arr); // [1, 2, 3, 'test', 'hello']

```


# Введение в методы и свойства

Методы JavaScript это действия, которые можно выполнить с объектами.

Метод JavaScript это свойство, содержащее определение функции.


Обращение к методам объекта

```
имяОбъекта.имяМетода()

```

Пример: 

```
const person = {
 name: 'Vlad',
 sayHi: function() {
   console.log('Hello');
 }
}

person.sayHi(); // Hello

```

Так же можно использовать return и аргементы:

```
const person = {
 name: 'Vlad',
 saySomething: (a) => {
   console.log(a);
 },
 getName: function() {
   return 'Vlad';
 }
};

person.saySomething('Hello, how are you?');
const personName = person.getName(); // 'Vlad'

```

# Строка JavaScript может рассматриваться как массив символов.


```
let str = 'JavaScript';
/*
  str[0] - 'J'
  str[1] - 'a'
  str[2] - 'v'
  str[3] - 'a'
  str[4] - 'S'
  str[5] - 'c'
  str[6] - 'r'
  str[7] - 'i'
  str[8] - 'p'
  str[9] - 't'
*/

//можно обратиться к любому символу
console.log(str[4]); //'S'

//можно узнать длину строки
console.log(str.length); //10

```

# Использование встроенных методов строк

В JavaScript имеется широкий набор методов для работы со строками. 

Рассмотрим наиболее популярные из них:

- split

Метод split разбивает строку на массив по заданному разделителю

Синтаксис

```
string.split(delim)

```

Пример:

```
let names = 'Вася, Петя, Маша';

let arr = names.split(', ');

console.log(arr);  ["Вася", "Петя", "Маша"]
```

- toUpperCase() / toLowerCase() - преобразование значения в верхний/нижний регистр;

```
let greeting = 'Hello';
console.log(greeting.toUpperCase()); //HELLO
console.log(greeting.toLowerCase()); //hello

```
- indexOf() - поиск заданного значения в строке; метод возвращает индекс первого найденного значения или же -1, если совпадений нет; поиск чувствителен к регистру

```
let str = 'Welcome to WebDraftt. Learn JavaScript on WebDraftt';
console.log(str.indexOf('WebDraftt')); // 11
console.log(str.indexOf('webdraftt')); // -1

```

- slice() - возвращает часть строки по заданным начальному и конечному индексам; если передать отрицательные значения, то отсчет будет осуществляться с конца строки

```
let str = 'Hello. My name is Molly.';
console.log(str.slice(7, 9)); // My

//если второй параметр не указан, то будет возвращена часть //строки с заданного индекса и до ее конца  
console.log(str.slice(7)); // My name is Molly.
//пример с отрицательными значениями
console.log(str.slice(-6, -1)); // Molly

```

- substring() - возвращает часть строки по заданным начальному и конечному индексам, не принимает отрицательных значений

```
let str = 'Hello. My name is Molly.';
console.log(str.substring(7, 9)); // My

//если второй параметр не указан, то будет возвращена часть //строки с заданного индекса и до ее конца  
console.log(str.substring(7)); // My name is Molly.

```

- replace() - заменяет вхождения заданного значения (значение может быть регулярным выражением) на новое значение, указываемое вторым параметром

```
let str = 'Jack is my friend. Jack is a doctor.';
console.log(str.replace('Jack', 'Paul')); // Paul is my friend. Jack is a doctor

```

- trim() - удаление пробелов в начале и конце строки

```
let str = '  String with extra spaces ';
console.log(str.trim()); // 'String with extra spaces'

```


# Использование встроенных методов массива

- toString / join

Метод toString() возвращает массив в виде строки, в которой все его элементы перечислены через запятую.

```
let arr = [1, 'John', true];
alert(arr.toString()); // "1,John,true"

```

Метод join() идентичен методу toString() и отличается от него лишь тем, что с его помощью можно задать разделитель, отличный от запятой (по умолчанию).

```
let arr = [1, 'John', true];

alert(arr.join('.')); // "1.John.true"
alert(arr.join('|')); // "1|John|true"

```

- unshift / push

В JavaScript методы unshift() и push() используются для добавления в массив новых элементов: unshift() добавляет элемент в начало, push() - в конец

```
let arr = [2, 3, 4];

arr.unshift(1);
console.log(arr); // [1,2,3,4]

arr.push(5);
console.log(arr); // [1,2,3,4,5]

```

- shift / pop

Методы shift() и pop() предназначены для удаления элементов из массива: shift() удаляет первый элемент, pop() - последний. Оба метода возвращают значение удаляемого элемента.

```
let arr = [1, 2, 3];

let firstElement = arr.shift(); // 1
console.log(arr); // [2,3]

let lastElement = arr.pop(); // 3
console.log(arr); // [2]

```

- concat / slice

Объединение массивов в JavaScript осуществляется с использованием метода concat(), а извлечение части массива в новый отдельный - с использованием метода slice()

Метод concat() принимает массив и возвращает новый объединенный массив, который является результатом объединения исходного массива (применительного к которому вызывается concat()) и массива, переданного в качестве параметра. 

При этом значения исходных массивов не изменяются.

```
let arr1 = [1, 2];
let arr2 = [3, 4, 5];

let arr3 = arr1.concat(arr2);

console.log(arr1); // [1,2]
console.log(arr2); // [3,4,5]
console.log(arr3); // [1,2,3,4,5]

```

Метод slice() возвращает часть исходного массива как новый массив, не изменяя при этом исходный массив, и принимает два параметра:

- индекс элемента, который будет первым в результирующем массиве;
- индекс элемента, до которого все предыдущие элементы будут частью результирующего массива, но сам он в результирующий массив включен не будет (необязательный параметр).

```
let arr1 = [1, 2, 3, 4, 5];

let part1_of_arr1 = arr1.slice(3); // [4,5]
let part2_of_arr1 = arr1.slice(2, 4); // [3,4]

console.log(arr1); // [1, 2, 3, 4, 5]

```

- splice

Метод splice() позволяет изменять содержимое массива, добавляя в него новые элементы или же удаляя существующие. JavaScript splice() может принимать следующие параметры:

- индекс, относительно которого будет происходить изменение массива;
- количество элементов, которое необходимо удалить;
- элементы, которые необходимо добавить в массив (в виде REST-аргумента).

Возвращаемым значением splice() является массив удаленных элементов.


```
let arr = ['a', 'b', 'c', 'd', 'e'];

let deletedItems = arr.splice(1, 2); // ['b', 'c']

console.log(arr); // ['a', 'd', 'e']

```

- forEach / map

Методы для перебора элементов в массиве.
Разница между ними в том, то метод map возвращает новый массив, а forEach - нет.

```
const arr = [1, 2, 3];
arr.forEach(function(item, i, arr) {
 console.log(item); // 1, 2, 3
 console.log(i); // 0, 1, 2
 console.log(arr); // [1, 2, 3]
});

const newArray = arr.map(function(item, i, arr) {
 console.log(item); // 1, 2, 3
 console.log(i); // 0, 1, 2
 console.log(arr); // [1, 2, 3]
 return item;
});

// newArray === [1, 2, 3];

```

- filter / sort

Фильтрация и сортировка массива в JavaScript осуществляется с использованием методов filter() и sort() соответственно.

Метод filter() возвращает новый массив элементов, которые удовлетворяет заданному переданной функцией условию, применяемой к кажому элементу. 

Так, filter() принимает первым аргументом функцию, которая принимает в себя 3 аргумента - item (элемент массива), i (индекс элемента массива), arr (сам массив)

```
let arr = [3, 5, 12, 9, 23, 93, 17];

let filteredArr = arr.filter(el => el <= 10); // [3, 5, 9]

```


Давайте отфильтруем массив [1,2,3,4,5,6,7,8,9,10] и получим новый массив только из четных чисел.

```
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
...

```

Метод sort() сортирует элементы массива по возрастанию, причем порядок элементов меняется прямо в массиве.

```
let arr = ['b', 'c', 'a', 'e', 'd'];

arr.sort();
    
console.log(arr); // ['a', 'b', 'c', 'd', 'e']

```

Важно понимать, что сортировка осуществляется по символам и по значениям их кодов в таблице Unicode. 

Например, сортировка массива чисел, в котором присутствуют двузначные числа, будет осуществляться сначала по первым символам, а потом уже по вторым.

```
let arr = [3, 5, 19, 1, 7, 23, 14, 9];

arr.sort();

console.log(arr); // [1, 14, 19, 23, 3, 5, 7, 9]

```

Для сортировки именно по числовым значениям (или любым другим) методу sort() необходимо задать функцию сравнения, которая будет определять, какой из элементов больше. 

Параметрами функции сравнении передаются два элемента массива, которые необходимо сравнить в данный момент. 

Сама функция вызывается для всех пар элементов массива.


```
let arr = [3, 5, 19, 1, 7, 23, 14, 9];

arr.sort((el1, el2) => {
  if(el1 == el2) {
    return 0;
  } else if(el1 > el2) {
    return 1;
  } else if(el1 < el2) {
    return -1;
  }
});

console.log(arr); // [1, 3, 5, 7, 9, 14, 19, 23]

```

- find / some / every

Методы find(), some() и every() используются для проверки соответствия элементов массива некоторому условию и имеют одинаковый набор параметров

Методы принимают в себя функцию с 3 аргументами. Первый аргумент (item) - сам элемент массива, второй аргумент (i) - индекс элемента, arr - сам массив

```
arr.find(function(item, i, arr) { 
 ...
});

arr.some(function(item, i, arr) {
 ... 
});

```

Метод find() используется для поиска в массиве элемента, удовлетворяющего заданному критерию.

Возвращаемым результатом вызова JavaScript find() является первый попавшийся элемент, который подходит под заданное условие. Если таких элементов в массиве нет - возвращается undefined.

```
let arr = [7, 6, 3, 5, 8, 2];

let result1 = arr.find(el => el % 4 == 0); // 8
    
let result2 = arr.find(el => el > 10); // undefined

```

Метод some() позволяет узнать, имеется ли в массиве хотя бы один элемент, удовлетворяющий заданному функцией условию, и возвращает true, если такой элемент есть, и false в противном случае.

```
let arr = [7, 6, 3, 5, 8, 2];

let result1 = arr.some(el => el % 4 == 0); // true

let result2 = arr.some(el => el > 10); // false

```

Метод every() возвращает true, если все элементы массив удовлетворяют заданному критерию или false в противном случае. Критерий задается функцией.

```
let arr = [7, 6, 3, 5, 8, 2];

let result1 = arr.every(el => el % 4 == 0); // false

let result2 = arr.every(el => el < 10); // true

```

# HomeWork

HW1:

Есть массив ['Капуста', 'Репа', 'Редиска', 'Морковка']. Надо вывести в консоль строку 'Капуста | Репа | Редиска | Морковка';

HW2:

Есть строка 'Вася;Петя;Вова;Олег'. Используя стандартные методы строк получить массив их имен;

```
const newArr = .....; // ["Вася", "Петя", "Вова", "Олег"]

```

HW3:

Напишите функцию hello2(), которая при вызове будет принимать переменную (в аргументы) name (например, «Василий») и выводить строку (в нашем случае «Привет, Василий»). 

В случае отсутствующего аргумента выводить «Привет, гость»

HW4:

Есть массив ['яблоко', 'ананас', 'груша']

Привести каждый элемент массива в верхний регистр (сделать все слово большими буквами) и получить результат (новый массив) в новую переменную.

```
const fruits = ['яблоко', 'ананас', 'груша'];

const fruitsInUpperCase = ....; // тут должен быть массив, в котором ['ЯБЛОКО', 'АНАНАС', 'ГРУША']

```

HW5: 

Написать функцию addOneForAll, которая может принять **неограниченное** кол-во аргументов.

Добавить к каждому аргументу 1 и вернуть новый массив с новыми значениями.

Пример:

передал в массив такие числа - 1, 2, 3, 4

функция добавляет к каждму числу + 1

функция возвращает новый массив, в котором новые значения

```

const val = addOneForAll(1, 2, 3, 4);
console.log(val); [2, 3, 4, 5]

```

HW6:

Написать функцию getSum, которая может принять **неограниченное** кол-во аргументов и возвращает их сумму.

```
const val = getSum(1, 2, 3, 4);
console.log(val); // 10
```

HW7: 

Есть массив [1, 'hello', 2, 3, 4, '5', '6', 7, null]. Отфильтровать массив так, чтобы остались только числа. Сделать можно любым способом из того, что учили.

```
const arr = [1, 'hello', 2, 3, 4, '5', '6', 7, null];

const numberArray = ....; // [1, 2, 3, 4, 7];

```

HW8:

Написать функцию arrayTesting, которая принимает в себя любой массив (в аргументы)

функция проверяет есть ли в массиве хоть одно true значение

и если оно есть, то возвращаем из функции строку 'Нашли true значение', если его нет - 'Ничего нет'

```
const haveTrueValue = arrayTesting([0, false, null, 1]); // Нашли true значение (потому что есть хотябы одно true значение - 1)

const dontHaveTrueValue = arrayTesting([0, false, null, 0]); // Ничего нет

```

