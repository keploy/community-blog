---
title: "What is a Python Bytestring?"
seoTitle: "Mastering Python Bytestrings: A Beginner's Guide"
seoDescription: "Python bytestrings: creation, encoding, decoding, NumPy and Pandas applications, practical examples, string comparisons"
datePublished: Wed Jul 23 2025 17:36:39 GMT+0000 (Coordinated Universal Time)
cuid: cmdg8wnb5000g02la9pyg6t79
slug: what-is-a-python-bytestring
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752045596963/806670cf-2ac5-4d4a-9314-dba62730ecbc.png
tags: python, strings, python-strings, python-bytestrings

---

In the programming language, Python, binary data and text are often used when working with files, APIs, data encoding and decoding, and networking. In this article, we are going to answer some of the common questions that arise in this process. Some of which include what bytestrings are, how to convert strings to bytes and vice versa, their differences, and many more.

This guide will cover all that is required for you to master bytestrings in Python. We do this by ensuring there are examples in almost every section.

With that, let’s dive right into the details.

## What is a Bytestring?

In Python, a bytestring is a sequence of bytes. It is represented by the `bytes` data type in Python3. As said in the introduction, we use them to handle binary data, for example, images, text, audio, etc. Below is a sample representation of a bytestring in Python.

```python
b_string = b"Hello, Keploy"
print(type(b_string)
```

## How to Create a Bytestring

There are several ways of creating bytestrings in Python. In this section, let’s have a look at some of the common ways below..

### Using a byte literal

Here, we create a bytestring by using the `b` prefix followed by the string literal as shown below.

```python
b_string = b"This is a python bytestring"
print(type(b_string))
```

Output:

```plaintext
b'This is a python bytestring’
```

### Using the bytes() constructor

Here, we create a bytestring using Python’s built-in constructor `bytes()`. This constructor takes in a string and an encoding method to use e.g. `utf` or `ascii`. An example code is shown below.

```python
b = bytes("Hello", "utf-8")
```

## Converting Strings to Bytestrings (Encoding)

String data types are representations of human-readable characters. They are  Unicode by default. For them to be stored, written, or sent, they need to be converted to raw bytes, a process termed encoding. In this section, we explore some of the common ways of converting strings to bytestrings.

![Encoding ](https://cdn.hashnode.com/res/hashnode/image/upload/v1751557603796/c837693c-2cb0-47e4-a70b-c1dd4cddd06b.png align="center")

### Using `.encode()` method

This is the most commonly used way of converting strings to bytestrings. We follow the syntax `string.encode(encoding)`. The snippet below shows a sample of how to use this encoding technique.

```python
s = "Hello"
a = s.encode("utf-8")
print(a)
print(type(a))
```

From the above snippet, a string,  `“Hello”`, is encoded and saved in the variable `a`. The output is `` b’Hello` ``, and when it's checked for data type, we get output `<class 'bytes’>`, meaning it's a byte.

### Using `bytes()` constructor

Here, we use the `bytes()` constructor to encode the string into a `bytes` object using the built-in `bytes()` **constructor**. It is shown in the snippet below.

```python
b = bytes("Hello", "utf-8")
```

In the above code, we are telling Python to convert the string `“Hello”` to a bytestring using `utf-8`.

### Using `codecs.encode()`

Another way is to use the `codecs` module found in Python. The `codecs` module is imported and then its `encode()` function is used to convert the string to a bytestring as shown in the snippet below.

```python
import codecs
b = codecs.encode("Hello", "utf-8")
```

This method is preferred because of its flexibility. It supports even custom or non-standard encodings, not just standard encodings like `utf-8`, `utf-16`, `ascii`, and so on. A sample of its usage with a custom encoding is shown below.

```python
import codecs
codecs.encode("hello", "rot_13")  # 'uryyb' as output
```

## Converting Bytestrings to Strings (Decoding)

Now that we are familiar with encoding, its reverse is termed decoding. The process of converting a bytestring to a string. This is done to get a readable string from a given byte. There are several ways of doing this in Python, and the common ones are explained below.

![Decoding](https://cdn.hashnode.com/res/hashnode/image/upload/v1751557291801/be08d71f-a15b-47ee-a824-0f274892fcfa.webp align="center")

### Using `.decode()` method

This is the most common technique among developers. The syntax followed is `byte.decode(encoding)`. The snippet below illustrates decoding using the `.decode()` method.

```python
b = b"Hello"
s = b.decode("utf-8")
```

From above, `b` with the byte `b’Hello`' is decoded while keeping in mind what encoding standard it is. Thus,  Python takes `0x48 0x65 0x6C 0x6C 0x6F` (the word `Hello` in `UTF‑8/ASCII`) and converts it into a string `“Hello”`.

Note that incase the bytes aren’t valid UTF-8, we get an error. This is shown below.

```python
b_bad = b"\xff"
s = b_bad.decode("utf-8")
```

Our output will be a `UnicodeDecodeError`.

### Using `codecs.decode()`

Just like in encoding, we can also use the `codecs` module for decoding. The snippet below illustrates this.

```python
import codecs
s = codecs.decode(b"Hello", "utf-8")
```

The `decode()` function tells Python to take the bytestring and convert them into a readable string using `utf-8`.

## Working with Bytestring Methods

Even if Python bytestrings are not Python strings, Python provides them with almost the same methods. All you need to keep in mind is not to forget the byte literals ( the `b” ”` suffix). These methods help in searching, transforming, and manipulating binary data as if they are text. They also enable one to deal with bytes as if they were of string data type. Let’s have a detailed look at each of the bytestring methods and their application.

![Word cloud for bytestring methods](https://cdn.hashnode.com/res/hashnode/image/upload/v1751558783753/b7ff514e-f738-4a9c-bc04-21ef0bac4e2a.png align="center")

Given a bytestring variable `b`,

### `b.upper()`

This method is used to convert the bytestring's lowercase letters to uppercase.  Example is shown below.

```python
b = b"hello"
print(b.upper())  # b'HELLO', will be the output
```

### `b.lower()`

This method is used to convert a bytestring’s uppercase letters to lowercase. This is illustrated in the code below.

```python
b = b"HELLO"
print(b.lower())  # b'hello', will be the output.
```

### `b.replace(old, new)`

This method is used to replace all occurrences of one byte sequence with another. This is shown below.

```python
b = b"byte string"
print(b.replace(b"string", b"data"))  # b'byte data'
```

In the above code, the byte sequence string in `b` is replaced with `data`.

### `b.find(sub)`

This method is used to return the index of the first occurrence of `sub`. If it is not found, it returns `-1`. Let’s look at this in the code below.

```python
b = b"python"
print(b.find(b"t"))  # 2, is the output here as t is at index 2
```

### `b.startswith(prefix)`

This method helps in checking if the bytestring starts with the given `prefix`. It usually returns `True` or `False`.

```python
b = b"hello world"
print(b.startswith(b"hello"))  # True
```

### `b.endswith(suffix)`

This method is used to check if the bytestring ends with the given `suffix`. It either returns `True` or `False`.

```python
b = b"report.pdf"
print(b.endswith(b".pdf"))  # True
```

### `b.split(sep)`

The `split(sep)` method is used to split the bytestring at each occurrence of `sep`. This method returns a list of bytes.

```python
b = b"one,two,three"
print(b.split(b","))  # [b'one', b'two', b'three']
```

### `b.strip()`

It is used to remove spaces at the end or start of a byte. It can also be used to remove a specified byte.

```python
b = b"  hello  "
print(b.strip())  # b'hello'
```

## Bytes vs Strings in Python

The table below shows a comparison between bytes and strings in Python.

| **Feature** | **Bytes** | **Strings** |
| --- | --- | --- |
| Type | `bytes` (binary) | `str` (Unicode text) |
| Mutable | They are immutable, meaning they can’t be changed once created | They are also immutable. To make any change, you need to create a new one |
| Uses | Used in files, APIs, and networks | Used in UI, logs, and display |
| Conversion | Uses `.decode()` to convert to `str` | Uses `.encode()` to convert to `bytes` |

## Concatenating Strings and Bytestrings

Concatenating refers to joining two things. When you try to concatenate a string to a bytestring, you will get an error. Let us try it out below.

```python
"hello" + b"world"
```

This returns the error below:

`TypeError: can only concatenate str (not "bytes") to str`

This means we can only concatenate a `str` to a `str`. This failure is because Python is strict with mixing binary data with text or `str` data.

![Do not concatenate a string with a bytestring ](https://cdn.hashnode.com/res/hashnode/image/upload/v1751559281326/5f1e48f4-9dc7-4883-864f-442a2b875637.png align="center")

To solve this problem, we apply encoding or decoding to one of them as shown below

```python
"hello" + b"world".decode("utf-8")       # Returns: 'helloworld'
b"hello" + "world".encode("utf-8")       # Returns: b'helloworld'
```

## Raw Strings and Format Strings

### What is a raw string (`r"..."`)?

A raw string is a string literal that considers backslashes (`\`) as literal characters, other than considering them as escape sequences. The syntax of raw strings is `r"text with \ backslashes"`

While Python doesn’t have a native switch-case like other languages, you can learn how to implement [switch case in Python](https://keploy.io/blog/community/python-switch-case-how-to-implement/) using dictionary mappings or functions.

As seen in normal strings, escape sequences like `\n` is interpreted as newline, `\t` is interpreted as tab, and `\\` as backslash (`\`). When we use raw strings, we skip the escaping bit and consider a `\` as a literal character.

The advantage of raw strings is seen in file paths, where initially, with a normal string, it has to be represented in this format.

```python
normal = "C:\\Users\\Keploy"     # Escaped string
print(normal) # output here is C:\Users\Keploy
```

But now with a raw string, you do not need double backslashes, as seen below.

```python
raw = r"C:\Users\Keploy"         # Raw string
print(raw) # The output here is also C:\Users\Keploy
```

A point to note is that a raw string interprets the final quote, e.g, `r"hello\"` will return an error. So you should never end a raw string with an unescaped backslash.

### What is a format string (`f””`)?

A format string, also referred to as a formatted string literal or f-string, is used to embed or add Python code into a string. This is done using curly bracket placeholders `{ }` as shown below.

```python
name = "Angelo"
f_str = f"Hello {name}"  # Hello Angelo is the output
```

It is preferred due to it being short, easy to read, and can be used to dynamically build text. Let’s continue with the above code to illustrate more power of the f-string.

```python
age = 25
print(f"My name is {name}, and I am {age} years old.")
# Output: My name is Angelo, and I am 25 years old.
```

You notice how powerful that is, right?

F-strings can also be converted to bytestrings, a process also termed encoding. This is shown in the code below.

```python
name = "Angelo"
f_str = f"Hello {name}"
b_str = f_str.encode("utf-8")  # Convert to bytes
print(b_str)  # b'Hello Angelo', will be our output
```

## Strings and Bytes in NumPy and Pandas

NumPy and Pandas are popular Python libraries used in data analysis, data science, and machine learning. These fields work with real-world datasets, and often there is a need to handle data in string and byte form. This is made possible with the above Python libraries. NumPy and Pandas handle strings and bytes in different ways.

For advanced data analysis, you might also explore how to create a [Pandas pivot table](https://keploy.io/blog/community/how-to-create-a-pandas-pivot-table-in-python/) in Python.

Let’s go on and discuss how they handle strings and bytes.

### Working in NumPy

This powerful Python library supports both arrays from strings and bytestrings. In case a developer creates an array in NumPy using byte literals ( `b” “`), they are stored by NumPy as a special data type.

```python
import numpy as np
arr = np.array([b"one", b"two"])
print(arr.dtype)  # Output: ('|S3')
```

From the code above, we created an array using NumPy and then printed its data type. We noticce this special data type is `|S3`, known as fixed-width byte strings

Bytestrings are used in numpy because they are memory efficient compared to Unicode strings. Also given the fact that they are best for low-level operations,  they are the best choice for machine learning research where numpy is also commonly used.

For strings, the code below shows how to use them in numpy.

```python
arr = np.array(["one", "two"], dtype='<U3')  # Unicode string array`
```

### Converting Strings to Bytes in Pandas

Text data type comes as a regular `str` (Unicode) in Pandas. So, in case one needs to manipulate (send, store) text in binary form, encoding it to bytes is required.

Let’s demonstrate this in the snippet below.

```python
import pandas as pd
df = pd.DataFrame({"col": ["apple", "banana"]})
df["col_bytes"] = df["col"].str.encode("utf-8")
```

The `.str.encode("utf-8")` method applies the `.encode()` method to each row in the column, and a new column `col_bytes` is created, which now contains `bytes` objects.

```plaintext
      col   col_bytes

0    apple   b'apple'

1   banana   b'banana'
```

Each string is now a UTF-8 encoded byte string, ready for binary storage or transmission.

### Decoding Bytes Back to Strings

A developer may like to convert a bytestring back to a Unicode string; hence will utilize the `.str.decode()` method as shown below.

```python
df["col_str"] = df["col_bytes"].str.decode("utf-8")
```

This is mostly useful in case of reading binary-encoded data from files or APIs.

## Validating Software components that handle Binary data

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752913951371/01a0aef8-f2c7-4358-9c31-781df7c70d9f.png align="center")

So far, we have discussed a lot about Python bytestrings. We talked about some of their applications. One major problem developers face when dealing with software that processes binary data, performs encoding and decoding, is bugs that hide in these components.

Therefore, developers embrace software testing to solve this problem before pushing to production. But manual testing is a tedious process. Thus, to streamline the process of software testing, using the [Keploy Python SDK](https://github.com/keploy/python-sdk) is the most ample and fastest solution.

The Keploy Python SDK automates end-to-end software testing, provides built-in mocking for API testing, and automatically generates detailed reports for the testing process.

## Conclusion

Now that we have come to the end of our comprehensive and hands-on guide on Python bytestrings, let’s have a recap of what we covered. We learnt what bytestrings are and how to create them, encode and decode them, compared them with the normal string, `str`, data type, and explored how they are applied in numpy and pandas. We also had a look at the special raw and format strings.

## Related Keploy blogs

For more understanding of Python, explore some of our related blog articles below.

* [Python Switch Case: How To Implement Switch Statements In Python](https://keploy.io/blog/community/python-switch-case-how-to-implement). This blog explores practical ways to implement switch-case in Python with real examples and how to make sure they work using automated tests.
    
* [Finding Elements In A List Using Python](https://keploy.io/blog/community/guide-finding-elements-in-a-list-using-python). This blog provides a detailed, practical guide on how to find elements in a `list`
    
* [Python Unit Testing: A Complete Guide](https://keploy.io/blog/community/python-unit-testing-a-complete-guide). In this blog, you will explore details on what unit testing is and how to implement it in Python, as well as use keploy to streamline the testing process.
    
* [What does enumerate mean in Python?](https://keploy.io/blog/community/what-does-enumerate-mean-in-python) In this blog, you will get to know how Python’s `enumerate()` function is very helpful and how it is used to let you look through a `list` and gives you both the index and the item at the same time.
    

## FAQs

### **1\. What is the difference between a string and a bytestring in Python?**

A string is human-readable text (`str`), while a bytestring (`bytes`) is raw binary data.

### 2\. **When should I use bytes instead of strings?**

You should use `bytes` while working with binary data, file I/O, or data transmission

### 3\. **How can I convert a string to a bytestring and vice versa?**

To do these conversions, make use of `.encode()` to get bytes and `.decode()` to get back a string.

### 4\. **Why do I get a TypeError when adding a string and a bytestring?**

This is because Python doesn't allow mixing `str` and `bytes` , so you must convert one to the other to do away with this error.