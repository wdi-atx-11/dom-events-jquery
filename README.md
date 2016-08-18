<!--
Creator: <Name>
Location: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# DOM Events

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

Most of the interactivity for JavaScript in the web is based around events.  The browser detect user actions or page status changes and "emits" or sends out an event. Developers then specify behaviors that will happen when a particular event occurs.

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

- Explain what DOM events are and how they are triggered.
- Attach event listeners to DOM elements.
- Target the source of an event.
- Respond to events with a callback.
- Explain event propagation.

### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- Trace the control flow of a program including functions.
- Distinguish between function definitions and function calls.
- Select DOM elements with jQuery.

### Aside: Callbacks

A **callback** is a function that is passed into another function as an argument and then used. A function that can take in a callback as an argument is known as a higher order function.

```js

function higherOrderFunction (phrase, callback) {
  console.log("I'm the first class function, now calling the callback...");
  callback(phrase);
}

function shoutItCallback(message){
  console.log(message.toUpperCase());
}

function splitItCallback(str){
  console.log(str.split(""));
}

higherOrderFunction("Functions are fun!", shoutItCallback);

higherOrderFunction("functions are fun!", splitItCallback);

```

The callback pattern is used a lot in JavaScript. As an example, `.forEach` is a built-in Array **iterator method** that takes in a callback.

```js
var numbers = [123, 45, 0];
numbers.forEach(isEven);

function isEven(num){
  if (num % 2 === 0){
    console.log(num + " is even!");
    return true;
  } else {
    console.log(num + " is odd!");
  }
}
```

Often, the callback function definition is written inside the higher order function call.


```js
var numbers = [123, 45, 0];
numbers.forEach(function isEven(num){
  if (num % 2 === 0){
    console.log(num + " is even!");
    return true;
  } else {
    console.log(num + " is odd!");
  }
});
```

In these cases, the callback often won't be given a name.  A function without a name is called an **anonymous function**.

> When you use a jQuery selector like `$('p')`, the collection you get back isn't exactly the same as a native JavaScript array. To iterate over a jQuery collection, use jQuery's `.each` iterator method.



### Events

**Events are always happening!**

![Click Event](http://www.createursoftware.net/Windows/XP/img/tous/gif/clicking.gif)

In Chrome, we can use the following utility function to log some events occurring in the window:

```js
monitorEvents(window, ["click", "keypress", "resize", "scroll"]);
```

Here's a larger list of DOM event types:

* `click`
* `mouseenter`, `mouseleave`
* `load`
* `DOMContentLoaded`
* `keydown`, `keypress`, `keyup`
* `scroll`
* `resize`
* `change`

> Note: some events can only be listened to by certain DOM elements. Check documentation.

Events tell us a lot of information. For example, a "click" event includes not just the fact that a "click" occurred but also where, when and what was clicked:

	* `x` and `y` screen coordinates.
	* DOM element that got clicked.
	* Time (timestamp) when it happened.

### Event Listeners

The browser is sending out these events all the time. In order to capture and act on them, we can add event listeners to elements. We'll use JavaScript to tell an element to *listen* for a certain type of event and what to do when that event occurs.


So long as we know the name of the event we're listening for, we can "attach" or "bind" an event listener to our element!

| metaphor                                      | JavaScript example  | jQuery example               |
|----------------------------------------------:|----------------------:| ----------------------:|
| A subject (the thing doing the listening).    | document.getElementById('greeting') | `$('#greeting')`              |
| A trigger (the "event" to listen for).        | .addEventListener('mouseover', ...) |`.on('mouseover', ...)`    |
| An action (how to respond).                   | `function popUpYay(){ ... }` |`function popUpYay(){ ... }`   |


Here's how this looks all together with "vanilla" or "native" JavaScript:

```js
  var greeting = document.getElementById('greeting');
  greeting.addEventListener('mouseover', popUpYay);

  function popUpYay(event){
    alert("Yay!");
  }
```

When we've selected an element or elements with jQuery, we can use jQuery's `.on` method to add an event listener:

```js
var greeting = $('#greeting');
greeting.on('mouseover', popUpYay);

function popUpYay(event){
  alert('Yay!');
}
```
Sometimes you will see this shorthand:

```js
$('#greeting').mouseover(popUpYay);
```

The `.mouseover(...)` method is equivalent to `.on('mouseover', ...)`. We recommend using `.on`.

####Check for Understanding

In the last example:
  * What is listening for the event?
  * What trigger are we waiting for?
  * What is the action tied to this event?
  * When is the `popUpYay` function actually executed?

####Check for Understanding

Open your developer console on [jQuery.com](https://jquery.com).

1. Can you capture the `scroll` event?
  <details><summary>Hint</summary>The subject listening should be the `window` object - try using `$(window)`).</details>  
  Add a simple event handler to `console.log` a message every time the event occurs.

  <details>
    <summary>answer</summary>
    ``` javascript
    $(window).on("scroll", function handleScroll(){
        console.log("just keep scrolling, scrolling, scrolling");
    })
    ```
  </details>

2. Modify your event handler so it adds a new paragraph, `<p>to infinity... and beyond!</p>`, at the bottom of the page every time the user scrolls.

  <details>
    <summary>answer</summary>
    ``` javascript
    $(window).on("scroll", function handleScroll(){
        $("body").append("<p>to infinity... and beyond!</p>");
    })
    ```
  </details>


### Default Behaviors

As a digital native, you take these behaviors for granted:

* When you "submit" a form, you want it to send your data somewhere.
* When you "click" on a link, you expect to navigate to a new page.

Try this on [jQuery's homepage](http://jquery.com/):

```js
var $links = $("a"); // every link on the page
$links.on("click", function handleClick(event){
    alert("You just clicked a link. You are about to be redirected.");
});
```

Redirecting to a new page is the *default behavior* of anchor tags (`a` elements). How would we stop this behavior? What if we need to "prevent (the) default" ?

We have two options: we can `return false` or we can use a special method called `.preventDefault()`.

**`.preventDefault()`** (preferred because it is explicit):

```js
$("a").on("click", function handleClick(event){
    event.preventDefault();
    // more code down here
});
```

**`return false`** (this works too!):

```js
$("a").on("click", function handleClick(event){
    // more code up here
    return false;
});
```

### Independent Practice

Practice with this [training](https://github.com/sf-wdi-31/jquery-events-training).  


### Closing Thoughts

* Event-driven behaviors are the expectation for modern sites.

* Practice selecting DOM elements. This can be done with native JavaScript language features, but we're mainly going to use jQuery library methods.

* Remember, jQuery is a library, not a language.
