C# Coding Style
===============

### Use [Allman style](http://en.wikipedia.org/wiki/Indent_style#Allman_style) braces, where each brace begins on a new line.
```C#
// Good
if (x == y)
{
    something();
}

// Bad
if (x == y) {
    something();
}

if (x == y) { something(); }
```

### Use a single line statement block without braces when the block ends flow control (only).
```C#
// Good
if (source == null) throw new ArgumentNullException("source");

if (source == null) return;

// Bad
if (source == null)
    throw new ArgumentNullException("source");

if (x == y) x = 5;
```

### Use the following `switch` style. Be consistent across the entire `switch`.
```C#
// Good
switch (type)
{
    case Type.Foo: return new Foo();
    case Type.Bar: return new Bar();
    default: throw NotImplementedException("Nooooo!");
}

switch (type)
{
    case Type.Foo: // Don't use compact style here -- since other cases can't
        return new Foo();

    case Type.Bar:
        thing = new Bar();
        break;

    case Type.Baz:
        if (bad) break;
        thing = new Baz();
        break;

    default:
        throw NotImplementedException("Nooooo!");
}

// use brackets if new scope is needed, but be consistent for all multi-line cases
switch (type)
{
    case Type.Foo: // Don't use compact style here -- since other cases can't
        return new Foo();

    case Type.Bar:
    {
        var thing = new Bar();
        thing.Foo();
        break;
    }
    case Type.Baz:
    {
        if (bad) break;
        var thing = new Baz();
        thing.Bar();
        break;
    }

    default:
        throw NotImplementedException("Nooooo!");
}

// Bad
switch (type)
{
    case Type.Foo: return new Foo();
    case Type.Bar:
        if (bad) break;
        return new Bar();
    default: throw NotImplementedException("Nooooo!");
}

switch (type)
{
    case Type.Foo: 
        if (good) break;
        return new Foo();
        
    case Type.Bar:
    {
        if (bad) break;
        return new Bar();
    }
    default:
        throw NotImplementedException("Nooooo!");
}
```

### Use four spaces of indentation (no tabs).

### Use `camelCase` to name all local variables.

### Use `_camelCase` for internal and private fields and use `readonly` where possible.
```C#
// Good
private Foo _bar;

private readonly Foo _bar;

// Bad
private Foo bar;
```

### Use `PascalCase` for static internal and private fields and use `readonly` whenever possible.
```C#
// Good
private static Foo Bar;

private static readonly Foo Bar;

// Bad
private readonly static Foo Bar;
```

### Use `PascalCase` to name all constant local variables and fields.

### Avoid `this.` unless absolutely necessary.

### Always specify the visibility, even if it's the default. Visibility should be the first modifier.
```C#
// Good
private string _foo;

public abstract class ...

// Bad
abstract public class ...
```
### Always specify namespace imports at the top of the file, *outside* of `namespace` declarations and should be grouped (`System.*`, `Ours.*`, `3rdParty.*`) and sorted alphabetically within each group.
```C#
// Good
using System;
using System.Linq;
using ion.Common;
using ion.Bar.Foo;
using ion.Foo.Bar;
using Newtonsoft.Json;

// Bad
using ion.Common;
using System.Linq;
using System;
using ion.Bar.Foo;
using Newtonsoft.Json;
using ion.Foo.Bar;
```

### Use the following spacing style.
```C#
// Good
if (var) return;

new Foo { Bar = "Bar" };

new Foo
    {
        Bar = "Bar",
        Baz = "Baz",
        FooBarBaz = "FooBarBaz"
    };

var list = items.Where(_ => _.Value > 5).Select(_ => new { _.Id, _.Name });

var list = items.Where(_ => _.Value > 5)
                .OrderBy(_ => _.Rate)
                .Select(_ => new { _.Id, _.Name });

// Bad
if(var) return;

new Foo{Bar="Bar"};

var list = items.Where(_ => _.Value > 5)
    .OrderBy(_ => _.Rate)
    .Select(_ => new { _.Id, _.Name });
```

### Avoid more than one empty line at any time. For example, do not have two blank lines between members of a type.

### Avoid spurious free spaces. Consider enabling viewable whitespace in your editor to aid detection.
```C#
// Good
if (someVar == 0)

// Bad
if (someVar == 0)... (where the dots mark the spurious free spaces)
```

### Use `var` whenever possible unless it significantly impacts readability.
```C#
// Good
var stream = new FileStream();

var stream = OpenMyStream();

// Bad
Stream stream = new FileStream();
```

### Use language keywords instead of BCL types for type references.
```C#
// Good
int foo;

string bar;

float baz;

// Bad
Int32 foo;

String bar;

Single baz;
```

### Use BCL types instead of language keywords (i.e. `Int32, String, Single` instead of `int, string, float`, etc) for type references.
```C#
// Good
Int32.Parse();

String.Format();

// Bad
int.Parse();

string.Format();
```

### Use the following lamba style.
```C#
// Good
var foo = list.Select(_ => _.Id);

var foo = customers.Select(c =>
    {
        if (c != null) return null;
        return c.Name;
    });

// Bad
var foo = list.Select(item => item.Id);

var foo = list.Select(item => { return item.Id; });

var foo = list.Select(item => { return item.Id; });
```

### Use `_` in simple lambda expressions when possible and doesn't sacrifice readability.
```C#
// Good
var foo = list.Select(_ => _.Id);

// Bad
var foo = list.Select(item => item.Id);
```

### Use the following ternary style.
```C#
// Good
return foo == null ? null : foo.Name;

return foo == null
    ? null
    : foo.Name;

// Bad
return foo == null
  ? null : foo.Name;

return foo == null ?
  null :
  foo.Name;
```

### Never use more than a single ternary. Ever.

### Use `nameof(...)` instead of `"..."` whenever possible and relevant.

### Use Null Conditional `Foo?.Name` when reasonable.

### Use Expression Bodied Members (ESB) for methods whenever possible.
```C#
// Good
public String Name => Bar.Name;

// Bad
public String Name
{
    get
    {
      return Bar.Name;
    }
}

public String Name
{
    get { return Bar.Name; }
}

public String Name { get { return Bar.Name; } }
```

### Use ESBs for properties whenever possible.
```C#
// Good
public String Name => Bar.Name;

public String Name =>
  This.Is.A.Really.Long.Line.That.Is.Almost.Unreadable.If.It.Was.All.On.The.Same.Line.Bar.Name;

public String Name =>
  This.Is.A.Really.Long.Line.That.Is.Almost.
  Unreadable.If.It.Was.All.On.The.Same.Line.Bar.Name;

// Bad
public String Name
{
    get
    {
      return Bar.Name;
    }
}

public String Name
{
    get { return Bar.Name; }
}

public String Name { get { return Bar.Name; } }

public String Name
   => This.Is.A.Really.Long.Line.That.Is.Almost.Unreadable.If.It.Was.All.On.The.Same.Line.Bar.Name;

public String Name => This.Is.A.Really.Long.Line.That.Is.Almost.
  Unreadable.If.It.Was.All.On.The.Same.Line.Bar.Name;
```

### Use terse properties when using an ESB isn't possible.
```C#
// Good
public String Name
{
    get { return Bar.Name; }
    set { Bar.Name = value; }
}

public String Name
{
    get { return Bar.Name; }
    set
    {
        if (value != null)
        {
            Bar.Name = value;
        }
    }
}

// Bad
public String Name
{
    get
    {
        return Bar.Name;
    }
    set
    {
        Bar.Name = value;
    }
}
```

### Use `readonly` properties when possible (i.e. a property with a private `set` but only used in the constructor).
```C#
// Good
public class Foo
{
    public Foo { get; }

    public Foo()
    {
        Foo = "42";
    }
}

// Bad
public class Foo
{
    public Foo { get; private set; }

    public Foo()
    {
        Foo = "42";
    }
}
```

### Use auto-properties whenever possible.

### Use the following style for properties with backing fields.
```C#
// Good
public List<Foo> Foos
{
    get { return _foos ?? (_foos = new List<Foo>(); }
    set { _foos = value; }
}
private List<Foo> _foos;

// Bad
private List<Foo> _foos;
public List<Foo> Foos
{
    get { return _foos ?? (_foos = new List<Foo>(); }
    set { _foos = value; }
}
```

### Example Class
```C#
using System;
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.ComponentModel;
using System.Diagnostics;
using Microsoft.Win32;

namespace System.Collections.Generic
{
    public partial class ObservableLinkedList<T> : INotifyCollectionChanged, INotifyPropertyChanged
    {
        private ObservableLinkedListNode<T> _head;

        public ObservableLinkedList(IEnumerable<T> items)
        {
            if (items == null) throw new ArgumentNullException(nameof(items));

            foreach (T item in items)
            {
                AddLast(item);
            }
        }

        public event NotifyCollectionChangedEventHandler CollectionChanged;

        public int Count => _count;
        private int _count;

        public ObservableLinkedListNode AddLast(T value)
        {
            var newNode = new LinkedListNode<T>(this, value);

            InsertNodeBefore(_head, node);
        }

        protected virtual void OnCollectionChanged(NotifyCollectionChangedEventArgs e) =>
            CollectionChanged?.Invoke(this, e);
        }

        private void InsertNodeBefore(LinkedListNode<T> node, LinkedListNode<T> newNode)
        {
           ...
           _count++;
        }

        ...
    }
}
```

# General rules

## Follow standard conventions
If you're just starting out on a project or have just defined your conventions, follow them! If they use for example constants in capital letters, enumerators with E as a prefix, it doesn't matter! Always follow design standards.

## KISS
The code should be kept simple and straightforward. Simplicity helps in improving readability, maintainability, and reducing the chances of introducing unnecessary complexity.

## Boy scout rule
"Always leave camp cleaner than you found it!" The same goes for our code. Always return the code better than you got it. If every developer on the team has this vision, and returns a little piece of code better than it was before, we will soon have a big change.

## Always find root cause
Always look for the root cause of the problem, never solve things superficially. On a daily basis, in the rush, we tend to correct problems superficially and not delve into them, which often causes rework!
Always try to look for the root cause and thus solve the problem once and for all!


# Design rules

## Keep configurable data at high levels
Configuration data should be placed at higher levels of abstraction to promote flexibility and reusability.

Bad:
```C#
public class OrderProcessor
{
    private int maxRetries = 3;
    private int retryIntervalSeconds = 10;

    public void ProcessOrder(Order order)
    {
        for (int i = 0; i < maxRetries; i++)
        {
            try
            {
                // Process order
                // ...
                break;
            }
            catch (Exception ex)
            {
                // Handle exception
                // ...
                Thread.Sleep(retryIntervalSeconds * 1000);
            }
        }
    }
}
```

Good:
```C#

public static class OrderSettings {
    public static int MaxRetries { get; set; }
    public static int RetryIntervalSeconds { get; set; }
}

public Startup(IConfiguration configuration)
{    
    Settings.MaxRetries = configuration.GetValue<int>("OrderProcessing:MaxRetries");
    Settings.RetryIntervalSeconds = configuration.GetValue<int>("OrderProcessing:RetryIntervalSeconds");
}

public class OrderProcessor
{
    public void ProcessOrder(Order order)
    {
        int maxRetries = OrderSettings.MaxRetries;
        int retryIntervalSeconds = OrderSettings.RetryIntervalSeconds;

        for (int i = 0; i < maxRetries; i++)
        {
            try
            {
                // Process order
                // ...
                break;
            }
            catch (Exception ex)
            {
                // Handle exception
                // ...
                Thread.Sleep(retryIntervalSeconds * 1000);
            }
        }
    }
}
```

## Prefer polymorphism in place of if/else or switch/case.
 Polymorphism promotes code readability and maintainability by eliminating the need for complex if-else or switch statements. By encapsulating different behaviors within separate classes and using polymorphic references, the code becomes more modular and easier to understand, modify, and extend.

 Benefits:
- Readability and maintainability
- Respect the Open/Closed Principle
- Code reusability
- Ease of testing
- Flexibility and extensibility
- Abstraction and encapsulation

Bad:

```C#
public class Purchase {
    public void ProcessPayment(EPaymentType paymentType) {
        if(paymentType == EPaymentType.Ticket)
        {
            if (IsWeekend(dueDate.Day))
            {
                ...
            }
            else
            {
                ...
            }                
        }

        else if (paymentType == EPaymentType.CreditCard)
        {
            if (CreditCardHasLimit())
            {
                ...
            }
        }        
        else if (paymentType == EPaymentType.BankTransfer)
        {
            if (HasMoney())
            {
                ...
            }
        }            
        else if (paymentType == EPaymentType.Bitcoin)
        {
            if (BitcoinAccountIsValid())
            {
                ...
            }
        }
    }
}
```

Good:

```C#
public abstract class Purchase {
    public virtual void ProcessPayment();
}

public class TicketPurchase : Purchase {

    public override bool ProcessPayment() {        
        
        if (IsWeekend(dueDate.Day))
        {
            ...
        }
        else
        {
            ...
        }
    }
}

public class CreditCardPurchase : Purchase {

    public override bool ProcessPayment() {

        if (CreditCardHasLimit())
        {
            ...
        }
    }
}

public class BankTransferPurchase : Purchase {

    public override bool ProcessPayment() {

        if (HasMoney())
        {
            ...
        }
    }
}

public class BitcoinPurchase : Purchase {

    public override bool ProcessPayment() {

        if (BitcoinAccountIsValid())
        {
            ...
        }
    }
}
```

## Mult-thread
Whenever necessary, use processing in separate threads. Multi-threading and parallelism support has been available in C# for a long time and Async/Await itself helps with that.

Without async/await

```C#
[HttpGet("repondent")]
public IActionResult Index([FromServices] IRespondentRepository respondentRepository, int respondentId)
{
    Respondent respondent = respondentRepository.GetById(respondentId);
    return View(respondent);
}
```

With async/await

```C#
[HttpGet("repondent")]
public async Task<IActionResult> Index([FromServices] IRespondentRepository respondentRepository, int respondentId)
{
    Respondent respondent = await respondentRepository.GetByIdAsync(respondentId);
    return View(respondent);
}
```

## Use Async as suffix
If a method is asynchronous, always use the *async* suffix to identify it.

Bad: 
```C#
public async Task<IEnumerable<MySuperClass>> Get()
```

Good: 
```C#
public async Task<IEnumerable<MySuperClass>> GetAsync()
```

## Consider separating multi-threaded code
It's good practice to keep what's asynchronous separate from what's synchronous, so you don't force a method to be asynchronous or not because of another piece of code.

Bad:

```C#
public async Task<IEnumerable<Creative>> GetAsync()
{
    var creative = new Creative();
    creative.elements = await _context.Elements.ToListAsync(); //async
    creative.Tags = _context.Tags.ToList(); //non-async
    
    return creative;
}
```

Good:

```C#
public async Task<IEnumerable<Creative>> GetAsync()
{
    var creative = new Creative();
    creative.elements = await _context.Elements.ToListAsync(); //async
    creative.Tags = await _context.Tags.ToListAsync(); //async
    
    return creative;
}
```

# Understandability tips

## Be consistent
If you do something one way, do everything else the same way. Be consistent in how you apply the code. Always follow the set pattern. In the case of ION, we always use english language for all projects.

Bad:
```C#
// coding in english
public class CustomerRepository { ... }

// Now we switch to "portuenglish" (portuguese with english)
public class ProdutoRepository { ... }

// And now only in portuguese
public class RepositorioRespondente { ... }
```

Good:
```C#
public class CustomerRepository { ... }
public class ProductRepository { ... }
public class RespondentRepository { ... }
```

## Use explanatory variables
Opt for concise variables even if they result in a longer name. They should be self-explanatory, with no need for additional comments or information.

Bad:
```C#
// Total about what?
decimal total = 0;

// Date about what? Date of birth? Current date? Processed date?
DateTime date = DateTime.Now;

// Duration about what? What metrics are we using here?
int duration = 300;
```

Good:
```C#
// This is so descriptive that we don't need to put any comments
decimal shoppingCartTotal = 0;
DateTime purchaseDate = DateTime.Now;
int purchaseDurationInSeconds = 300;
```

## Avoid the primitive obsession. Prefer dedicated value objects to primitive type.
In everyday life we ​​tend to focus only on primitive types (Built-in), creating distinct properties where they shouldn't be, or rather, causing an obsession with primitive types instead of using value objects. We can create and use value objects to better serve this need.

Bad:
```C#
public class Product
{
    public Product()
    {
        //Validate Currency/Price
    }

    public string Name { get; set; }
    public string Currency { get; set; }
    public decimal Price { get; set; }    
}

public class Plan
{    
    public Plan()
    {
        //Validate Currency/Price
    }

    public string Name { get; set; }
    public string Currency { get; set; }
    public decimal Price { get; set; }
}
```

Good:
```C#
//Value Object
public record Money(decimal Amount, string Currency)
{    
    public bool IsValid()
    {
        //Validate Currency/Price
    }
}

public class Product
{
    public Product (string name, Money price)
    {
        Name = name;
        Price = price;
    }

    public string Name { get; set; }
    public Money Price { get; set; }
}

public class Plan
{
    public Plan (string name, Money price)
    {
        Name = name;
        Price = price;
    }

    public string Name { get; set; }
    public Money Price { get; set; }
}
```
Usage example:

```C#
Product usdProduct = new Product("Phone", new Money(10.50m, "USD"));
Product eurProduct = new Product("Phone", new Money(10.50m, "EUR"));
Product brlProduct = new Product("Phone", new Money(50.75m, "BRL"));

Plan usdPlan = new Plan("Phone", new Money(110.00m, "USD"));
Plan eurPlan = new Plan("Phone", new Money(100.00m, "EUR"));
Plan brlPlan = new Plan("Phone", new Money(1500.00m, "BRL"));
```

## Prefer positive conditions over negative ones
Of course, this is not mandatory, but it is a good approach for readability and code maintenance.

Example:

```C#
// Avoid
if (!IsAdmin) { ... }

// Use
if (IsAdmin) { ... }


//Avoid
if (!IsManager) 
{ 
    ... 
}
else
{
    ...
}

//Use

if (IsManager) 
{ 
    ... 
}
else
{
    ...
}
```

# Names rules

## 1. Avoid string obsession. Replace magic numbers with named constants.
Using the creation of strings for the same purpose where it will be used by several places in your project can bring a lot of headache and end up spending hours and hours wasting time with string comparisons. Avoid typing the same string multiple times, use constants or enums instead.

Bad:
```C#
if (environment == "PROD")
    ...
```
Good:
```C#
const string ENV = "PROD";

if(environment == ENV)
```

## Do not use the variable type as a prefix. Avoid encodings. Don't append prefixes or type information
The statements (string, class, int, ...) already fulfill the role of meeting the needs of the type

Bad:
```C#
public class ClsRespondent { ... }
string strRespondentEmail = "ion@rockcontent.com";
int intDurationInMinute = 30;   
```

Good:
```C#
public class Respondent { ... }
string respondentEmail = "ion@rockcontent.com";
int durationInMinute = 30;   
```

# Rules for functions or methods

## Keep your functions or methods as small as possible
It's easier to have smaller, reusable methods than everything inside a single method.

Bad:
```C#
public void ProcessPurchase() 
{ 
    // Add customer    
    // Apply discount
    // Update stock
    // Process payment
    // Send e-mail to customer
}
```

Good:
```C#
public void ProcessPurchase() 
{ 
    AddCustomer();
    ApplyDiscount();
    UpdateStock();
    ProcessPayment();
    SendCustomerEmail();
}

public void AddCustomer() { ... }
public void ApplyDiscount() { ... }
public void UpdateStock() { ... }
public void ProcessPayment() { ... }
public void SendCustomerEmail() { ... }
```

## Do one thing
The function, method, or class should have a single, clear responsibility and should do it well.

Bad:
```C#
public class User
{
    public void Register(string username, string password)
    {
        // Validate username and password
        // Create user in the database
        // Send registration email
        // Log registration activity
    }
}
```

Good:
```C#
public class UserRegistration
{
    public bool IsValid(string username, string password)
    {
        // Validate username and password
        return true;
    }
}

public class UserRepository
{
    public void Save(User user)
    {
        // Create user in the database
    }
}

public class EmailService
{
    public void SendRegistrationEmail(string emailAddress)
    {
        // Send registration email
    }
}

public class UserRegistrationLogger
{
    public void LogRegistration(string username)
    {
        // Log registration activity
    }
}
```

## Use descriptive names.
It is very important to choose meaningful and descriptive names for variables, functions, classes and other code elements.

Bad:
```C#
public class MyClass
{
    private int x;

    public void a()
    {
        int y = 5;
        // Perform some operations using y
    }

    public void b()
    {
        int z = 10;
        // Perform some operations using z
    }
}
```

Good:
```C#
public class Customer
{
    private int customerId;

    public void CalculateInvoice()
    {
        int totalItems = 5;
        // Perform calculations based on the totalItems
    }

    public void SendNotification()
    {
        int notificationId = 10;
        // Send a notification using the notificationId
    }
}
```

## Opt for fews arguments
Avoid requiring many parameters to construct the object, as well as using and abusing C#'s optional parameters.

Bad:
```C#
public void AddCustomer(string name, DateTime birthDate, string email, string street, string number, string neighborhood, string city, string state, string country, string zipCode) 
{
    ...
}
```

Good:
```C#
public void AddCustomer(Customer customer) 
{
    ...
}

public class Customer
{
    public string Name { get; set; };
    public DateTime BirthDate{ get; set; };
    public Email Email { get; set; };
    public Address Address { get; set; };
}
```

## Avoid side effects.
Functions or methods should not have any unexpected or hidden effects on the system or other parts of the codebase. 

Bad:
```C#
public class Order 
{
    public decimal Total { get; set; }
}

var order = new Order();

// Anyone outside the class Where can update your total
order.Total = 3550;
```

Good:
```C#
public class Order 
{
    public decimal Total { get; private set; }

    public void CalculateTotal() { ... }
}

var order = new Order();

// Now the total variable is private, no one outside can modify it, avoiding side effects
order.Total = 3550; // ERRO
```

## Don't use flag arguments.
Avoid using boolean flag arguments in methods. Instead, it encourages splitting a method into several independent methods that can be called from the client without the need for flags.

Bad:
```C#
public class PaymentProcessor
{
    public void ProcessPayment(string paymentType, decimal amount, bool isExpress)
    {
        // Perform payment processing based on the paymentType and isExpress flag
        if (paymentType == "creditCard")
        {
            if (isExpress)
            {
                // Process express credit card payment
            }
            else
            {
                // Process regular credit card payment
            }
        }
        else if (paymentType == "bankTransfer")
        {
            if (isExpress)
            {
                // Process express bank transfer payment
            }
            else
            {
                // Process regular bank transfer payment
            }
        }
        // ... more payment types and conditions
    }
}
```

Good:
```C#
public class PaymentProcessor
{
    public void ProcessCreditCardPayment(decimal amount)
    {
        // Perform credit card payment processing
    }

    public void ProcessBankTransferPayment(decimal amount)
    {
        // Perform bank transfer payment processing
    }

    // Add more specific payment processing methods for other payment types

    public void ProcessExpressPayment(decimal amount)
    {
        // Perform express payment processing
    }
}
```

# Comments rules

## Always try to explain yourself in code.
The code should be self-explanatory and should strive to minimize the need for comments. While comments can be helpful in certain situations, the goal is to write code that is expressive and understandable on its own. 

Bad:
```C#
public class TemperatureConverter
{
    // Convert temperature from Celsius to Fahrenheit
    public double ConvertToFahrenheit(double celsiusTemperature)
    {
        // Multiply by 9/5 and add 32 to convert to Fahrenheit
        return (celsiusTemperature * (9 / 5)) + 32;
    }
}
```

Good:
```C#
public class TemperatureConverter
{
    public double CelsiusToFahrenheit(double celsiusTemperature)
    {
        double fahrenheitTemperature = (celsiusTemperature * 9 / 5) + 32;
        return fahrenheitTemperature;
    }
}
```

## Don't be redundant.
Avoid redundant or repetitive comments that add no meaningful information. Redundant comments can clutter up code and make it harder to read and maintain.

Bad:
```C#
public class Order
{
    // Order ID
    public int OrderId { get; set; }

    // Customer name
    public string CustomerName { get; set; }

    // Order total
    public decimal Total { get; set; }

    // Get the order ID
    public int GetOrderId()
    {
        return OrderId;
    }
}
```

Good:
```C#
public class Order
{
    public int OrderId { get; set; }

    public string CustomerName { get; set; }

    public decimal Total { get; set; }

    public int GetOrderId()
    {
        return OrderId;
    }
}
```

## Don't add obvious noise.
Avoid comments that state the obvious or provide trivial information that can be easily inferred from the code itself. Comments should add value by providing additional context or explaining complex logic.

Bad:
```C#
public class Order
{
    // This is the order ID
    public int OrderId { get; set; }

    // This is the customer name
    public string CustomerName { get; set; }
}
```

Good:
```C#
public class Order
{
    public int OrderId { get; set; }
    public string CustomerName { get; set; }
}
```

## Don't use closing brace comments.
There is no need to close comments.

Bad: 
```C#
// Comment // <- Unnecessary
public void Main() { ... }
```

## Don't comment out code. Just remove.
Don't leave dirt in your code, instead of leaving something commented, remove it. Today we have code versioners, you can "go back in time" easily.

Bad: 
```C#
public void MyFunction() 
{ 
    // string text = "My simple text";
    // public void Method() {... }
}
```

## Use as explanation of intent.
Use comments to clarify the code's intent or purpose, rather than simply restating what the code is doing. Comments should provide insight into the reason behind the code, helping other developers understand the reasoning or motivation.

Bad: 
```C#
public class DiscountCalculator
{
    // Calculate discount
    public decimal CalculateDiscount(decimal amount, decimal discountPercentage)
    {
        decimal discountAmount = amount * (discountPercentage / 100);
        decimal finalAmount = amount - discountAmount;
        return finalAmount;
    }
}
```

Good: 
```C#
public class DiscountCalculator
{
    // Calculate discounted amount after applying the provided percentage
    // The discount is subtracted from the original amount to obtain the final amount
    public decimal CalculateDiscount(decimal amount, decimal discountPercentage)
    {
        decimal discountAmount = amount * (discountPercentage / 100);
        decimal finalAmount = amount - discountAmount;
        return finalAmount;
    }
}
```

## Use as clarification of code.
Another interesting use for comments is clarification of code.

Good:
```C#
public void CalculateOrder() 
{ 
    // If the order has already been shipped, it can no longer be cancelled.    
    if(SendDate > DateTime.Now)
    {
        AddNotification("The order has already been shipped and cannot be canceled.");
    }
}
```

## Use as warning of consequences.
We can use comments to warn about code snippets that may have more serious consequences. In this case I recommend using a more elaborate XML comment.

Good:
```C#
/// <summary>
/// ATTENTION: This method cancels the order and reverses the payment.
/// </summary>
public void CancelOrder() 
{ 
    // Perform the processing of the canceled order 
}
```

# Source code structure

## Separate concepts vertically
Maintain a healthy and organized folder structure. You don't need to create a folder for each file, but there can be a separation by context or feature.

- MyApp
    - MyApp.Domain
        - MyApp.Domain.Contexts
            - MyApp.Domain.Contexts.PaymentContext
                - MyApp.Domain.Contexts.PaymentContexts.Entities
                - MyApp.Domain.Contexts.PaymentContexts.ValueObjects
                - MyApp.Domain.Contexts.PaymentContexts.Enums
            - MyApp.Domain.Contexts.AccountContext
                - MyApp.Domain.Contexts.AccountContext.Entities
                - MyApp.Domain.Contexts.AccountContext.ValueObjects
                - MyApp.Domain.Contexts.AccountContext.Enums

## Declare variables close to their usage.
Variables should be declared as close as possible to the point where they are first used. This helps improve code readability by reducing the distance between variable declaration and usage, making it easier for developers to understand the context and purpose of the variables.

Bad:
```C#
public class Calculation
{
    public double PerformCalculation(double input)
    {
        double result; // this variable is far from its use.
        double square = CalculateSquare(input);
        double cube = CalculateCube(input);

        if (input > 0)
        {
            result = square + cube;
        }
        else
        {
            result = square - cube;
        }

        return result;
    }
}
```

Good:
```C#
public class Calculation
{
    public double PerformCalculation(double input)
    {
        double square = CalculateSquare(input);
        double cube = CalculateCube(input);

        double result;

        if (input > 0)
        {
            result = square + cube;
        }
        else
        {
            result = square - cube;
        }

        return result;
    }
}
```

## Similar functions should be close
If a function belongs to a group within an object, keep them close at all times.

Bad:
```C#
public class Order
{
    public void CreateCustomer() { ... } 
    public void CheckInventory() { ... }
    public void CreateOrder() { ... }
    public void UpdateCustomer() { ... }
    public void CalculateTotal() { .. }
}
```

Good:
```C#
public class Order
{
    public void CreateCustomer() { ... } 
    public void UpdateCustomer() { ... }

    public void CheckInventory() { ... }
    public void CreateOrder() { ... }
    public void CalculateTotal() { .. }
}
```

## Place functions in the downward direction.
Ordering the functions is also important. In addition to their order of magnitude, your signatures should also be well organized.

Bad:
```C#
public class Customer
{
    public void Create(string name, int age, Address address, bool active) { ... }
    public void Create(string name, int age) { ... } 
    public void Create(string name) { ... } 
    public void Create(string name, int age, Address address) { ... }
}
```

Good:
```C#
public class Customer
{
    public void Create(string name) { ... }
    public void Create(string name, int age) { ... }
    public void Create(string name, int age, Address address) { ... }
    public void Create(string name, int age, Address address, bool active) { ... }
}
```

## Keep lines short.
Avoid functions with long lines or many lines. There is no correct number, but surely the more code in a function, the harder it will be to maintain.

Good:
```C#
public void CreateCustomer(string name) 
{ 
    var customer = new Customer(name);
    _repository.Customers.Add(customer);
    _repository.SaveChanges();
} 
```

## Don't break indentation.
Maintain consistent and proper indentation in code to improve readability and maintainability. Consistent indentation makes code structure and nesting levels easier to understand.

Bad:
```C#
public class ShoppingCart
{
public List<Product> Products { get; set; }

public decimal CalculateTotal()
{
decimal total = 0;
foreach (var product in Products)
{
total += product.Price;
}
return total;
}
}
```

Good:
```C#
public class ShoppingCart
{
    public List<Product> Products { get; set; }

    public decimal CalculateTotal()
    {
        decimal total = 0;
        foreach (var product in Products)
        {
            total += product.Price;
        }
        return total;
    }
}
```

# Objects and data structures

## Hide internal structure.
Objects should hide their internal details and provide a well-defined interface to interact with them. This principle is closely related to encapsulation, which promotes information hiding and abstraction.

Bad: In this example, the *Rectangle* class exposes its internal structure by directly exposing the *Width* and *Height* properties as public. This violates the principle of hiding internal structure.
```C#
public class Rectangle
{
    public double Width { get; set; }
    public double Height { get; set; }
}

public class RectangleCalculator
{
    public double CalculateArea(Rectangle rectangle)
    {
        return rectangle.Width * rectangle.Height;
    }

    public double CalculatePerimeter(Rectangle rectangle)
    {
        return 2 * (rectangle.Width + rectangle.Height);
    }
}
```

Good:In this improved example, the internal structure of the *Rectangle* class is hidden from external code. The *width* and *height* fields are declared as private, and the class provides public methods (*CalculateArea* and *CalculatePerimeter*) to interact with the object and obtain the desired information.
```C#
public class Rectangle
{
    private double _width;
    private double _height;

    public Rectangle(double width, double height)
    {
        _width = width;
        _height = height;
    }

    public double CalculateArea()
    {
        return _width * _height;
    }

    public double CalculatePerimeter()
    {
        return 2 * (_width + _height);
    }
}
```

## Prefer data structures.
Data structures represent the way data is organized and can be a class or a struct. We usually associate structs more with data structures than classes, but we can structure data with any of them.
The difference is that when using class (OOP) we have features such as abstraction, inheritance, polymorphism, among others.

Example using *struct*
```C#
public struct Email 
{
    public Email(string address) 
    { 
        // Allow only email types: hotmail, gmail, yahoo...
    }

    public string Address { get; private set; }
}

public class Customer
{
    public Email Email { get; private set; }
}
```

Example using *class*
```C#
public class Email 
{
    public Email(string address) 
    { 
        // Allow any email types
    }

    public string Address { get; private set; }
}

public class CommonEmail : Email
{
    public Email(string address) 
        : base(address)
    { 
        // Allow only hotmail, gmail, yahoo...
    }        
}

public class Customer
{
    public Email Email { get; private set; }
}
```

## Avoid hybrids structures (half object and half data).
The creation of structures that are a mix of object-oriented design and pure data structures. Hybrid structures often lead to code that is difficult to understand and maintain.

Bad: In this example, the *Employee* class contains both data properties (*Name*, *Salary*) and behavior (*IsValid*, *Save*). This violates the principle of avoiding hybrid structures. The class mixes the responsibilities of representing data with additional behaviors that go beyond the scope of a pure data structure. This can lead to confusion and difficulty in understanding the class's purpose and responsibilities.
```C#
public class Employee
{
    public string Name { get; set; }
    public decimal Salary { get; set; }

    public bool IsValid()
    {
        // Validation logic
        return true;
    }

    public void Save()
    {
        // Database persistence logic
    }
}
```

Good:
```C#
public class Employee
{
    public string Name { get; set; }
    public decimal Salary { get; set; }
}

public class EmployeeValidator
{
    public bool IsValid(EmployeeData employee)
    {
        // Validation logic
        return true;
    }
}

public class EmployeeRepository
{
    public void Save(EmployeeData employee)
    {
        // Database persistence logic
    }
}
```

## Prefer non-static methods to static methods.
It is preferable to favor non-static methods over static methods whenever possible. Static methods are often used for utility functions or when there is no need for an instance of a class. However, they can lead to tight coupling, reduced testability, and difficult extensibility.

Bad: In this example, the *CalculateCircleArea* method is defined as *static* in the *MathUtils* class. It provides a utility function to calculate the area of a circle based on the radius. However, using static methods in this way can lead to code that is tightly coupled and difficult to test and mock.
```C#
public class MathUtils
{
    public static double CalculateCircleArea(double radius)
    {
        return Math.PI * radius * radius;
    }
}

double radius = 5;
double area = MathUtils.CalculateCircleArea(radius);
```

Good: In this improved example, the area calculation is encapsulated within the *Circle* class as a non-static method *CalculateArea()*. By using non-static methods, we ensure that the behavior is associated with an instance of the class, promoting encapsulation and loose coupling. It also allows for better testability and extensibility, as the behavior can be overridden or modified in derived classes.
```C#
public class Circle
{
    public double Radius { get; set; }

    public double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
}

Circle circle = new Circle { Radius = 5 };
double area = circle.CalculateArea();
```

# Tests

## One assert per test.
each test case should focus on asserting a single, specific behavior or outcome. This helps in keeping the tests focused, clear, and easier to understand and diagnose.

Bad:
```C#
[Test]
public void CalculateTotalPrice_WithDiscountAndShipping_CorrectlyCalculatesTotal()
{
    // Arrange
    var order = new Order();
    
    // Act
    order.AddProduct("Product 1", 10.0);
    order.ApplyDiscount(0.1);
    order.AddShipping(5.0);
    double totalPrice = order.CalculateTotalPrice();
    
    // Assert
    Assert.AreEqual(14.0, totalPrice);
    Assert.IsTrue(order.IsDiscountApplied);
    Assert.AreEqual(5.0, order.ShippingCost);
}
```

Good:
```C#
[Test]
public void CalculateTotalPrice_WithDiscount_CorrectlyCalculatesTotal()
{
    // Arrange
    var order = new Order();
    order.AddProduct("Product 1", 10.0);
    order.ApplyDiscount(0.1);
    
    // Act
    double totalPrice = order.CalculateTotalPrice();
    
    // Assert
    Assert.AreEqual(9.0, totalPrice);
}

[Test]
public void CalculateTotalPrice_WithShipping_CorrectlyCalculatesTotal()
{
    // Arrange
    var order = new Order();
    order.AddProduct("Product 1", 10.0);
    order.AddShipping(5.0);
    
    // Act
    double totalPrice = order.CalculateTotalPrice();
    
    // Assert
    Assert.AreEqual(15.0, totalPrice);
}
```

## Readable.
Treat your tests as a fundamental part of your code, not an afterthought. Tests have to be organized and well written just like the rest of your software.

Example:
```C#
//None
```

## Fast.
tests should be fast-running, providing quick feedback on the code's correctness. Slow-running tests can significantly slow down the development process and hinder productivity.

Bad:
```C#
[Test]
public void CalculateTotalPrice_WithLargeOrder_CorrectlyCalculatesTotal()
{
    // Arrange
    var order = new Order();
    
    // Add a large number of products to the order
    for (int i = 1; i <= 1000000; i++)
    {
        order.AddProduct($"Product {i}", 10.0);
    }
    
    // Act
    double totalPrice = order.CalculateTotalPrice();
    
    // Assert
    Assert.AreEqual(10000000.0, totalPrice);
}
```

Good:
```C#
[Test]
public void CalculateTotalPrice_WithSmallOrder_CorrectlyCalculatesTotal()
{
    // Arrange
    var order = new Order();
    order.AddProduct("Product 1", 10.0);
    order.AddProduct("Product 2", 20.0);
    
    // Act
    double totalPrice = order.CalculateTotalPrice();
    
    // Assert
    Assert.AreEqual(30.0, totalPrice);
}
```

## Independent.
tests should be independent of each other, meaning that the outcome of one test should not affect the outcome of another test. Each test should be able to run in isolation, without any dependencies on the state or results of other tests.

Bad: In this example, the *ShoppingCartTests* class contains two test methods: *CalculateTotalPrice_ReturnsCorrectTotal* and *RemoveItem_RemovesItemFromCart*. However, the tests are not independent because the *SetUp* method is shared between them, setting up the initial state of the shopping cart.
This violates the principle of "Independent" because the outcome of the CalculateTotalPrice_ReturnsCorrectTotal test can affect the state of the shopping cart and, in turn, affect the result of the RemoveItem_RemovesItemFromCart test.
```C#
[TestFixture]
public class ShoppingCartTests
{
    private ShoppingCart cart;

    [SetUp]
    public void SetUp()
    {
        // Set up the shopping cart with some initial items
        cart = new ShoppingCart();
        cart.AddItem("Product 1", 10.0);
        cart.AddItem("Product 2", 20.0);
    }

    [Test]
    public void CalculateTotalPrice_ReturnsCorrectTotal()
    {
        // Act
        double totalPrice = cart.CalculateTotalPrice();

        // Assert
        Assert.AreEqual(30.0, totalPrice);
    }

    [Test]
    public void RemoveItem_RemovesItemFromCart()
    {
        // Act
        cart.RemoveItem("Product 1");

        // Assert
        Assert.AreEqual(1, cart.ItemsCount());
    }
}
```

Good: Now, each test method sets up its own instance of the *ShoppingCart* and defines its own test scenario. The tests are independent and can run in isolation without any dependencies on each other.
```C#
[TestFixture]
public class ShoppingCartTests
{
    [Test]
    public void CalculateTotalPrice_ReturnsCorrectTotal()
    {
        // Arrange
        var cart = new ShoppingCart();
        cart.AddItem("Product 1", 10.0);
        cart.AddItem("Product 2", 20.0);

        // Act
        double totalPrice = cart.CalculateTotalPrice();

        // Assert
        Assert.AreEqual(30.0, totalPrice);
    }

    [Test]
    public void RemoveItem_RemovesItemFromCart()
    {
        // Arrange
        var cart = new ShoppingCart();
        cart.AddItem("Product 1", 10.0);
        cart.AddItem("Product 2", 20.0);

        // Act
        cart.RemoveItem("Product 1");

        // Assert
        Assert.AreEqual(1, cart.ItemsCount());
    }
}
```

## Repeatable
We must be able to repeat the same test, but with different parameters.

Example:
```C#
[TestMethod]
[DataRow("mygoodmail@ion.com", "mygoodemail@rockcontent.com")]
public void ShouldValidateEmail(string email)
{
    Assert.IsTrue(new Email(email).IsValid());
}
```



## Rigidity
The software that is difficult to change is a sign of poor design and can lead to a cascade of subsequent changes. When making a small change in rigid code, it often requires modifying multiple interconnected components, making the codebase fragile and prone to errors.

Bad: In the below example, the *ShoppingCart* class handles adding and removing items from a shopping cart and calculates the total price. However, the design is rigid because the total price is updated directly within the *AddItem* and *RemoveItem* methods. This tightly couples the logic of updating the total price with the operations of adding and removing items. Any change in the total price calculation or the way items are added or removed would require modifying multiple methods, making the code difficult to change.
```C#
public class ShoppingCart
{
    private List<Product> items;
    private double totalPrice;

    public void AddItem(Product item)
    {
        items.Add(item);
        UpdateTotalPrice();
    }

    public void RemoveItem(Product item)
    {
        items.Remove(item);
        UpdateTotalPrice();
    }

    private void UpdateTotalPrice()
    {
        totalPrice = 0;
        foreach (var item in items)
        {
            totalPrice += item.Price;
        }
    }

    public double GetTotalPrice()
    {
        return totalPrice;
    }
}
```

Good:
```C#
public class ShoppingCart
{
    private List<Product> items;

    public void AddItem(Product item)
    {
        items.Add(item);
    }

    public void RemoveItem(Product item)
    {
        items.Remove(item);
    }

    public double GetTotalPrice()
    {
        double totalPrice = 0;
        foreach (var item in items)
        {
            totalPrice += item.Price;
        }
        return totalPrice;
    }
}
```

## Fragility
The application is fragile when making a small change in one place causes unexpected failures in other unrelated parts of the code. Fragile code is sensitive to changes and often requires extensive testing and careful coordination when making modifications.

Bad: In the below example, if there is a change in the payment processing implementation or the email service, it can introduce fragility in the codebase. A small change or bug fix in one part can inadvertently break the unrelated part, causing unexpected failures.
```C#
public class OrderProcessor
{
    private PaymentProcessor paymentProcessor;
    private EmailService emailService;

    public OrderProcessor()
    {
        paymentProcessor = new PaymentProcessor();
        emailService = new EmailService();
    }

    public void ProcessOrder(Order order)
    {
        // Process payment
        bool paymentResult = paymentProcessor.ProcessPayment(order);

        // Send email notification
        if (paymentResult)
        {
            emailService.SendEmail(order.CustomerEmail, "Order Confirmation", "Your order has been processed successfully.");
        }
        else
        {
            emailService.SendEmail(order.CustomerEmail, "Payment Failed", "There was an issue processing your payment.");
        }
    }
}
```

Good: In this improved example, the *OrderProcessor* class depends on abstractions (*IPaymentProcessor* and *IEmailService*) rather than concrete implementations.
This design promotes loose coupling and makes the code less fragile. Changes or updates in the payment processing or email service can be made without impacting other parts of the codebase, as long as the new implementations adhere to the interface contracts.
```C#
public class OrderProcessor
{
    private IPaymentProcessor paymentProcessor;
    private IEmailService emailService;

    public OrderProcessor(IPaymentProcessor paymentProcessor, IEmailService emailService)
    {
        this.paymentProcessor = paymentProcessor;
        this.emailService = emailService;
    }

    public void ProcessOrder(Order order)
    {
        // Process payment
        bool paymentResult = paymentProcessor.ProcessPayment(order);

        // Send email notification
        if (paymentResult)
        {
            emailService.SendEmail(order.CustomerEmail, "Order Confirmation", "Your order has been processed successfully.");
        }
        else
        {
            emailService.SendEmail(order.CustomerEmail, "Payment Failed", "There was an issue processing your payment.");
        }
    }
}
```

## Immobility
You can't reuse parts of your code in other projects because it takes a lot of effort. In short, everything is very tightly coupled.

Example:
```C#
//None
```

## Needless Complexity
The application is immobile when it is difficult to reuse or extract parts of the code for use in other contexts. Immobility often arises from tightly coupled and highly interdependent code, making it challenging to separate and reuse components.

Bad:
```C#
public class OrderProcessor
{
    public void ProcessOrder(Order order)
    {
        // Complex logic to validate order
        if (order != null && order.Items != null && order.Items.Count > 0 && order.Customer != null && !string.IsNullOrEmpty(order.Customer.Name) && !string.IsNullOrEmpty(order.Customer.Email))
        {
            // Process the order
        }
        else
        {
            // Handle invalid order
        }
    }
}
```

Good:
```C#
public class OrderProcessor
{
    public void ProcessOrder(Order order)
    {
        if (IsValidOrder(order))
        {
            // Process the order
        }
        else
        {
            // Handle invalid order
        }
    }

    private bool IsValidOrder(Order order)
    {
        if (order == null || order.Items == null 
        || order.Items.Count == 0 || order.Customer == null 
        || string.IsNullOrEmpty(order.Customer.Name) 
        || string.IsNullOrEmpty(order.Customer.Email))
        {
            return false;
        }

        return true;
    }
}
```


## Needless Repetition
Code should avoid unnecessary duplication or repetition. Duplication can lead to maintenance issues, increased code size, and the risk of inconsistency when making changes.

Bad:
```C#
public class OrderProcessor
{
    public void ProcessOrder(Order order)
    {
        // Process order
        // ...

        // Update order status in two places
        order.Status = OrderStatus.Paid;
        order.PaidAt = DateTime.Now;

        // ...
    }
}
```

Good:
```C#
public class OrderProcessor
{
    public void ProcessOrder(Order order)
    {
        // Process order
        // ...

        // Update order status and paid timestamp in one place
        UpdateOrderPayment(order);

        // ...
    }

    private void UpdateOrderPayment(Order order)
    {
        order.Status = OrderStatus.Paid;
        order.PaidAt = DateTime.Now;
    }
}
```



## Opacity
Code should be clear, easy to understand, and should avoid confusing or misleading constructs. Opacity refers to code that is difficult to comprehend, leading to decreased maintainability and increased likelihood of introducing bugs.

Bad:
```C#
public class Calculator
{
    public double Calculate(double a, double b, string operation)
    {
        if (operation == "add")
        {
            return a + b;
        }
        else if (operation == "subtract")
        {
            return a - b;
        }
        else if (operation == "multiply")
        {
            return a * b;
        }
        else if (operation == "divide")
        {
            return a / b;
        }
        else
        {
            throw new InvalidOperationException("Invalid operation specified.");
        }
    }
}
```

Good:
```C#
public class Calculator
{
    public double Add(double a, double b)
    {
        return a + b;
    }

    public double Subtract(double a, double b)
    {
        return a - b;
    }

    public double Multiply(double a, double b)
    {
        return a * b;
    }

    public double Divide(double a, double b)
    {
        return a / b;
    }
}
```