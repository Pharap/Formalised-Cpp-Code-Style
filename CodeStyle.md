# Formal Declaration Of Code Style

# Definition Of Terminology

### Pascal case -
	The case in which identifiers begin with an uppercase letter,
	and each new word follows with an uppercase letter.
	
### Camel case -
	The case in which identifiers being with a lowercase letter,
	and each new word follows with an uppercase letter.
	
### Macro case -
	The case in which all letters are uppercase,
	and each new word is separated via an underscore.
	
### Function -
	The term function refers to a function as in the language-level definition.
	Thus it acts as a generalisation of and can refer to all free functions,
	static functions, friend functions and member functions.
	
### Free function -
	A free function is any function that does not belong to a class or type in any way.
	A free function may however exist in a namespace.
	```
	namespace Maths
	{
		float Lerp(float value0, float value1, float factor)
		{
            return (value0 * (1.0f - factor)) + (value1 * factor);
		}
	}
	```
	
### Member function -
	A member function is a function that belongs directly to a class and is
	invoked	upon an instance of the class via the `.` or `->` operators.
	The term 'member function' is synonymous with the term 'method'.
	```
	// Entity.h
	class Entity
	{
		// ...
	public:
		void Update(void);
		// ...
	};
	
	// Main.cpp
	void main(void)
	{
		Entity  entity;
		entity.Update();
		
		Entity *entityPtr = &entity;
		entityPtr->Update();
	}
	```
	
### Static function -
	A static function is a function that belongs to the type of a class and is
	invoked with the class's type name via the '::' operator.
	```
	// Point.h
	class Point
	{
		// ...
	public:
		static bool TryParse(const char * text, Point & result);
		// ...
	};
	
	// Main.cpp
	void main(void)
	{
		Point result;
		const char * input = readString();
		Point::TryParse(input, result);
	}
	```
	
### Pure function -
	A pure function is a free function or static function whose output is solely
	reliant upon its input arguments and is in no way reliant upon any kind of
	modifiable state.
	Some commonly used pure functions include `sin`, `cos` and `tan`.
	```
	// Lerp is pure because it is stateless.
	// A call to Lerp could be replaced by its own source code as if
	// it were a macro, and the result would be the same.
	float Lerp(float value0, float value1, float factor)
	{
		return (value0 * (1.0f - factor)) + (value1 * factor);
	}
	```
	
# Casing Style

|--------------------|-------------|
| Feature            | Casing      |
|--------------------|-------------|
| class name         | pascal case |
| struct name        | pascal case |
| enum name          | pascal case |
|--------------------|-------------|
| class typedef      | pascal case |
| struct typedef     | pascal case |
| enum typedef       | pascal case |
| integral typedef   | camel case  |
|--------------------|-------------|
| global variable    | pascal case |
| local variable     | camel case  |
|--------------------|-------------|
| defined constant   | macro case  |
| defined macro      | macro case  |
|--------------------|-------------|
| public field       | pascal case |
| protected field    | pascal case |
| private field      | camel case  |
|--------------------|-------------|
| function arguments | camel case  |
|--------------------|-------------|
| public method      | pascal case |
| protected method   | pascal case |
| private method     | camel case  |
|--------------------|-------------|
| public function    | pascal case |
| protected function | pascal case |
| private function   | camel case  |
| free function      | pascal case |
|--------------------|-------------|
| header file        | pascal case |
| c++ source file    | pascal case |
| arduino ino file   | pascal case |
|--------------------|-------------|

# Specific Rules

## Naming Conventions Of Other APIs

The naming conventions of other APIs are to be accepted as an artefact of their use.
When attempting to extend said APIs with new functionality,
the style of the API is to be followed rather than the style of the main project.
However if the new functionality could exist without depending on and external API
then it should be added as if it were a part of the main project.

## Abbreviations

Avoid abbreviations where possible.
Widely known acronyms such as 'Ram', 'Cpu', 'Xml' and 'Http' are acceptable,
but must follow pascal case style rather than appearing in all caps
(except in the case of macros).
Mathematical terms such as 'X', 'Y', 'sin' and 'arctan' are acceptable,
but generic use of letters such as 'a' and 'b' are not,
unless the reason for use is clear
e.g. 'i' is acceptable for integer loop variables,
'c' is acceptable for briefly used char variables,
'f' is acceptable for briefly used float variables.
Use of generic letters such of this is prohibited for anything but local variables.

## Brace Style

For all code blocks, curly braces are to be given their own line.
This applies to classes, structs and functions.
The exception to this rule is getter methods that do nothing but return a private variable.
Single return statement getter method bodies may exist on the same line as their function definition.

## Const Correctness

Code must be const correct where possible.
If the const correctness of a given class is questionable,
raise this as an issue.

## Use Of `inline`

Inline should only be used on functions that do minimal work.
This includes methods that only return a value,
methods that only set a few variables and 'pure' functions.

## Use Of `struct`

Structs should be avoided where possible.
When used, structs should be used for structures that have little or no
behaviour associated with them, such that they exist purely as a data construct.
There may be another class that operates upon them,
but otherwise they should do very little.

## Use Of `enum` And `enum class`

When possible, enum classes should always be preferred over plain enums because they provide
extra type safety by only being explicitly castable to and from integers rather than implicitly castable.
This makes it less likely that subtle bugs will be introduced.

## Use of 'class'

Classes should be the preferred method of defining new types.
Despite popular belief, classes are reasonably lightweight and typically take up no more space than
what is required by their fields. I.e. a Point class comprised of two chars takes up the same space
on the stack as two individual char variables would require.
In addition, defining a method within a class takes up no more space than defining the equivalent
free function would require.

It is the fact that classes package both data and functionality into one unit that make them superior to
C-style structs. That combined with their capability for inheritance and their access modifiers make them
the most sensible choice in most cases where a new type is required.

## Use of 'typedef'

Typedefs may be used to simplify complex classes such as those involving const and pointers, however,
typedefs should be kept local to a class where possible such that the new type exists within the class.
They may be public, protected or private.
Typedefs should only ever occur in the global namespace under circumstances where the benefits outweigh the costs.
