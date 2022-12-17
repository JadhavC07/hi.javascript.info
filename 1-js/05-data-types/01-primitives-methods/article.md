# प्राथमिक (primitives) के तरीके

JavaScript हमें (strings, numbers, आदि.) के साथ काम करने की अनुमति देता है , जैसे कि वे वस्तु(objects) हों. वे call(वापस) करने के तरीके भी प्रदान करते हैं। हम जल्द ही इनका अध्ययन करेंगे , लेकिन पहले हम देखेंगे कि यह कैसे काम करता है क्योंकि, बिल्कुल , प्राथमिक (primitives) वस्तु(objects) नहीं है (और यहां हम इसे और भी स्पष्ट करेंगे).


आइए प्राथमिक(primitives) और वस्तु(objects) के बीच मुख्य अंतर देखें .

एक प्राथमिक (primitives)

- एक प्राथमिक(primitives) प्रकार का एक मूल्य है
- प्राथमिक (primitives) के कुल 7 प्रकार हैं: `string`, `number`, `bigint`, `boolean`, `symbol`, `null` और `undefined`.

एक वस्तु(objects)

- गुणों के रूप में एकाधिक मानों(values) को संग्रहीत करने में सक्षम है। 
- `{}` से बनाया जा सकता है, उदाहरण के लिए: `{name: "John", age: 30}`. JavaScript में अन्य प्रकार की वस्तु(objects) भी हैं: functions, for example, are objects. कार्य (functions), उदाहरण के लिए, वस्तु(objects) हैं।

One of the best things about objects is that we can store a function as one of its properties.
वस्तु(objects) के बारे में सबसे अच्छी चीजों में से एक यह है कि हम किसी कार्य (functions) को उसके गुणों(properties) में से एक के रूप में संग्रहीत कर सकते हैं।

```js run
let john = {
  name: "John",
  sayHi: function() {
    alert("Hi buddy!");
  }
};

john.sayHi(); // Hi buddy!
```

तो यहाँ हमने एक `john` वस्तु(objects)  बनाई है , `sayHi` तरीका(method) के साथ.

कई में निर्मित वस्तु(objects) पहले से मौजूद हैं, जैसे वे जो दिनांक (dates), त्रुटियों (errors), HTML तत्वों(elements) आदि. के साथ काम करते हैं। उनके अलग-अलग गुण(properties) और विधियाँ(methods) हैं।

लेकिन, इन सुविधाओं की कीमत चुकानी पड़ती है!

वस्तु(objects) प्राथमिक(primitives) की तुलना में "भारी" हैं। उन्हें आंतरिक तंत्र को सहारा देने के लिए अतिरिक्त संसाधनों की आवश्यकता होती है।

## एक प्राथमिक(primitives) वस्तु(objects) के रूप में

यहां JavaScript निर्माता द्वारा सामना किए गए विरोधाभास(paradox) हैं

- ऐसी कई चीजें हैं जो एक प्राथमिक(primitives) के साथ एक string या एक number की तरह करना चाहती हैं। तरीकों(methods) के रूप में उन्हें access(प्रवेश) करना बहुत अच्छा होगा।

- प्राथमिक(primitives) जितना संभव हो उतना तेज़ और हल्का होना चाहिए।

समाधान थोड़ा अजीब लग रहा है, लेकिन यहाँ यह है:

1. प्राराराथमिक(primitives) अभी भी प्राथमिक(primitives) हैं। एक मान,इच्छानुसार।
2. भाषा स्ट्रिंग्स (string), संख्याओं(number), बूलियन्स(booleans) और चिह्न(symbol) के तरीकों(methods) और गुणों(properties) तक पहुंच की अनुमति देती है।
3. इसके काम करने के लिए, एक विशेष "ऑब्जेक्ट रैपर(object wrapper)" बनाया जाता है जो अतिरिक्त कार्यक्षमता प्रदान करता है और फिर नष्ट हो जाता है।

"ऑब्जेक्ट रैपर" प्रत्येक प्राथमिक(primitives) प्रकार के लिए अलग-अलग होते हैं और इन्हें कहा जाता है: `स्ट्रिंग`, `संख्या`, `बूलियन` और `सिंबल`। इस प्रकार, वे तरीकों के अलग-अलग सेट प्रदान करते हैं।

For instance, there exists a string method [str.toUpperCase()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) that returns a capitalized `str`.

Here's how it works:

```js run
let str = "Hello";

alert( str.toUpperCase() ); // HELLO
```

Simple, right? Here's what actually happens in `str.toUpperCase()`:

1. The string `str` is a primitive. So in the moment of accessing its property, a special object is created that knows the value of the string, and has useful methods, like `toUpperCase()`.
2. That method runs and returns a new string (shown by `alert`).
3. The special object is destroyed, leaving the primitive `str` alone.

So primitives can provide methods, but they still remain lightweight.

The JavaScript engine highly optimizes this process. It may even skip the creation of the extra object at all. But it must still adhere to the specification and behave as if it creates one.

A number has methods of its own, for instance, [toFixed(n)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) rounds the number to the given precision:

```js run
let n = 1.23456;

alert( n.toFixed(2) ); // 1.23
```

We'll see more specific methods in chapters <info:number> and <info:string>.


````warn header="Constructors `String/Number/Boolean` are for internal use only"
Some languages like Java allow us to explicitly create "wrapper objects" for primitives using a syntax like `new Number(1)` or `new Boolean(false)`.

In JavaScript, that's also possible for historical reasons, but highly **unrecommended**. Things will go crazy in several places.

For instance:

```js run
alert( typeof 0 ); // "number"

alert( typeof new Number(0) ); // "object"!
```

Objects are always truthy in `if`, so here the alert will show up:

```js run
let zero = new Number(0);

if (zero) { // zero is true, because it's an object
  alert( "zero is truthy!?!" );
}
```

On the other hand, using the same functions `String/Number/Boolean` without `new` is a totally sane and useful thing. They convert a value to the corresponding type: to a string, a number, or a boolean (primitive).

For example, this is entirely valid:
```js
let num = Number("123"); // convert a string to number
```
````


````warn header="null/undefined have no methods"
The special primitives `null` and `undefined` are exceptions. They have no corresponding "wrapper objects" and provide no methods. In a sense, they are "the most primitive".

An attempt to access a property of such value would give the error:

```js run
alert(null.test); // error
````

## Summary

- Primitives except `null` and `undefined` provide many helpful methods. We will study those in the upcoming chapters.
- Formally, these methods work via temporary objects, but JavaScript engines are well tuned to optimize that internally, so they are not expensive to call.
