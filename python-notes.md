# Python Notes

#### Variables and Simple Data Types

* string - series of characters, anything inside of quotes is considered a string
* method - an action that Python can perform on a piece of data
  * every method is followed by a set of (), methods often need more info in those ()
    * title()
      * `print(name.title())`
    * upper()
      * `print(name.upper())`
    * lower()
      * `print(name.lower())`
      * useful for string data, often you will want all lower case

#### Combining or Concatenating Strings
* often useful to combine strings, ex. combining first and last names
  * ```
      first_name = "duke"
      last_name = "nukem"
      full_name = first_name + " " + last_name
      print(full_name)
    ```

#### Adding Whitespace to Strings with Tabs or Newlines
* \n - newline
  * ```
    >>> print("python")
    >>> python
    ```
* \t - tab
  * ```
    >>> print("\tpython"
    >>>     python
    ```
#### Stripping Whitespace
* rstrip() - strips whitespace to the right
* lstrip() - strips whitespace to the left
* strip() - strips whitespace from both sides

#### Avoiding Type Errors with the str() Function
* Often you will want to use a variables value within a message. eg.
* ````
  age = 23
  message = "Happy " + age + "rd Birthday!"
  print(message)
  ````
  * this will return the following error becuase Python doesn't know what to do with that data
  * ```` 
    Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
    TypeError: can only concatenate str (not "int") to str
    ````
  * to solve this you have to convert the int to a str
    * ````
      age = 23
      message = "Happy " + str(age) + "rd Birthday!"
      print(message)
      ````

#### Lists
* list - collection of items in a particular order
* [] indicate a list
* lists are ordered collections, you can access any item in a list by telling python the location, or _index_
  * `print(bicycles[0].title())`
* to access the last elemenet in a list, you would use [-1]
  * `print(bicycles[-1])`

