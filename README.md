# Progbits
> Small snippets of just enough code to get something to work

## `C#` Sorted list, and iterating through them 

```C#
int value = 1;
SortedList<string, int> sl = new SortedList<string, int>();
sl.add("key", value)

foreach(KeyValuePair<string,int> kv in sl) 
{
  Console.WriteLine(kv.Key + "=>" + kv.Value);
}
```

## `C#` Get underlying type of a Nullable Object

http://stackoverflow.com/questions/5644587/find-type-of-nullable-properties-via-reflection

```C#
propertyType = Nullable.GetUnderlyingType(propertyType) ?? propertyType
```

## `C#` `.toString()` format specifiers

http://msdn.microsoft.com/en-us/library/0c899ak8.aspx

```C#
double val = 1.2;
val.toString("#.##"); // prints 1.2
val.toString("0.00"); // prints 1.20
```

## `C#` Attribute classes

http://www.dotnetperls.com/attribute

```C#
[AttributeUsage(AttributeTargets.Class)]
public class route : Attribute { ... }

[route("/")]
public class index { ... }

[route("/customers")]
public class customers { ... }
```

## `SQLServer` Get a whole number if no decimal points are available

```SQL
select cast(COL_NAME as real) from TABLE_NAME
```

## `Bootstrap` Collapsible Panel

```HTML
<div class="panel panel-default">
    <div class="panel-heading">
      <a href="#filters-body" data-toggle="collapse">Filters</a>
    </div>
    <div id="filters-body" class="panel-collapse collapse in">
      <div class="panel-body">Body</div>
    </div>
</div>
```
