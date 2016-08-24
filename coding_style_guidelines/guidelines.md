<!--
guidelines.md
Provides principles of good coding style for COMP 2201.

HTML generated by:

$ pandoc guidelines.md -o guidelines.html -S -s -t html --highlight-style tango

@author		Dean Hendrix (dh@auburn.edu)
@contrib	Sarath Kuchi (sck0013@auburn.edu)
@version	2016-08-12

 -->

# Coding Guidelines for COMP 2210

> "Programs are meant to be read by humans and only incidentally for computers
> to execute."
> -- *Structure and Interpretation of Computer Programs*, Abelson, Sussman, and Sussman

That [SICP](https://mitpress.mit.edu/sicp/full-text/book/book.html) quote is
great  for emphasizing the importance of code as a communication tool. We
should write code for others (and ourselves) to read and understand, and not just
to execute. [This article](https://www.smashingmagazine.com/2012/10/why-coding-style-matters/)
by [Nicholas Zakas](https://www.nczonline.net/about/) is also good for a more
applied and detailed rationale for why coding guidelines are important.

Guidelines for writing code are to some extent subjective, but we try to
choose only principles that are commonly accepted. Our list of "principles" is
written and structured much like *Effective Java*, 2nd edition, by [Josh Bloch](https://en.wikipedia.org/wiki/Joshua_Bloch).
A preview of that book is available
[here](https://books.google.com/books?id=ka2VUBqHiWkC&lpg=PP1&pg=PP1#v=onepage&q&f=false).
Some of the principles here are duplicated in *Effective Java* and we try to
include a reference to Bloch's corresponding "Item".

Another excellent resource for good coding practices and more is [*The Practice of Programming*](http://www.cs.princeton.edu/~bwk/tpop.webpage/) by [Brian Kernighan](https://en.wikipedia.org/wiki/Brian_Kernighan) and [Rob Pike](https://en.wikipedia.org/wiki/Rob_Pike).


---

## Principle: Follow an accepted coding style when writing programs.

One of the most contentious coding practices is *style* or *format* - choices
of [indentation](https://en.wikipedia.org/wiki/Indent_style), whitespace,
bracket placement, and other layout options. Each programming language and its
community of developers will typically have an accepted style and set of
conventions that provide a lingua franca for that community.

Most languages in the [C family](https://en.wikipedia.org/wiki/List_of_C/family_programming_languages)
have accepted styles based on some variation of
[K&R style](https://en.wikipedia.org/wiki/Indent_style#K.26R_style) or
[GNU style](http://www.gnu.org/prep/standards/standards.html#Formatting).
The ["official" Java style guide](http://www.oracle.com/technetwork/java/index-135089.html)
was published by Sun/Oracle, but is now considered archived and is no
longer maintained.

We will base our course coding conventions on a style known as
[Google Java Style](https://google.github.io/styleguide/javaguide.html).
This is the promoted style for Java programming in the [Google Developers
program](https://developers.google.com/), and closely mirrors the coding
guidelines used internally by Google. Source code submitted as part of
graded items in COMP 2210 will be audited by [Checkstyle](http://checkstyle.sourceforge.net/)
using [this configuration file](2210.google_checks.xml),
which is a modified version of Google Java Style.


---

## Principle: Limit the nesting depth of the code.

*Nesting depth* refers to the maximum number of nested levels of scope. For
example, the code snippet below has a nesting depth of three.

```java
if (cond1) {
	if (cond2) {
		if (cond3) {
			foo();
		}
		bar();
	}
	baz();
}
```

The greater the nesting depth, the more difficult it is to reason about the
code and trace through the different execution paths that exist. Using compound
boolean conditions makes the nesting depth problem even worse.

For example, in the code below, what condtions must be true in order for the
method call to `foo()` to execute? How many different combinations of conditional
states would cause the method call to `bar()` to execute?

```java
if (cond1 || cond2 || cond3) {
	if (!cond4 && cond5) {
		if (cond6 || !cond7 && cond8) {
			foo();
		}
		bar();
	}
	baz();
}
```

Where possible, we should prefer code with lower nesting depth. Such code is
easier to read, understand, test, modify, and maintain.



---

## Principle: Prefer `if` statements with no `else` when the one branch will return or exit.

If one branch of an `if` statement transfers control outside of the current
sequential control flow, there is no need for an `else`. Three examples are
given below.

This:

```java
	if (cond) {
	   return;
	}
	actions();
```

is preferable to:

```java
	if (cond) {
	   return;
	}
	else {
	   actions();
	}
```

This:

```java
	if (error_cond) {
	   throw new SomeException();
	}
	actions();
```

is preferable to:

```java
	if (error_cond) {
	   throw new SomeException();
	}
	else {
	   actions();
	}
```

This:

```java
	if (!cond) {
	   throw new SomeException();
	}
	actions();
```

is preferable to:

```java
	if (cond) {
	   actions();
	}
	else {
	   throw new SomeException();
	}
```


---

## Principle: Don't compare boolean expressions to boolean literals.

Boolean expressions *evaluate* to either `true` or `false`, so we shouldn't
compare those expressions to the boolean literals.

This:

```java
if (cond) {
   action();
}
```

is preferable to:

```java
if (cond == true) {
   action();
}
```

This:

```java
return cond;
```

is preferable to:

```java
return (cond == true);
```

This:

```java
if (!cond) {
	action();
}
```

is preferable to:

```java
if (cond == false) {
	action();
}
```


---

## Principle: Adhere to generally accepted naming conventions. (Bloch Item 56)

Both [*Effective Java*](https://books.google.com/books?id=ka2VUBqHiWkC&lpg=PP1&pg=PP1#v=onepage&q&f=false)
and [Google Java Style](https://google.github.io/styleguide/javaguide.html) explain
this principle well. We will only mention the following as principles to pay
particular attention to in this course.

- Class and interfaces should be named with one or more words in
[CamelCase](https://en.wikipedia.org/wiki/CamelCase). The words should be
chosen to be indicative of the meaning or purpose of the class or interface.

- Methods and fields should be named like classes and interfaces **except** the
first word should begin with a lowercase letter.

- Constant fields should be named like classes and interfaces except they
should appear in all upper case and each word should be separated by
an underscore.

- Local variables should follow the typographical and naming conventions of
fields except that abbreviations and single-letter identifiers are
permitted. If abbreviations or single-letter identifiers are used, the
meaning should be clear from the context or convention. For example `i` is a
common loop or array index identifier.

- Type parameters should normally consist of a single captialized letter.

In all cases, the intended meaning or use of the thing being named should be
as clear as possible.


---

## Principle: Eliminate unchecked warnings. (Bloch Item 24)

Here's a quote from
[*Effective Java*](https://books.google.com/books?id=ka2VUBqHiWkC&lpg=PP1&pg=PP1#v=onepage&q&f=false):

> Unchecked warnings are important. Don't ignore them. Every unchecked
> warning represents the potential for a `ClassCastException` at runtime.
> Do your best to eliminate these warnings.

Most unchecked warnings **can** be eliminated and we should always do so. If
you can't eliminate a warning in the code you plan to submit for a grade, be
sure to ask a member of the course staff for help. If the warning can't be
eliminated, you can use the `@SuppressWarnings` annotation at the smallest
scope possible. If you use this annotation, always include a comment as to why
you know the statement that generates the warning is, in fact, type-safe.


---

## Principle: Prefer to declare reference variables with interfaces rather than classes. (Bloch Item 52)

Here's a quote from
[*Effective Java*](https://books.google.com/books?id=ka2VUBqHiWkC&lpg=PP1&pg=PP1#v=onepage&q&f=false):

> If appropriate interface types exist, then parameters, return values,
> variables, and fields should all be declared using interface types. The
> only time you really need to refer to an object's class is when you're
> creating it with a constructor.

Applying this principle does a couple of good things.

- It makes our code more general. We can plug-and-play different classes for the
behavior provided by the interface. We're not forcing only one implementation of
that behavior.

- It communicates to others exactly what behavior is required. If any
implementation of the interface will do, then typing with the interface
communicates exactly that. If the specific behavior of a class is required,
then typing with the class communicates that.


---

## Principle: Declare fields and local variables one per line.

Each declaration should be for exactly one variable. Each declaration should,
if possible, be on exactly one line. Declaring multiple variables in a single
declaration is less readable and it often leads to confusion. This is
especially true when some of the variables are also initialized.

This:

```java
int counter;
int total;
```
is preferable to:

```java
int counter, total;
```

This:

```java
int product;
int sum = 0;
```

is preferrable to:

```java
int product, sum = 0;
```


---