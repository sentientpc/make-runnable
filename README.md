# Run Exported Functions Directly From The Command Line

What's the quickest way to trial a function you're exporting? 

Doing this in your shell?

    $node
    >require('your_file.js').addOneTo(3)
    4

No. That's annoying. And you have to repeat it all every time you change `your_file.js`. 

Rather insert the following anywhere in `your_file.js` to expose its exports to the command line:

    require('make-runnable');

That's it. Now you can do:

    $node your_file.js addOneTo 3
    4


## Syntax

Call your function with several args:

    node [your_file] [function_name] firstArg secondArg 
    
Or call it with a single object:

    node [your_file] [function_name] key1=value1 key2=value2 
    
    
## Full Example

Let's say you have the following file:

**your_file.js**

    require('make-runnable');
    module.exports = {
        addTogether: function(x,y){
            return x + y
        }, doSomethingWithObject: function(object){
            object.newKey = "easy AF";
            return object;
        }, simpleValue: 'also works'
    };

You can now do the following:

**$sh**

    node your_file.js addTogether 1 2
    > 3
    node your_file.js doSomethingWithObject --x 1 --y hello
    > {x: 1, y: 'hello', newKey: 'easy AF'}
    node your_file.js simpleValue
    > also works

## How does it work?

1. `require.main === module` is used to check if the module is being run directly, or imported.
2. If it's being run directly, then [yargs](https://www.npmjs.com/package/yargs) is used to parse `process.argv` so that the target function may be called with the desired arguments.