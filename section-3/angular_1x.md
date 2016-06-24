# Angular 1.x

**This page is under construction.**

TODO: link to articles

AngularJS is a framework for developing applications that run inside the browser. It is built upon HTML, CSS and JS. If you haven't read our best practices for those languages yet, check them out first!

These best practices assume we're using Angular 1.5 or above. And they will make use of ES6 syntax.

## Best Practices

TODO: add some best practices

### Components

Designing good components can be deceptively tricky. These guidelines outline what has been working well for us. They are subject to change, but we hope they will be helpful!

TODO: guidelines

### Directives

#####Use directives to encapsulate custom element behaviour.

### Templates

##### Avoid putting logic in templates.

We should write logic inside the controller instead. 

* Templates become hard to read if they contain snippets of logic.
* We can't reuse these snippets unless we copy and paste them around the template.
* We can't test logic that lives in HTML.

Bad:

```
/* template.html */

<div ng-show="$ctrl.isValid || $ctrl.state !== $ctrl.STATES.READY">
```

Good:

```
/* controller.js */
class MyController {
  // ...
  
  shouldDisplay() {
    return this.isValid || this.state !== $ctrl.STATES.READY;
  }
}

/* template.html */
<div ng-show="$ctrl.shouldDisplay()">
```

### Modules

##### Create a new Angular module for every new component, service or directive.**

TODO: example

##### Avoid polluting root module dependencies.

Instead, declare dependencies in the module which uses them.

TODO: example

##### Export modules from index.js

This means we can import with a more concise syntax.

TODO: example

## Code Style

TODO: add pedantic styling stuff here