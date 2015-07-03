# Progbits
> Small snippets of just enough code to get something to work

## C Sharp

### Variable arguments

```C#
void doSomething(params string[] args) 
{
    // use args
}

doSomething("hello");
doSomething("hello", "world");
```

[MSDN](https://msdn.microsoft.com/en-us/library/w5zay9db.aspx)

### Lazy Sequences with `IEnumerable` and `yield`

```C#
private IEnumerable<int> infiniteList()
{
    int i = 0;
    while (true)
    {
        i++;
        yield return i;
    }
}

foreach (int i in infiniteList().Take(10))
    Console.WriteLine(i);
```

Even though `infiniteList()` at first sight would `return` an infinite list, that isn't the case. `yield` yields a value and pauses the execution there

### `DataTable` initialization

```C#
DataTable dt = new DataTable
{
    Columns = { { "id", typeof(int) }, { "name", typeof(string) } },
    Rows = { { 1, "foo" }, { 2, "bar" }, { 3, "baz" } }
};
```

### Anonymous Types

```C#
// Anonymous Objects
var emp = new { Name = "Foo", Id = 1 };

// Anonymous Lists
var nums = new[] { 1, 2, 3 };
var letters = new[] { 'a', 'b', 'c' };

// Anonymous Lists of Anonymous Objects
var emps = new[] { new { Name = "Foo", Id = 1 }, new { Name = "Bar", Id = 2 } };

```

### Timing a statement's execution

```C#
DateTime start = DateTime.Now;
// statements 
Console.WriteLine(DateTime.Now.Subtract(start).TotalSeconds);
```

### Database Connections

#### SQLite

    Data Source=C:\Path\To\Db\File.db
    
#### SQL Server

    server=<server>;user id=<username>;password=<password>;database=<database>
    
### Connecting to a DB

Since everything inherits the `DBConnection` class of C#, connecting to a database it pretty standard

```C#
T conn = new T("<connection string>");
conn.open();
conn.close();
```

Where `T` is the class that can be used to connect to the db of your choice. 


| DB  | Class |
| ------------- | ------------- |
| SQLite  | `SQLiteConnection`  |
| SQL Server  | `SQLConnection`  |

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

### Using `row_number()` 

```SQL
select row_number() over (partition by d.id order by e.name) as id, e.name, d.name 
from employee as e
inner join dept as d on e.id = d.empid
```

[Documentation](https://msdn.microsoft.com/en-us/library/ms186734.aspx)

This would take all employees belonging to a particular department, group them up and assign row numbers to them ordered by their names. 

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

## JQuery

### Extract values from a number of text boxes whose ids follow a pattern and return a dictionary

```javascript
[1,2,3,4,5,6,7,8,9,10,11,12].reduce(function(prev, curr) {
  var classStrength = $("#foo-" + curr).val();
  prev["foo" + curr] = classStrength;
  return prev;
}, {})
```

### Get an object containing all elements within the form

```javascript
$("#some-form").submit(function(e) {
    var formData = $(this).serializeArray().reduce(function(ac, curr) {
        ac[curr.name] = curr.value;
        return ac;
    }, {});
    
    console.log(formData);
});
```

### Toggling a set of checkboxes to follow one checkbox

```javascript
$(document.body).on("click", "#select-all-checkbox", function() {
    var state = this.checked;
    $(".checkbox").each(function(key, val) {
        this.checked = state;
    });
});
```

### Make a bootstrap modal undismissable 

```javascript
$('#myModal').modal({
  backdrop: 'static',
  keyboard: false
})
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

### How to find duplicate entries in a collection based on a key

```javascript
var duplicates = {};
db.catalog.find()
.forEach(function(doc)
{

        var title = doc.title;
        if(duplicates[title] != null){
            var count = duplicates[title];
            count = count + 1;
            duplicates[title] = count;
        }else{
            duplicates[title] = 1;
        }
    
});
```


## Shell

### In a file, print all lines from starting till a match is reached

```shell
$ sed -n '1/pattern/p' <filename>
```

## Racket 

### Comparing `struct`s with the `equals?` predicate

Structures (user defined) in Racket are defined to be "opaque" by default. Opaque structures cannot be compared using the `equals?` precicate. This is because for an opaque structure, `equals?` works on the instance IDs. In order to do such comparisons, the `struct` must be made transparent

```racket
(struct employee (name age))
(define bob (employee "Bob" 25))
(equals? bob (employee "Bob" 25)) ; Returns false. Default struct type is opaque

(struct employee-trans (name age)
        #:transparent)
(define rob (employee "Rob" 24))
(equals? rob (employee "Rob" 24)) ; Returns true
```

## Java

### Check if a 64bit vm is installed

```
$ java -d64 -version
```
