Angular Template
==============================
[![build status](https://secure.travis-ci.org/allenhwkim/angular-template.png)](http://travis-ci.org/allenhwkim/angular-template)

Angular-Like HTML Template Engine For NodeJS
-----------------------------------------------

Why do I need this?
By unknown reason, I feel all server-side template engines are somewhat invasive.
It looks like an odd language have been invaded HTML space.
The only template I feel good about it is AngularJS, but it's all about client-side, not server-side part.
If you are a big fan of AngularJS and you want to use AngularJS as a template engine, this node module will do the job.

This template converts the following one time binding expressions on the server-side;

  1. inline expression  
     e.g. `{{ foo }}`  

  2. `ht-if` attribute  
     e.g., `<div ht-if="foo">..</div>`

  3. `ht-repeat` attribute  
     e.g., `<li ht-repeat="el in list">..</li>`  
     e.g., `<li ht-repeat="(k,v) in list">..</li>`  

  4. ht-include attribute  
     e.g., `<div ht-include="'file.html'"></div>`  

Install
-------

    npm install angular-template

Usage
------

    var htmlTemplate = require('angular-template');
    htmlTemplate('{{foo}}', {foo:'Hello'}); //Hello

    // or
    var htmlTemplate = require('angular-template');
    var path = "emails/template.html";
    var options = {prefix:'ng'}; // so that ng-if, ng-repeat, ng-include would work
    htmlTemplate(path, {name:'John'}, options);

Options
------

    prefix: <string>, override default ht prefix with anything, typically ng to reuse angular templates
    preprocess: <function>, enables you to modify your template before parsing it as HTML. E.g. You can remove some attributes with RegExp
    includeDirs: <array of string>, a list of paths where to look for template
    cache: <string>, specify cache key to avoid reading files from disk every time

Converting Angular-Like Expressions
------------------------------------------------
This will convert the angular-like expressions into html.

1. Curly braces expression.

  Assuming foo has the value of `123`

        Input                                | Output
        -------------------------------------+---------------------------------
        {{foo}}                              | 123

2.  **`ht-if`** attribute

  Assuming foo has value `true`, and bar has value `false`

        Input                                | Output
        -------------------------------------+---------------------------------
        <p ht-if="foo">SHOW</p>              | <p>SHOW</p>    
        <p ht-if="bar">NO SHOW</p>           | <p></p>

3. **`ht-include`** attribute

  Assuming foo.html has the following contents `<b>file contents</b>`

 The input and output would like;

        Input                                | Output
        -------------------------------------+------------------------------------
        <p ht-include="'foo.html'"></p>      | <p>
                                             |   <b>file contents</b>
                                             | </p>


4. **`ht-repeat`** attribute

  Assuming collection has the vaulue of `{a:1, b:2, c:3, d:4, e:5}`

        Input                                         | Output
        ----------------------------------------------+------------------------------------
        <ul>                                          | <ul>
           <li ht-repeat="(key, val) in collection">  |   <li> a : 1</li>
             {{key}} : {{val}}                        |   <li> b : 2</li>
           </li>                                      |   <li> c : 3</li>
        </ul>                                         |   <li> d : 4</li>
                                                      |   <li> e : 5</li>
                                                      | </ul>

  Assuming collection has the vaulue of `[1,2,3,4,5]`

        Input                                         | Output
        ----------------------------------------------+------------------------------------
        <ul>                                          | <ul>
           <li ht-repeat="num in collection">         |   <li> 1 </li>
             {{num}}                                  |   <li> 2 </li>
           </li>                                      |   <li> 3 </li>
        </ul>                                         |   <li> 4 </li>
                                                      |   <li> 5 </li>
                                                      | </ul>

5.  **`ht-class`** attribute

  Assuming classes has value `foo`, and classes2 has value `{baz:true}`.

        Input                                | Output
        -------------------------------------+---------------------------------
        <p ht-class="classes">SHOW</p>       | <p class="foo">SHOW</p>    
        <p ht-class="classes2">SHOW</p>      | <p class="baz">SHOW</p>

This accepts the same format as [ng-class](https://docs.angularjs.org/api/ng/directive/ngClass)

6.  **`ht-bind`**, **`ht-bind-html`** attribute

  Assuming content has value `<span>YES</span>`, and status has value `Done`.

        Input                                | Output
        -------------------------------------+---------------------------------
        <p ht-bind="status"></p>             | <p>Done</p>    
        <p ht-bind-html="content">SHOW</p>   | <p><span>YES</span></p>


7.  **`ht-style`** attribute

  Assuming color has value `red`, and styles has value `{color:green,'font-size':'12px'}`.

        Input                                                      | Output
        -----------------------------------------------------------+---------------------------------
        <p ht-style="styles">SHOW</p>                              | <p class="color:green;font-size:12px;">SHOW</p>    
        <p style="font-size:14px" ht-style="{color:color}">SHOW</p>| <p style="font-size:14px;color:red">SHOW</p>

This accepts the same format as [ng-style](https://docs.angularjs.org/api/ng/directive/ngStyle)


LICENSE: MIT
