---
layout: post
title: "Getting Help With Get-Help"
categories: Programming
tags: [powershell]
author:
  - Charles Winters

excerpt: This is the cmdlet that I tend to use the most often. Especially when I am new to a cmdlet or new module.
---
# The Get-Help Cmdlet
This is the cmdlet that I tend to use the most often. Especially when I am new to a cmdlet or new module. To sum it up from the doc itself:
> The Get-Help cmdlet displays information about PowerShell concepts and commands, including cmdlets, functions, CIM commands, workflows, providers, aliases and scripts.
> 
> [https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-help?view=powershell-6](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-help?view=powershell-6)

When I am exploring a new module or command that I haven’t used before, this is a great starting point. Let’s say I am exploring the **Get-Process** cmdlet. To get a better understanding of the syntax of the **Get-Process** cmdlet I could type:

{% highlight powershell %}
Get-Help Get-Process
{% endhighlight %}

The output may look similar to this:

{% highlight powershell %}
NAME
    Get-Process

SYNTAX
    Get-Process [[-Name] <string[]>] [-ComputerName <string[]>] [-Module] [-FileVersionInfo]  [<CommonParameters>]

    Get-Process [[-Name] <string[]>] -IncludeUserName  [<CommonParameters>]

    Get-Process -Id <int[]> -IncludeUserName  [<CommonParameters>]

    Get-Process -Id <int[]> [-ComputerName <string[]>] [-Module] [-FileVersionInfo]  [<CommonParameters>]

    Get-Process -InputObject <Process[]> -IncludeUserName  [<CommonParameters>]

    Get-Process -InputObject <Process[]> [-ComputerName <string[]>] [-Module] [-FileVersionInfo]  [<CommonParameters>]


ALIASES
    gps
    ps


REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. It is displaying only partial help.
        -- To download and install Help files for the module that includes this cmdlet, use Update-Help.
        -- To view the Help topic for this cmdlet online, type: "Get-Help Get-Process -Online" or
           go to https://go.microsoft.com/fwlink/?LinkID=113324.
{% endhighlight %}

Notice the remarks at the bottom? It is only displaying partial help! If you see this, run the cmdlet **Update-Help**. You may need to run PowerShell as an Administrator to update these help files.

After updating the Help files this is what the output of running **Get-Help Get-Process**.

{% highlight powershell %}
NAME
    Get-Process

SYNOPSIS
    Gets the processes that are running on the local computer or a remote computer.


SYNTAX
    Get-Process [[-Name] <String[]>] [-ComputerName <String[]>] [-FileVersionInfo] [-Module] [<CommonParameters>]

    Get-Process [-ComputerName <String[]>] [-FileVersionInfo] -Id <Int32[]> [-Module] [<CommonParameters>]

    Get-Process [-ComputerName <String[]>] [-FileVersionInfo] -InputObject <Process[]> [-Module] [<CommonParameters>]

    Get-Process -Id <Int32[]> -IncludeUserName [<CommonParameters>]

    Get-Process [[-Name] <String[]>] -IncludeUserName [<CommonParameters>]

    Get-Process -IncludeUserName -InputObject <Process[]> [<CommonParameters>]


DESCRIPTION
    The Get-Process cmdlet gets the processes on a local or remote computer.

    Without parameters, this cmdlet gets all of the processes on the local computer. You can also specify a particular process by process name or process ID (PID) or
    pass a process object through the pipeline to this cmdlet.

    By default, this cmdlet returns a process object that has detailed information about the process and supports methods that let you start and stop the process. You
    can also use the parameters of the Get-Process cmdlet to get file version information for the program that runs in the process and to get the modules that the
    process loaded.


RELATED LINKS
    Online Version: http://go.microsoft.com/fwlink/?linkid=821590
    Debug-Process
    Get-Process
    Start-Process
    Stop-Process
    Wait-Process

REMARKS
    To see the examples, type: "get-help Get-Process -examples".
    For more information, type: "get-help Get-Process -detailed".
    For technical information, type: "get-help Get-Process -full".
    For online help, type: "get-help Get-Process -online"

{% endhighlight %}

Much better!

The Description section gives a great overview of the scope of the command and what to look out for. The Syntax field shows different variations of using the command. Related Links is great for similar cmdlets that are often used in conjunction with the one you’re reading about.

When checking out a new cmdlet, the -examples switch is a great resource with **Get-Help**. Often, these examples will show an example of each syntax type and also provide a description on the intended result. Below are a couple of examples from **Get-Help Get-Process -examples**.

{% highlight powershell %}
Example 1: Get a list of all active processes on the local computer

    PS C:\>Get-Process

    This command gets a list of all active processes running on the local computer. For a definition of each column, see the "Additional Notes" section of the Help
    topic for Get-Help.
    Example 2: Get all available data about one or more processes

    PS C:\>Get-Process winword, explorer | Format-List *

    This command gets all available data about the Winword and Explorer processes on the computer. It uses the Name parameter to specify the processes, but it omits
    the optional parameter name. The pipeline operator (|) passes the data to the Format-List cmdlet, which displays all available properties (*) of the Winword and
    Explorer process objects.

    You can also identify the processes by their process IDs. For instance, `Get-Process -Id 664, 2060`.
{% endhighlight %}

Through this cmdlet, I have seen examples that have helped create an easier solution! These are also great for a quick refresher on syntax. I tend to use it quite a bit, especially for the Active Directory module.

In future posts, I will probably expand on this. PowerShell offers a lot of great learning resources within the Get-Help cmdlet.