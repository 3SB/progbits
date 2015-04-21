# Progbits
> Small snippets of just enough code to get something to work

## C Sharp

### Extension methods

Extension methods are a way to extend certain `type`s without hampering with the `type`'s code. Suppose we want to add a `isValidEmail` method on the `string` type itself, you'd do it this way: 

```C#
namespace foo
{
    public static class CustomExtensions
    {
        public static bool isValidEmail(this string instance)
        {
            if (instance.Contains('@'))
                return true;
            else
                return false;
        }
    }
}
```

And in another class, import the namespace and you're good to go!!!! 

```C#
using foo;

"foo.bar@gmail.com".isValidEmail() // Returns true
```

We just added a new function to the `string` type without modifying the `string` class itself. 

### Arguments to functions can be named

```C#
private void greet(string name, int times)
{
    for (int i = 0; i < times; i++)
    {
        Console.WriteLine("Hello " + name);
    }
}

greet(
  name: "Shrayas",
  times: 5
) // prints "Hello Shrayas" 5 times.

```

### `DataColumn`s inside `DataTable`s are case insensitive and can contain spaces

```C#
DataTable dt = new DataTable();
dt.Columns.Add(new DataColumn("Hello World", typeof(string)));

DataRow dr = dt.NewRow();
dr["HeLLO WORLd"] = "Hello"; // Note the case
dt.Rows.Add(dr); // Adds the row to table

MessageBox.Show(dt.Columns.Contains("hELLo wORld").ToString()); // Shows "true"
```

### Sorted list, and iterating through them 

```C#
int value = 1;
SortedList<string, int> sl = new SortedList<string, int>();
sl.add("key", value)

foreach(KeyValuePair<string,int> kv in sl) 
{
  Console.WriteLine(kv.Key + "=>" + kv.Value);
}
```

### Get underlying type of a Nullable Object

http://stackoverflow.com/questions/5644587/find-type-of-nullable-properties-via-reflection

```C#
propertyType = Nullable.GetUnderlyingType(propertyType) ?? propertyType
```

### `.toString()` format specifiers

http://msdn.microsoft.com/en-us/library/0c899ak8.aspx

```C#
double val = 1.2;
val.toString("#.##"); // prints 1.2
val.toString("0.00"); // prints 1.20
```

### Attribute classes

http://www.dotnetperls.com/attribute

```C#
[AttributeUsage(AttributeTargets.Class)]
public class route : Attribute { ... }

[route("/")]
public class index { ... }

[route("/customers")]
public class customers { ... }
```

## SQLServer

### Get a whole number if no decimal points are available

```SQL
select cast(COL_NAME as real) from TABLE_NAME
```

## Bootstrap 

### Collapsible Panel

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

## Flask

### Display messages queue'd up with `flash()`

```Jinja
{% with msgs = get_flashed_messages(with_categories=True) %}
  {% if msgs %}
    {% for category, msg in msgs %}
      {% if category == "error" %}
        <div class="alert alert-danger"><strong>ERROR</strong>: {{ msg }}</div>
      {% elif category == "success" %}
        <div class="alert alert-success"><strong>SUCCESS</strong>: {{ msg }} </div>
      {% elif category == "info" %}
        <div class="alert alert-info"><strong>INFO</strong>: {{ msg }} </div>
      {% else %}
        <div class="alert alert-default">{{ msg }} </div>
      {% endif %}
    {% endfor %}
  {% endif %}
{% endwith %}
```

## Mongo

### Update all documents in a collection adding some default values

```javascript
db.<collection>.update({},{$set: {"k1":"v1", "k2":"v2"}}, {multi:true})
```

### Update document, arrays of subdocument 

```javascript
 db.<collection>.find({},{"main_doc":1,"sub_doc":1})
  .forEach(function(doc)
  {    
      var sub_document_array = doc.sub_document_array;
      for(i=0;i<sub_document_array.length;i++){
          sub_document_array[i].title = sub_document_array[i].title.trim();
          var sub_documents_sub_document_array = sub_document_array[i].types;
          if(sub_documents_sub_document_array != null && sub_documents_sub_document_array.length > 0){
             for(j=0;j<sub_documents_sub_document_array.length;j++){
              sub_documents_sub_document_array[j].name = sub_documents_sub_document_array[j].name.trim();   
              }
          }
      }
      db.categories.update(
        {"_id" : doc._id},
        {"$set":{"main_doc": doc.key1.trim(),"sub_doc":sub_document_array}}
      );
  })
```

### How to check if a sub document that has array has any value

```javascript
db.<collection>.find({'subdocument':{$size:1})
```
