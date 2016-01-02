[![Build Status](https://travis-ci.org/mthaler/xmlconfect.png)](https://travis-ci.org/mthaler/xmlconfect)
[![codecov.io](http://codecov.io/github/mthaler/xmlconfect/coverage.svg?branch=master)](http://codecov.io/github/mthaler/xmlconfect?branch=master)

_xmlconfect_ is a type class based library to serialize Scala classes, in particular case classes, to XML / deserialize XML to Scala classes

It sports the following features:

* Based on Scala XML
* Type-class based (de)serialization of custom objects
* Supports reading / writing attributes or elements for basic types
* No external dependencies

## Installation

## Usage
xmlconfect is really easy to use:

```scala
import com.mthaler.xmlconfect._
import com.mthaler.xmlconfect.BasicAttrFormats._
import com.mthaler.xmlconfect.ProductFormatInstances._
implicit val f = xmlFormat2(Person)
val p = Person("Albert Einstein", 42)
p.toNode
```
This will create an XML element with two attributes, name and age:

```xml
<Person name="Albert Einstein" age="42"/>
```

It is just as easy to deserialize XML:

```scala
import com.mthaler.xmlconfect._
import com.mthaler.xmlconfect.BasicAttrFormats._
import com.mthaler.xmlconfect.ProductFormatInstances._
implicit val f = xmlFormat2(Person)
<Person name="Albert Einstein" age="42"/>.convertTo[Person]
```

This will result in:

```scala
Person("Albert Einstein", 42)
```

To make it clear what happens in the above examples, we walk through them step by step. The first line

```scala
import com.mthaler.xmlconfect._
```

brings the `toNode` and the `convertTo` methods into scope. 

```scala
import com.mthaler.xmlconfect.BasicAttrFormats._
```
brings attribute formats for basic Scala types like int, double and String into scope.

```scala
import com.mthaler.xmlconfect.ProductFormatInstances._
```
imports all methods from ProductFormatInstances which are used to create formats for case classes.

```scala
implicit val f = xmlFormat2(Person)
```
defines a format for the Person class.

```scala
val p = Person("Albert Einstein", 42)
p.toNode
```
creates a new person instance and calls the `toNode` method to serialize it to XML. Instead of using implicits we could also pass the format directly using `p.toNode(f)`.

```scala
<Person name="Albert Einstein" age="42"/>.convertTo[Person]
```
deserializes a class. Again, instead of using implicits, we could pass the format directly using `<Person name="Albert Einstein" age="42"/>.convertTo[Person](f)`.

###Elements and Attriutes
XML offers two ways to store data: elements and attributes. In the above example we used attributes to store the name and the age. But we could also use child elements to store them. _xmlconfect_ supports both:

```scala
import com.mthaler.xmlconfect._
import com.mthaler.xmlconfect.BasicElemFormats._
import com.mthaler.xmlconfect.ProductFormatInstances._
implicit val f = xmlFormat2(Person)
val p = Person("Albert Einstein", 42)
p.toNode
```
This will create an XML element with two children, name and age:

```xml
<Person><name>Albert Einstein</name><age>42</age></Person>
```
The only difference is, that we import `com.mthaler.xmlconfect.BasicElemFormats._` instead of `com.mthaler.xmlconfect.BasicAttrFormats._`. To deserialize the class, we do

```scala
import com.mthaler.xmlconfect._
import com.mthaler.xmlconfect.BasicElemFormats._
import com.mthaler.xmlconfect.ProductFormatInstances._
implicit val f = xmlFormat2(Person)
<Person><name>Albert Einstein</name><age>42</age></Person>.convertTo[Person]
```
Again, the only difference is that we import `com.mthaler.xmlconfect.BasicElemFormats._` instead of `com.mthaler.xmlconfect.BasicAttrFormats._`.
