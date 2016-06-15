#JavaScript (ES6)
This chapter aims to answer common questions and prevent frequent mistakes made in JavaScript.

We are using ES6.

> **Note** This whole chapter is under construction. Take care!

More coming soon.

## ES6
https://babeljs.io/docs/learn-es2015/  
http://ilikekillnerds.com/2015/02/a-guide-to-es6-classes/  

More coming soon.  

## AngularJS 1.x


In this order:

https://thinkster.io/mean-stack-tutorial

https://thinkster.io/angularjs-es6-tutorial (Make sure you go thorugh the required prerequisits links)

https://toddmotto.com/rewriting-angular-styleguide-angular-2 and obviously https://github.com/toddmotto/angular-styleguide

More coming soon.

### Code structure

* Prefix custom directives (components) with `df` to namespace them. This prevents collisions with 3rd party directives or future standard HTML tags. It also prepares our components for publishing.

  If the directive is not reusable across projects, use a project-specific namespace-prefix instead.

More coming soon.

### Unit testing
We use Jasmine as our test framework: http://jasmine.github.io/2.0/introduction.html

More coming soon.

#### IDE setup
In WebStorm you can add auto-completion for `jasmine` by the following steps:

1. Open Settings
2. Go to Languages & Frameworks > JavaScript > Libraries
3. Click Download...
4. Add `jasmine`
5. Restart WebStorm

### Dependency Injection
We are using `ngAnnotate` to automate dependency injections. Define the dependencies for your directives, controllers, services, etc as follows:
```
let myService = ($scope) => {
    // Do stuff
}

export default /*@ngInject*/myService;
```
Or for elements defined as classes:
```
class Controller {
  /*@ngInject*/
  constructor(someService) {
    // Do stuff
  }
}
```


## Writing minification-ready code (using webpack)

### Naming

#### Directives
Name your directive:
```
let myDirective = () => {
  return {
    restrict: 'E',
    template: `<div>hello</div>,
  };
};

myDirective.NAME = 'myDirective';

export default myDirective;
```

When you load it in a module, use the name you given to it:
```
import myDirective from './myDirective.directive.js';

angular.module('module', [])
       .directive(myDirective.NAME, myDirective) // The directive is available as <my-directive> in the code
```

#### Services
Name your services/factories, same way as described above for directives.

#### Controllers, modules, routers, runs, configs
Controllers, modules, routers, runs, configs, etc do not need a separate NAME specified.
