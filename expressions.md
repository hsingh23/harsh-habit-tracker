Expressions let you present the current value of properties in other elements and perform simple computations.

Using Expressions
By default, Mavo expressions are delineated by square brackets, e.g. [5 + 5]. We explored many different syntaxes and found that format to have the best readability.

If your HTML text contains brackets whose content you don’t want to be an expression, you can change the syntax with the mv-expressions attribute. For example, to switch to a double curly brace syntax (common in other systems), you can use mv-expressions="{{ }}" and then expressions would be specified like {{5 + 5}}. To disable expressions in an element and its descendants altogether, use mv-expressions="none".

Disabling expressions
You can disable expressions in two ways:

mv-expressions="none" disables expressions for an entire element and its descendants (you can override it on descendants however, by specifying mv-expressions="[ ]" or whatever syntax you prefer). mv-value still works even if expressions are disabled via mv-expressions="none".
mv-expressions-ignore accepts a comma-separated list of attributes in which to ignore expressions. E.g. mv-expressions-ignore="mv-storage, title". mv-expressions-ignore is not inherited.
MavoScript
Mavo’s expression syntax is called MavoScript. It is similar to spreadsheet formulas, but designed to be more readable and to accommodate nested relations (which spreadsheets cannot do).

They mostly consist of the following:

Property names
You use the value of a property you have defined by just typing its name. You can see some examples in the next section. You can use any property value from anywhere in your Mavo, or even in other Mavos (in that case, you'd need to add the app id before it, like appid.propertyName).

Example
Play!
Move the slider and check how both expressions are updated:

<p>Slider value: [strength]/100</p>
<input type="range" property="strength" title="[strength]%" />

The values of properties depend on where your expression is placed. When you refer to properties that are contained in lists, when your expression is outside the list you get all values, and when your expression is inside a list item you get just the value of the current item.

Function calls
Functions transform values into different values, for example they can do math on a number, or tell if you some text contains some other text. They are written with their name, and then a comma-separated list of parameters in parentheses, e.g. min(2, 5).

All MavoScript functions

Operators
Operators are a shorter and more readable way to write common functions. For example, imagine if every time we wanted to subtract two numbers we had to write e.g. subtract(2, 3). We would end up with a ton of parentheses for even simple expressions and it would be hard to understand what the expression actually does. Instead, we can write 2 - 3 instead, which is much shorter and more readable. Mavo supports all common math and logical operators, as well as some more like where.

All MavoScript operators

Special properties
Special properties are available in every Mavo, and start with a dollar sign ($). They contain metadata, such as the position of the current list item (as a number starting from 0) or the current date/time.

All MavoScript special properties

Raw data
A lot of the data you will be using in expressions will come from your properties. However, sometimes you want to provide your own values as parameters. These can be:

Numbers, by just writing the number and using . as the decimal separator, e.g. 5 or -0.3.
The keyword true and the keyword false represent yes/no values. For example, 30 > 3 is equal to true because 30 is indeed larger than 3, and 3 > 30 is equal to false because 3 is not larger than 30.
Textual values, such as "cat" or 'Mary'. You can use single quotes, double quotes, or even no quotes if the text contains only letters, numbers, and underscores. However, if you do that, beware of potential clashes with your property names!
Lists of values, just like those you get by referring a list property from outside the list, can be written as list(value1, value2, value3, …). You can read more about list() here.
Grouped key-value pairs, just like the data you get when you reference a group can be created by using the group() function. E.g. group(name: 'Vector', age: 12, hobby: list('Eating', 'Sleeping', 'Purring'))
Using Properties
We saw above that you can dynamically refer to the value of any property you have defined, anywhere, by putting its name inside brackets (like this: [propertyName]).

We can also do math with properties, e.g. divide by 100.

Example
Play!
Move the slider and check how both expressions are updated:

<p>Slider value: [strength/100]</p>
<input type="range" property="strength" title="[strength]%" />

If you set a property attribute on an element containing an expression, you can use that property in other expressions too, regardless of whether they come before or after the element.

Example
Play!
Making a bare-bones HSL color picker with Mavo expressions:

<div mv-app="colorpicker" mv-storage="local"
     style="background-color: [color];">
    <input type="range" property="hue" max="360" />
    <input type="range" property="saturation" />
    <input type="range" property="lightness" />

    <input property="color" readonly
           value="hsl([hue], [saturation]%, [lightness]%)" />

</div>

If you’re just using an element to hold an intermediate result so you can refer to it, we recommend using <meta property="propertyName" content="[expression]" /> which is hidden outside edit mode, both semantically and visually.

Properties whose values are expressions are called “computed properties” and are not saved.

Expression Functions
References and simple arithmetic are useful, but not always enough. For this purpose, Mavo defines a number of useful functions you can use inside expressions, such as: pow(base, exponent), round(number, decimals), sqrt(number) and many more.

Example
Play!
<input property="base" type="number" value="2" />
<input property="exponent" type="number" value="3" />
= [pow(base, exponent)]

If what you’re printing contains characters other than letters, numbers and underscores, you will need to use quotes around it, just like in CSS font-family or HTML attributes.

Expressions and Groups
Groups are properties that contain other properties, or that explicitly have a typeof attribute (also called "complex properties", and they are analogous to objects in programming languages).
If there are groups and there are properties with the same name inside and outside the group, the value closest to your expression will be used, with child (inner) groups having priority over parent (outer) groups.

Example
Play!

<p>Hi, I’m [firstname] [lastname]</p> <!-- From inner group -->

<div property="person">
    <span property="firstname">Lea</span>
    <span property="lastname">Verou</span>
    <p>Hi, I’m [firstname] [lastname]</p>

    <div property="mother">
        <span property="firstname">Maria</span>
        <!-- Last name from parent group -->
        <p>Hi, I’m [firstname] [lastname]</p>

        <div property="mother">
            <span property="firstname">Areti</span>
            <!-- Last name from grandparent group -->
            <p>Hi, I’m [firstname] [lastname]</p>
        </div>
    </div>

</div>

Expressions and Lists
Using a property with multiple values (i.e. referring to a property inside a list from outside the list) in an expression is not very useful on its own. However, there are functions that help you use such lists of values in interesting ways, such as count(property), sum(property), average(property) and more.

Example
Play!
Try adding and removing elements!

<div mv-app="catbook" mv-storage="#data">
    <h2>My cats</h2>
    <ul mv-list>
        <li property="cat" mv-list-item>
            <div property="name">Vector</div>
            <div property="age">10</div>
            <!-- Note that expression inside list item refers to the value in the current item -->
            <p>Age in 2 years: [age + 2]</p>
        </li>
    </ul>

    <h2>My cats’ ages</h2>
    <ul>
        <li>All cat ages: [age]</li>
        <li>Number of cats: [count(cat)]</li>
        <li>Number of cats with age: [count(age)]</li>
        <li>Average cat age: [average(age)]</li> <!-- avg(age) also works -->
        <li>Sum of all ages (why? because we can): [SUM(age)]</li> <!-- caps don’t matter -->
        <li>Number of cats over 5 years old: [count(age > 5)]</li>
    </ul>

</div>
<script type="application/json" id="data">{
    "cat": [
        {
            "name": "Vector",
            "age": 12
        },
        {
            "name": "Jean"
        },
        {
            "name": "Pixel",
            "age": 6
        },
        {
            "name": "Adam Catlace",
            "age": 2.5
        },
        {
            "name": "Eva"
        },
        {
            "name": "Chlebis",
            "age": 8
        }
    ]
}</script>

Note that if your expression is inside a list item, any properties inside that item will refer to their current values, whereas if your expression is outside the list you will get all values.

You can also do math or comparisons with lists. For example, [sum(age + 5)] will add 5 to every age and then sum the result. You can use either symbols (e.g. +, \*, >, ==) etc to perform these operations, or their function counterparts.

Conditionals: if() and the mv-if attribute
You can use the if(condition, ifyes, ifno) function to print out different text depending on a condition. The third parameter is optional: If you omit it, nothing will be printed out if the condition is false.

The if() function is very powerful, but can be confusing when all you want is to toggle content. For this, you can also use the mv-if attribute to hide or show content based on the value of an expression. Just like mv-value, mv-if does not require braces or other syntax. Besides readability, mv-if has another benefit: elements hidden with it are completely removed from the page, not just hidden.

Example
Play!

<style>
.bad { background: #fdd; }
.ok { background: #ffd; }
.good { background: #dfc; }
</style>
<div mv-app class="[if(rating < 3, bad, if(rating > 3, good, ok))]">
    <label>Rating:
        <input property="rating" type="range" min="1" max="5" />
    </label>

    <p mv-if="rating = 1">Wow, you hated that movie!</p>
    <p mv-if="rating > 3">Glad you liked it!</p>

</div>

Providing fallbacks: The mv-value attribute
If you are concerned about the expression content being shown while the page loads, you can use the mv-value attribute instead, and provide a fallback as content.

Example: Using mv-value
Play!

<p>Slider value: <span mv-value="strength">0</span>/100</p>
<input type="range" property="strength" />

mv-value does not require braces or whatever other syntax you have defined via mv-expressions. If you include them, they will be considered part of your expression. Also mv-value will work even when you specify mv-expressions="none" (from v0.1.5).

If your expression contains a mistake, the expression itself will be shown instead of a result. If you’re using mv-value however, its fallback content will be shown instead.

Example: Incorrect expressions
Play!
[5 +]

<div mv-value="5 +">Fallback</div>

Dynamic lists with mv-value
We’ve seen above how mv-value can be used to create fallback content for expressions. However, it’s much more powerful than that. When used on a list, mv-value renders its value on the entire list, enabling you to create lists of items that depend on other lists.

As a simple example, note how these two lists are synchronized:

Example
Play!

<div mv-list mv-mode="edit"><span property="list1" mv-list-item>[$index + 1]</span></div>
<div mv-list mv-value="list1"><span property="list2" mv-list-item></span></div>

The mv-attr-\* family of attributes
Mavo provides a way for attributes to contain expressions without triggering browser errors or false loading.

Suppose we have <img src="[foo]" />. Till the expression is evaluated, this causes a browser error since [foo] is not a valid URL. With the mv-attr-src, we can avoid this error, like so: <img src="fallback_URL" mv-attr-src="foo" />. When foo is evaluated, its value will replace fallback_URL, and a new image will be loaded.

If we have both attributes, foo and mv-attr-foo, the value of foo becomes the fallback value for the mv-attr-foo expression. At the same time, if foo is an expression too, its value will be ignored, i.e., mv-attr-foo has priority over foo.

Example
Play!

<meta property="url" content="https://designftw.mit.edu/lectures/html/img/adamcatlace.jpg" />
<img property="image"
     src="https://designftw.mit.edu/lectures/html/img/html5.jpg"
     mv-attr-src="url" />
<p>Image URL: [image]</p>

The value of mv-attr-\* is an expression, and it doesn't require braces (or whatever other syntax is defined via mv-expressions) around it. If you include them, they will be considered part of your expression.

Advanced MavoScript vs JavaScript
This section requires an understanding of JavaScript. It is aimed at advanced users and plugin developers. You do not need to understand JavaScript to use Mavo!

MavoScript is based on a subset of JavaScript, with a few simplifications to make it easy to use even by people with no JavaScript knowledge.

For those who know JavaScript, these are a few key differences:

JavaScript MavoScript
Math and logical operations with arrays produce nonsensical results Array math & logical operations works element-wise. Operations between an array and a scalar are performed on every array element. This is why things like count(rating > 3) work in MavoScript, but wouldn't in JS.
All strings have to be quoted Strings that only consist of letters, numbers, and underscores don't need quotes.
== operator for equality = operator for equality
&& operator for logical and and operator for logical and
|| operator for logical or or operator for logical or
% operator for remainder mod operator for remainder
if is a control structure if() is a function
a + b might be addition or concatenation depending on types a + b is always addition, a & b is concatenation
Things like 3 > 2 > 1 return unexpected results (false in this case) Every operator can have multiple operands, including >, so 3 > 2 > 1 has the same result as in math (true)
foo.bar.baz will throw an error if either foo or foo.bar don’t exist. If either foo or foo.bar don’t exist, foo.bar.baz will just return "".
If you need something more advanced, you can invoke custom JavaScript functions (both synchronous and asynchronous) in your expressions. Function calls are perfectly valid MavoScript.

Example: Function calls
Play!
Invoke the encodeURIComponent() function to encode the URL passed to it as a parameter.

<div mv-app="function_calls">
  <input type="url" property="url" value="https://mavo.io/docs/storage#url-params" />
  <output>[encodeURIComponent(url)]</output>
</div>
