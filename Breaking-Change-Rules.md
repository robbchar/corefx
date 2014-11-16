## Definitions

**Behavioral Change**  
A behavioral change represents changes to the behavior of a member. A behavioral change may including throwing a new exception, adding or removing internal method calls, or alternating the way in which a return value is calculated. Behavioral changes can be the hardest type of change to categorize as acceptable or not - they can be severe in impact, or relatively innocuous.

**Binary Compatibility**  
Refers to the ability of existing consumers of an API to be able to use a newer version without recompilation. By definition, if an assembly's public signatures have been removed, or altered so that consumers cannot no longer access the same interface exposed by the assembly, the change is said to be a _binary incompatible change_.

**Source Compatibility**  
Refers to the ability of existing consumers of an API to recompile against a newer version without any source changes. By definition, if a consumer needs to make changes to its code in order to for it build successfully against a newer version of an API, the change is said to be a _source incompatible change_.

**Design-Time Compatibility**  
_Design-time compatibility_ refers to preserving the design-time experience across versions of Visual Studio and other design-time environments. This can involve details around the UI of the designer, but by far the most interesting design-time compatibility is project compatibility. A potential project (or solution), must be able to be opened, and used on a newer version of a designer.

**Backwards Compatibility**  
_Backwards compatibility_ refers to the ability of an existing consumer of an API to run against, and behave in the same way against a newer version. By definition, if a consumer is not able to run, or behaves differently against the newer version of the API, then the API is said to be _backwards incompatible_. 

Changes that affect backwards compatibility are strongly discouraged. All alternates should be actively considered, since developers will, by default, expect backwards compatibility in newer versions of an API.

**Forwards Compatibility**
Forwards compatibility is the exact reverse of backwards compatibility; it refers to the ability of an existing consumer of an API to run against, and behave in the way against a _older_ version. By definition, if a consumer is not able to run, or behaves differently against an older version of the API, then the API is said to be _forwards incompatible_.

Changes that affect forwards compatibility are generally less pervasive, and there is not as stringent a demand to ensure that such changes are not introduced. Customers accept that a consumer which relies upon a newer API, may not function correctly against the older API.

This document does not attempt to detail forwards incompatibilities.

## Unacceptable Changes

### Unacceptable Behavioral Changes

**Out Parameters and Return Values**
* Increasing the range of returned values for the return value
* Changing the precision of a numerical return value
* Changing a return value, such as as the value returned from ToString. If you had an API which returned a value from 0-10, but actually intended to divide the value by two and forgot (return only 0-5) then changing the return to now give the correct value is a breaking change.

**In Parameters**
* Decreasing the range of accepted values within a given parameter, such as change in parsing of input and throwing new errors (even if parsing behavior is not specified in the docs)

**Platform Support Changes**
* An operation previously supported on a specific platform is no longer supported, or now requires a specific service-pack

**Property/Field Changes**
* Changing the value of a public or protected field
* Changing the default value for a property
* Changing the value of an enum member

**Exceptions**
* Throwing a new exception. However, it is not breaking when:
  * The exception applies only to a new code-path which can only be involved by a call with new parameter values, or state (that couldn't hit by existing code targeting the previous version)
  * You throw a more derived exception than an existing exception. For example, `CultureInfo.GetCultureInfo(String)` used to throw `ArgumentException` in .NET Framework 3.5. In .NET Framework 4.0, this was changed to throw `CultureNotFoundException` which derives from `ArgumentException`, and therefore is an acceptable change. 
  * You are currently throwing `NotSupportedException`, `NotImplementedException`, 'NullReferenceException' or an exception that is considered unrecoverable. Unrecoverable exceptions should not be getting caught and will be dealt with on a broad level or by a high-level catch-all handler. Therefore, users are not expected to have code that catches these explicit exceptions. The list of unrecoverable exceptions are:
     * `StackOverflowException`
     * `SEHException`
     * `ExecutionEngineException`
     * `AccessViolationException`
* Removing an exception that was being throw. However, it is not breaking when:
  * The API allows more robust behavior. For example, a Divide method which only worked on positive values, but threw an exception otherwise, can be changed to support all values and the exception is no longer thrown.

**Code Changes**
* Adding the `checked` keyword to a code-block. This may cause code in a block to to begin to throwing exceptions, an unacceptable change.
* Changing the order in which events are fired. Developers can reasonably expect events to fire in the same order.
* Removing the raising of an event on a given action
* Changing a synchronous API to asynchronous (and vice versa)
* Firing an existing event when it was never fired before
* Changing the number of times given events are called

### Unacceptable Binary and Source Compatibility changes

**Type Attribute and Modifier Changes**
* Adding the `sealed` or `abstract` keyword to a type. However, it is not breaking when:
  * There are no accessible (public or protected) constructors
* Reducing the visibility of a type
* Removing the implementation of an interface on a class. However, it is not breaking when:
  * If you added the implementation of an interface which derives from the removed interface. For example, you removed `IDisposable`, but implemented `IComponent`, which derives from `IDisposable`.
  * The interface is already implemented lower in the hierarchy.
* Remove an attribute, or changing values of an attribute. However, it is not breaking when:
  * These values are not observable by consumers, such as `EditorBrowsable`.

**Type Signature Changes**
* Changing the base class for a class. However, it is not breaking when:
  * The new base class is more derived from the original base class, -and- the new base class does not shadow any of the members of the original base class.
* Changing the name of a type

**Namespace Changes**
* Moving a type to a new namespace
* Removing a type from a namespace in which it resided

**Assembly Changes**
* Changing the name of an assembly

**Location Changes**
* Moving a type from one assembly into another assembly. However, it is not breaking when:
  * The old assembly is marked with `TypeForwardedToAttribute` pointing to the new location

**Member Changes**
* Adding a member to an interface
* Adding an abstract member to a public type. However, it is not breaking when:
  * There are no accessible (public or protected) constructors
  * The type is sealed
* Adding a constructor to a class which previously had no constructor, without also adding the default constructor
* Adding an overload that precludes an existing overload, and defines different behavior.
  * This will break existing clients that were bound to the previous overload. For example, if you have a class that has a single version of a method that accepts a UInteger, an existing client will successfully bind to that overload, if simply passing an Integer value. However, if you add an overload that to this method that accepts an Integer, recompiling the application will now bind to the new overload. If different behavior results, then this is a breaking change.

**Member Removals**
* Removing a public member. However, it is not breaking when:
  * The member is moved onto a class higher in the hierarchy tree of the type from which it was removed.
* Renaming a public member
* Reducing the scope of a member. However, it is not breaking when the member is protected and:
  * There are no accessible (public or protected) constructors
  * The type is sealed

**Member Signature Changes**
* Changing the type of a property or field to an unrelated type
* Changing a property type to a wider type, when that property has a get accessor
* Adding the readonly keyword to a field
* Removing the getter or setter from a property
* Removing an element from an enum
* Adding an enum value to an enum
* Adding the FlagsAttribute to an enum


**Parameter Type Changes**
* Changing a passed parameter type to an unrelated (no inheritance relationship) type
* Changing a parameter type from a base class to an inherited class

**Parameters**
* Adding a new parameter 
* Removing a parameter 
* Renaming a parameter (including case). This is considered breaking for two reasons:
  * It breaks late-bound scenarios, such as Visual Basic's late-binding feature and C#'s `dynamic`
  * It breaks source compatibility when developers use [named parameters](http://msdn.microsoft.com/en-us/library/dd264739.aspx).
* Changing the order of the parameters on an API
* Changing the order of the parameters on an API 
* Adding or removing the `params` keyword to a parameter
* Adding or removing `out` or `ref` keywords to a parameter
* Changing a parameter modifier from `ref` to `out`, or vice versa.
  * These are source incompatible rather than not binary incompatible changes.
* Increasing the range of accepted values within a given parameter, and the member is virtual
  * This is considered breaking since any overridden members will now not function properly, for the extended range of values

**Return Type Changes**
* Changing the return type to an unrelated (no inheritance relationship) type
* Changing the return type from an inherited class to a base class					

**Member Modifier Changes**
* Adding or removing the `abstract` keyword to a member. However, it is not breaking when:
  * Removing `abstract` and adding the `virtual` keyword
* Removing the `virtual` keyword from a member
* Adding or removing the `static` keyword from a member
* Removing an attribute applied to a member
  * Although this item can be addressed on a case to case basis, removing an attribute will often be breaking. An example is the NonSerializedAttribute
