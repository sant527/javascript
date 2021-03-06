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
let cloned = c; // this will copy everything from original 
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


# when to use spread operator {...orignal}/\[...original\] instead of JSON.parse(JSON.stringify(original))

```js
          axios.get("https://someapi/rest/",{
              params: {
                ...params_general,
                ...params_date
              }
            })
            .then((response) => {
              console.log(response)

              //There are two ways to copy 
              // Method 1
              const resp1 = {...response})
              //Use this when we are not changing any nested objects OR even we
              //change nested objects we dont need the response variable anymore
              //in the code
              // Eg:
              resp1["data"]["photos"] = ""
              console.log(response.["data"]["photos"])
              // this will show ""

              
              // Method 2
              const resp2 = setResponse(JSON.parse(JSON.stringify(response))) 
              //Use this when we change any nested objects like 
               // and also want the response object not to change
              // since we want to use again the response 
              //somewhere in the code
              // Eg:
              resp2["data"]["photos"] = ""
              console.log(response.["data"]["photos"])
              // this will not show ""

              
            })
            .catch((error) => {
              console.log(error);
            });
        }
```


# javascript sort function

https://www.freecodecamp.org/news/javascript-array-sort-tutorial-how-to-use-js-sort-methods-with-code-examples/

When we use the sort( ) method, elements will be sorted in ascending order (A to Z) by default:

```
const teams = ['Real Madrid', 'Manchester Utd', 'Bayern Munich', 'Juventus'];

teams.sort(); 
// ['Bayern Munich', 'Juventus', 'Manchester Utd', 'Real Madrid']

teams.reverse();
// ['Real Madrid', 'Manchester Utd', 'Juventus', 'Bayern Munich']
```

It also sorts numbers alphabetically instead of numerically

```
const numbers = [3, 23, 12];

numbers.sort(); // --> 12, 23, 3
```
like 1, 2, 3 as alphabetical order

## Sort number using the compare function

```
function(a, b) {return a - b}
```

The sort method, fortunately, can sort negative, zero, and positive values in the correct order. When the sort( ) method compares two values, it sends the values to our compare function and sorts the values according to the returned value.

- If the result is negative, a is sorted before b.
- If the result is positive, b is sorted before a.
- If the result is 0, nothing changes.

All we need to is using the compare function inside the sort( ) method:

```
const numbers = [3, 23, 12];
numbers.sort(function(a, b){return a - b}); // --> 3, 12, 23
```

If we want to sort the numbers in descending order, this time we need to subtract the second parameter (b) from the first one (a):

```
const numbers = [3, 23, 12]
numbers.sort(function(a, b){return b - a}); // --> 23, 12, 3
```


# javscript filter

https://www.digitalocean.com/community/tutorials/js-filter-array-method

The `filter()` Array method creates a new array with elements that fall under a given criteria from an existing array:

```
var numbers = [1, 3, 6, 8, 11];

var lucky = numbers.filter(function(number) {
  return number > 7;
});

// [ 8, 11 ]
```

The example above takes the `numbers` array and returns a new filtered array with only those values that are greater than seven.

## Filter syntax
```
var newArray = array.filter(function(item) {
  return condition;
});
``` 

The item argument is a reference to the current element in the array as filter() checks it against the condition. This is useful for accessing properties, in the case of objects.


# findIndex

The arr.findIndex() method used to return the index of the first element in a given array that satisfies the provided testing function. Otherwise, -1 is returned.

It does not execute the method once it finds an element satisfying the testing method.
It does not change the original array.

```js
<script> 
    // input array contain some elements. 
    var array = [-10, -0.20, 0.30, -40, -50]; 
  
    // function (return element > 0). 
    var found = array.findIndex(function (element) { 
        return element > 0; 
    }); 
  
    // Printing desired values. 
    document.write(found); 
</script>
```

```
Output:
2
```
If the current item passes the condition, it gets sent to the new array.



# boostrap4 toast disappears very quickly even after setting data-delay=5000
https://stackoverflow.com/a/426276

https://github.com/twbs/bootstrap/issues/28164#issuecomment-560694369

                    $("#liveAlertToast").toast('dispose')
                    $("#liveAlertToastContent").html(err.message);
                    $("#liveAlertToast").toast('show')




# prop vs attr vs dom checkbox

Modern jQuery
----

Use [`.prop()`][1]:

    $('.myCheckbox').prop('checked', true);
    $('.myCheckbox').prop('checked', false);

DOM API
----

If you're working with just one element, you can always just access the underlying [`HTMLInputElement`][2] and modify its [`.checked`][3] property:

    $('.myCheckbox')[0].checked = true;
    $('.myCheckbox')[0].checked = false;

The benefit to using the `.prop()` and `.attr()` methods instead of this is that they will operate on all matched elements.

jQuery 1.5.x and below
----

The `.prop()` method is not available, so you need to use [`.attr()`][4].

    $('.myCheckbox').attr('checked', true);
    $('.myCheckbox').attr('checked', false);

Note that this is [the approach used by jQuery's unit tests prior to version 1.6][5] and is preferable to using `$('.myCheckbox').removeAttr('checked');` since the latter will, if the box was initially checked, change the behaviour of a call to [`.reset()`][6] on any form that contains it – a subtle but probably unwelcome behaviour change.

For more context, some incomplete discussion of the changes to the handling of the `checked` attribute/property in the transition from 1.5.x to 1.6 can be found in the [version 1.6 release notes][7] and the **Attributes vs. Properties** section of the [`.prop()` documentation][1].


  [1]: https://api.jquery.com/prop
  [2]: https://developer.mozilla.org/en/docs/Web/API/HTMLInputElement
  [3]: https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement#Properties_checkbox_radio
  [4]: https://api.jquery.com/attr
  [5]: https://github.com/jquery/jquery/blob/1.5.2/test/unit/attributes.js#L157
  [6]: https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement.reset
  [7]: https://blog.jquery.com/2011/05/03/jquery-16-released/


# check if checkbox is checked or not

            if($(this).prop("checked") == true){

                alert("you checked checkbox.");

            }else if($(this).prop("checked") == false){

                alert("you unchecked checkbox.");

            }
            
 OR


```
       if ($(this).is(':checked')){
           msg = "{{__('The set status will be Active again! Are you sure?')}}"
           a = confirm(msg);
           if (a){
               return true;
           }
           $(this).prop('checked',false)
       }
       else{
           msg = "{{__('The set will be Inactive! Are you sure?')}}"
           a = confirm(msg);
            if (a){
               return true;
           }
           $(this).prop('checked',true);
       }
```
 
 
 https://stackoverflow.com/questions/16211700/jquery-checkbox-prop-click-preventdefault-behaviour
 
 # how to to decide if a checkbox could be checked/unchecked according to other fields.
 
 Checkboxes are a special case where the `change` event and `click` can replace each-other since the `change event` is also fired by clicking on the checkbox.

That said the only big difference between `click` and `change` is the usability of `.preventDefault()`. The change event is better in cases where the value of the checkbox is being changed using any other methods than clicking.

In this case you choose whichever you prefer. An example would be: [*Fiddle here*](http://jsfiddle.net/Spokey/HPUyv/9/)


    $('input[type="checkbox"]').on('change', function () {
        var ch = $(this), c;
        if (ch.is(':checked')) {
            ch.prop('checked', false);
            c = confirm('Do you?');
            if (c) {
                ch.prop('checked', true);
            }
        } else {
            ch.prop('checked', true);
            c = confirm('Are you sure?');
            if (c) {
                ch.prop('checked', false);
            }
        }
    });


# 'Checkbox change' results in infinite loop

https://stackoverflow.com/a/46886548

Disable bootstrapToggle and change input, then enable it again.

```
$(".chk").change(function(event) {
  var id = this.id;
  var status = !this.checked;
  $('input:checkbox:not("#' + id + '")').each(function(){
    $(this).bootstrapToggle('destroy');
    $(this).prop('checked', status);
    $(this).bootstrapToggle();
  });
});
```
