# Tema 21 Plantillas FreeMarker

[Fundamentos](https://freemarker.apache.org/docs/dgui.html)

[Guía básica para programadores](https://freemarker.apache.org/docs/pgui.html)

[Manual](https://freemarker.apache.org/docs/index.html)

[Chuleta](https://freemarker.apache.org/docs/dgui_template_exp.html#exp_cheatsheet)
## Conceptos básicos


Desde un punto de vista conceptual, útil para el diseñador de plantillas, el modelo de datos se puede ver como una estructura en árbol.

Hay tres tipos de variables:

- escalares: almacenan un valor de tipo string, number boolean o date-like (date-time/date/time)
- hashes (diccionarios): almacenan una o más variables, cada una de ellas asociada a un nombre
- secuencias: almacenan variables en una secuencia ordenada. Las variables se acceden con un índice numérico que empieza por 0.

Las subvariables se acceden usando natural naming:

- hashes: animal.python.length
- sequences: fruits[3].name

The simplest template is a plain HTML file (or whatever text file; FreeMarker is not confined to HTML). When the client visits that 
page, FreeMarker will send that HTML to the client as is. However if you want that page to be more dynamic then you begin to put 
special parts into the HTML which will be understood by FreeMarker:

${...}: FreeMarker will replace it in the output with the actual value of the expression inside the curly brackets. They are called 
**interpolations**.

FTL tags (for FreeMarker Template Language tags): FTL tags are a bit similar to HTML tags, but they are instructions to FreeMarker and 
will not be printed to the output. The name of these tags start with #.

Comments: Comments are similar to HTML comments, but they are delimited by `<#--` and `-->`. Unlike HTML comments, FTL comments won't 
get into the output (won't be visible in the page source for the visitor), because FreeMarker skips them.

Anything not an FTL tag or an interpolation or comment is considered static text and will not be interpreted by FreeMarker; it is just 
printed to the output as-is.

## Directivas

With FTL tags you refer to so-called directives. This is the same kind of relationship as between HTML tags (e.g.: `<table>` and 
`</table>`) and HTML elements (e.g., the `table` element) to which you refer to with the HTML tags. (If you don't understand this 
difference then consider "FTL tag" and "directive" synonyms.)

```{Important} *In general, unquoted words inside directives or interpolations are treated as references to variables*.

A frequent mistake of users is the usage of interpolations in places where they needn't/shouldn't/can't be used. *Interpolations work 
only in text sections (e.g. `<h1>Hello ${name}!</h1>`) and in string literals (e.g. `<#include "/footer/${company}.html"`>)*. A typical 
WRONG usage is `<#if ${big}>...</#if>`, which will cause a syntactical error. You should simply write `<#if big>...</#if>`. Also, 
`<#if "${big}">...</#if>` is WRONG, since it converts the parameter value to string and the if directive wants a boolean value, so it 
will cause a runtime error.
```

[Ejemplos](https://freemarker.apache.org/docs/dgui_quickstart_template.html)

    <h1>
      Welcome ${user}<#if user == "Big Joe">, our beloved leader</#if>!
    </h1>

    <#if animals.python.price < animals.elephant.price>
      Pythons are cheaper than elephants today.
    <#elseif animals.elephant.price < animals.python.price>
      Elephants are cheaper than pythons today.
    <#else>
      Elephants and pythons cost the same today.
    </#if>

The generic form of the list directive is:

    <#list sequence as loopVariable>
        repeatThis
    </#list>

or

    <#list sequence>
      <#items as loopVariable>
        repeatThis
      </#items>
      <#else>
        Do this only once
    </#list>


The `repeatThis` part will be repeated for 
each item in the sequence that you have specified with `sequence`, one after the other, starting from the first item. In all repetitions 
`loopVariable` will hold the value of the current item. This variable exists only between the `<#list ...>` and `</#list>` tags.

The sequence can be any kind of expression.

A complete list example:

    <#list misc.fruits>
      <p>Fruits:
      <ul>
        <#items as fruit>
          <li>${fruit}<#sep> and</#sep>
        </#items>
      </ul>
    <#else>
      <p>We have no fruits.
    </#list>
## Built-ins

Los built-ins se aplican a variables y expresiones se invocan con un `?`:

    expr?builtin_name

The reason why built-ins are crucial is that normally (though it depends on configuration settings), FreeMarker doesn't expose the Java 
API of the objects. So despite that Java's String has a `length()` method, it's hidden from the template, so you have to use 
the built-in `path?length` instead.

Examples with some of the most commonly used built-ins:

- `user?upper_case` gives the upper case version of the value of user (like "JOHN DOE" instead of "John Doe")
- `animal.name?cap_first` give the animal.name with its first letter converted to upper case (like "Mouse" instead of "mouse")
- `user?length` gives the number of characters in the value of user (8 for "John Doe")
- `animals?size` gives the number of items in the animals sequence (3 in our example data-model)

If you are between `<#list animals as animal>` and the corresponding `</#list>` tag:

- `animal?index` gives the 0-based index of animal inside animals
- `animal?counter` is like index, but gives the 1-based index
- `animal?item_parity` gives the strings "odd" or "even", depending on the current counter parity. This is commonly used for coloring 
  rows with alternating colors, like in `<td class="${animal?item_parity}Row">`.

Some built-ins require parameters to specify the behavior more, for example:

- `animal.protected?string("Y", "N")` return the string "Y" or "N" depending on the boolean value of animal.protected.
- `animal?item_cycle('lightRow', 'darkRow')` is the more generic variant of item_parity from earlier.
- `fruits?join(", ")`: converts the list to a string by concatenating items, and inserting the parameter separator between each items 
  (like "orange, banana")
- `user?starts_with("J")` gives boolean true of false depending on if user starts with the letter "J" or not.
- `animals?filter(it -> it.protected)` gives the list of protected animals. To list protected animals only, you could use 
  `<#list animals?filter(it -> it.protected) as animal>...</#list>`.

Built-in applications can be chained, like `fruits?join(", ")?upper_case` will first convert the list a to a string, then converts it 
to upper case. (This is just like you can chain .-s (dots) too.)

Dealing with missing variables: a non-existent variable and a variable with null value is the same for FreeMarker. The "missing" term 
used here covers both cases.

- `user!"visitor"`: gives a default value to the `user` variable
- `user??`: returns true if the variable is *not* missing

Methods ara invoked with the `()` operator, passing the required parameters i.e. they are invoked like built-ins with parameters.
