# Angular 1.x

**This page is under construction.**

TODO: link to articles

AngularJS is a framework for developing applications that run inside the browser. It is built upon HTML, CSS and JS. If you haven't read our best practices for those languages yet, check them out first!

These best practices assume we're using Angular 1.5 or above. And they will make use of ES6 syntax.

## Best Practices

### Components

Designing good components can be deceptively tricky. These guidelines outline what has been working well for us. They are subject to change, but we hope they will be helpful!

##### Create components for purely presentational elements.

It might seem like this HTML snippet doesn't warrant its own component:

```
<h3 class="DashboardSubheader">
    My header
</h3>
```

But actually, there are many benefits to creating a component:

```
<dashboard-subheader>
    My header
</dashboard-subheader>
```

* If we want to change the CSS class, we only need to change it in one place.
* We might extend the header functionality later. Maybe we will want anchor links to be generated for all dashboard subheaders. This will be easier if we only have to make the change in one place.
* We don't have to remember which tag types go with which CSS classes.
* It makes our templates more readable, and less... div-y.

In short: creating HTML templates now feels like software engineering instead of a copy and paste exercise!

##### Don't set `controllerAs` in components.

Use the default name, which is `$ctrl`. **TODO: link to Angular docs**

We don't have to worry about controllers having the same name, because of isolated scopes. Using the default name reduces boilerplate and encourages consistency.

##### Use `require` for parent-child co-ordination?

You only want to use this pattern when the child component wouldn’t make sense on its own.

Use the `$onInit` component lifecycle hook to access the controllers that you request using `require`.

**TODO: example.**

##### Use services for other component co-ordination.

Sometimes we want to co-ordinate components that aren't visually grouped together. In this case, a parent component is not the correct solution for communicating between components. Instead use a service.

Pros:

Cons:

* Services are singletons. This means your state is available globally in the app. It's generally preferable to isolate state to the components that need it. See **TODO: link to "Data + state should live as far down the component tree as it can."**

**TODO: example.**

##### Make use of container components.

It's easy to do all your data fetching on the view. Likewise, it's a common pattern to add a lot of callbacks and logic for your components in a view-controller.

This gets messy.

If some data or state is only relevant for a particular component (or group), create a container component, and move the controller code there.

**TODO: example**

**TODO: link**

##### Consider using a service instead of passing data through 

Events and callbacks must bubble up and down the component tree by their nature.

### Directives

##### Use directives to encapsulate custom element behaviour.

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
  
  isDisplayed() {
    return this.isValid || this.state !== $ctrl.STATES.READY;
  }
}

/* template.html */
<div ng-show="$ctrl.isDisplayed()">
```

### Modules

##### Create a new Angular module for every new component, service or directive.

TODO: example

##### Avoid polluting root module dependencies.

Instead, declare dependencies in the module which uses them.

TODO: example

##### Export modules from `index.js`.

This means we can import with a more concise syntax.

TODO: example

### Dependency Injection

##### Use strict DI.

##### Give NAMEs to things.

Reason 1: minification.
Reason 2: globals.

##### Use ngInject.

Use it where functions or classes are declared. Then you don’t have to manually annotate using `$inject`.
 
 ```
/*@ngInject*/
function MyDirective($timeout) {
  ...
}
```

```
/*@ngInject*/
class MyController {
  constructor($state, Restangular, DfService) {
    this.$state = $state;
    …
  }
}
```

### General

##### Avoid inheritance.

1. Inhertiance doesn’t play nicely with dependency injection. 

   Child classes need to inject all the same things as their parents, and then they need to pass the dependencies up via super.
   
   ```
   class ParentController {
     constructor($log) {
       this.$log = $log;
     }
   }
   
   class ChildController {
     constructor($log, $scope) {
       super($log);

       this.$scope = $scope;
     }
   }
   ```
   
    This isn’t DRY, and results in tight coupling between the constructors.

2. Inheritance in JavaScript can behave in unexpected ways, due to JavaScript’s use of prototypes instead of real classes.

3. Even in programming languages where inheritance is well-behaved, composition is often encouraged as a safer method of code reuse.

In Angular, a factory or service is usually your best bet for reusing code.

## Code Style

TODO: add pedantic styling stuff here