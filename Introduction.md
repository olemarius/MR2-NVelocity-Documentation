##User Guide - Contents

The original source of this user guide can be found at http://velocity.apache.org/engine/devel/user-guide.html

* ####[About this Guide](#about-this-guide)
	* [What is Velocity?](#what-is-velocity)

* ####[What can Velocity do for me?](#what-can-velocity-do-for-me)
	* [The Mud Store Example](#the-mud-store-example)

* ####[What jar should I use?](#what-jar-should-i-use)
	* [Maven users](#maven-users)
	* [Other users](#other-users)

* ####[Velocity Template Language (VTL): An Introduction](#velocity-template-language-vtl-an-introduction)

* ####[Hello Velocity World!](#hello-velocity-world)

* ####[Comments](#comments)

* ####[References](#references)
	* [Variables](#variables)
	* [Properties](#properties)
	* [Methods](#methods)
	* [Property Lookup Rules](#property-lookup-rules)
	* [Rendering](#rendering)
	* [Index Notation](#index-notation)

* ####[Formal Reference Notation](#formal-reference-notation)

* ####[Quiet Reference Notation](#quiet-reference-notation)

* ####[Strict Reference Mode](#string-reference-mode)

* ####[Case Substitution](#case-substitution)

* ####[Directives](#directives)
	* [Set](#set)
	* [Literals](#literals)
	* [If-Else Statements](#if-else-statements)
	  * [Relational and Logical Operators](#relational-and-logical-operators)
	* [Foreach Loops](#foreach-loops)
	* [Include](#include)
	* [Parse](#parse)
	* [Break](#break)
	* [Stop](#stop)
	* [Evaluate](#evaluate)
	* [Define](#define)
	* [Velocimacros](#velocimacros)

* ####[Getting literal](#getting-literal)
	* [Currency](#currency)
	* [Escaping Valid VTL References](#escaping-valid-vtl-references)
	* [Escaping Invalid VTL References](#escaping-invalid-vtl-references)
	* [Escaping VTL Directives](#escaping-vtl-directives)

* ####[VTL: Formatting Issues](#vtl-formatting-issues)
* ####[Other Features and Miscellany](#other-features-and-miscellany)
	* [Math](#math)
	* [Range Operator](#range-operator)
	* [Advanced Issues: Escaping and !](#advanced-issues-escaping-and-!)
	* [Velocimacro Miscellany](#velocitymacro-miscellany)
	* [String Concatenation](#string-concatenation)

## <a name="about-this-guide">About this Guide</a>

The Velocity User Guide is intended to help page designers and content providers get acquainted with Velocity and the syntax of its simple yet powerful scripting language, the Velocity Template Language (VTL). Many of the examples in this guide deal with using Velocity to embed dynamic content in web sites, but all VTL examples are equally applicable to other pages and templates.

Thanks for choosing Velocity!

###<a name="what-is-velocity">What is Velocity?</a>

Velocity is a Java-based template engine. It permits web page designers to reference methods defined in Java code. Web designers can work in parallel with Java programmers to develop web sites according to the Model-View-Controller (MVC) model, meaning that web page designers can focus solely on creating a well-designed site, and programmers can focus solely on writing top-notch code. Velocity separates Java code from the web pages, making the web site more maintainable over the long run and providing a viable alternative to Java Server Pages (JSPs) or PHP.

Velocity can be used to generate web pages, SQL, PostScript and other output from templates. It can be used either as a standalone utility for generating source code and reports, or as an integrated component of other systems. When complete, Velocity will provide template services for the Turbine web application framework. Velocity+Turbine will provide a template service that will allow web applications to be developed according to a true MVC model.

##<a name="what-can-velocity-do-for-me">What can Velocity do for me?</a>

###<a name="the-mud-store-example">The Mud Store Example</a>

Suppose you are a page designer for an online store that specializes in selling mud. Let's call it "The Online Mud Store". Business is thriving. Customers place orders for various types and quantities of mud. They login to your site using their username and password, which allows them to view their orders and buy more mud. Right now, Terracotta Mud is on sale, which is very popular. A minority of your customers regularly buys Bright Red Mud, which is also on sale, though not as popular and usually relegated to the margin of your web page. Information about each customer is tracked in your database, so one day the question arises, Why not use Velocity to target special deals on mud to the customers who are most interested in those types of mud?

Velocity makes it easy to customize web pages to your online visitors. As a web site designer at The Mud Room, you want to make the web page that the customer will see after logging into your site.

You meet with software engineers at your company, and everyone has agreed that $customer will hold information pertaining to the customer currently logged in, that $mudsOnSpecial will be all the types mud on sale at present. The $flogger object contains methods that help with promotion. For the task at hand, let's concern ourselves only with these three references. Remember, you don't need to worry about how the software engineers extract the necessary information from the database, you just need to know that it works. This lets you get on with your job, and lets the software engineers get on with theirs.

You could embed the following VTL statement in the web page:

```html
<html>
<body>
Hello $customer.Name!
<table>
#foreach( $mud in $mudsOnSpecial )
   #if ( $customer.hasPurchased($mud) )
      <tr>
        <td>
          $flogger.getPromo( $mud )
        </td>
      </tr>
   #end
#end
</table>
```

The exact details of the foreach statement will be described in greater depth shortly; what's important is the impact this short script can have on your web site. When a customer with a penchant for Bright Red Mud logs in, and Bright Red Mud is on sale, that is what this customer will see, prominently displayed. If another customer with a long history of Terracotta Mud purchases logs in, the notice of a Terracotta Mud sale will be front and center. The flexibility of Velocity is enormous and limited only by your creativity.

Documented in the VTL Reference are the many other Velocity elements, which collectively give you the power and flexibility you need to make your web site a web presence. As you get more familiar with these elements, you will begin to unleash the power of Velocity.

##<a name="what-jar-should-i-use">What jar should I use?</a>

Velocity Engine 2.x distribution has several jars, related to the Maven artifacts that are published to the Maven repository.

###<a name="maven-users">Maven users</a>

Include the following dependency to your POM file:

```xml
<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-core</artifactId>
  <version>x.x.x</version>
</dependency>
``` 

If you want to connect Velocity log facility to Commons Logging, SLF4J, Log4j or the servlet logger, choose the appropriate dependency:

```xml
<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-commons-logging</artifactId>
  <version>x.x.x</version>
</dependency>

<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-slf4j</artifactId>
  <version>x.x.x</version>
</dependency>

<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-log4j</artifactId>
  <version>x.x.x</version>
</dependency>

<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-servlet</artifactId>
  <version>x.x.x</version>
</dependency>
```
    

###<a name="other-users">Other users</a>

First of all download Velocity Engine distribution.

If you want to jumpstart your application, simply include velocity-x.x.x.jar in your classpath: it includes all classes of Velocity Engine (with the exclusion of the examples). Include all the jars in the "lib" subdirectory too.

The other jars are finely-grained jars that contain only what you might need.

- velocity-engine-core-x.x.x.jar: The main Velocity library. It must be included at all times.
- velocity-engine-commons-logging-x.x.x.jar: It contains the binding to Commons Logging, so all log messages will be directed to it. To be used in conjunction with Commons Logging (included in "lib" directory).
- velocity-engine-slf4j-x.x.x.jar: It contains the binding to SLF4J, so all log messages will be directed to it. To be used in conjunction with SLF4J (included in "lib" directory).
- velocity-engine-log4j-x.x.x.jar: It contains the binding to Log4j, so all log messages will be directed to it. To be used in conjunction with Log4j (included in "lib" directory).
- velocity-engine-servlet-x.x.x.jar: It contains the binding to the servlet logger. To be used in a servlet container.

The extra jars you will always need always are Commons Collections and Commons Lang, included in "lib" directory.


##<a name="velocity-template-language-vtl-an-introduction">Velocity Template Language (VTL): An Introduction</a>

The Velocity Template Language (VTL) is meant to provide the easiest, simplest, and cleanest way to incorporate dynamic content in a web page. Even a web page developer with little or no programming experience should soon be capable of using VTL to incorporate dynamic content in a web site.

VTL uses references to embed dynamic content in a web site, and a variable is one type of reference. Variables are one type of reference that can refer to something defined in the Java code, or it can get its value from a VTL statement in the web page itself. Here is an example of a VTL statement that could be embedded in an HTML document:

```text
#set( $a = "Velocity" )
```

This VTL statement, like all VTL statements, begins with the # character and contains a directive: set. When an online visitor requests your web page, the Velocity Templating Engine will search through your web page to find all \# characters, then determine which mark the beginning of VTL statements, and which of the # characters that have nothing to do with VTL.

The \# character is followed by a directive, set. The set directive uses an expression (enclosed in brackets) -- an equation that assigns a value to a variable. The variable is listed on the left hand side and its value on the right hand side; the two are separated by an = character.

In the example above, the variable is $a and the value is Velocity. This variable, like all references, begins with the $ character. String values are always enclosed in quotes, either single or double quotes. Single quotes will ensure that the quoted value will be assigned to the reference as is. Double quotes allow you to use velocity references and directives to interpolate, such as "Hello $name", where the $name will be replaced by the current value before that string literal is assigned to the left hand side of the =

The following rule of thumb may be useful to better understand how Velocity works: References begin with $ and are used to get something. Directives begin with # and are used to do something.

In the example above, #set is used to assign a value to a variable. The variable, $a, can then be used in the template to output "Velocity".

##<a name="hello-velocity-world">Hello Velocity World!</a>

Once a value has been assigned to a variable, you can reference the variable anywhere in your HTML document. In the following example, a value is assigned to $foo and later referenced.

```html
<html>
<body>
#set( $foo = "Velocity" )
Hello $foo World!
</body>
<html>
```

The result is a web page that prints "Hello Velocity World!".

To make statements containing VTL directives more readable, we encourage you to start each VTL statement on a new line, although you are not required to do so. The set directive will be revisited in greater detail later on.


##<a name="comments">Comments</a>

Comments allows descriptive text to be included that is not placed into the output of the template engine. Comments are a useful way of reminding yourself and explaining to others what your VTL statements are doing, or any other purpose you find useful. Below is an example of a comment in VTL.

```text
## This is a single line comment.
```

A single line comment begins with ## and finishes at the end of the line. If you're going to write a few lines of commentary, there's no need to have numerous single line comments. Multi-line comments, which begin with #* and end with *#, are available to handle this scenario.

This is text that is outside the multi-line comment.
Online visitors can see it.

```text
#*
 Thus begins a multi-line comment. Online visitors won't
 see this text because the Velocity Templating Engine will
 ignore it.
*#
```

Here is text outside the multi-line comment; it is visible.

Here are a few examples to clarify how single line and multi-line comments work:

	This text is visible. ## This text is not.
	This text is visible.
	This text is visible. #* This text, as part of a multi-line
	comment, is not visible. This text is not visible; it is also
	part of the multi-line comment. This text still not
	visible. *# This text is outside the comment, so it is visible.
	## This text is not visible.

There is a third type of comment, the VTL comment block, which may be used to store any sort of extra information you want to track in the template (e.g. javadoc-style author and versioning information):

```text
#**
This is a VTL comment block and
may be used to store such information
as the document author and versioning
information:
@author
@version 5
*#
```

##<a name="references">References</a>

There are three types of references in the VTL: variables, properties and methods. As a designer using the VTL, you and your engineers must come to an agreement on the specific names of references so you can use them correctly in your templates.

###<a name="variables">Variables</a>

The shorthand notation of a variable consists of a leading "$" character followed by a VTL Identifier. A VTL Identifier must start with an alphabetic character (a .. z or A .. Z). The rest of the characters are limited to the following types of characters:


- alphabetic (a .. z, A .. Z)
- numeric (0 .. 9)
- hyphen ("-")
- underscore ("_")


Here are some examples of valid variable references in the VTL:

```text
$foo
$mudSlinger
$mud-slinger
$mud_slinger
$mudSlinger1
```

When VTL references a variable, such as $foo, the variable can get its value from either a set directive in the template, or from the Java code. For example, if the Java variable $foo has the value bar at the time the template is requested, bar replaces all instances of $foo on the web page. Alternatively, if I include the statement

```text
#set( $foo = "bar" )
```

The output will be the same for all instances of $foo that follow this directive.

###<a name="properties">Properties</a>

The second flavor of VTL references are properties, and properties have a distinctive format. The shorthand notation consists of a leading $ character followed a VTL Identifier, followed by a dot character (".") and another VTL Identifier. These are examples of valid property references in the VTL:

```text
$customer.Address
$purchase.Total
```

Take the first example, $customer.Address. It can have two meanings. It can mean, Look in the hashtable identified as customer and return the value associated with the key Address. But $customer.Address can also be referring to a method (references that refer to methods will be discussed in the next section); $customer.Address could be an abbreviated way of writing $customer.getAddress(). When your page is requested, Velocity will determine which of these two possibilities makes sense, and then return the appropriate value.

###<a name="methods">Methods</a>
A method is defined in the Java code and is capable of doing something useful, like running a calculation or arriving at a decision. Methods are references that consist of a leading "$" character followed a VTL Identifier, followed by a VTL Method Body. A VTL Method Body consists of a VTL Identifier followed by an left parenthesis character ("("), followed by an optional parameter list, followed by right parenthesis character (")"). These are examples of valid method references in the VTL:

```text
$customer.getAddress()
$purchase.getTotal()
$page.setTitle( "My Home Page" )
$person.setAttributes( ["Strange", "Weird", "Excited"] )
```

The first two examples -- $customer.getAddress() and $purchase.getTotal() -- may look similar to those used in the Properties section above, $customer.Address and $purchase.Total. If you guessed that these examples must be related some in some fashion, you are correct!

VTL Properties can be used as a shorthand notation for VTL Methods. The Property $customer.Address has the exact same effect as using the Method $customer.getAddress(). It is generally preferable to use a Property when available. The main difference between Properties and Methods is that you can specify a parameter list to a Method.

The shorthand notation can be used for the following Methods

```text
$sun.getPlanets()
$annelid.getDirt()
$album.getPhoto()
```

We might expect these methods to return the names of planets belonging to the sun, feed our earthworm, or get a photograph from an album. Only the long notation works for the following Methods.

```text
$sun.getPlanet( ["Earth", "Mars", "Neptune"] )
## Can't pass a parameter list with $sun.Planets

$sisyphus.pushRock()
## Velocity assumes I mean $sisyphus.getRock()

$book.setTitle( "Homage to Catalonia" )
## Can't pass a parameter
```

As of Velocity 1.6, all array references are now "magically" treated as if they are fixed-length lists. This means that you can call java.util.List methods on array references. So, if you have a reference to an array (let's say this one is a String[] with three values), you can do:

```text
$myarray.isEmpty()
$myarray.size()
$myarray.get(2)
$myarray.set(1, 'test')
```

Also new in Velocity 1.6 is support for vararg methods. A method like public void setPlanets(String... planets) or even just public void setPlanets(String[] planets) (if you are using a pre-Java 5 JDK), can now accept any number of arguments when called in a template.

```text
$sun.setPlanets('Earth', 'Mars', 'Neptune')

$sun.setPlanets('Mercury')

$sun.setPlanets()
## Will just pass in an empty, zero-length array
```

###<a name="property-lookup-rules">Property Lookup Rules</a>
As was mentioned earlier, properties often refer to methods of the parent object. Velocity is quite clever when figuring out which method corresponds to a requested property. It tries out different alternatives based on several established naming conventions. The exact lookup sequence depends on whether or not the property name starts with an upper-case letter. For lower-case names, such as $customer.address, the sequence is

```text
getaddress()
getAddress()
get("address")
isAddress()
```

For upper-case property names like $customer.Address, it is slightly different:

```text
getAddress()
getaddress()
get("Address")
isAddress()
```

###<a name="rendering">Rendering</a>
The final value resulting from each and every reference (whether variable, property, or method) is converted to a String object when it is rendered into the final output. If there is an object that represents $foo (such as an Integer object), then Velocity will call its .toString() method to resolve the object into a String.

###<a name="index-notation">Index Notation</a>
Using the notation of the form $foo[0] can be used to access a given index of an object. This form is synonymous with calling the get(Object) method on a given object i.e, $foo.get(0), and provides essentially a syntactic shorthand for such operations. Since this simply calls the get method all of the following are valid uses:

```text
$foo[0]       ## $foo takes in an Integer look up
$foo[$i]      ## Using another reference as the index
$foo["bar"]   ## Passing a string where $foo may be a Map
```

The bracketed syntax also works with Java arrays since Velocity wraps arrays in an access object that provides a get(Integer) method which returns the specified element.

The bracketed syntax is valid anywhere .get is valid, for example:

```text
$foo.bar[1].junk
$foo.callMethod()[1]
$foo["apple"][4]
```

A reference can also be set using index notation, for example:

```text
#set($foo[0] = 1)
#set($foo.bar[1] = 3)
#set($map["apple"] = "orange")
```

The specified element is set with the given value. Velocity tries first the 'set' method on the element, then 'put' to make the assignment.

###<a name="formal-reference-notation">Formal Reference Notation</a>
Shorthand notation for references was used for the examples listed above, but there is also a formal notation for references, which is demonstrated below:

```text
${mudSlinger}
${customer.Address}
${purchase.getTotal()}
```

In almost all cases you will use the shorthand notation for references, but in some cases the formal notation is required for correct processing.

Suppose you were constructing a sentence on the fly where $vice was to be used as the base word in the noun of a sentence. The goal is to allow someone to choose the base word and produce one of the two following results: "Jack is a pyromaniac." or "Jack is a kleptomaniac.". Using the shorthand notation would be inadequate for this task. Consider the following example:

```Jack is a $vicemaniac.```

There is ambiguity here, and Velocity assumes that $vicemaniac, not $vice, is the Identifier that you mean to use. Finding no value for $vicemaniac, it will return $vicemaniac. Using formal notation can resolve this problem.

```Jack is a ${vice}maniac.```

Now Velocity knows that $vice, not $vicemaniac, is the reference. Formal notation is often useful when references are directly adjacent to text in a template.

###<a name="quiet-reference-notation">Quiet Reference Notation</a>
When Velocity encounters an undefined reference, its normal behavior is to output the image of the reference. For example, suppose the following reference appears as part of a VTL template.

```html
<input type="text" name="email" value="$email"/>
```

When the form initially loads, the variable reference $email has no value, but you prefer a blank text field to one with a value of "$email". Using the quiet reference notation circumvents Velocity's normal behavior; instead of using $email in the VTL you would use $!email. So the above example would look like the following:

```html
<input type="text" name="email" value="$!email"/>
```

Now when the form is initially loaded and $email still has no value, an empty string will be output instead of "$email".

Formal and quiet reference notation can be used together, as demonstrated below.

```html
<input type="text" name="email" value="$!{email}"/>
```

##<a name="strict-reference-mode">Strict Reference Mode</a>

Velocity 1.6 introduces the concept of strict reference mode which is activated by setting the velocity configuration property 'runtime.references.strict' to true. The general intent of this setting is to make Velocity behave more strictly in cases that are undefined or ambiguous, similar to a programming language, which may be more appropriate for some uses of Velocity. In such undefined or ambiguous cases Velocity will throw an exception. The following discussion outlines the cases in which strict behavior is different from traditional behavior.

With this setting references are required to be either placed explicitly into the context or defined with a #set directive or Velocity will throw an exception. References that are in the context with a value of null will not produce an exception. Additionally, if an attempt is made to call a method or a property on an object within a reference that does not define the specified method or property then Velocity will throw an exception. This is also true if there is an attempt to call a method or property on a null value.

In the following examples $bar is defined but $foo is not, and all these statements will throw an exception:

```text
$foo                         ## Exception
#set($bar = $foo)            ## Exception
#if($foo == $bar)#end        ## Exception
#foreach($item in $foo)#end  ## Exception
```

Also, The following statements show examples in which Velocity will throw an exception when attempting to call methods or properties that do not exist. In these examples $bar contains an object that defines a property 'foo' which returns a string, and 'retnull' which returns null.

```text
$bar.bogus          ## $bar does not provide property bogus, Exception
$bar.foo.bogus      ## $bar.foo does not provide property bogus, Exception
$bar.retnull.bogus  ## cannot call a property on null, Exception
```

In general strict reference behavior is true for all situations in which references are used except for a special case within the #if directive. If a reference is used within a #if or #elseif directive without any methods or properties, and if it is not being compared to another value, then undefined references are allowed. This behavior provides an easy way to test if a reference is defined before using it in a template. In the following example where $foo is not defined the statements will not throw an exception.

```text
#if ($foo)#end                  ## False
#if ( ! $foo)#end               ## True
#if ($foo && $foo.bar)#end      ## False and $foo.bar will not be evaluated
#if ($foo && $foo == "bar")#end ## False and $foo == "bar" wil not be evaluated
#if ($foo1 || $foo2)#end        ## False $foo1 and $foo2 are not defined
```

Strict mode requires that comparisons of >, <, >= or <= within an #if directive makes sense. Also, the argument to #foreach must be iterable (this behavior can be modified with the property directive.foreach.skip.invalid). Finally, undefined macro references will also throw an exception in strict mode.

References that Velocity attempts to render but evaluate to null will cause an Exception. To simply render nothing in this case the reference can be preceded by '$!' instead of '$', similar to non strict mode. Keep in mind this is different from the reference not existing in the context which will always throw an exception when attempting to render it in strict mode. For example, below $foo has a value of null in the context

```text
this is $foo    ## throws an exception because $foo is null
this is $!foo   ## renders to "this is " without an exception
this is $!bogus ## bogus is not in the context so throws an exception
```

##<a name="case-substitution">Case Substitution</a>

Now that you are familiar with references, you can begin to apply them effectively in your templates. Velocity references take advantage of some Java principles that template designers will find easy to use. For example:

```
$foo

$foo.getBar()
## is the same as
$foo.Bar

$data.setUser("jon")
## is the same as
#set( $data.User = "jon" )

$data.getRequest().getServerName()
## is the same as
$data.Request.ServerName
## is the same as
${data.Request.ServerName}
```

These examples illustrate alternative uses for the same references. Velocity takes advantage of Java's introspection and bean features to resolve the reference names to both objects in the Context as well as the objects methods. It is possible to embed and evaluate references almost anywhere in your template.

Velocity, which is modelled on the Bean specifications defined by Sun Microsystems, is case sensitive; however, its developers have strove to catch and correct user errors wherever possible. When the method getFoo() is referred to in a template by $bar.foo, Velocity will first try $getfoo. If this fails, it will then try $getFoo. Similarly, when a template refers to $bar.Foo, Velocity will try $getFoo() first and then try getfoo().

Note: References to instance variables in a template are not resolved. Only references to the attribute equivalents of JavaBean getter/setter methods are resolved (i.e. $foo.Name does resolve to the class Foo's getName() instance method, but not to a public Name instance variable of Foo).

##<a name="directives">Directives</a>

References allow template designers to generate dynamic content for web sites, while directives -- easy to use script elements that can be used to creatively manipulate the output of Java code -- permit web designers to truly take charge of the appearance and content of the web site.

Directives always begin with a #. Like references, the name of the directive may be bracketed by a { and a } symbol. This is useful with directives that are immediately followed by text. For example the following produces an error:

```
#if($a==1)true enough#elseno way!#end
```

In such a case, use the brackets to separate #else from the rest of the line.

```
#if($a==1)true enough#{else}no way!#end
```


###<a name="set">\#set</a>

The #set directive is used for setting the value of a reference. A value can be assigned to either a variable reference or a property reference, and this occurs in brackets, as demonstrated:

```
#set( $primate = "monkey" )
#set( $customer.Behavior = $primate )
```

The left hand side (LHS) of the assignment must be a variable reference or a property reference. The right hand side (RHS) can be one of the following types:


    Variable reference
    String literal
    Property reference
    Method reference
    Number literal
    ArrayList


These examples demonstrate each of the aforementioned types:

```
#set( $monkey = $bill ) ## variable reference
#set( $monkey.Friend = "monica" ) ## string literal
#set( $monkey.Blame = $whitehouse.Leak ) ## property reference
#set( $monkey.Plan = $spindoctor.weave($web) ) ## method reference
#set( $monkey.Number = 123 ) ##number literal
#set( $monkey.Say = ["Not", $my, "fault"] ) ## ArrayList
#set( $monkey.Map = {"banana" : "good", "roast beef" : "bad"}) ## Map
```

NOTE: For the ArrayList example the elements defined with the [..] operator are accessible using the methods defined in the ArrayList class. So, for example, you could access the first element above using $monkey.Say.get(0).

Similarly, for the Map example, the elements defined within the { } operator are accessible using the methods defined in the Map class. So, for example, you could access the first element above using $monkey.Map.get("banana") to return a String 'good', or even $monkey.Map.banana to return the same value.

The RHS can also be a simple arithmetic expression:

```
#set( $value = $foo + 1 )
#set( $value = $bar - 1 )
#set( $value = $foo * $bar )
#set( $value = $foo / $bar )
```

If the RHS is a property or method reference that evaluates to null, it will not be assigned to the LHS. Depending on how Velocity is configured, it is usually not possible to remove an existing reference from the context via this mechanism. (Note that this can be permitted by changing one of the Velocity configuration properties). This can be confusing for newcomers to Velocity. For example:

```
#set( $result = $query.criteria("name") )
The result of the first query is $result

#set( $result = $query.criteria("address") )
The result of the second query is $result
```

If $query.criteria("name") returns the string "bill", and $query.criteria("address") returns null, the above VTL will render as the following:

```
The result of the first query is bill

The result of the second query is bill
```

This tends to confuse newcomers who construct #foreach loops that attempt to #set a reference via a property or method reference, then immediately test that reference with an #if directive. For example:

```
#set( $criteria = ["name", "address"] )

#foreach( $criterion in $criteria )

    #set( $result = $query.criteria($criterion) )

    #if( $result )
        Query was successful
    #end

#end
```

In the above example, it would not be wise to rely on the evaluation of $result to determine if a query was successful. After $result has been #set (added to the context), it cannot be set back to null (removed from the context). The details of the #if and #foreach directives are covered later in this document.

One solution to this would be to pre-set $result to false. Then if the $query.criteria() call fails, you can check.

```
#set( $criteria = ["name", "address"] )

#foreach( $criterion in $criteria )

    #set( $result = false )
    #set( $result = $query.criteria($criterion) )

    #if( $result )
        Query was successful
    #end

#end
```

Unlike some of the other Velocity directives, the #set directive does not have an #end statement.

##<a name="literals">Literals</a>

When using the #set directive, string literals that are enclosed in double quote characters will be parsed and rendered, as shown:

```
#set( $directoryRoot = "www" )
#set( $templateName = "index.vm" )
#set( $template = "$directoryRoot/$templateName" )
$template
```

The output will be

www/index.vm

However, when the string literal is enclosed in single quote characters, it will not be parsed:

```
#set( $foo = "bar" )
$foo
#set( $blargh = '$foo' )
$blargh
```

This renders as:

```
  bar
  $foo
```

By default, this feature of using single quotes to render unparsed text is available in Velocity. This default can be changed by editing velocity.properties such that stringliterals.interpolate=false.

Alternately, the #[[don't parse me!]]# syntax allows the template designer to easily use large chunks of uninterpreted and unparsed content in VTL code. This can be especially useful in place of escaping multiple directives or escaping sections which have content that would otherwise be invalid (and thus unparseable) VTL.

```
#[[
#foreach ($woogie in $boogie)
  nothing will happen to $woogie
#end
]]#
```

Renders as:

```
#foreach ($woogie in $boogie)
  nothing will happen to $woogie
#end
```

##<a name="if-else-statements">Conditionals</a>
###If / ElseIf / Else

The #if directive in Velocity allows for text to be included when the web page is generated, on the conditional that the if statement is true. For example:

```
#if( $foo )
   <strong>Velocity!</strong>
#end
```

The variable $foo is evaluated to determine whether it is true, which will happen under one of three circumstances:

- ```$foo``` is a boolean (true/false) which has a true value
- ```$foo``` is a string or a collection which is not null and not empty
- ```$foo``` is an object (other than a string or a collection) which is not null

Remember that the Velocity context only contains Objects, so when we say 'boolean', it will be represented as a Boolean (the class). This is true even for methods that return boolean - the introspection infrastructure will return a Boolean of the same logical value.

The content between the ```#if``` and the ```#end``` statements become the output if the evaluation is true. In this case, if $foo is true, the output will be: "Velocity!". Conversely, if $foo has a null value, or if it is a boolean false, the statement evaluates as false, and there is no output.

An ```#elseif``` or ```#else``` element can be used with an ```#if``` element. Note that the Velocity Templating Engine will stop at the first expression that is found to be true. In the following example, suppose that ```$foo``` has a value of 15 and ```$bar``` has a value of 6.

```html
#if( $foo < 10 )
    <strong>Go North</strong>
#elseif( $foo == 10 )
    <strong>Go East</strong>
#elseif( $bar == 6 )
    <strong>Go South</strong>
#else
    <strong>Go West</strong>
#end
```

In this example, $foo is greater than 10, so the first two comparisons fail. Next ```$bar``` is compared to 6, which is true, so the output is Go South.

####<a name="relational-and-logical-operators">Relational and Logical Operators</a>

Velocity uses the equivalent operator to determine the relationships between variables. Here is a simple example to illustrate how the equivalent operator is used.

```text
#set ($foo = "deoxyribonucleic acid")
#set ($bar = "ribonucleic acid")

#if ($foo == $bar)
  In this case it's clear they aren't equivalent. So...
#else
  They are not equivalent and this will be the output.
#end
```

Note that the semantics of == are slightly different than Java where == can only be used to test object equality. In Velocity the equivalent operator can be used to directly compare numbers, strings, or objects. When the objects are of different classes, the string representations are obtained by calling toString() for each object and then compared.

Velocity has logical AND, OR and NOT operators as well. Below are examples demonstrating the use of the logical AND, OR and NOT operators.

```html
## logical AND

#if( $foo && $bar )
   <strong> This AND that</strong>
#end
```

The #if() directive will only evaluate to true if both $foo and $bar are true. If $foo is false, the expression will evaluate to false; $bar will not be evaluated. If $foo is true, the Velocity Templating Engine will then check the value of $bar; if $bar is true, then the entire expression is true and This AND that becomes the output. If $bar is false, then there will be no output as the entire expression is false.

Logical OR operators work the same way, except only one of the references need evaluate to true in order for the entire expression to be considered true. Consider the following example.

```html
## logical OR

#if( $foo || $bar )
    <strong>This OR That</strong>
#end
```
If ```$foo``` is true, the Velocity Templating Engine has no need to look at ```$bar```; whether ```$bar``` is true or false, the expression will be true, and This OR That will be output. If ```$foo``` is false, however, $bar must be checked. In this case, if $bar is also false, the expression evaluates to false and there is no output. On the other hand, if $bar is true, then the entire expression is true, and the output is This OR That

With logical NOT operators, there is only one argument :

```html
##logical NOT

#if( !$foo )
  <strong>NOT that</strong>
#end
```

Here, the if ```$foo``` is true, then ```!$foo``` evaluates to false, and there is no output. If ```$foo``` is false, then ```!$foo``` evaluates to true and NOT that will be output. Be careful not to confuse this with the quiet reference ```$!foo``` which is something altogether different.

There are text versions of all logical operators, including ```eq```, ```ne```, ```and```, ```or```, ```not```, ```gt```, ```ge```, ```lt```, and ```le```.

One more useful note. When you wish to include text immediately following a ```#else``` directive you will need to use curly brackets immediately surrounding the directive to differentiate it from the following text. (Any directive can be delimited by curly brackets, although this is most useful for ```#else```).

```
#if( $foo == $bar)it's true!#{else}it's not!#end</li>
```

##<a name="loops">Loops</a>
###<a name="foreach-loops">Foreach Loop</a>

The #foreach element allows for looping. For example:

```html
<ul>
#foreach( $product in $allProducts )
    <li>$product</li>
#end
</ul>
```

This #foreach loop causes the $allProducts list (the object) to be looped over for all of the products (targets) in the list. Each time through the loop, the value from $allProducts is placed into the $product variable.

The contents of the $allProducts variable is a Vector, a Hashtable or an Array. The value assigned to the $product variable is a Java Object and can be referenced from a variable as such. For example, if $product was really a Product class in Java, its name could be retrieved by referencing the $product.Name method (ie: $Product.getName()).

Lets say that $allProducts is a Hashtable. If you wanted to retrieve the key values for the Hashtable as well as the objects within the Hashtable, you can use code like this:

```html
<ul>
#foreach( $key in $allProducts.keySet() )
    <li>Key: $key -> Value: $allProducts.get($key)</li>
#end
</ul>
```

Velocity provides an easy way to get the loop counter so that you can do something like the following:

```html
<table>
#foreach( $customer in $customerList )
    <tr><td>$foreach.count</td><td>$customer.Name</td></tr>
#end
</table>
```

Velocity also now provides an easy way to tell if you are on the last iteration of a loop:

```
#foreach( $customer in $customerList )
    $customer.Name#if( $foreach.hasNext ),#end
#end
```

If you want a zero-based index of the #foreach loop, you can just use $foreach.index instead of $foreach.count. Likewise, $foreach.first and $foreach.last are provided to compliment $foreach.hasNext. If you want to access these properties for an outer #foreach loop, you can reference them directly through the $foreach.parent or $foreach.topmost properties (e.g. $foreach.parent.index or $foreach.topmost.hasNext).

It's possible to set a maximum allowed number of times that a loop may be executed. By default there is no max (indicated by a value of 0 or less), but this can be set to an arbitrary number in the velocity.properties file. This is useful as a fail-safe.

```text
# The maximum allowed number of loops.
directive.foreach.maxloops = -1
```

If you want to stop looping in a foreach from within your template, you can now use the #break directive to stop looping at any time:

```
## list first 5 customers only
#foreach( $customer in $customerList )
    #if( $foreach.count > 5 )
        #break
    #end
    $customer.Name
#end
```

##<a name="include">Include</a>

The #include script element allows the template designer to import a local file, which is then inserted into the location where the #include directive is defined. The contents of the file are not rendered through the template engine. For security reasons, the file to be included may only be under TEMPLATE_ROOT.

```
#include( "one.txt" )
```

The file to which the #include directive refers is enclosed in quotes. If more than one file will be included, they should be separated by commas.

```
#include( "one.gif","two.txt","three.htm" )
```

The file being included need not be referenced by name; in fact, it is often preferable to use a variable instead of a filename. This could be useful for targeting output according to criteria determined when the page request is submitted. Here is an example showing both a filename and a variable.

```
#include( "greetings.txt", $seasonalstock )
```
```
##<a name="parse">Parse</a>
```

The #parse script element allows the template designer to import a local file that contains VTL. Velocity will parse the VTL and render the template specified.

```
#parse( "me.vm" )
```

Like the #include directive, #parse can take a variable rather than a template. Any templates to which #parse refers must be included under TEMPLATE_ROOT. Unlike the #include directive, #parse will only take a single argument.

VTL templates can have #parse statements referring to templates that in turn have #parse statements. By default set to 10, the directive.parse.max.depth line of the velocity.properties allows users to customize maximum number of #parse referrals that can occur from a single template. (Note: If the directive.parse.max.depth property is absent from the velocity.properties file, Velocity will set this default to 10.) Recursion is permitted, for example, if the template dofoo.vm contains the following lines:

```
Count down.
#set( $count = 8 )
#parse( "parsefoo.vm" )
All done with dofoo.vm!
```

It would reference the template parsefoo.vm, which might contain the following VTL:

```
$count
#set( $count = $count - 1 )
#if( $count > 0 )
    #parse( "parsefoo.vm" )
#else
    All done with parsefoo.vm!
#end
```

After "Count down." is displayed, Velocity passes through parsefoo.vm, counting down from 8. When the count reaches 0, it will display the "All done with parsefoo.vm!" message. At this point, Velocity will return to dofoo.vm and output the "All done with dofoo.vm!" message.
Break

The #break directive stops any further rendering of the current execution scope. An "execution scope" is essentially any directive with content (i.e. #foreach, #parse, #evaluate, #define, #macro, or #@somebodymacro) or any "root" scope (i.e. template.merge(...), Velocity.evaluate(...) or velocityEngine.evaluate(...)). Unlike #stop, #break will only stop the innermost, immediate scope, not all of them.

If you wish to break out of a specific execution scope that is not necessarily the most immediate one, then you can pass the scope control reference (i.e. $foreach, $template, $evaluate, $define, $macro, or $somebodymacro) as an argument to #break. (e.g. #break($macro)). This will stop rendering of all scopes up to the specified one. When within nested scopes of the same type, remember that you can always access the parent(s) via $<scope>.parent or $<scope>.topmost and pass those to #break instead (e.g. #break($foreach.parent) or #break($macro.topmost)).
Stop

The #stop directive stops any further rendering and execution of the template. This is true even when the directive is nested within another template accessed through #parse or located in a velocity macro. The resulting merged output will contain all the content up to the point the #stop directive was encountered. This is handy as an early exit from a template. For debugging purposes, you may provide a message argument (e.g. #stop('$foo was not in context') ) that will be written to the logs (DEBUG level, of course) upon completion of the stop command.
Evaluate

The #evaluate directive can be used to dynamically evaluate VTL. This allows the template to evaluate a string that is created at render time. Such a string might be used to internationalize the template or to include parts of a template from a database.

The example below will display abc.

```
#set($source1 = "abc")
#set($select = "1")
#set($dynamicsource = "$source$select")
## $dynamicsource is now the string '$source1'
#evaluate($dynamicsource)
```

##<a name="define">Define</a>

The #define directive lets one assign a block of VTL to a reference.

The example below will display Hello World!.

```
#define( $block )Hello $who#end
#set( $who = 'World!' )
$block
```

##<a name="velocitymacros">Velocimacros</a>

The #macro script element allows template designers to define a repeated segment of a VTL template. Velocimacros are very useful in a wide range of scenarios both simple and complex. This Velocimacro, created for the sole purpose of saving keystrokes and minimizing typographic errors, provides an introduction to the concept of Velocimacros.

```html
#macro( d )
<tr><td></td></tr>
#end
```

The Velocimacro being defined in this example is d, and it can be called in a manner analogous to any other VTL directive:

```
#d()
```

When this template is called, Velocity would replace #d() with a row containing a single, empty data cell. If we want to put something in that cell, we can alter the macro to allow for a body:

```html
#macro( d )
<tr><td>$!bodyContent</td></tr>
#end
```

Now, if we call the macro just a bit differently, using #@ before the name and providing a body and #end to the call, then Velocity will render the body when it gets to the $!bodyContent:

```
#@d()Hello!#end
```

You can still call the macro as you did before, and since we used the silent reference notation for the body reference ($!bodyContent instead of $bodyContent), it will still render a row with a single, empty data cell.

A Velocimacro can also take any number of arguments -- even zero arguments, as demonstrated in the first example, is an option -- but when the Velocimacro is invoked, it must be called with the same number of arguments with which it was defined. Many Velocimacros are more involved than the one defined above. Here is a Velocimacro that takes two arguments, a color and an array.

```html
#macro( tablerows $color $somelist )
#foreach( $something in $somelist )
    <tr><td bgcolor=$color>$something</td></tr>
#end
#end
```

The Velocimacro being defined in this example, tablerows, takes two arguments. The first argument takes the place of $color, and the second argument takes the place of $somelist.

Anything that can be put into a VTL template can go into the body of a Velocimacro. The tablerows Velocimacro is a foreach statement. There are two #end statements in the definition of the #tablerows Velocimacro; the first belongs to the #foreach, the second ends the Velocimacro definition.

```html
#set( $greatlakes = ["Superior","Michigan","Huron","Erie","Ontario"] )
#set( $color = "blue" )
<table>
    #tablerows( $color $greatlakes )
</table>
```

Notice that $greatlakes takes the place of $somelist. When the #tablerows Velocimacro is called in this situation, the following output is generated:

```html
<table>
    <tr><td bgcolor="blue">Superior</td></tr>
    <tr><td bgcolor="blue">Michigan</td></tr>
    <tr><td bgcolor="blue">Huron</td></tr>
    <tr><td bgcolor="blue">Erie</td></tr>
    <tr><td bgcolor="blue">Ontario</td></tr>
</table>
```

Velocimacros can be defined inline in a Velocity template, meaning that it is unavailable to other Velocity templates on the same web site. Defining a Velocimacro such that it can be shared by all templates has obvious advantages: it reduces the need to redefine the Velocimacro on numerous templates, saving work and reducing the chance of error, and ensures that a single change to a macro available to more than one template.

Were the #tablerows($color $list) Velocimacro defined in a Velocimacros template library, this macro could be used on any of the regular templates. It could be used many times and for many different purposes. In the template mushroom.vm devoted to all things fungi, the #tablerows Velocimacro could be invoked to list the parts of a typical mushroom:

```html
#set( $parts = ["volva","stipe","annulus","gills","pileus"] )
#set( $cellbgcol = "#CC00FF" )
<table>
#tablerows( $cellbgcol $parts )
</table>
```

When fulfilling a request for mushroom.vm, Velocity would find the #tablerows Velocimacro in the template library (defined in the velocity.properties file) and generate the following output:

```html
<table>
    <tr><td bgcolor="#CC00FF">volva</td></tr>
    <tr><td bgcolor="#CC00FF">stipe</td></tr>
    <tr><td bgcolor="#CC00FF">annulus</td></tr>
    <tr><td bgcolor="#CC00FF">gills</td></tr>
    <tr><td bgcolor="#CC00FF">pileus</td></tr>
</table>
```

###<a name="velocitymacro-arguments">Velocimacro Arguments</a>

Velocimacros can take as arguments any of the following VTL elements :

    Reference : anything that starts with '$'
    String literal : something like "$foo" or 'hello'
    Number literal : 1, 2 etc
    IntegerRange : [ 1..2] or [$foo .. $bar]
    ObjectArray : [ "a", "b", "c"]
    boolean value true
    boolean value false

When passing references as arguments to Velocimacros, please note that references are passed 'by name'. This means that their value is 'generated' at each use inside the Velocimacro. This feature allows you to pass references with method calls and have the method called at each use. For example, when calling the following Velocimacro as shown

```
     #macro( callme $a )
         $a $a $a
     #end

     #callme( $foo.bar() )
```

results in the method bar() of the reference $foo being called 3 times.

At first glance, this feature appears surprising, but when you take into consideration the original motivation behind Velocimacros -- to eliminate cut'n'paste duplication of commonly used VTL -- it makes sense. It allows you to do things like pass stateful objects, such as an object that generates colors in a repeating sequence for coloring table rows, into the Velocimacro.

If you need to circumvent this feature, you can always just get the value from the method as a new reference and pass that :

```
#set( $myval = $foo.bar() )
#callme( $myval )
```

###<a name="velocitymacro-properties">Velocimacro Properties</a>

Several lines in the velocity.properties file allow for flexible implementation of Velocimacros. Note that these are also documented in the Developer Guide.

```velocimacro.library``` - A comma-separated list of all Velocimacro template libraries. By default, Velocity looks for a single library: VM_global_library.vm. The configured template path is used to find the Velocimacro libraries.

```velocimacro.permissions.allow.inline``` - This property, which has possible values of true or false, determines whether Velocimacros can be defined in regular templates. The default, true, allows template designers to define Velocimacros in the templates themselves.

```velocimacro.permissions.allow.inline.to.replace.global``` - With possible values of true or false, this property allows the user to specify if a Velocimacro defined inline in a template can replace a globally defined template, one that was loaded on startup via the velocimacro.library property. The default, false, prevents Velocimacros defined inline in a template from replacing those defined in the template libraries loaded at startup.

```velocimacro.permissions.allow.inline.local.scope``` - This property, with possible values of true or false, defaulting to false, controls if Velocimacros defined inline are 'visible' only to the defining template. In other words, with this property set to true, a template can define inline VMs that are usable only by the defining template. You can use this for fancy VM tricks - if a global VM calls another global VM, with inline scope, a template can define a private implementation of the second VM that will be called by the first VM when invoked by that template. All other templates are unaffected.

```velocimacro.library.autoreload``` - This property controls Velocimacro library autoloading. The default value is false. When set to true the source Velocimacro library for an invoked Velocimacro will be checked for changes, and reloaded if necessary. This allows you to change and test Velocimacro libraries without having to restart your application or servlet container, just like you can with regular templates. This mode only works when caching is off in the resource loaders (e.g. file.resource.loader.cache = false ). This feature is intended for development, not for production.
Getting literal

VTL uses special characters, such as ```$``` and ```#```, to do its work, so some added care should be taken where using these characters in your templates. This section deals with escaping these characters.

###<a name="currency">Currency</a>
There is no problem writing "I bought a 4 lb. sack of potatoes at the farmer's market for only $2.50!" As mentioned, a VTL identifier always begins with an upper- or lowercase letter, so $2.50 would not be mistaken for a reference.

###<a name="escaping-valid-vtl-references">Escaping Valid VTL References</a>
Cases may arise where you do not want to have a reference rendered by Velocity. Escaping special characters is the best way to output VTL's special characters in these situations, and this can be done using the backslash ( \ ) character when those special characters are part of a valid VTL reference. *

```
#set( $email = "foo" )
$email
```

If Velocity encounters a reference in your VTL template to $email, it will search the Context for a corresponding value. Here the output will be foo, because $email is defined. If $email is not defined, the output will be $email.

Suppose that $email is defined (for example, if it has the value foo), and that you want to output $email. There are a few ways of doing this, but the simplest is to use the escape character. Here is a demonstration:

```
## The following line defines $email in this template:
#set( $email = "foo" )
$email
\$email
```

renders as

```
foo
$email
```

If, for some reason, you need a backslash before either line above, you can do the following:

```
## The following line defines $email in this template:
#set( $email = "foo" )
\\$email
\\\$email
```

which renders as

```
\foo
\$email
```

Note that the \ character bind to the $ from the left. The bind-from-left rule causes \\\$email to render as \$email. Compare these examples to those in which $email is not defined.
```
$email
\$email
\\$email
\\\$email
```

renders as

```
$email
\$email
\\$email
\\\$email
```

Notice Velocity handles references that are defined differently from those that have not been defined. Here is a set directive that gives $foo the value gibbous.

```
#set( $foo = "gibbous" )
$moon = $foo
```

The output will be: $moon = gibbous -- where $moon is output as a literal because it is undefined and gibbous is output in place of $foo.

###<a name="escaping-invalid-vtl-references">Escaping Invalid VTL References</a>
Sometimes Velocity has trouble parsing your template when it encounters an "invalid reference" that you never intended to be a reference at all. Escaping special characters is, again, the best way to handle these situations, but in these situations, the backslash will likely fail you. Instead of simply trying to escape the problematic $ or #, you should probably just replace this:

```
${my:invalid:non:reference}
```

with something like this

```
#set( $D = '$' )
${D}{my:invalid:non:reference}
```

You can, of course, put your $ or # string directly into the context from your java code (e.g. context.put("D","$");) to avoid the extra #set() directive in your template(s). Or, if you are using VelocityTools, you can just use the EscapeTool like this:

```
${esc.d}{my:invalid:non:reference}
```

Escaping of both valid and invalid VTL directives is handled in much the same manner; this is described in more detail in the Directives section.

###<a name="escaping-vtl-directives">Escaping VTL Directives</a>
VTL directives can be escaped with the backslash character ("\") in a manner similar to valid VTL references.

```
## #include( "a.txt" ) renders as <contents of a.txt>
#include( "a.txt" )

## \#include( "a.txt" ) renders as #include( "a.txt" )
\#include( "a.txt" )

## \\#include ( "a.txt" ) renders as \<contents of a.txt>
\\#include ( "a.txt" )
```

Extra care should be taken when escaping VTL directives that contain multiple script elements in a single directive (such as in an if-else-end statements). Here is a typical VTL if-statement:

```
#if( $jazz )
    Vyacheslav Ganelin
#end

```
If $jazz is true, the output is

```
Vyacheslav Ganelin
```

If $jazz is false, there is no output. Escaping script elements alters the output. Consider the following case:

```
#if( $jazz )
    Vyacheslav Ganelin
#end
```

This causes the directives to be escaped, but the rendering of $jazz proceeds as normal. So, if $jazz is true, the output is

```
\#if( true )
	Vyacheslav Ganelin
\#end
```

Suppose backslashes precede script elements that are legitimately escaped:

```
\\#if( $jazz )
	Vyacheslav Ganelin
\\#end
```

In this case, if $jazz is true, the output is

```
\ Vyacheslav Ganelin
\
```

To understand this, note that the #if( arg ) when ended by a newline (return) will omit the newline from the output. Therefore, the body of the #if() block follows the first '\', rendered from the '\\' preceding the #if(). The last \ is on a different line than the text because there is a newline after 'Ganelin', so the final \\, preceding the #end is part of the body of the block.

If $jazz is false, the output is

```
\
```

Note that things start to break if script elements are not properly escaped.

```
\\\#if( $jazz )
    Vyacheslave Ganelin
\\#end
```

Here the #if is escaped, but there is an #end remaining; having too many endings will cause a parsing error.

##<a name="vtl-formatting-issues">VTL: Formatting Issues</a>

Although VTL in this user guide is often displayed with newlines and whitespaces, the VTL shown below

```
#set( $imperial = ["Munetaka","Koreyasu","Hisakira","Morikune"] )
#foreach( $shogun in $imperial )
    $shogun
#end
```

is equally valid as the following snippet that Geir Magnusson Jr. posted to the Velocity user mailing list to illustrate a completely unrelated point:

```
Send me #set($foo=["$10 and ","a pie"])#foreach($a in $foo)$a#end please.
```

Velocity's behaviour is to gobble up excess whitespace. The preceding directive can be written as:

```
Send me
#set( $foo = ["$10 and ","a pie"] )
#foreach( $a in $foo )
$a
#end
please.
```

or as

```
Send me
#set($foo       = ["$10 and ","a pie"])
                 #foreach           ($a in $foo )$a
         #end please.
```

In each case the output will be the same.

##<a name="other-features-and-miscellany">Other Features and Miscellany</a>
###<a name="math">Math</a>

Velocity has a handful of built-in mathematical functions that can be used in templates with the set directive. The following equations are examples of addition, subtraction, multiplication and division, respectively:

```
#set( $foo = $bar + 3 )
#set( $foo = $bar - 4 )
#set( $foo = $bar * 6 )
#set( $foo = $bar / 2 )
```

When a division operation is performed between two integers, the result will be an integer, as the fractional portion is discarded. Any remainder can be obtained by using the modulus (%) operator.

```
#set( $foo = $bar % 5 )
```

###<a name="range-operator">Range Operator</a>

The range operator can be used in conjunction with #set and #foreach statements. Useful for its ability to produce an object array containing integers, the range operator has the following construction:

```
[n..m]
```

Both n and m must either be or produce integers. Whether m is greater than or less than n will not matter; in this case the range will simply count down. Examples showing the use of the range operator as provided below:

```
First example:
#foreach( $foo in [1..5] )
$foo
#end

Second example:
#foreach( $bar in [2..-2] )
$bar
#end

Third example:
#set( $arr = [0..1] )
#foreach( $i in $arr )
$i
#end

Fourth example:
[1..3]
```

Produces the following output:

```
First example:
1 2 3 4 5

Second example:
2 1 0 -1 -2

Third example:
0 1

Fourth example:
[1..3]
```

Note that the range operator only produces the array when used in conjunction with #set and #foreach directives, as demonstrated in the fourth example.

Web page designers concerned with making tables a standard size, but where some will not have enough data to fill the table, will find the range operator particularly useful.
Advanced Issues: Escaping and !

When a reference is silenced with the ! character and the ! character preceded by an \ escape character, the reference is handled in a special way. Note the differences between regular escaping, and the special case where \ precedes ! follows it:

```
#set( $foo = "bar" )
$\!foo
$\!{foo}
$\\!foo
$\\\!foo
```

This renders as:

```
$!foo
$!{foo}
$\!foo
$\\!foo
```

Contrast this with regular escaping, where \ precedes $:

```
\$foo
\$!foo
\$!{foo}
\\$!{foo}
```

This renders as:

```
$foo
$!foo
$!{foo}
\bar
```

###<a name="velocitymacro-miscellany">Velocimacro Miscellany</a>

This section is a mini-FAQ on topics relating to Velocimacros. This section will change over time, so it's worth checking for new information from time to time.

Note : Throughout this section, 'Velocimacro' will commonly be abbreviated as 'VM'.
Can I use a directive or another VM as an argument to a VM?

Example : ```#center( #bold("hello") )```

No. A directive isn't a valid argument to a directive, and for most practical purposes, a VM is a directive.

However..., there are things you can do. One easy solution is to take advantage of the fact that 'doublequote' (") renders its contents. So you could do something like

```
#set($stuff = "#bold('hello')" )
#center( $stuff )
```

You can save a step...

```
#center( "#bold( 'hello' )" )
```

Please note that in the latter example the arg is evaluated inside the VM, not at the calling level. In other words, the argument to the VM is passed in in its entirety and evaluated within the VM it was passed into. This allows you to do things like :

```
#macro( inner $foo )
  inner : $foo
#end

#macro( outer $foo )
   #set($bar = "outerlala")
   outer : $foo
#end

#set($bar = 'calltimelala')
#outer( "#inner($bar)" )
```

Where the output is

```
Outer : inner : outerlala
```

because the evaluation of the "#inner($bar)" happens inside #outer(), so the $bar value set inside #outer() is the one that's used.

This is an intentional and jealously guarded feature - args are passed 'by name' into VMs, so you can hand VMs things like stateful references such as

```html
#macro( foo $color )
  <tr bgcolor=$color><td>Hi</td></tr>
  <tr bgcolor=$color><td>There</td></tr>
#end

#foo( $bar.rowColor() )
```

And have rowColor() called repeatedly, rather than just once. To avoid that, invoke the method outside of the VM, and pass the value into the VM.

```
#set($color = $bar.rowColor())
#foo( $color )
```

Can I register Velocimacros via #parse() ?

Yes! This became possible in Velocity 1.6.

If you are using an earlier version, your Velocimacros must be defined before they are first used in a template. This means that your #macro() declarations should come before using the Velocimacros.

This is important to remember if you try to #parse() a template containing inline #macro() directives. Because the #parse() happens at runtime, and the parser decides if a VM-looking element in the template is a VM at parsetime, #parse()-ing a set of VM declarations won't work as expected. To get around this, simply use the velocimacro.library facility to have Velocity load your VMs at startup.
What is Velocimacro Autoreloading?

There is a property, meant to be used in development, not production :

```text
velocimacro.library.autoreload
```

which defaults to false. When set to true along with

```text
<type>.resource.loader.cache = false
```

(where <type> is the name of the resource loader that you are using, such as 'file') then the Velocity engine will automatically reload changes to your Velocimacro library files when you make them, so you do not have to dump the servlet engine (or application) or do other tricks to have your Velocimacros reloaded.

Here is what a simple set of configuration properties would look like.

```text
file.resource.loader.path = templates
file.resource.loader.cache = false
velocimacro.library.autoreload = true
```    

Don't keep this on in production.


###<a name="string-concatination">String Concatenation</a>

A common question that developers ask is How do I do String concatenation? Is there any analogue to the '+' operator in Java?.

To do concatenation of references in VTL, you just have to 'put them together'. The context of where you want to put them together does matter, so we will illustrate with some examples.

In the regular 'schmoo' of a template (when you are mixing it in with regular content) :

```text
#set( $size = "Big" )
#set( $name = "Ben" )

The clock is $size$name.
```

and the output will render as 'The clock is BigBen'. For more interesting cases, such as when you want to concatenate strings to pass to a method, or to set a new reference, just do

```text
#set( $size = "Big" )
#set( $name = "Ben" )

#set($clock = "$size$name" )

The clock is $clock.
``` 

Which will result in the same output. As a final example, when you want to mix in 'static' strings with your references, you may need to use 'formal references' :

```text
#set( $size = "Big" )
#set( $name = "Ben" )

#set($clock = "${size}Tall$name" )

The clock is $clock.
 ```   

Now the output is 'The clock is BigTallBen'. The formal notation is needed so the parser knows you mean to use the reference '$size' versus '$sizeTall' which it would if the '{}' weren't there.
