#JavaScript (ES6)
This chapter aims to answer common questions and prevent frequent mistakes made in JavaScript.

We are using ES6.

TBC.

## ES6
Coming soon.  
https://babeljs.io/docs/learn-es2015/  
http://ilikekillnerds.com/2015/02/a-guide-to-es6-classes/  

## AngularJS 1.x
Coming soon.

### Code structure

* Prefix custom directives (components) with `wc` to namespace them. This prevents collisions with 3rd party directives or future standard HTML tags. It also prepares our components for publishing.

### Unit testing
We use Jasmine as our test framework: http://jasmine.github.io/2.0/introduction.html

#### IDE setup
In WebStorm you can add auto-completion for `jasmine` by the following steps:

1. Open Settings
2. Go to Languages & Frameworks > JavaScript > Libraries
3. Click Download...
4. Add `jasmine`
5. Restart WebStorm

TBC.


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

### Dependency Injection
Define the dependencies for your directives, controllers, services, etc as follows:
```
let myService = ($scope) => {
    // Do stuff
}

myService.$inject = ['$scope'];

export default myService;
```

### Inline controllers
For inline controllers (e.g. in dialogs, etc), you can annotate your code with `/*@ngInject*/`, which will allow a 
plugin to inject your inline controller.
```
let MemberSearchOverlayController = ($scope, callbacks, MemberFilter) => {
    // ...
};

class ClientsController {
  // ...

    this.$mdDialog.show({
      controller: /*@ngInject*/MemberSearchOverlayController,
      locals: {
       callbacks: this.callbacks,
       MemberFilter: this.MemberFilter
      }
    });
}
```