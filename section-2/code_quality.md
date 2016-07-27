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

##### Use all caps for constants.

```
// ES6
const MY_CONST = 'const';

// Python
MY_CONST = 'const';
```

##### Use single quotes whenever possible.

```
# Bad
my_variable = "hiya"

# Good
my_variable = 'hiya'
```

## Javascript / ES6 specific rules

### Error messages must include the name of the component
Error messages must include the name of the component they are coming from otherwise it is difficult to see what they refer to.
Company convention is to follow the pattern below:
```
$log.error(`[${Component.NAME}] error message`);
```

## Python specific rules

### Line breaks

Lists and objects should always be in multiple lines as follows:

```python
# Lists:

# Good:
a_list = [
    'aplphabetically',
    'ordered',
    'values',
]
# Note: Order values alphabetically unless order of items matters.

# Bad:
a_list = ['aplphabetically',
          'ordered',
          'values',]

# Dictionaries
# Good:
a_dict = {
    'a_key: 'value1',
    'complex_key': {
        'nested_key': 'value',
    },
}
# Note: Order keys alphabetically unless order of items matters.

# Bad:
a_dict = {'a_key: 'value1',
          'complex_key': {'nested_key': 'value',},
         }
```

This is easier to maintain when variable names change, because the indentation doesn't depend on the position of the opening brace.

Apply similar logic when you break function calls across multiple lines. This can be especially beneficial if the function has a long name.

```
# Bad
mock_send_profile_completion_email.delay.assert_called_with('profile-complete',
                                                            ['john@wintercircle.co'],
                                                            merge_data={
                                                                'john@wintercircle.co': {
                                                                    'name': 'John',
                                                                    'link':
                                                                    'http://localhost:8200/#/members/profile'
                                                                }                                                                  
                                                            },
                                                            tags=['member-profile-complete'])

# Good
mock_send_profile_completion_email.delay.assert_called_with(
    'profile-complete',
    ['john@wintercircle.co'],
    merge_data={
        'john@wintercircle.co': {
            'name': 'John',
            'link': 'http://localhost:8200/#/members/profile'
        }
    },
    tags=['member-profile-complete']
)
```

## CSS / Sass / Scss specific rules
