# AMD
The Asynchronous Module Definition (AMD) API specifies a mechanism for defining modules such that the module and its dependencies can be asynchronously loaded.

# AMDLoader
Minimal file and module loader that implements the AMD, compliant with require.js.

Mainly it allows you to:
  - load resources on demand
  - define dependencies that must load before a module is executed

# API Specification
## define() function

The specification defines a single function "define" that is available as a free variable or a global variable. The signature of the function:

```javascript
define(id?, dependencies?, definition);
```

### id
The first argument, id, is a string literal. It specifies the id of the module being defined. This argument is optional, and if it is not present, the module id should default to the id of the module that the loader was requesting for the given response script. When present, the module id MUST be a "top-level" or absolute id (relative ids are not allowed).

### dependencies
The second argument, dependencies, is an array literal of the module ids that are dependencies required by the module that is being defined. The dependencies must be resolved prior to the execution of the module factory function, and the resolved values should be passed as arguments to the factory function with argument positions corresponding to indexes in the dependencies array.

The dependencies ids may be relative ids, and should be resolved relative to the module being defined. In other words, relative ids are resolved relative to the module's id, and not the path used to find the module's id.

The dependencies argument is optional.

### definition
The third argument, definition, is a function that should be executed to instantiate the module or an object. If the definition is a function it should only be executed once. If the definition argument is an object, that object should be assigned as the exported value of the module.

## require(String) function
Synchronously returns the module export for the module ID represented by the String argument. Based on the [CommonJS Modules 1.1.1 require](http://wiki.commonjs.org/wiki/Modules/1.1.1#Require).

It MUST throw an error if the module has not already been loaded and evaluated. In particular, the synchronous call to require MUST NOT try to dynamically fetch the module if it is not already loaded, but throw an error instead.

## require(Array, Function) function
The Array is an array of String module IDs. The modules that are represented by the module IDs should be retrieved and once all the modules for those module IDs are available, the Function callback is called, passing the modules in the same order as the their IDs in the Array argument.

## Examples
### Configuration
```javascript
AMDLoader.config = {
	baseUrl: '/app',
	urlArgs: 't=123456',
	paths: {
		libs: '../libs',
		i18n: '../resources/i18n',
		css: '../resources/css'
	}
};
```

### Example 1
```javascript
require/define(['app/MyModule'], function(MyModule){
    var module = new MyModule();
    module.doStuff();
});
```

### Example 2
```javascript
require/define(function(MyModule){
    var module = new MyModule();
    module.doStuff();
});
```

### Example 3
```javascript
require/define(['lib/Deferred'], function(Deferred){
    var defer = new Deferred(); 
    require(['lib/templates/index', 'text!lib/data/stats.dat'],
        function(template, data){
            defer.resolve({template: template, data: data});
        }
    );
    return defer.promise();
});
```

### Example 4
```javascript
//'app/MyModule' should already be loaded
var MyModule = require('app/MyModule');
```

### Supports domReady, js, css, text, uint8, and img plugins
```javascript
require(['domReady!'], function(readme){});

require(['js!./test.js'], function(readme){
	//loads non-AMD files
});

require(['css!./test.css'], function(readme){});

require(['text!./test.md'], function(readme){});

require(['uint8!./test.dat'], function(readme){});

require(['img!./test.jpg'], function(readme){
	//this implies DOM to be ready
});
```
