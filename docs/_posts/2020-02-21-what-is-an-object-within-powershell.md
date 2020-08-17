---
layout: post
title: "What is an Object within PowerShell?"
categories: Programming
tags: [classes, fileInfo, object, powershell]
author:
  - Charles Winters
---
I think one of the most beautiful things about PowerShell is how easy it is to interact with the data. Most of the time, this interaction can be handled with the built-in Cmdlets thanks to the properties and functions that are readily available.

# What constitutes an object in programming?
In programming, an object is a specific instance of a class. Classes are created by other programmers to describe common methods and properties for something that will commonly be used within their program. These properties and methods are known as members. If you’re unfamiliar with methods and properties, I’ll do my best to give a general overview in this post but may do a deeper dive in a future post.

If you’re working with PowerShell, I am betting that you are familiar with Active Directory. Within that system, there are several different components that I think are a good example for explaining objects.

So, in Active Directory there are User Objects and Computer Objects. For a particular user object there are a ton of properties available to describe that specific user such as their name, department, phone number, and so on. Each one of these users would be a particular instance of that user class. Each with the same properties available but the values of those may be different.

When you create a new instance of a class, it *instantiates* that class ultimately creating an object.

Now back to PowerShell. Everything *(as far as I know of at this point)* that you interact within the shell is going to be an object since everything stems from a class from .Net or some user-defined class.

Take a simple text file for example with the name **Test.txt**. With the **Get-ChildItem** Cmdlet, I can check the file system for this file.

{% highlight powershell %}
Get-ChildItem .\Test.txt


    Directory: C:\Users\CharlesWinters


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/20/2020   8:50 PM              0 Test.txt

{% endhighlight %}

At first glance, this just seems like some information that you could get by just looking at this file in file explorer but one of the great things about PowerShell is more often than not there is something beneath the surface.

By using the **Get-Member** Cmdlet we can see what properties and functions are available to us.

{% highlight powershell %}
 Get-ChildItem .\Test.txt |gm


   TypeName: System.IO.FileInfo

Name                      MemberType     Definition
----                      ----------     ----------
LinkType                  CodeProperty   System.String LinkType{get=GetLinkType;}
Mode                      CodeProperty   System.String Mode{get=Mode;}
Target                    CodeProperty   System.Collections.Generic.IEnumerable`1[[System.String, mscorlib, Version=...
AppendText                Method         System.IO.StreamWriter AppendText()

\# There are more but to save screen space this is redacted :-)

{% endhighlight %}

At the very top of this output there is TypeName. This describes the class that this object inherited from. This can be extremely useful if you are doing some troubleshooting. If it’s a standard built-in Cmdlet, you can often find some .NET documentation on this! For example, here is the documentation for this one, [System.IO.FileInfo](https://docs.microsoft.com/en-us/dotnet/api/system.io.fileinfo?view=netframework-4.8).

By checking the members of this particular object, there are some pretty useful properties predefined that can make other scripts that we write easier. A common one that I find myself using when working with files or directories are the properties **FullName**, **Extension**, **LastWriteTime**, **LastAccessTime**.

{% highlight powershell %}
Get-ChildItem .\Test.txt |gm


   TypeName: System.IO.FileInfo

Name                      MemberType     Definition
----                      ----------     ----------
...
<#
There are more but to save screen space this is redacted :-)
#>
...
Extension                 Property       string Extension {get;}
FullName                  Property       string FullName {get;}
LastAccessTime            Property       datetime LastAccessTime {get;set;}
LastWriteTime             Property       datetime LastWriteTime {get;set;}

{% endhighlight %}

{% highlight powershell %}
# Checking the value of a property
$File = get-childitem .\Test.txt
$file.Extension
.txt
{% endhighlight %}

By knowing that this property exists on this object, it should be safe to assume that any other object that is instantiated from [System.IO.FileInfo](https://docs.microsoft.com/en-us/dotnet/api/system.io.fileinfo?view=netframework-4.8) should share those same properties. This allows you to do some scalable scripting.

Here is an example of a possible use-case:

We are needing to find every single .txt file that is present in a directory. We know that each file that is created, is instantiated from [System.IO.FileInfo](https://docs.microsoft.com/en-us/dotnet/api/system.io.fileinfo?view=netframework-4.8) so therefore it should have the Extension property. If the extension is .txt, then checking if the property is equal to “.txt”, it should evaluate to true.

{% highlight powershell %}
Get-ChildItem | Where-Object {$_.Extension -eq ".txt"}


    Directory: C:\Users\CharlesWinters


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/20/2020   9:22 PM              0 PhoneNumbers.txt
-a----        2/20/2020   9:22 PM              0 random-notes.txt
-a----        2/20/2020   8:55 PM             34 Test.txt

{% endhighlight %}

By using that property that is readily available, you are able to check for any file with that extension with just one line of code.

At some point down the road, I’d like to write a couple of posts about creating your own custom classes and objects. Is there anything you’d like to see me write about? Shoot me a message or post a comment and I’ll do my best to follow up!