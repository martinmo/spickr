# Thymeleaf

## Selection Expression

Select an object with `th:object` and reference it later with the `*{attr_name}` expression:

    <div th:object="${user}">
      <p>Hi, <span th:text="*{firstName}>username</span>!</p>
    </div>

(Alternatively, use `${#object.attr_name}`.)


## Elvis Operator

If the second argument of the ternary operator is left out, we get the elvis operator `?:`:

    <p th:text="${maybe} ?: 'default'">...</p>

Should be used in conjunction with no-op.


## No-Op

Use `_` as a no-op:

    <p th:text="${maybe} ?: _">default value</p>

Means: if `maybe == null`, display `default value`.


## Double Brace Fires Conversion Service

Use `${{someVar}}` or `*{{someAttrOfSelectedObject}}`.


## Expression Utility Objects

Accessed with a hash sign (`#`). For example, the `dates` utility object: `${#dates.format(date,
'dd/MMM/yyyy HH:mm')}`. Other objects: `#calendars`, `#strings`, etc.


## Spring Beans

Accessed with a `@` sign. For example: `${@beanName.someThing}`.


## Set, Prepend and Append to Attribute

Equivalent:

    <p th:attr="class=${some.var}">
    <p th:class="${some.var}">

    <p th:class="'message message-' + ${messageType}">
    <p class="message message-" th:attrappend="class=${' ' + messageType}">

Even better:

    <p class="message" th:classappend="'message-' + (${messageType} ?: 'default')">


## Alternative Attribute Syntax (HTML5 compatible)

`th:block` vs. `data-th-block`


## Literal String Substitutions

Instead of using string concatenation, it's often more elegant to use the following form:

    <p th:text="|Hi there, ${user.name}!|">

This is equivalent to:

    <p th:text="'Hi there, ' + ${user.name} + '!'">


## Message Properties

Message properties implement i18n support. Strings are loaded from a language specific
`messages.properties` file such as `messages_de.properties`, if it exists on the classpath.

The value of a message property `foo.bar` can be accessed with `#{foo.bar}`. It can also be
parameterized, e.g., `#{message.welcome(${user.name})`. The corresponding property declaration in
`messages.properties` should look like this:

    message.welcome = Welcome to our site, {0}!

Beware that the message property files are expected to be encoded in ISO-8859-1 (and *not* UTF-8).


## Pluralization of Messages

Let's say I want to display `1 item` and `x items` for `x > 1`. Message properties support
pluralization, albeit with a funny syntax. Suppose our template uses a parameterized message:

    <p th:text="#{cart.status(${numItems})}">2 items</p>

A corresponding message.properties file could look like this:

    cart.status = {0,choice,0#no items|1#1 item|1<{0} items}
    #                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^--- format style with 3 alternatives
    #                ^^^^^^--- format type
    #              ^--- argument index

The syntax is described in the Javadoc for [`MessageFormat`][MessageFormat]. For example, with
`numItems` equal to `3`, this renders to:

    <p>3 items</p>

[MessageFormat]: https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/MessageFormat.html


## Disable HTML Escapes

Use `th:utext` instead of `th:text`.
