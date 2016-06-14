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

###Use four spaces of indentation (no tabs).

###Use `camelCase` to name all local variables.

###Use `_camelCase` for internal and private fields and use `readonly` where possible.
```C#
// Good
private Foo _bar;

private readonly Foo _bar;

// Bad
private Foo bar;
```

###Use `PascalCase` for static internal and private fields and use `readonly` whenever possible.
```C#
// Good
private static Foo Bar;

private static readonly Foo Bar;

// Bad
private readonly static Foo Bar;
```

###Use `PascalCase` to name all constant local variables and fields.

###Avoid `this.` unless absolutely necessary.

###Always specify the visibility, even if it's the default. Visibility should be the first modifier.
```C#
// Good
private string _foo;

public abstract class ...

// Bad
abstract public class ...
```
###Always specify namespace imports at the top of the file, *outside* of `namespace` declarations and should be grouped (`System.*`, `Ours.*`, `3rdParty.*`) and sorted alphabetically within each group.
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

###Use the following spacing style.
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

// Bad
if(var) return;

new Foo{Bar="Bar"};
```

###Avoid more than one empty line at any time. For example, do not have two blank lines between members of a type.

###Avoid spurious free spaces. Consider enabling viewable whitespace in your editor to aid detection.
```C#
// Good
if (someVar == 0)

// Bad
if (someVar == 0)... (where the dots mark the spurious free spaces)
```

###Use `var` whenever possible unless it significantly impacts readability.
```C#
// Good
var stream = new FileStream();

var stream = OpenMyStream();

// Bad
Stream stream = new FileStream();
```

###Use language keywords instead of BCL types for type references.
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

###Use BCL types instead of language keywords (i.e. `Int32, String, Single` instead of `int, string, float`, etc) for type references.
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

###Use `_` in simple lambda expressions when possible and doesn't sacrifice readability.
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

###Use `nameof(...)` instead of `"..."` whenever possible and relevant.

###Use Null Conditional `Foo?.Name` when reasonable.

###Use Expression Bodied Members (ESB) for methods whenever possible.
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

###Use ESBs for properties whenever possible.
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

###Use terse properties when using an ESB isn't possible.
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

###Use `readonly` properties when possible (i.e. a property with a private `set` but only used in the constructor).
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

###Use auto-properties whenever possible.

###Use the following style for properties with backing fields.
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

###Example Class
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
