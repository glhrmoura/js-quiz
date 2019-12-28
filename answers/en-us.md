# Answers

### Question 1:

```javascript
('b'+'a'+ +'a'+'a').toLocaleLowerCase()
```

**c) `'banana'`**

When we have this expression `'b'+'a'+ +'a'+'a'` it looks like it will be returned `'baaa'` because we are only concatenating strings, but there is one detail that changes everything:

```'a'+ +'a'```

The `+` operator can appear in prefixed unary form or in binary form:

Unary: `+x` <br>
Binary: `x + y`

In unary form it will try to convert the operand to a number:

```+'1' === 1```

In binary form it will add the two operands, if both are numbers, `1 + 3 // result 4`, and if either operand is of string type, it will convert the other operand to string and then concatenate them:

```'1' + 3 // result '13'```

In the case of the snippet `'a'+ +'a'`, both unary operation and binary operation are being performed.

First it will perform the unary operation that has the highest precedence `+'a'`, so it will try to convert the string `'a'` to a number, resulting in the value `NaN`, because the string `'a'` is not a numeric value valid.

Right after that, it will perform the binary operation `'a' + NaN`, in this case as one of the operands is a string it will try to convert the other operand to a string, in this case the value `NaN`, which will result in a string `'NaN'`.

When we execute:

```'b'+'a'+ +'a'+'a'```

This is what really happens:

```'b'+'a'+'NaN'+'a'```

Finally, just use the `toLocaleLowerCase` function to make everything lowercase, which will result in the final string `'banana'`.

### Question 2:

```javascript
let num = '10'

num += num++
```

**a) `'1010'`**

At first we have a common assignment `let num = '10'`. So far so normal, we just create a `num` variable and assign it the string `'10'`. Following in the code we have:

```num += num++```

Here we have two operations, a unary post increment operation, and a binary assignment operation with addition, which will define our end result will be the precedence of these operators.

The postincrement operator has a precedence of `17`, while the assignment operator with addition has a precedence of `3`, here, the higher the precedence value, the better. So our post increment operator wins.

First our post increment operator will try to increment `1` to our variable `num++`. But this variable is a string, so he tries to convert it to a numeric value before adding more `1` to its value, in which case we get the numeric value `10`. Right after doing this it will increment `1` to this value, generating the numeric value `11`, and finally the operator would return the value `11`, right? Wrong!

The post increment operation, unlike the pre increment operation, returns the initial value of the operand, in this case our string `'10'`.

At this point the post increment operation has already been completed, and we only have the assignment operation with addition. This operation will first use the addition operator and then the assignment operator:

```num += num++```

Since the value returned from the `num++` operation was `'10'`, our addition assignment operator will try to add this value to the value of the `num` variable which is also `'10'`, resulting in string concatenation `'1010'`;

### Question 3:

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

**c) `30`**

In this question we have a classic case of <a href="https://developer.mozilla.org/en-US/docs/Glossary/Hoisting" target="_blank">Hoisting</a>, that the process where the JS interpreter takes all <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function" target="_blank">function declarations</a> and variables declared with `var`, and throw to the top of the scope (the global scope or function).

In the case of variables, they receive the value undefined, and continue to do so until explicitly given a value. The <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function" target="_blank">function declarations</a> behave differently and raise the function name to the top of the scope, the interpreter will also raise his definition, so we can call it before his statement.

This is exactly what happens in the code in question. When the <a href="https://developer.mozilla.org/en-US/docs/Glossary/IIFE" target="_blank">IIFE</a> is executed the JS interpreter takes all variables declared with `var` and all <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function" target="_blank">function declarations</a>, and elevates to the top of the scope of the function. Our <a href="https://developer.mozilla.org/en-US/docs/Glossary/IIFE" target="_blank">IIFE</a> will look like this:

```javascript
(function () {
  let num1 = 10
  let num2 = 20
  function sum() {
    return num1 + num2
  }

  return sum()
})()
```

Thus, when the `sum` function is executed even before its declaration, it returns the sum of the values ​​of the `num1` and `num2` variables.

### Question 4:

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}

let person = Person('Guilherme', 'Moura')

console.log('log 1:', person)
console.log('log 2:', firstName, lastName)
```

**b) `log 1: undefined | log 2: Guilherme Moura`**

Here we have a `Person` constructor function, which creates an object with the properties `firstName` and `lastName`. After this we have the following line:

`let person = Person('Guilherme', 'Moura')`

If we do not pay attention, we may find that this line is creating an object with the properties `firstName = 'Guilherme'` and `lastName = 'Moura'`, and that this object is being assigned the variable `person`. But if we look closely at this operation, the constructor function is being called without the `new` keyword, and this is where things change.

Because it is not being called with the `new` keyword, the constructor function is executing normally, returning an `undefined` value by default. It is this value that is being assigned to the `person` variable.

Well, that answers why the `person` variable is `undefined`, but why are the `firstName` and `lastName` variables accessible outside the function?

In common function calls, the `this` variable within it will always point to the `window` global object. This is only true when the code is executed with `strict mode`, in which case `this` within the functions will be `undefined`.

Since `strict mode` is not being used, `this` within the function is pointing to `window`. This is what really happens when the `Person` function is executed:

```javascript
function Person(firstName, lastName) {
  window.firstName = firstName
  window.lastName = lastName
}

// ...
```
Two global variables are created: `firstName` and `lastName`. They can be accessed anywhere in the code.

### Question 5:

```javascript
(3,5 - 3) * 2
```

**d) `4`**

Looking quickly we can think that the operation is taking `3.5` subtracting `3` and multiplying by `2`. But if we look closely, what separates `3` from `5` is not a floating point, but the <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comma_Operator" target="_blank">comma operator</a>. This operator evaluates all its operands and returns the last of them.

Here there are two operands: the number `3` and the result of the `5 - 3` subtraction operation. The value `2` will be returned, which is the result of the subtraction. This value will be multiplied by `2` giving the value `4` as the final result.

### Question 6:

```javascript
let card = {};

(['audi','bmw'].map(item => card[item] = undefined), card);
```

**b)** `{ audi: undefined, bmw: undefined }`</br>

One of the most common ways to dynamically assign a javascript object is through square bracket notation, so you can dynamically insert value into an attribute until you create an attribute that does not already exist in the object.

In this case even identifying the value as undefined, the object will mark these attributes as proper to the object and will be explicitly displayed on the object. This is quite common when we want the object to have a set of properties.

### Question 7:

```javascript
Object.prototype.toString.call([1, 2, 3, 4, 5])
```

**a)** `[object Array]`</br>

Letter **b**! If you didn't get this question right then that was probably your answer, assuming that if the `toString` method is running with `this` pointing to an `Array`, it will have the same behavior as the call to `[1 , 2, 3, 4, 5].toString() // '1,2,3,4,5`. However, despite the methods <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/ToString">`Object.prototype.toString`</a> and <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toString">`Array.prototype.toString`</a> have the same nomenclature, underneath the cloth they have very distinct behaviors.

Both <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toString">`Array.prototype.toString`</a> and <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/ToString">`Object.prototype.toString`</a> follow these steps when they are executed. I will list the steps in a more abstract way (more abstract than the specification), and using JS own syntax to simplify things.

`Array.prototype.toString`:

- The `array` variable will receive the value of `this` converted to object.
- The variable `func` will get the value of `array ['join'] `
- Here it is checked if `func` is a function
  - If not, the `func` variable will get the value of` Object.prototype.toString`
- Will be returned `func.call (array)`

`Object.prototype.toString`:

> I will skip a few steps so that we can focus only on what is most important to this case.

- The `obj` variable will receive the value of `this` converted to object.
- Here a series of checks will be made regarding the type of the variable `obj`
  - If the variable `obj` is of type` Array`, the variable `tag` will receive `'Array'`
  - If not, if the `obj` variable is a function, the` tag` variable will receive `'Function'`
  - If not, if the `obj` variable is an error, the` tag` variable will receive `'Error'`
  - If not, if the `obj` variable is a Boolean, the` tag` variable will receive `'Boolean'`
  - If not, if the `obj` variable is a numeric value, the` tag` variable will receive `'Number'`
  - If not, if the `obj` variable is a string, the` tag` variable will receive `'String'`
  - If not, if the `obj` variable is a value of type date, the` tag` variable will receive `'Date'`
  - If not, if the `obj` variable is a regular expression, the` tag` variable will receive `'RegExp'`
  - If not, the `tag` variable will receive the value `'Object'`
- After all checks, the value `'[object' + tag + ']'` will be returned

In the example from 7th question, the `Object.prototype.toString` method is executed with a` this` of type `Array`, so it fell on the first `If` and assigned the value `Array` to the variable `tag` . Finally, it returned the string `'[object' + tag + ']'`.

### Question 8:

```javascript
let john = { name: 'John' }
let array = [john]

john = null

array[0].name
```
**c)** `'John'`</br>

When we instantiate an object and assign it to a variable, in fact what will be stored in that variable is not an object in itself, but a reference to an object just created in memory.

```javascript
let obj = {}

// internal behavior
let obj = '89E3N9RJ43J0' // abstract code representing the address of the object in memory
```

If we assign the `obj` variable to another variable, we are actually creating a new reference for the object in memory.

```javascript
let obj2 = obj

// internal behavior
let obj2 = '89E3N9RJ43J0'
```

When this code snippet is executed `let array = [john]`, we are not literally taking the `john` variable and throwing it into the array, but rather storing a new reference inside the array.

```javascript
let array = [john]

// internal behavior
let array = ['89E3N9RJ43J0']
```

We can imagine each reference to an object in memory as a direct communication channel with it. When we want to extract or add some information from this object, we will contact it through some of its direct communication channels (references).

The code snippet `john = null` is overriding the `john` variable, in other words, it is cutting a communication channel with that object, but it is not changing the object itself. If there are no more references to the object, it will be removed from memory by the Garbage collection.

But we still have a reference to this object in index `0` of our array. Then we can either access it directly, or access one of its properties through this reference.

```javascript
array[0] // { name: 'John' }
array[0].name // 'John'
```