# Code Quality

## Linting

We use ESLint (ES6) and Flake 8 (Python) to run code quality checks on the code before committing.
Please inspect their output and fix any errors and warnings before committing.

Please read the `.eslint.yml` and `.flake8` files and understand rules. If you have any questions or suggestions, let us know.

In your IDE you should set up linting so that you can make use of static analysis (i.e. error highlighting while typing).

## General rules

### Never commit commented out code.

We have a repo so that we can go back to older versions simply.

### Think twice before committing comments.
If you need to make inline comments, rethink your code.   
Maybe there is too much complexity?  
Name your variables in a way that it is easy to read your code, and you might find that your comment is not necessary after all.

## Javascript / ES6 specific rules

### Error messages must include the name of the component
Error messages must include the name of the component they are coming from otherwise it is difficult to see what they refer to.
Company convention is to follow the pattern below:
```
$log.error(`[${Component.NAME}] error message`);
```

## Python specific rules

## CSS / Sass / Scss specific rules