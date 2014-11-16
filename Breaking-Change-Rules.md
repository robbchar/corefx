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
* Changing a return value, such as as the value returned from ToString

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
  * You are currently throwing `NotSupportedException`, `NotImplementedException` or an exception that is considered unrecoverable. Unrecoverable exceptions should not be getting caught and will be dealt with on a broad level or by a high-level catch-all handler. Therefore, users are not expected to have code that catches these explicit exceptions. The list of unrecoverable exceptions are:
     * `StackOverflowException`
     * `SEHException`
     * `ExecutionEngineException`
     * `AccessViolationException`
* Removing an exception that was being throw. However, it is not breaking when:
  * The API allows more robust behavior. For example, a Divide method which only worked on positive values, but threw an exception otherwise, can be changed to support all values and the exception is no longer thrown.




