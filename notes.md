# Javascript basics

Definitions, standards, etc
JavaScript or vanilla JS

ECMAScript ie official description/specification (browser specification of the js lang)
ES6, ES2015, ES2017 ES2020, etc: a specification version, usually people mean a "cutting edge" lang spec

Babel.js:
a script to convert one of the above to a current browser implementation

TypeScript:
a JavaScript version/variation/dialect/flavor that includes strong typing and other features (.ts)

JSX:
JavaScriptXML a mix of JS and HTML

React, Vue, Angular \* are js front-end frameworks
React uses JSX (JavaScript XML)
Use tools like Babel, WebPack, Node.js

npm, WebPack, Grunt, Gulp (also Babel):
build and infrastructure tools to optimize js to browser deliverables (also optimize for their performance?)

Node.js:
JS server runtime
Node:
server backend (runtime)

## VSCode plugins:

prettier : formatting
ESLint : kind of compiler errors in editor
live server : run local web server to test what you are doing
install node.js and then run npm install in course's github repo to install these dependencies

## browser console

- on firefox: web dev tools ctrl+shift+i
- window.document
- console logging:
  console.log("my pipa class: ", backpack);
- document returns the full html doc

## js code loading

Always use async or defer, footer loading is an antipattern:

- async: <script src="script.js" async></script>
  downloads js asyncrously with HTML parsing.
  once downloaded, stops parsing to execute js
- defer: <script src="script.js" defer></script>
  downloads js asyncrously with HTML parsing.
  js execution is done after dl AND HTML parsing is complete
- modules
  - imply defer: <script type="module" src="backpack.js"></script>
  - export default foo;
  - import foo from foo.js;
  - modules are not available in console to be accessed directly

## syntax

- property name can only contain: letters, digits, dollar sign and underscore
  - conventions is to use camelCase
- bracket notation:
  - backpack.pocketNum is the same as backpack["pocketNum"]
  - this is stupid but it is used like this:
    var query = "pocketNum";
    backpack[query]
  - it also allows you to access non stadard property names (only letters, digits, etc)

## Objects / Classes

- prototype
  - every object in JavaScript is based on a prototype object, a template build into JavaScript itself. That prototype becomes part of every object you make, and you can find it as a hidden property called [[Prototype]]
  - "prototype inheritance" in JavaScript. Every object you work with inherits its structure from another prototype object, all the way up to the core Object.prototype. This means every object you work with inherits the built-in properties and methods of its prototypes! eg valueOf() toString()
  - object literal
    - created with curly braces {} rather than object constructor
    - Object.prototype
- function definition:
  - functioName: function(argument) {}
  - equal(?) to functioName(argument) {} but apparently js people are stupid to prefer the clutter
- class declaration, similarly stupid:
  - class Name {} : "class declaration"
  - const name = class {} : "class expression"
- class instantiation:
  - const classInst = new className(with, arguments, etc, foo);
- class with constructor:
  ```JS
    class Backpack {
    constructor(
      // Defines parameters:
      name,
      volume,
      color,
      pocketNum,
      strapLengthL,
      strapLengthR,
      lidOpen
    ) {
      // Define properties:
      this.name = name;
      this.volume = volume;
      this.color = color;
      this.pocketNum = pocketNum;
      this.strapLength = {
        left: strapLengthL,
        right: strapLengthR,
      };
      this.lidOpen = lidOpen;
    }
    // Add methods like normal functions:
    toggleLid(lidStatus) {
      this.lidOpen = lidStatus;
    }
    newStrapLength(lengthLeft, lengthRight) {
      this.strapLength.left = lengthLeft;
      this.strapLength.right = lengthRight;
    }
  }
  ```
- "object constructor function" : yet another stupidity, different syntac to declare a class: all inside the "object constructor function"
  ```JS
  function Classname( param, foo, bar) {
    this.param = param;
    this.foo=foo;
    this.bar=bar;
    this.functionOne = function(param){
      this.foo = param;
    }
    this.functionTwo = function(param){
      this.bar = param;
    }
  }
  ```
- class allows more things: class extension. is the currently prefered/trend but object constructor functions were used in the past
- class extension:
  ```JS
  class HikingBackpack extends Backpack {
    constructor(
      name,
      volume,
      color,
      pocketNum,
      strapLengthL,
      strapLengthR,
      lidOpen,
      hydrationCapacity
      ) {
        // Initialize the parent class properties
        super(name, volume, color, pocketNum, strapLengthL, strapLengthR, lidOpen);
        // New property specific to HikingBackpack
        this.hydrationCapacity = hydrationCapacity; // Capacity in liters
      }
      toggleLid(lidStatus) {
        super.toggleLid(lidStatus); // Call the parent method
        if (lidStatus) {
          console.log("Your hiking backpack lid is open. Remember to check to make sure the hydration pack is inserted.");
        } else {
          console.log("Your hiking backpack lid is closed. Remember to check to make sure the hydration pack is inserted.");
      }
    }
  }
  ```
- "js standard builtin objects, see Mozilla Dev Net for reference

### String output

- template literals: mix html with javascript expressions. inside backticks
  - use this `document.body.innerHTML = content;` to inject directly in the DOM
    ```JS
    const content = `
      <main>
        <article>
          <h1>${everydayPack.name}</h1>
          <ul>
            <li>Volume: ${everydayPack.volume}</li>
            <li>Color: ${everydayPack.color}</li>
            <li>Age: ${everydayPack.backpackAge()}</li>
            <li>Number of pockets: ${everydayPack.pocketNum}</li>
            <li>Left strap length: ${everydayPack.strapLength.left}</li>
            <li>Right strap length: ${everydayPack.strapLength.right}</li>
            <li>Lid status: ${everydayPack.lidOpen}</li>
          </ul>
        </article>
      </main>
    `;
    ```
- standard strings: they were used before the template literals. even more discusting (rofl)
  ```Javascript
  const content = "<h1>" + everydayPack.name + "</h1>";
  ```

# DOM: Document Object Model

- Hierarchical tree structure model of the HTML doc
- `Document.querySelector()` and `Document.querySelectorAll()` use standard CSS queries to find elements
  - usually take css query ie string inside quotation marks
  - can take a string if it is an element, eg:
    - `document.querySelector("main")`
    - `document.querySelector(".maincontent")`
    - `document.querySelector("main li:last-child")` (last item from list in main)
    - `document.querySelectorAll("main li").forEach(item => item.style.backgroundColor= "red")`
- before the introduction of `querySelector` `element.getElementsByClassName()` and `element.getElementById()` where the only way to retrieve things.
  - `element.getElementsByClassName()` returns an array-like object of all the nodes or child elements that match the query (ie a string of space seperated class names)
- `document.querySelector("h1").className = "pipa-new-name"`

  - DONT DO it
  - this doesn't properly update class name properties name
  - in react and a few other frameworks `className` is used instead of `class` to avoid collision with js
  - use this instead:
    `document.querySelector("main li:first-child".).classList.{add("new-class"),remove("new-class"),toggle("new-class"),replace("packprop","new-class")}`
  - you can use Element only to search for a string in all classes to do sth else with them, not modify the class names

  - Element.attributes can be used to change any attribute.
    - it returns a NamedNodeMap, not the same as Element() or HTMLCollection(). it is more complex and has key/value

- eg `document.querySelector("img").hasAttribute("src")` returns true or false
- eg `document.querySelector("img").getAttribute("src")` returns the actual value

- element inline CSS style declaration is stored in the style property of the element
  - `Document.querySelector(".site-title).style` => CSSStyleDeclaration
  - `Document.querySelector(".site-title).style.color = "blue"`
