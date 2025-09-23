---
title: "Learn to add comments to JSON"
seoTitle: "Learn How to add comments to JSON"
seoDescription: "Learn practical methods to include comments in JSON files while ensuring they remain valid, using keys, libraries, external docs, or embedded strings."
datePublished: Tue Jun 11 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clxcl4way000308mpess370he
slug: how-to-write-comments-in-json
canonical: https://keploy.io/blog/community/4-ways-to-write-comments-in-json
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1718248631681/907e3213-43f8-4ca0-a859-8b6cd0ef6c9b.png

---

JSON (**JavaScript Object**) Notation is a popular data interchange format used by developers to store and exchange data. It's lightweight, easy to read, and easy to parse, making it ideal for various applications. However, a key limitation is that **JSON does not support comments** natively. This can be a problem when you want to include explanations, notes, or any sort of annotation within your JSON files.

> ⚠️ JSON does not officially support comments (per RFC 8259). The methods below are workarounds you can use depending on your use case.

In this article, we'll explore some practical workarounds to add comments to a JSON file without breaking its structure or causing issues during parsing.

## Understanding JSON and Its Limitations

JSON is designed to be a minimal and straightforward way to represent structured data. Its simplicity is one of its strengths, but it also means there are some limitations. Unlike other data formats such as XML or YAML, JSON does not support comments. This is because JSON is primarily intended to be a data format, not a document format.

The official JSON specification (RFC 8259) makes it clear that comments are not part of the format. However, developers often find comments useful for documentation, debugging, or simply to make the data more understandable.

## **Why JSON Doesn’t Support Comments**

* Short history: Douglas Crockford excluded comments to keep JSON minimal.
    
* Strict parsers (**Python json**, **Go**, **Java**) will not work if you add comments.
    
* Some of the derivative formats (**JSON5**, **HJSON**) support comments, but these are not standard.
    

## Workarounds to Add Comments in JSON

Although JSON doesn't support comments, there are several ways you can include comments in your JSON files without breaking them. Here are some common techniques:

### 1\. Using a "comments" Key

One straightforward method is to include comments as part of the JSON data by using a dedicated key, such as `_comment` or `__note`. This key will hold the comment string.

```bash
{
  "name": "John Doe",
  "age": 30,
  "_comment": "This is a comment about the user object",
  "email": "john.doe@example.com"
}
```

This method is simple and keeps the comment close to the relevant data. However, be mindful that any JSON parser or application processing this file should be aware of and ignore these keys.

## 2\. Using Non-standard JSON Parsers

Some JSON parsers and libraries extend the standard JSON format to support comments. For example, libraries like `JSON5` and `Hjson` allow comments and then convert the extended JSON to standard JSON.

* **JSON5**: Allows comments similar to JavaScript (`//` and `/* */`).
    
    ```bash
    // This is a comment
    {
      "name": "John Doe", /* Inline comment */
      "age": 30,
      "email": "john.doe@example.com"
    }
    ```
    
* **Hjson**: A more human-readable version of JSON that supports comments.
    
    ```bash
    {
      # This is a comment
      name: "John Doe", # Inline comment
      age: 30,
      email: "john.doe@example.com"
    }
    ```
    

Using these libraries can make your JSON files more readable and maintainable, but remember they are not universally supported and require specific parsers.

### 3\. Including Comments in External Documentation

Another approach is to keep the JSON clean and free of comments by maintaining external documentation. This could be in the form of a README file, markdown documents, or even inline comments in the code that generates or processes the JSON.

For example, if you have a JSON configuration file, you could document its structure and each field's purpose in a separate README.md file:

```bash
# Configuration File Documentation
## Fields

- `name`: The name of the user.
- `age`: The age of the user.
- `email`: The user's email address.
```

This method ensures your JSON remains standard-compliant while still providing the necessary documentation.

### 4\. Embedding Comments in Strings

In some cases, developers embed comments directly within string values. This is not a clean solution, but it can work for small notes.

```bash
{
  "name": "John Doe",
  "age": 30,
  "email": "john.doe@example.com",
  "metadata": "This is a comment about the user object"
}
```

Be cautious with this approach, as it can make your data harder to use programmatically and may confuse other developers.

## Comparison of Methods

| Method | Compatibility | Pros | Cons |
| --- | --- | --- | --- |
| `_comment` key | Universal | Simple, standard JSON | Pollutes data |
| Separate docs/schema | Universal | Clean JSON, scalable | Two files to maintain |
| JSON5 / CommentJSON | Limited | Real comments, readable | Not spec-compliant |
| Preprocess (strip) | Limited | Flexible, dev-friendly | Adds complexity |

### Common Mistakes and Recommendations

1. **Avoid adding comments directly by // or /\* \*/ as they** fail on strict parsers.
    
2. **Keep comments concise and relevant** → extended explanations shouldn't be in the codebase and can belong in documentation.
    
3. **If the project is API-facing** → consider avoiding comments altogether and just keep pure JSON as the format.
    
4. **If you have config files in dev tools** → prefer based on JSON5 or a preprocessing step.
    
5. **Security note:** Preprocessing libraries should be trusted to prevent injection attacks.
    

## Conclusion

While JSON's lack of native comment support can be a drawback, there are several strategies you can employ to include comments in your JSON files. Each method has its pros and cons, and the best choice depends on your specific use case and the tools you're using. Whether you choose to use a dedicated key, non-standard parsers, external documentation, or embedded strings, it's important to ensure that your comments do not interfere with the functionality of your JSON data.

By following these tips, you can keep your JSON files informative and maintainable, making them easier for yourself and others to understand and work with.

## FAQs

### **1\. Why doesn't JSON support comments?**

JSON was designed to be a lightweight data interchange format with simplicity in mind. Comments are not included in the JSON specification (RFC 8259) because the focus is purely on data representation rather than documentation or annotation. This decision ensures that JSON remains simple and efficient for parsing and processing.

### **2\. Can using a "comments" key cause issues with JSON parsing?**

Using a "comments" key like `_comment` does not break JSON parsing, as it is just another key-value pair. However, it's important to ensure that any application or parser processing your JSON file is designed to ignore or handle these comment keys appropriately. Otherwise, they might be treated as regular data, potentially causing confusion or errors.

### 3\. Can I use `//` or `/* */` comments in JSON?

No. Standard JSON parsers will raise an error to this syntax. Only extended formats like JSON5 or HJSON will allow this syntax.

### 4.Are comments in JSON a good practice?

In general, no, particularly in the case of APIs. For configurations, you can include limited comments to help developers, but if you are looking to include heavy comments, then documentation as an external document or using JSON alternatives may be the best format.