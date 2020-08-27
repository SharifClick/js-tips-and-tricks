## JS Tips and Tricks WIP


        
Check if date in past
```javascript
    let is_date_in_past = (date) => (+(new Date()) - 86400000) > +(new Date(date));
```

Check if year is leap year 
```javascript
    let isLeapYear = (year) => !(year&3||year&15&&!(year%25))
```


Another way of IIFE
```javascript
    !function(){console.log('hi')}()
```
Cast a list of primitive values to a different type
```javascript

    const naiveList = ['1500', '1350', '4580'];
    const castedList = naiveList.map(Number);
    // castedList = [1500, 1350, 4580]
```

Checking 'falsy' props much easier than u think :D
```javascript
    // In some cases you need to check if prop has a 'falsy' value, then your code maybe....
    
    if(value != 'undefined' || value != null || value != '' || value != NaN || value != 0){
        // do something
    }
   
   // you can do same thing..
   
   if(!!value){
        // do something
    }
```
A tricky way to normalize fetched data  

```javascript

 var data = { 
    title : '',
    name : 'Jhon Doe',
    present_addr : '1255 Heron Way',
    permanent_addr : undefined,
    mobile: '503-804-1602',
    phone : null
  }
  
  //this will filter all garbage data like undefined or null or false and return rest
  
  let details = [...Object.values(data)].filter(Boolean).join(', ')

  //now `details` contains "Jhon Doe, 1255 Heron Way, 503-804-1602"

 ```
 
 Some essential functions for Good Practice   
 ###### Debounce
```html

<!--Dbouncing means delaying any function to run until the right time come -->
<!--usages: any frequently fired event  like resize, scroll, key-->

<!--Returns a function, that, as long as it continues to be invoked, will not-->
<!--be triggered. The function will be called after it stops being called for-->
<html>  
<body> 
<button id="debounce"> 
    Debounce 
</button> 
<script> 
var button = document.getElementById("debounce"); 
const debounce = (func, delay) => { 
    let debounceTimer 
    return function() { 
      const context = this
      const args = arguments 
      clearTimeout(debounceTimer) 
      debounceTimer = setTimeout(() => func.apply(context, args), delay) 
    } 
}  
button.addEventListener('click', debounce(function() { 
        alert("Hello\nNo matter how many times you" + 
            "click the debounce button, I get " + 
            "executed once every 3 seconds!!") 
                        }, 3000)); 
</script> 
</body> 
</html> 

 ```
 
 ###### getAbsoluteUrl
```javascript

//A good trick for getting an absolute URL from a variable string
var getAbsoluteUrl = (function() {
    var a;
    return function(url) {
        if (!a) a = document.createElement('a');
        a.href = url;
        return a.href;
    };
})();
// Usage
getAbsoluteUrl('/something'); // https://jhondoe.name/something

```

###### insertRule
```javascript
//it enables you to change the stylesheet halfway down the line, 
//which means you don’t have to go back and customize the entire stylesheet.

var style = (function() {
    // Create the <style> tag
    var style = document.createElement("style");

    // WebKit hack
    style.appendChild(document.createTextNode(""));

    // Add the <style> element to the page
    document.head.appendChild(style);
  
    console.log(style.sheet.cssRules); // length is 0, and no rules

    return style;
})();
style.sheet.insertRule('.foo{color:red;}', 0);
console.log(style.sheet.cssRules); // length is 1, rule added

```

###### once
```javascript
// it only runs one time for a page, 
//this avoid duplicating initializations

function once(fn, context) {
    var result;
    return function() {
        if (fn) {
            result = fn.apply(context || this, arguments);
            fn = null;
        }
        return result;
    };
}
// Usage
// it is used while calling a function 
//not when defining a function
var canOnlyFireOnce = once(function() {
    console.log('Fired!');
});
canOnlyFireOnce(); // "Fired!"
canOnlyFireOnce(); // nada

```
###### poll
```javascript
// it allows developers to periodically “pol” the server 
//and request new information like new emails, messages, etc.

// The polling function
function poll(fn, timeout, interval) {
    var endTime = Number(new Date()) + (timeout || 2000);
    interval = interval || 100;
    var checkCondition = function(resolve, reject) {
        // If the condition is met, we're done!
        var result = fn();
        if (result) {
            resolve(result);
        }
        // If the condition isn't met but the timeout hasn't elapsed, go again
        else if (Number(new Date()) < endTime) {
            setTimeout(checkCondition, interval, resolve, reject);
        }
        // Didn't match and too much time, reject!
        else {
            reject(new Error('timed out for ' + fn + ': ' + arguments));
        }
    };
    return new Promise(checkCondition);
}
// Usage: ensure element is visible
poll(function() {
    return document.getElementById('lightbox').offsetWidth > 0;
}, 2000, 150).then(function() {
    // Polling done, now do something else!
}).catch(function() {
    // Polling timed out, handle the error!
});
```

###### matchesSelector
```javascript

//We can validate our element before move ahead

    function matchesSelector(el, selector) {
    var p = Element.prototype;
    var f = p.matches || p.webkitMatchesSelector || p.mozMatchesSelector || p.msMatchesSelector || function(s) {
        return [].indexOf.call(document.querySelectorAll(s), this) !== -1;
    };
    return f.call(el, selector);
}
// Usage
matchesSelector(document.getElementById('myDiv'), 'div.someSelector[some-attribut]')
```
###### get x,y coordinate to degree
```javascript

  function getDegrees(x, y, width, height){
    let deltaX = (width / 2) - x;
    let deltaY = (height / 2) - y;
    let deg  = Math.atan2(deltaY, deltaX) * (180 / Math.PI) + 270;
    return deg % 360;
  }
```

###### get moving average of a given array 
```javascript

  function arr_sum(arr) {
    var len = arr.length;
    var num = 0;
    while (len--) num += Number(arr[len]);
    return num;
  }
  function avg(arr, idx, range) {
    return arr_sum(arr.slice(idx - range, idx)) / range;
  }
  function toFixed(n) {
    return n.toFixed(2);
  }
  function simple_rolling_average(arr, range) {
    var num = range || arr.length;
    var res = [];
    var len = arr.length + 1;
    var idx = num - 1;
    while (++idx < len) {
      res.push(toFixed(avg(arr, idx, num)));
      //res.push(avg(arr, idx, num));
    }
    return res;
  }
  let items = [...Array(101).keys()];
  simple_rolling_average(items, 5)
  
```

~~Think twice~~ Think thrice before work with numbers!


```javascript

    // When converting something to integer
    // the thriller begins :p


    +""          // 0
    +"5"         // 5
    +"5.00"      // 5
    +"5.00r"     // NaN


    Number("")         // 0
    Number("5")        // 5
    Number("5.00")     // 5
    Number("5.00r")    // NaN
    Number("str")   // NaN


    parseInt("")       // NaN
    parseInt("5")      // 5
    parseInt("5.00")   // 5
    parseInt("5.00r")  // 5
    parseInt("str0")   // NaN

    Number.parseInt("")         // NaN
    Number.parseInt("5")        // 5
    Number.parseInt("5.00")     // 5
    Number.parseInt("5.00r")    // 5
    Number.parseInt("str5")   // NaN



    +2.345                   //2.345
    Number(2.345)            //2.345
    parseInt(2.345)          //2
    Number.parseInt(2.345)   //2

    +(-1)                 //-1
    Number(-1)            //-1
    parseInt(-1)          //-1
    Number.parseInt(-1)   //-1


    +(-0)                  //-0
    Number(-0)             //-0
    parseInt(-0)           //0
    Number.parseInt(-0)    //0



    +NaN         //NaN
    +undefined   //NaN
    +null        //0
    +true        //1
    +false       //0

    Number(NaN)          //NaN
    Number(undefined)    //NaN
    Number(null)         //0
    Number(true)         //1
    Number(false)        //0

    parseInt(NaN)         //NaN
    parseInt(undefined)   //NaN
    parseInt(null)        //NaN
    parseInt(true)        //NaN
    parseInt(false)       //NaN

    Number.parseInt(NaN)        //NaN
    Number.parseInt(undefined)  //NaN
    Number.parseInt(null)       //NaN
    Number.parseInt(true)       //NaN
    Number.parseInt(false)      //NaN



    +[]                   //0
    Number([])            //0
    parseInt([])          //NaN
    Number.parseInt([])   //NaN

    +['somevalue'])                    //NaN
    Number(['somevalue']))             //NaN
    parseInt(['somevalue']))           //NaN
    Number.parseInt(['somevalue']))    //NaN


    +{}                   //NaN
    Number({})            //NaN
    parseInt({})          //NaN
    Number.parseInt({})   //NaN

    +{value:'somevalue'}                   //NaN
    Number({value:'somevalue'})            //NaN
    parseInt({value:'somevalue'})          //NaN
    Number.parseInt({value:'somevalue'})   //NaN



```
