# AMDLoader
Minimal file and module loader that implements the Asynchronous module definition (AMD), compliant with require.js.

Mainly it allows you to:
  - load resources on demand
  - define dependencies that must load before a module is executed

## Configuration
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

## Supports domReady, js, css, text, uint8, and img plugins
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
