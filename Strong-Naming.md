All .NET Core assemblies are [strong-named](http://msdn.microsoft.com/en-us/library/wd40t7ad.aspx). We do this for two reasons:

1. _Compatibility_. We want to maintain type identity with previous versions of our assemblies that have shipped across all of our platforms. Removing a strong-name from an assembly is a breaking change, and would break consumption and runability of libraries built against the old identities.

2. _Serviceability_. When running on .NET Framework some of .NET Core assemblies ship locally ("app-local") with the application, this is in contrast to other framework assemblies that are placed in the [GAC](http://msdn.microsoft.com/en-us/library/yf1d93sz.aspx)). To continue to be able to service these libraries for critical security updates, we make use of the [app-local servicing](http://blogs.msdn.com/b/dotnet/archive/2014/01/22/net-4-5-1-supports-microsoft-security-updates-for-net-nuget-libraries.aspx) feature which has a strict requirement that assemblies have strong names.

##  FAQ

### 1. Why do we need strong names?
For the most part, the majority of applications and libraries do not need strong names. They are mostly left over from previous eras of .NET where [sandboxing](http://en.wikipedia.org/wiki/Sandbox_(computer_security)) needed to differentiate between code that was trusted, versus code that was untrusted. However in recent years, sandboxing via AppDomains, especially to [isolate ASP.NET web applications] (http://support.microsoft.com/kb/2698981), is no longer guaranteed and is not recommended. 

Strong names are still required in some rare situations, most of which are called out on this page: [Strong-Named Assemblies](http://msdn.microsoft.com/en-us/library/wd40t7ad.aspx).

### 2. What kinds of problems do I run into after strong naming?
There are really two major problems that developers run into after strong naming their assemblies:

1. _Virality_. Once you've strong named an assembly, you can only statically reference other strong named assemblies. 

2. _Binding Policy_. When developers talk about strong names, they are usually conflating it with the strict binding policy of the .NET Framework that kicks in _when_ you strong name. This binding policy _is_ problematic as it forces an exact match between reference and version, and requires developers to author complex [binding redirects](http://msdn.microsoft.com/en-us/library/eftw1fys.aspx) to work around the policy. In recent versions of Visual Studio, however, we've added [Automatic Binding Redirection](http://msdn.microsoft.com/en-us/library/2fc472t2.aspx) as an attempt to reduce pain of this policy on developers. On top of this, other newer platforms, including _Silverlight_, _WinRT-based platforms_, _.NET Native_ and _ASP.NET 5_ the policy has been loosened, allowing later versions of an assembly to satisfy earlier references, thereby completely removing the need to ever write a binding redirect.