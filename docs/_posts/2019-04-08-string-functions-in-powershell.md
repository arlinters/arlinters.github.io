---
layout: post
title: "String Functions In PowerShell"
categories: Programming
tags: [methods, powershell, programming, strings]
author:
  - Charles Winters
---
When working with objects in PowerShell, it can be useful to know how to manipulate strings.

This post covers some methods that I find pretty useful for manipulating strings. These methods are:
- [IndexOf](#indexof)
- [Insert](#insert)
- [Remove](#remove)
- [Split](#split)
- [Substring](#substring)
- [Trim](#trim)

With each object type, you inherit a set of methods that you can use pretty much universally. To check out which methods are available to you, you can use the Get-Member function. This function is incredibly helpful for mapping out what is available to you! To use this, pipe the object to Get-Member.

{% highlight powershell %}
#String to Get-Member
"Hello World" | Get-Member 

#Variable to Get-Member
$variable | Get-Member
{% endhighlight %}

When you check the members for a string type, it will output a lot of methods that are available to use. The output will look something like this:

![Get-Member Example](/assets/img/string-functions-in-powershell/get-member.png)

For the purpose of manipulating a string, the key methods are *Insert*, *IndexOf*, *Remove*, *Replace*, *Split*, *Substring*, *Trim*, *TrimEnd* and *TrimStart*. In the sections below I’ll try to cover each method. In the examples, I will have the string "Hello World" contained in the variable **$var**.

{% highlight powershell %}
$var = "Hello World"
{% endhighlight %}

# IndexOf
The IndexOf method is really useful in locating a spot in a string to perform your manipulations. One thing to keep in mind is that it will return the first match. In the example of the string “Hello World”, if you search for the index of the character **l**, it will return the index of the very first **l**.

{% highlight powershell %}
PS C:\Users\noxil> $var.indexOf('l')
2
{% endhighlight %}

For the next method, we’ll insert at the space. Let’s get the index of that character.

{% highlight powershell %}
PS C:\Users\noxil> $var.indexOf(" ")
5
{% endhighlight %}


# Insert
The insert method is used when you need to insert a string into the string you are calling the method on. It will insert the string before the index you specify.

When we check the Method for insert with the Get-Member cmdlet, the definition says string **Insert(int *startIndex*, string *value*)**. When we call this method, we need to provide the index of where to insert the string and then the string that we’ll be inserting.

If I wanted to insert the word Beautiful into the string *Hello World* in between *Hello* and *World*, we could do this with insert. Here’s an example.

{% highlight powershell %}
PS C:\Users\noxil> $var.Insert(5, " Beautiful")
Hello Beautiful World
{% endhighlight %}

Notice how the index for the space was chosen, and part of the string we were inserting contained a space before? The string was inserted prior to the startIndex.

# Remove
The Remove method can be used to remove a section of your string. For this method it’s overloaded so there are essentially two methods wrapped into one name that you can choose from.

When checking the method for Remove with the Get-Member cmdlet, the definitions are **string Remove(int *startIndex*, int *count*)**, and **string Remove(int *startIndex*)**.

The first definition, you specify the index of the first character to remove, and the next parameter is how many characters after that to remove. In the string Hello World, we’ll start at the 2nd index and increment through to display the different possibilities.

{% highlight powershell %}
PS C:\Users\noxil> $var.Remove(2,0)
Hello World
PS C:\Users\noxil> $var.Remove(2,1)
Helo World
PS C:\Users\noxil> $var.Remove(2,2)
Heo World
PS C:\Users\noxil> $var.Remove(2,3)
He World
PS C:\Users\noxil> $var.Remove(2,4)
HeWorld
{% endhighlight %}

For the second definition, you specify the startIndex to begin the remove and it will remove all characters following that index.

{% highlight powershell %}
PS C:\Users\noxil> $var.Remove(2,0)
Hello World
PS C:\Users\noxil> $var.Remove(2,1)
Helo World
PS C:\Users\noxil> $var.Remove(2,2)
Heo World
PS C:\Users\noxil> $var.Remove(2,3)
He World
PS C:\Users\noxil> $var.Remove(2,4)
HeWorld
{% endhighlight %}

# Split
The split method is probably one that I use the most. It is useful for breaking a string out into an array. There are seven definitions for this function, but I will focus on the first definition, **string[] Split(Params char[] separator)**.

When calling this method on a string, the separator is the character you use as a delimiter to break the string into an array. It’s really common for if there is a string with spaces and you want to break it into an array of words.

{% highlight powershell %}
PS C:\Users\noxil> $var.split(' ')
Hello
World
{% endhighlight %}

If you were to save this to a variable, you could then iterate over each word, or select a word by index.

{% highlight powershell %}
PS C:\Users\noxil> $sentence = $var.split(' ')
PS C:\Users\noxil> $sentence[0]
Hello
PS C:\Users\noxil> $sentence[1]
World
{% endhighlight %}

Another example, one that I use quite frequently, is if there string array that contains a list of computer names. You can break out the computers and get a list of what identifiers you use.

In this example, lets say there are computers in NYC, LA and KC, the names might look like this.

- NYC-HR-PC01
- LA-IT-PC05
- KC-MKT-PC02

If we wanted to generate list of the unique identifiers, we could split the strings using the ‘-‘ character. It would produce 3 indices, the first being the location, the second being the department and the last being the computer number.

{% highlight powershell %}
PS C:\Users\noxil> $list
NYC-HR-PC01
LA-IT-PC05
KC-MKT-PC02

PS C:\Users\noxil> $list[0].split('-')
NYC
HR
PC01
{% endhighlight %}

Here’s an example that would display these descriptors a bit cleaner.

{% highlight powershell %}
PS C:\Users\noxil> $list | ForEach-Object {
>> $temp = $_.split('-')
>> [pscustomobject]@{
>> City = $temp[0]
>> Department = $temp[1]
>> Machine = $temp[2]
>> }
>> }

City Department Machine
---- ---------- -------
NYC  HR         PC01
LA   IT         PC05
KC   MKT        PC02
{% endhighlight %}

# Substring
Substring offers up two different methods. **string Substring(int *startIndex*)**, **string Substring(int *startIndex*, int *length*)**.

The first definition, essentially will remove everything before the index you specified. The second definition will return length characters after the startIndex.

Here is an example of the first definition.

{% highlight powershell %}
PS C:\Users\noxil> $var.Substring(5)
 World
{% endhighlight %}

Notice how we selected index 5, the index of the space, and it removed everything preceding it and left the space? Here’s an example of the second definition.

{% highlight powershell %}
PS C:\Users\noxil> $var.substring(6,4)
Worl
{% endhighlight %}
This starts at the sixth index and then only returns the next four characters.

# Trim
The Trim method is used to remove a character or set of characters from either the beginning or the end of a string. It tries to match starting at the beginning and run through to the end of the string. If it matches, it will try to remove it. If it doesn’t match anything it will return the original string.

{% highlight powershell %}
PS C:\Users\noxil> $var.trim('h')
Hello World

PS C:\Users\noxil> $var.trim('H')
ello World

PS C:\Users\noxil> $var.trim('e')
Hello World

PS C:\Users\noxil> $var.trim('Hel')
o World

PS C:\Users\noxil> $var.trim('d')
Hello Worl

PS C:\Users\noxil> $var.trim('orld')
Hello W
{% endhighlight %}

Those are the main methods for strings that I find useful with PowerShell. These types of methods are very common for strings in other languages. Once learned, it came become incredibly useful!