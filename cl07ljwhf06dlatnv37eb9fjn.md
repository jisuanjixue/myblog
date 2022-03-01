## Design Patterns

> The design pattern is always an abstract concept. The previous person summarizes a set of written code through countless practices. The code written in this way can make others easier to read, maintain and reuse.

This chapter we will learn several most common design patterns.

# Factory model

The factory model is divided into several kinds, here is nothing to explain, the following is an example of a simple factory model

```
class Man {
  constructor(name) {
    this.name = name
  }
  alertName() {
    alert(this.name)
  }
}

class Factory {
  static create(name) {
    return new Man(name)
  }
}

Factory.create('bobo').alertName()

``` 
Of course, factory model is not just an example of New.
You can imagine a scene. Suppose there is a very complex code that requires a user to call, but the user does not care about these complex code, only you need to give me an interface to call, the user is only responsible for the transmission required parameters, as for these parameters how to use, what is inside? Logic is not concerned, just need to return to me one instance. This construction process is a factory.

The role of the factory is to hide the complexity of the creation instance, just provide an interface, simple and clear.

In the VUE source, you can also see the use of factory models, such as creating asynchronous components.

```
export function createComponent (
  Ctor: Class<Component> | Function | Object | void,
  data: ?VNodeData,
  context: Component,
  children: ?Array<VNode>,
  tag?: string
): VNode | Array<VNode> | void {
    
    // Logical processing ...
  
  const vnode = new VNode(
    `vue-component-${Ctor.cid}${name ? `-${name}` : ''}`,
    data, undefined, undefined, undefined, context,
    { Ctor, propsData, listeners, tag, children },
    asyncFactory
  )

  return vnode
}
``` 

In the above code, we can see that we can create a component instance, but create this instance is a complicated process, the factory helps us hide this complex process, only need a code call Enter the function.

# Single case mode

Single example mode is common, such as global cache, global status management, etc. These only need to use only one object, you can use single case mode.

The core of the single example mode is to ensure that only one object is available globally. Because JS is a language uncommon language, other language implementation single cases cannot be set in JS, we only need to use a variable to ensure that the instance is only created once, the following is how to implement the example mode

```
class Singleton {
  constructor() {}
}

Singleton.getInstance = (function() {
  let instance
  return function() {
    if (!instance) {
      instance = new Singleton()
    }
    return instance
  }
})()

let s1 = Singleton.getInstance()
let s2 = Singleton.getInstance()
console.log(s1 === s2) // true

``` 
# Adapter mode

The adapter is used to solve the two interfaces that are incompatible, and there is no need to change the existing interface, and the normal collaboration of the two interfaces is achieved by packaging one layer.

Here's how to implement an adapter mode

```
class Person {
  getName() {
    return 'bobo'
  }
}

class Target {
  constructor() {
    this.name = new Person()
  }
  getName() {
    return this.name.getName() + 'nice'
  }
}

let target = new Target()
target.getName() // bobo nice
``` 
In Vue, we are actually used in the adapter mode. For example, the parent component passes to the child component a timestamp properties, the internal components need to turn the timestamp to the normal date display, which usually uses compute to switch this matter, this process uses the adapter mode.

# Decorative mode

Decorative mode does not need to change existing interfaces, and the role is to add functions to the object. As we often need to wear a protective set to the phone, do not change the mobile phone itself, add a protective cover to the phone to provide anti-fall function.

Here is how to implement the decorative mode, use the decorator syntax in ES7

```
function readonly(target, key, descriptor) {
  descriptor.writable = false
  return descriptor
}

class Test {
  @readonly
  name = 'bobo'
}

let t = new Test()

t.yck = '666' // not editable

``` 
In React, the decorative model is actually visible everywhere.

```
import { connect } from 'react-redux'
class MyComponent extends React.Component {
    // ...
}
export default connect(mapStateToProps)(MyComponent)
``` 
# Agent mode

The agent is to control access to objects and do not allow external access to objects. In real life, there are also many agents. For example, you need to buy a foreign product, then you can buy products through purchasing.

There are many scenarios in actual code, and there is also an example in the framework, such as the event agent uses the agent mode.


```
<ul id="ul">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
<script>
    let ul = document.querySelector('#ul')
    ul.addEventListener('click', (event) => {
        console.log(event.target);
    })
</script>

``` 
Because there is too much LI, it is impossible to bind events. At this time, you can bind an event by binding the parent node, let the parent node take the actual click of the node.

# Publishing - Subscription Mode

Publishing - subscription mode is also called observer mode. Through a pair one or more dependency, the subscription party will receive a notice when the object changes. In real life, there are also many similar scenes. For example, I need to buy a product on the shopping website, but I found that the product is currently out of stock. At this time, I can click on the button notification to let the website are in the product. Notify me by SMS.

In actual code, it is actually published - the subscription mode is also very common. For example, we click on a button to trigger a click event to use this mode.

```
<ul id="ul"></ul>
<script>
    let ul = document.querySelector('#ul')
    ul.addEventListener('click', (event) => {
        console.log(event.target);
    })
</script>
``` 

In VUE, how to implement a response is also used. For objects that require a response type, dependent collections are rely on GET. When changing the properties of the object, the distribution update is triggered.

# Appearance mode

The appearance mode provides an interface that hides internal logic, more convenient to external calls.

For example, we now need to implement a method of adding an incident compatible with a variety of browsers.

```
function addEvent(elm, evType, fn, useCapture) {
  if (elm.addEventListener) {
    elm.addEventListener(evType, fn, useCapture)
    return true
  } else if (elm.attachEvent) {
    var r = elm.attachEvent("on" + evType, fn)
    return r
  } else {
    elm["on" + evType] = fn
  }
}
``` 

For different browsers, the way to add events may have compatibility issues. If you need to write this over again every time, we must unacceptably, so we will uncheck these judgments in an interface, and external needs to add events only need to call Addevent.

# summary

This chapter has learned several common design patterns. In fact, there are still a lot of design model, some content is very simple, I have not written in the chapter, such as iterator mode, prototype mode, and some content is not often used, so it will be not listed.





