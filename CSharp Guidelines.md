# 🔡 C# Guidelines

## Contents

- [Helpers and Enforcers](#helpers-and-enforcers)
- [Capitalization](#capitalization)
  - [Notation](#notation)
  - [Rules](#rules)
- [Naming](#naming)
  - [Local Variables](#local-variables)
  - [Private Variables](#private-variables)
  - [Properties](#properties)
  - [Constants](#constants)
  - [Consistency](#consistency)
  - [Methods](#methods)
  - [Classes and Interfaces](#classes-and-interfaces)
  - [Generic typing](#generic-typing)
  - [Enumerators](#enumerators)
  - [Source Code Files](#source-code-files)
  - [Namespaces](#namespaces)
- [Comments](#comments)
  - [Simple](#simple)
  - [Class documenting](#class-documenting)
  - [Source Control](#source-control)
- [Data Types](#data-types)
- [Formatting](#formatting)
  - [Spaces and lines](#spaces-and-lines)
  - [Indentation](#indentation)
  - [Curly Braces](#curly-braces)
  - [Conditionals](#conditionals)
  - [Using vs Namespace](#using-vs-namespace)
- [Good Practices](#good-practices)
  - [Style Guide](#style-guide)
  - [Calling RESTful APIs](#calling-restful-apis)
  - [Design](#design)
  - [Logging](#logging)
  - [Error handling](#error-handling)

---

## Helpers and Enforcers

There are a couple of ways to enforce (some) aspects of this convention/guideline. One of them is the usage of an [`.editorconfig`](https://github.com/lfmundim/Blip.Api.Template.K8s/blob/master/content/Api/.editorconfig) bundled with our [API Template](https://github.com/lfmundim/Blip.Api.Template.K8s)

- If your API was created with the template, you shouldn't need to add anything
- If it wasn't, just add the file to your solution **at the same level of your `.sln` file**
  - the only acceptable reason for this to happen is if your API is _legacy_. If you're creating a brand new one **you should use the template**

---

## Capitalization

### Notation

The following terms describe different forms of capitalization:

- `PascalCase`
- `camelCase`
- `UPPER_CASE`
- `_camelCase with underscore`

### Rules

Whenever an identifier is composed by many words, **do not** use underscores (`_`) or hyphens (`-`), except on `UPPER_CASE` scenarios, where word separation would be tricky.
| Identifier     | Notation                   |
|----------------|----------------------------|
| Class          | PascalCase                 |
| Interface      | PascalCase                 |
| Enum Type      | PascalCase                 |
| Enum Value     | PascalCase                 |
| Private fields | _camelCase with underscore |
| Static fields  | PascalCase                 |
| Constants      | UPPER_CASE                 |
| Parameters     | camelCase                  |
| Methods        | PascalCase                 |
| Public fields  | PascalCase                 |

> the _only_ exception for these rules is for files generated via `svcutil`
---

## Naming

The following definitions apply to local and private variables, properties and parameters:

- Use something **descriptive**, **meaningful** and **in english**
  - the _only_ exception for these rules is for files generated via `svcutil`
- Even for a temporary variable name that might appear only in a few lines of code, **use a descriptive name**
- Use single char variables (such as `i`) **only** for indexes on a loop or `lambda expressions`
  - Avoid it whenever possible, in favor of a descriptive name: if your `lambda` is iterating through `Contacts` do not use `x`, but `c` or even `contact` as the singular form of the iterable
- Object collections (`IList<T>`, `IDictionary<TKey, TValue>`, `Array<T>` etc) are identified by words in their **plural form**

### Local Variables

Local variable names (declared within methods and used as parameters) should follow:

- `camelCase`
- In `lambda expressions` use the **first letter of the related class or iterable**, or the singular form
- When declaring variables, prefer _implicit typing_ (`var`) over _explicit typing_

Do:

```cs
var messages = new List<Message>();
var sourceMessages = messages.Where(m => m.Source.Equals(comparingString));
```

Don't:

```cs
List<Message> messages = new List<Message>();
var sourceMessages = messages.Where(x => x.Source.Equals(comparingString));
```

### Private Variables

- `_camelCase with underscore`
- Use nouns or noun clauses

Do:

```cs
// Class variable
private static ServiceHost _serviceHost;
// Local variable
var userName = "User Name";
```

Don't:

```cs
// Class variable
public ArrayList glst;  
// Local variable
string UserName;
```

### Properties

- `PascalCase`
- Use nouns, noun clauses or adjectives
- Consider prefixing `bool` properties with `Is`, `Can`, `Has`, `Should`
- For identifiers or data entity keys, include the suffix `Id`
- Don't create properties conflicting with methods
  - Example: don't create the property `PersonAddress` if you have a method `GetPersonAddress`
- Consider giving your property the same name as your type -- specially `enum`s
- Tip: if on `VisualStudio`, the snippet `prop` followed by two tab-presses gives you a property template

Do:

```cs
public class Message
{
    public int MessageId { get; set; }
    public Peer From { get; set; }
    public Peer To { get; set; }
    public Content Content { get; set; }
    public string AppSpecific { get; set; }
    public bool ShouldNotifySource { get; set; }
}
```

Don't:

```cs
public class Message
{
    public int IDMessage { get; set; }
    public Content ContentData { get; set; }
    public bool SourceNotification { get; set; }
    public Content GetContentData()
    {
        return ContentData;    
    }
}
```

### Constants

- `UPPER_CASE`
- Straightforward naming, indicating it's use with precision
- Consider using `MIN`, `MAX` or `DEFAULT` prefixes for limits or default values

Do:

```cs
public class MessageDispatcher
{
    public const int MAX_SEND_RETRIES = 3;
    public const string DEFAULT_SENDER_ADDRESS = "sender@server.com"; 
}
```

Don't:

```cs
public class MessageDispatcher
{
    public const int RETRY = 3;
    public const string SenderAddress = "sender@server.com"; 
}
```

### Consistency

- Variables representing the _same data_ within the program should be consistently named throughout every part of the program
- The only time you should change this name is when you need to add a prefix or change its capitalization

Do:

```cs
var publishDate = DateTime.Parse(publishDateInput.Text);
// (...)
books[index].PublishDate = publishDate;
// (...)
publishDate = book.PublishDate;
```

Don't:

```cs
DateTime dtPub;
// (...)
dtPub = DateTime.Parse(txtPubDate.Text);
// (...)
mBooksArray[index].published = dtPub;
// (...)
pubDate = book.DatePublshd;
```

### Methods

- `PascalCase`
- Method names should be descriptive, meaningful and in english
- Method names should describe **what** a method does, not **how**
- Use verbs or expressions
- `async` methods (that return `Task` or `Task<T>`) must have the `Async` suffix

Examples:

```cs
private bool IsDefault()
{
    // (...)
}

public Task<string> GetStringAsync(Uri address)
{
    // (...)
}

public decimal GetInvoiceTotal()
{
    // (...)
}
```

### Classes and Interfaces

- `PascalCase`
- Class/Interface names should be descriptive, meaningful and in english
- Class/Interface names should describe what they represent
- Usually formed by nouns
- Prefix interfaces with `I`
- **Don't** prefix classes with letters
- Access modifiers should be declared **explicitly** _(this applies to classes members, too)_

Do:

```cs
public interface ISender
{
    // (...)
}

public class ItemType
{
    // (...)
}
```

Don't:

```cs
public interface Sender
{
    // (...)
}

public class TItemType
{
    // (...)
}

class Foo
{
    // (...)
}
```

- Consider adding the `base` class name as a suffix when using inheritance, specially from `abstract` classes
- Always add the `base` class name when using .NET defined classes as base, such as `Exception`, `Attribute`, `Stream`
- The default interface implementation must have the same name, without the `I` prefix

### Generic typing

- Use `T` as the generic type parameter if there is only one present
- If more than one generic type is present, use `PascalCase` to create a descriptive name, and prefix it with `T`

Example:

```cs
public class Message<TContent, TDestination> where TDestination : Peer
{
    public TContent Content { get; set; }

    public TDestination Destination { get; set; }
}
```

### Enumerators

- Same principles from classes apply here

### Source Code Files

- Give files the same name as the class they contain.

Example: a file containing the class `ItemType` should be called `ItemType.cs`

### Namespaces

Namespaces must follow the pattern `Take.[Product].[Module].[SubModule]`

- Use meaningful, descriptive, singular names, in english
- `PascalCase`

Example:

```cs
namespace Take.Api.Localiza
{
    // (...)
}

namespace Take.Api.Localiza.Services
{
    // (...)
}
```

---

## Comments

### Simple

- **Avoid comments whenever possible**
- Source Code comments should be clear, concise, in english and must have proper grammar
- Begin comment lines with `//` at the current indent level
- **Don't** use `/*...*/`
- Don't make use of comments for humour
- Your comments shouldn't try to teach the reader how to program, presume they know just as much as you do concerning the programming language
- Comments should not be used to describe the obvious, but **remember that your logic is not that obvious for other people**.
  - Comments exist to improve code readability
  - That said, frequently good variable/method names already do that
- **Never** comment-out code. Commented-out code **must** be removed
  - Should you need that code in the future, version control will help you
- Tip: `Ctrl+K, Ctrl+C` comments a block of code, `Ctrl+K, Ctrl+D` uncomments

### Class documenting

- Try to comment every public method and property, _specially if it is a library or module available for more projects_
  - Even methods with a clear name can be better descripted with documentation.
- When dealing with Interfaces and Classes, prefer documenting the Interface, most of the time it is what is exposed

Example:

```cs
/// <summary>
/// Get the default product price in the current store
/// </summary>
/// <param name="product">Instance of <see cref="Product"/></param>
/// <returns>Instance of <see cref="Price"/> with the valid prices
/// for the product</returns> 
private Price GetPrice(Product product)
{
    (...)
}
```

### Source Control

- Comments are mandatory on every `commit`
- They should explain in a direct way the main changes
- In english
- To avoid long commit messages, commit as often as you can
  - Git good practices dictate that you should commit on every functional portion of the code that is changed. Even if your new method is not yet accessible, if it is functional, you should commit. This way you won't forget important changes that were made during the commit but aren't in the comment

---

## Data Types

Always use `C#` pre-defined type, **not** `System`s aliases

Do:

```cs
private readonly string _firstName { get; set; }
private readonly bool _isPerson  { get; set; }
private readonly short _itemCounter  { get; set; }
```

Don't

```cs
private readonly String _firstName { get; set; }
private readonly Boolean _isPerson { get; set; }
private readonly Int16 _itemCounter { get; set; }
```

---

## Formatting

### Spaces and lines

- Use one space before and after mos operators.
  - Exceptions include `++`,`--`
  - `;` should be the last character of the line, with no whitespaces after it
- Separate code blocks with blank lines
- One expression per line

Do:

```cs
string city;
string state;
string zip;
```

Don't:

```cs
string city, state, zip;
```

### Indentation

- Every expression inside classes, methods, loops, switches, try/catch blocks should be indented
- Use the `tab` key to indent
  - define the indentation as **soft tabs** of **four** spaces (**not** the `tab` character or hard tab)
- Tip: `Ctrl+K, Ctrl+D` helps you to indent your file

### Curly Braces

- Align curly braces vertically, with the opening and closing being the only expression in line

Do:

```cs
public void ShowMessage(string message, bool showTime)
{
    if (showTime)
    {
        DateTime currentTime = DateTime.Now;
        message = string.Format("{0}. {1}", message, 
            currentTime);
    }

    MessageBox.Show(message);
}
```

Don't:

```cs
public void ShowMessage(string message, bool showTime)   {
    if (showTime)  {
    DateTime currentTime = DateTime.Now;
    message = string.Format("{0}. {1}", message, 
    currentTime);
    }

    MessageBox.Show(message);
}
```

### Conditionals

- **Always** use curly braces on conditionals, even if they execute a single line

Do:

```cs
if(this)
{
    that();
}
```

Don't:

```cs
if(this)
    that();
```

- When there's more than one condition, break lines into singles or, at max, doubles
  - start lines with the logic operators
  - indent one level

Do:

```cs
if (txtID.Text.Length == 5
    && !string.IsNullOrEmpty(txtFirstName.Text)
    && txtClass.Text.Length == 2)
{
    cmdCalculate.Enabled = true;
}
```

Don't:

```cs
if (txtID.Text.Length == 5 && txtFirstName.Text != "" && test == true && txtLastName.Text != "" && txtStartDate.Text != "" && txtClass.Text.Length == 2)
{
    cmdCalculate.Enabled = true;
}
```

### Using vs Namespace

- Prefer importing the namespace via `using` rather than fully quallifying the class

Do:

```cs
using System.Net;
// (...)
var ipAddress = IPAddress.Parse("192.168.0.1");
```

Don't:

```cs
var ipAddress = System.Net.IPAddress.Parse("192.168.0.1");
```

---

## Good Practices

### Style Guide

- Use one class per file, [matching the file name convention](https://dev.azure.com/curupira/Takepedia/_wiki/wikis/Takepedia.wiki/1126/%F0%9F%94%A1-.NET-(CSharp)-Guidelines?anchor=2.10-source-code-files)
- **Never** use hard-coded variables such as a folder path or a table identifier. Use the settings file for that.
  - Regarding folder paths, prefer relative paths or dynamically find them using `Path`
- **Avoid at all costs** "magic variables", prefer extracting them to `constant`s within your file, **specially** if the value is repeated
  - Know your types: when initializing variables, consider using `default` to replace magic values, when applicable.
    - `default(int)` is `0`
    - `default(double)` is `0`
    - `default(long)` is `0`
    - `default(CustomClass)` is `null` unless specified otherwise
    - `default(string)` is `null`
    - `default(bool)` is `false`
    - if you have a type that the compiler can already infer, you can simply use `default` insead of `default(type)`
- Compare strings using `.Equals()`, use the `StringComparison.OrdinalIgnoreCase` parameter if you need the comparison to be **case insensitive**
- Check for empty or null strings with `string.IsNullOrWhitespace()`
- Use `string.Empty` instead of `""` to declare empty strings
- If you return a collection, return an **empty collection** instead of `null` on no data to return
- Use `StringBuilder` to concatenate strings on loops
  - Whenever you concatenate a `string` on .NET, the original object is discarded and a new one is instantiated, which is costly
- `Regex` should not be instantiated within methods. Should you need to use a regular expression within a single method, use the static method `Regex.Match(string, pattern)`
  - if the regular expression is used in more than one method, create a private readonly field to ensure a single instance
- Priorize parallel tasks whenever possible, using `Task.WhenAll()` or `Task.WhenAny()` when applicable
  - [This](https://dev.azure.com/curupira/_git/Takepedia?path=%2FAsyncSample.ipynb&_a=preview) article explains the advantages and how to use `Task.WhenAll()`
- File organization should follow this structure:
  - usage of `#region` is optional and helps organization

```cs
// imports, separated or not by whitelines
using System;

// one whiteline between imports and namespace
namespace Csharp.Guideline.Sample
{
    public class SampleClass
    {
        // public properties, separated by whitelines
        // do not separate xml doc from property

        /// <summary>
        /// Sample String Doc
        /// </summary>
        public string SampleString { get; set; }

        /// <summary>
        /// Sample Int Doc
        /// </summary>
        public int SampleInt { get; set; }

        // private fields. Separate with whitelines only readonly from const
        // also separate with whiteline from public props
        private readonly ISender _sender;
        private readonly IBucketExtension _bucket;

        private const string SAMPLE = "sample";

        // public constructors. Separated from each other with one whiteline. 
        // Separate from the fields with one whiteline.
        // do not separate xml docs from constructors

        /// <summary>
        /// Sample Ctor doc
        /// </summary>
        public SampleClass(ISender sender, IBucketExtension bucket)
        {
            _sender = sender;
            _bucket = bucket;
        }
        
        // public methods. Separated from each other by a whiteline
        // do not separate XML docs from methods
        // separate public methods from ctors with one whiteline

        /// <summary>
        /// sample public method doc
        /// </summary>
        public async Task SampleMethodAsync()
        {
        }

        // private methods. Separated from each other by a whiteline
        // separate private methods from public methods with one whiteline
        private async Task SamplePrivateMethodAsync()
        {
        }
    }
}
```

### Calling RESTful APIs

Most of the time when creating programs you will need to integrate with third party APIs, maybe a customer's, maybe from a service provider (such as Google Maps), and so on.

If you're lucky, those APIs will be `RESTful` APIs, which are usually very easy to test and integrate with. `C#` offers many ways to do so, including `RestSharp` or via the native `HttpClient`.

The _preferred_ way though, **which we adopt as a convention**, is using [RestEase](https://github.com/canton7/RestEase). It is _fairly well documented_ within the readme and we have [an article](https://dev.azure.com/curupira/Takepedia/_wiki/wikis/Takepedia.wiki/1070/%F0%9F%8C%90-RestEase-.NET-Rest-clients-with-ease) written by André Minelli that explains it even further. **Use RestEase every time you're integrating with a RESTful API and have more time to implement what _actually_ brings value to the project**

If you choose to use the native `HttpClient`, make sure to use [IHttpClientFactory](https://docs.microsoft.com/pt-br/aspnet/core/fundamentals/http-requests?view=aspnetcore-3.0) to make Http Requests, since the misuse of HttpClient can lead to [many problems](https://docs.microsoft.com/pt-br/dotnet/architecture/microservices/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests#issues-with-the-original-httpclient-class-available-in-net), including [socket exhaustion](https://www.aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### Design

- Avoid long methods. Aim for anywhere in between 1 to 25 lines. If your method is longer than 25 lines, consider refactoring your code
  - the [extract method refactor](https://refactoring.guru/extract-method) usually is a good, simple choice
- Your methods should do a single job. Don't combine more than one functionality within a method.
- Never assume the route for your application
- Avoid long files. If your file has more than 1000 lines of code (LOC), it should be refactored into two or more classes
- Avoid public methods unless they are needed outside the class
- Use `enum` whenever you need. Don't use numbers or strings for discrete values

Do:

```cs
public enum MailType
{
    Html,
    PlainText
}

private void SendMail(string message, MailType mailType)
{
    switch (mailType)
    {
        case MailType.Html:
        // HTML
        break;
        case MailType.PlainText:
        // Text
        break;
    }
}
```

Don't:

```cs
void SendMail(string message, int mailType)
{
    switch (mailType)
    {
        case 1:
            // HTML
            break;
        case 2:
            // Text
            break;
    }
}
```

- Avoid passing many parameters to a method. If you're passing more than 5 or 6, create a class and use it instead
- Organize your project with folders
- Declare variables as close as possible to where you're using them
- Always define `interface`s for your `class`es
  - this helps a lot on unit tests, allowing you to mock the reference

### Logging

- Use SEQ's special syntax that separate variables into readable models
  - use `{@property}` for classes, `{property}` for `string`
  - the name inside the curly bracket does not need to match the param name, the order is what defines which param will be used. Use this to give consistent names for the properties on SEQ
  - the ammount of params _can be greater_ than the ammount of brackets. The property will still be added to the log

Do:

```cs
_logger.Debug("Received {@contact} from BLiP using {identifier}", contact, userIdentifier);
_logger.Error(ex, "Failed to receive contact from BLiP using {identifier}", userIdentifier);
```

Don't:

```cs
_logger.Debug($"Received {contact} from BLiP using {identifier}", contact, userIdentifier);
_logger.Error(ex, $"Failed to receive contact from BLiP using {identifier}", userIdentifier);
```

- Use [propper logging levels](https://dev.azure.com/curupira/Takepedia/_wiki/wikis/Takepedia.wiki/323/Padr%C3%B5es-de-Log-usando-o-Seq-e-o-Serilog)

### Error handling

- Error messages should _help_ the user to understand what went wrong, don't use simple messages such as "Error" or "Something went wrong". Use descriptive messages such as "An error occurred while accessing the Database. Check user/password combination".
  - that said, **never** expose **any** thrown `Exception`. **At most** return the `Exception.Message` property.
- Log **every error**
  - When logging exceptions, prefer the overloads that receive the exception as a separate parameter, alongside the [logging styleguide](https://dev.azure.com/curupira/Takepedia/_wiki/wikis/Takepedia.wiki/1126/%F0%9F%94%A1-.NET-(CSharp)-Guidelines?anchor=6.4-logging)

Do:

```cs
try
{
    // (...)
}
catch (Exception ex)
{
    _logger.Error(ex, "Failed to do this. Request {@request}", request);
    // or
    _logger.Warning(ex, "Failed to do this. Request {@request}", request);
}
```

Don't:

```cs
try
{
    // (...)
}
catch (Exception ex)
{
    _logger.Error("Failed to do this. Request {@request}. Exception: {@ex}", request, ex);
    // or
    _logger.Warning("Failed to do this. Request {@request}. Exception: {@ex}", request, ex);
}
```

- When throwing a captured exception, throw the original one, not the materialized version. The materialized version creates a new stacktrace from that point on, and you'll lose the original source of the problem

Do:

```cs
try
{
    // (...)
}
catch (Exception ex)
{
    // handling
    throw;
}
```

Don't:

```cs
try
{
    // (...)
}
catch (Exception ex)
{
    // handling
    throw ex;
}
```
