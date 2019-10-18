## JS Tips and Tricks WIP

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
A trick that can normalize fetched data  

```javascript
 var data = { 
    title: "",
    name : 'Jhon',
    surname:'Doe',
    age : 34,
    addr1 : undefined,
    addr2 : null,
    
 }
 
 var arr = [ data.title, data.name, data.surname, data.age, data.addr1, data.addr2 ] ;
 
 //Now  here is the trick.this will filter all  garbage data like undefined or null or false and return rest
  arr.filter(Boolean).join(', '); 
  
 //result  "Jhon, Doe, 34"

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
