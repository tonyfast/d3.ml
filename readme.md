# ``d3.template`` Hello World

Data-driven webpages from structured data.  This example is created from [hello-world.yaml](templates/hello-world.yaml).

## About this readme

> This readme is written in literate coffeescript.  It serves the purpose of a readme 
and the most basic example of d3.template.

# Hello World Example

1. Load a template

  Use YAML to read and create data-driven webpages.  Everything happens after the YAML is loaded.
      
      d3.text 'templates/hello-world.yaml', (d)->
        yaml = jsyaml.load d
        console.log yaml

2. Create a d3 selection

  This hello-world example will change the webpage ``<body>``.
    
        body = d3.select 'body'
  
3. Apply the template to the selection

        body.template yaml, \
          __data__: 
            'private-data': "Available to the template in ``_data``"
        
4. Append YAML text and final HTML to the template contexts.
        
        document.__data__.template.body.yaml= d
        document.__data__.template.body.html= body.select('.hello-world').html()
          .split '><'
          .join '>\n<'
          
          
## Benefits of d3.template

``d3.template`` aims for a minimal abstraction of data into the document object model.  A minimal abstraction
allows many artifacts to be made.  ``d3.template`` converts and stores structured objects describing a webpage to:

* YAML
* JSON
* Javascript
* CoffeeScript
* HTML

All of the contexts are contained in ``document.__data__.template.body``.  Open you console to have a look.

        console.log 'All of the template contexts are presented in the object below'
        console.log document.__data__.template.body

### Add some interactions

        body.select '#toggle-context'
          .datum 
            keys: d3.keys document.__data__.template.body
            index: -1
          .on 'click', (d)->
            @__data__.index = d.index = d.index + 1
            if d.index == d.keys.length
              @__data__.index = d.index = 0
            key = d.keys[d.index]
            @innerText = key
            d3.select '#view-context'
              .text ()->
                if key in ['__data__','object']
                  out = JSON.stringify document.__data__.template.body[key], null, 2
                  key = 'json'
                  out 
                else
                  document.__data__.template.body[key]
              .call (s)-> 
                s.attr 'class', key
                hljs.highlightBlock s.node()
                
            
        
    
