# JavaScript Quiz :telescope: 

I have compiled some basic JavaScript language questions so developers can test their knowledge, and also learn some nuances of the language from the detailed answers to each question.

The repository is open to contributions, whether new questions or even translations of answers into other languages.

## A few notes about the code
- Assuming ECMAScript 6
- Every snippet is run as a global code
- The code is not running in strict mode.

### 1. What is the result of:

```javascript
('b'+'a'+ +'a'+'a').toLocaleLowerCase()
```

**a)** `SyntaxError`</br>
**b)** `'baaa'`</br>
**c)** `'banana'`</br>
**d)** `'baa'`</br>

### 2. What is the result of:

```javascript
let num = '10'

num += num++
```

**a)** `'1010'`</br>
**b)** `NaN`</br>
**c)** `'1011'`</br>
**d)** `21`</br>

### 3. What is the result of:

```javascript
(function () {
  let num1 = 10
  let num2 = 20

  return sum()

  function sum() {
    return num1 + num2
  }
})()
```

**a)** `undefined`</br>
**b)** `NaN`</br>
**c)** `30`</br>
**d)** `ReferenceError`</br>

### 4. What will be the output value of log 1 and log 2?

```javascript
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}

let person = Person('Guilherme', 'Moura')

console.log('log 1: ', person)
console.log('log 2: ', firstName, lastName)
```

**a)** `log 1: Guilherme Moura | ReferenceError`</br>
**b)** `log 1: undefined | log 2: Guilherme Moura`</br>
**c)** `log 1: { firstName: 'Guilherme', lastName: 'Moura'  } | log 2: Guilherme Moura`</br>
**d)** `log 1: { firstName: 'Guilherme', lastName: 'Moura'  } | ReferenceError`</br>

### 5. What is the result of:

```javascript
(3,5 - 3) * 2
```

**a)** `0.999999`</br>
**b)** `0,5`</br>
**c)** `1`</br>
**d)** `4`</br>

### 6. What is the value of the object?

```javascript
let card = {};

(['audi','bmw'].map(item => card[item] = undefined), card);
```

**a)** `{}`</br>
**b)** `{ audi: undefined, bmw: undefined }`</br>
**c)** `['audi','bmw']`</br>
**d)** `[{ audi: undefined }, { bmw: undefined }]`</br>


### 7. What is the result of:

```javascript
Object.prototype.toString.call([1, 2, 3, 4, 5])
```

**a)** `'[object Array]'`</br>
**b)** `'1,2,3,4,5'`</br>
**c)** `undefined`</br>
**d)** `'[object Object]'`</br>

## Check the detailed answers to each question :pencil:

- <a href="https://github.com/mouraggui/js-quiz/blob/master/answers/pt-br.md" target="_blank">pt-BR</a>
- <a href="https://github.com/mouraggui/js-quiz/blob/master/answers/en-us.md" target="_blank">en-US</a>
