All .NET Core assemblies are [strong-named](http://msdn.microsoft.com/en-us/library/wd40t7ad.aspx). We do this for two reasons:

1. _Compatibility_. We want to maintain type identity with previous versions of our assemblies that have shipped across all of our platforms. Removing a strong-name from an assembly is a breaking change, and would break consumption and runability of libraries built against the old identities.

2. _Serviceability_. When running on .NET Framework, in contrast to other framework assemblies that are placed in the [GAC](http://msdn.microsoft.com/en-us/library/yf1d93sz.aspx), some of .NET Core assemblies ship locally ("app-local") with the application. To continue to be able to service these libraries for critical security updates, we make use of our [app-local servicing](http://blogs.msdn.com/b/dotnet/archive/2014/01/22/net-4-5-1-supports-microsoft-security-updates-for-net-nuget-libraries.aspx) feature which has a strict requirement that assemblies have strong names.

##  FAQ

### 1. Why do we need strong names?
For the most part, the majority of applications and libraries do not need strong names. They are mostly left over from v1 days of .NET where [sandboxing](http://en.wikipedia.org/wiki/Sandbox_(computer_security)) needed to differentiate between code that was trusted, versus code that was untrusted. However in recent years, sandboxing via AppDomains, especially to [isolate ASP.NET web applications] (http://support.microsoft.com/kb/2698981), is no longer guaranteed and is not recommended. 

Strong names are still required in some small situations, most of which are called on this page: [Strong-Named Assemblies](http://msdn.microsoft.com/en-us/library/wd40t7ad.aspx).