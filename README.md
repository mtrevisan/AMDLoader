# AMDLoader
Minimal file and module loader that implements the Asynchronous module definition (AMD), compliant with require.js.

Mainly it allows you to
  - load resources on demand
  - define dependencies that must load before a module is executed

## Example 1
```javascript
require(['app/MyModule'], 
    function(MyModule){
        //start the main module which in-turn loads other modules
        var module = new MyModule();
        module.doStuff();
});
```

## Example 2
```javascript
define(['lib/Deferred'], function(Deferred){
    var defer = new Deferred(); 
    require(['lib/templates/?index.html', 'lib/data/?stats'],
        function(template, data){
            defer.resolve({template: template, data:data });
        }
    );
    return defer.promise();
});
```

## Example 3
```javascript
//'app/MyModule' should already be loaded
var MyModule = require('app/MyModule');
```
