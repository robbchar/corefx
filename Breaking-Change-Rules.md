## Behavioral Changes

### Property, Field, Parameter and Return Values
&#10003; **Allowed**
* Increasing the range of accepted values for a property or parameter if the member _is not_ `virtual`

* Returning a more derived type for a property, field, return or `out` value

&#10007; **Disallowed**  
* Increasing the range of accepted values for a property or parameter if the member _is_ `virtual`   
> This is breaking because any existing overridden members will now not function correctly for the extended range of values.

* Decreasing the range of accepted values for a property or parameter, such as a change in parsing of input and throwing new errors (even if parsing behavior is not specified in the docs)

* Increasing the range of returned values for a property, field, return or `out` value

* Changing the returned values for a property, field, return or 'out' value, such as the value returned from `ToString`
> If you had an API which returned a value from 0-10, but actually intended to divide the value by two and forgot (return only 0-5) then changing the return to now give the correct value is a breaking.

* Changing the default value for a property, field or parameter (either via an overload or default value)

* Changing the value of an enum member

* Changing the precision of a numerical return value

### Exceptions
&#10003; **Allowed**
* Throwing a more derived exception than an existing exception  
> For example, `CultureInfo.GetCultureInfo(String)` used to throw `ArgumentException` in .NET Framework 3.5. In .NET Framework 4.0, this was changed to throw `CultureNotFoundException` which derives from `ArgumentException`, and therefore is an acceptable change.

* Throwing a more specific exception than `NotSupportedException`, `NotImplementedException`, `NullReferenceException` or an exception that is considered unrecoverable  
> Unrecoverable exceptions should not be getting caught and will be dealt with on a broad level by a high-level catch-all handler. Therefore, users are not expected to have code that catches these explicit exceptions. The list of unrecoverable exceptions are:
     * `StackOverflowException`
     * `SEHException`
     * `ExecutionEngineException`
     * `AccessViolationException`

* Throwing a new exception that only applies to a ode-path which can only be observed with new parameter values, or state (that couldn't hit by existing code targeting the previous version)

* Removing an exception that was being thrown when the API allows more robust behavior or enables new scenarios  
> For example, a Divide method which only worked on positive values, but threw an exception otherwise, can be changed to support all values and the exception is no longer thrown.

&#10007; **Disallowed**
* Throwing a new exception in any other case not listed above

* Removing an exception in any other case not listed above

### Platform Support Changes

&#10003; **Allowed**
* An operation previously not supported on a specific platform, is now supported

&#10007; **Disallowed**
* An operation previously supported on a specific platform is no longer supported, or now requires a specific service-pack

### Code Changes
&#10003; **Allowed**
* A change which is directly intended to increase performance of an operation  
> The ability to modify the performance of an operation is essential in order to ensure we stay competitive, and we continue to give users operational benefits. This can break anything which relies upon the current speed of an operation, sometimes visible in badly built code relying upon asynchronous operations. Note that the performance change should have no affect on other behavior of the API in question, otherwise the change will be breaking.

* A change which indirectly, and often adversely, affects performance
> Assuming the change in question is not categorized as breaking for some other reason, this is acceptable. Often, actions need to be taken which may include extra operation calls, or new functionality. This will almost always affect performance, but may be essential to make the API in question function as expected.

&#10007; **Disallowed**

* Adding the `checked` keyword to a code-block. This may cause code in a block to to begin to throwing exceptions, an unacceptable change.
* Changing the order in which events are fired. Developers can reasonably expect events to fire in the same order.
* Removing the raising of an event on a given action
* Changing a synchronous API to asynchronous (and vice versa)
* Firing an existing event when it was never fired before
* Changing the number of times given events are called

## Source and Binary Compatibility Changes

### Type Attribute and Modifier Changes
* Adding the `sealed` or `abstract` keyword to a type. However, it is not breaking when:
  * There are no accessible (public or protected) constructors
* Reducing the visibility of a type
* Removing the implementation of an interface on a class. However, it is not breaking when:
  * If you added the implementation of an interface which derives from the removed interface. For example, you removed `IDisposable`, but implemented `IComponent`, which derives from `IDisposable`.
  * The interface is already implemented lower in the hierarchy.
* Remove an attribute, or changing values of an attribute. However, it is not breaking when:
  * These values are not observable by consumers, such as `EditorBrowsable`.

### Type Signature Changes
* Changing the base class for a class. However, it is not breaking when:
  * The new base class is more derived from the original base class, -and- the new base class does not shadow any of the members of the original base class.
* Changing the name of a type

### Namespace Changes
* Moving a type to a new namespace
* Removing a type from a namespace in which it resided

### Assembly Changes
* Changing the name of an assembly

### Location Changes
* Moving a type from one assembly into another assembly. However, it is not breaking when:
  * The old assembly is marked with `TypeForwardedToAttribute` pointing to the new location

### Member Changes
* Adding a member to an interface
* Adding an abstract member to a public type. However, it is not breaking when:
  * There are no accessible (public or protected) constructors
  * The type is sealed
* Adding a constructor to a class which previously had no constructor, without also adding the default constructor
* Adding an overload that precludes an existing overload, and defines different behavior.
  * This will break existing clients that were bound to the previous overload. For example, if you have a class that has a single version of a method that accepts a UInteger, an existing client will successfully bind to that overload, if simply passing an Integer value. However, if you add an overload that to this method that accepts an Integer, recompiling the application will now bind to the new overload. If different behavior results, then this is a breaking change.

### Member Removals
* Removing a public member. However, it is not breaking when:
  * The member is moved onto a class higher in the hierarchy tree of the type from which it was removed.
* Renaming a public member
* Reducing the scope of a member. However, it is not breaking when the member is protected and:
  * There are no accessible (public or protected) constructors
  * The type is sealed

### Member Signature Changes
* Changing the type of a property or field to an unrelated type
* Changing a property type to a wider type, when that property has a get accessor
* Adding the readonly keyword to a field
* Removing the getter or setter from a property
* Removing an element from an enum
* Adding an enum value to an enum
* Adding the FlagsAttribute to an enum

### Parameter Type Changes
* Changing a passed parameter type to an unrelated (no inheritance relationship) type
* Changing a parameter type from a base class to an inherited class

### Parameters
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

### Return Type Changes
* Changing the return type to an unrelated (no inheritance relationship) type
* Changing the return type from an inherited class to a base class					

### Member Modifier Changes
* Adding or removing the `abstract` keyword to a member. However, it is not breaking when:
  * Removing `abstract` and adding the `virtual` keyword
* Removing the `virtual` keyword from a member
* Adding or removing the `static` keyword from a member
* Removing an attribute applied to a member
  * Although this item can be addressed on a case to case basis, removing an attribute will often be breaking. An example is the NonSerializedAttribute