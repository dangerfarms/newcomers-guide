# Code Quality

## Linting

We use ESLint (ES6) and Flake 8 (Python) to run code quality checks on the code before committing.
Please inspect the output of the scripts and fix any errors and warnings before committing.

Please read the `.eslint.yml` and `.flake8` files and understand rules. If you have any questions or suggestions, let us know.

In your IDE you should set up linting so that you can make use of static analysis (i.e. error highlighting while typing), according to the rules that are specified for the project.

## General rules

### Never commit commented out code.
We have a code repository for a reason: we can go back to older versions.

### Think twice before committing comments.
If you need to make inline comments, rethink your code.   
Maybe your code is too complex?  
Name your variables in a way that it is easy to read your code, and you might find that comments are not necessary after all.

### Keep things organized
It helps maintenance to keep the following things in alphabetical order:
- css attributes,
- javascript object keys
- angular depenency injection arguments
- python class properties
- list of field names in serializers
- ...

Your IDE should have a plugin for organising highlighted blocks of code.

For WebStorm / PyCharm the [Line sorter](https://plugins.jetbrains.com/plugin/?idea&id=4055) plugin does the job.

## Javascript / ES6 specific rules

### Error messages must include the name of the component
Error messages must include the name of the component they are coming from otherwise it is difficult to see what they refer to.
Company convention is to follow the pattern below:
```
$log.error(`[${Component.NAME}] error message`);
```

## Python specific rules

Coming soon.

## CSS / Sass / Scss specific rules