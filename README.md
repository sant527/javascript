# javascript

# Why Can’t I Use = to Copy an Array?
https://www.samanthaming.com/tidbits/35-es6-way-to-clone-an-array/

> Because arrays in JS are reference values, so when you try to copy it using the = it will only copy the reference to the original array and not the value of the array. To create a real copy of an array, you need to copy over the value of the array under a new value variable. That way this new array does not reference to the old array address in memory.

```
const sheeps = ['🐑', '🐑', '🐑'];

const fakeSheeps = sheeps;
const cloneSheeps = [...sheeps];

console.log(sheeps === fakeSheeps);
// true --> it's pointing to the same memory space

console.log(sheeps === cloneSheeps);
// false --> it's pointing to a new memory space
```



# Best way to copy an array:
https://stackoverflow.com/a/48219524/2897115
```
let original = [
  [1, 2],
  [3, 4]
];
let cloned = JSON.parse(JSON.stringify(original)); // this will copy everything from original 
original[0][0] = -1;
console.log(cloned); // the cloned array element value changes too
console.log(original);
```
output
```
[
  [
    1,
    2
  ],
  [
    3,
    4
  ]
]
[
  [
    -1,
    2
  ],
  [
    3,
    4
  ]
]
```


# deep copy vs shallow copy and spread operator to clone an nested object
https://stackoverflow.com/a/61422332/2897115

So, for this problem, you have to understand what is the `shallow` copy and `deep` copy.

**Shallow copy** is a bit-wise copy of an object which makes a new object by copying the memory address of the original object. That is, it makes a new object by which memory addresses are the same as the original object.

**Deep copy**, copies all the fields with dynamically allocated memory. That is, every value of the copied object gets a new memory address rather than the original object.

***Now, what a spread operator does? It deep copies the data if it is not nested. For nested data, it deeply copies the topmost data and shallow copies of the nested data.***

In your example,

    const oldObj = {a: {b: 10}};
    const newObj = {...oldObj};
It deep copy the top level data, i.e. it gives the property `a`, a new memory address, but it shallow copy the nested object i.e. `{b: 10}` which is still now referring to the original `oldObj`'s memory location.

If you don't believe me check the example,

<!-- begin snippet: js hide: false console: true babel: false -->

<!-- language: lang-js -->

    const oldObj = {a: {b: 10}, c: 2};
    const newObj = {...oldObj};

    oldObj.a.b = 2; // It also changes the newObj `b` value as `newObj` and `oldObj`'s `b` property allocates the same memory address.
    oldObj.c = 5; // It changes the oldObj `c` but untouched at the newObj



    console.log('oldObj:', oldObj);
    console.log('newObj:', newObj);

<!-- language: lang-css -->

    .as-console-wrapper {min-height: 100%!important; top: 0;}

<!-- end snippet -->

You see the `c` property at the `newObj` is untouched.

### How do I deep copy an object.
There are several ways I think. A common and popular way is to use `JSON.stringify()` and `JSON.parse()`.

<!-- begin snippet: js hide: false console: true babel: false -->

<!-- language: lang-js -->

    const oldObj = {a: {b: 10}, c: 2};
    const newObj = JSON.parse(JSON.stringify(oldObj));

    oldObj.a.b = 3;
    oldObj.c = 4;

    console.log('oldObj', oldObj);
    console.log('newObj', newObj);

<!-- language: lang-css -->

    .as-console-wrapper {min-height: 100%!important; top: 0;}

<!-- end snippet -->

Now, the `newObj` has completely new memory address and any changes on `oldObj` don't affect the `newObj`.

Another approach is to assign the `oldObj` properties one by one into `newObj`'s newly assigned properties.

<!-- begin snippet: js hide: false console: true babel: false -->

<!-- language: lang-js -->

    const oldObj = {a: {b: 10}, c: 2};
    const newObj = {a: {b: oldObj.a.b}, c: oldObj.c};

    oldObj.a.b = 3;
    oldObj.c = 4;

    console.log('oldObj:', oldObj);
    console.log('newObj:', newObj);

<!-- language: lang-css -->

    .as-console-wrapper {min-height: 100%!important; top: 0;} 

<!-- end snippet -->

There are some libraries available for deep-copy. You can use them too.
