---
title: "Building Custom YAML-DSL in Python"
seoTitle: "Building Custom YAML-DSL in Python"
seoDescription: "In the realm of developement, there are instances when a generic programming language might not fully capture the specificity of a domain."
datePublished: Thu Jan 25 2024 18:30:37 GMT+0000 (Coordinated Universal Time)
cuid: clrtjus5f000108jy4cf09up7
slug: building-custom-yaml-dsl-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706176448627/aa815cbd-197b-492d-9f1e-9454b52ec817.png

---

In the realm of developement, there are instances when a generic programming language might not fully capture the specificity of a domain or the nuances of a particular problem. This is where **Domain-Specific Languages** or **DSLs** come into play. DSLs are specialized languages designed to address specific problem domains, offering a more expressive and tailored solution.

In this blog post, I will guide you through the process of building a custom DSL in Python, a versatile and widely-used programming language. Before delving into the intricacies of building a custom DSL, it's crucial to grasp the fundamental concepts. A DSL, in essence, is a language tailored to a specific problem domain. In our case, it will be created using Python, a language renowned for its readability and simplicity.

## YAML with *DSL* ?

YAML proves to be a suitable choice for both individuals, with technical backgrounds and for their non-technical colleagues, particularly when dealing with simple syntax.

```yaml
tasks:
  - title: Buy groceries
    priority: high
    completed: false
    due_date: 2022-02-28
    tags:
      - shopping
  - title: Finish report
    priority: medium
    completed: true
    due_date: 2022-03-10
    tags:
      - work
    subtasks:
      - title: Research
        completed: true
      - title: Write
        completed: true
      - title: Edit
        completed: false
```

YAML exhibits high readability and appears quite natural, especially when one doesn't focus on distinct brackets and colons. The second version, being more concise, may appear more convenient to certain individuals, as we can see above. You can find the [source code on GitHub](https://github.com/Sonichigo/yaml-dsl/).

Now, what?

We must take this YAML File and convert it into an JSON, to make it usable in Insight's dashboard based on how long does it take user to complete it's task and substacks for example. Let's create `schema.py` where we will define the yaml schema based on above: -

```yaml
task_list_schema = {
    'type': 'object',
    'properties': {
        'tasks': {
            'type': 'array',
            'items': {
                'type': 'object',
                'properties': {
                    'title': {'type': 'string'},
                    'priority': {'type': 'string'},
                    'completed': {'type': 'boolean'},
                    'due_date': {'type': ['string', 'null']},
                    'tags': {'type': 'array', 'items': {'type': 'string'}},
                    'subtasks': {
                        'type': 'array',
                        'items': {
                            'type': 'object',
                            'properties': {
                                'title': {'type': 'string'},
                                'completed': {'type': 'boolean'}
                            },
                            'required': ['title', 'completed']
                        }
                    }
                },
                'required': ['title', 'priority', 'completed']
            }
        }
    },
    'required': ['tasks']
}
```

Here we can see few fields that are in format of datetime, let's first convert them into string before parsing them.

```python
from schema import task_list_schema

# Function to convert date objects to strings recursively
def convert_dates_to_strings(obj):
    if isinstance(obj, dict):
        return {key: convert_dates_to_strings(value) for key, value in obj.items()}
    elif isinstance(obj, list):
        return [convert_dates_to_strings(item) for item in obj]
    elif isinstance(obj, date):
        return obj.strftime('%Y-%m-%d')
    else:
        return obj

def parse_dates(obj):
    if isinstance(obj, dict):
        return {key: parse_dates(value) for key, value in obj.items()}
    elif isinstance(obj, list):
        return [parse_dates(item) for item in obj]
    elif isinstance(obj, (date, datetime)):
        return obj.strftime('%Y-%m-%d')
    elif isinstance(obj, str):
        return obj
    else:
        return obj
```

With both of functions ready to work along with dates mentioned in the `schema.py`, let's move to loading yaml to the program functionality

```python
# existing code
def load_tasks_from_yaml(yaml_string):
    loaded_tasks = yaml.safe_load(yaml_string)
    return parse_dates(loaded_tasks)
```

The function `load_tasks_from_yaml` takes a YAML string as input, loads it using [`yaml.safe`](http://yaml.safe)`_load` to convert it into a Python object (likely a dictionary or a list), and then passes the loaded data through the `parse_dates` function. The purpose of calling `parse_dates` is to handle date objects within the loaded YAML data.

We need tovalidate the `tasks` object against the specified schema (`task_list_schema`), before we can convert the YAML to JSON

```python
def validate_tasks(tasks):
    try:
        validate(instance=tasks, schema=task_list_schema)
        print("Validation successful.")
    except ValidationError as e:
        print(f"Validation error: {e}")
```

The `validate_tasks` function checks if the provided set of tasks conforms to a predefined JSON schema. If the tasks pass validation, it prints a success message; otherwise, it prints an error message indicating the validation error. This type of validation is useful for ensuring that the structure of the tasks adheres to a specified format or set of rules.

Let's have one last step before we convert the YAML to JSON, is to make changes in subtask so in the `modified yaml` or `JSON` file we can verfiy the code worked perfectly fine or not.

```python
# Load tasks from YAML DSL
tasks = load_tasks_from_yaml(yaml_dsl)

# Validate tasks against the schema
validate_tasks(tasks)

tasks['tasks'].append({
    'title': 'Plan vacation',
    'priority': 'medium',
    'completed': False,
    'due_date': date(2022, 4, 15),
    'tags': ['travel'],
    'subtasks': [
        {'title': 'Choose destination', 'completed': False},
        {'title': 'Book flights', 'completed': False},
        {'title': 'Pack bags', 'completed': False}
    ]
})
```

With the changes done, it's time to write our conversion function().

```python
def convert_yaml_to_json(yaml_string):
    loaded_data = yaml.safe_load(yaml_string)
    loaded_data = convert_dates_to_strings(loaded_data)
    json_data = json.dumps(loaded_data, indent=2)
    return json_data
```

The `convert_yaml_to_json` function converts a YAML string to a JSON string, handling date objects during the conversion process. This function can be useful when you need to work with data interchangeably in YAML and JSON formats, ensuring that date representations are consistent between the two formats.

But we need JSON file, ***right?*** Let's add code to create the folder, and save the json each time the file is run. Modify the above function as : -

```python
def convert_yaml_to_json(yaml_string, folder_path):
    loaded_data = yaml.safe_load(yaml_string)
    loaded_data = convert_dates_to_strings(loaded_data)
    json_data = json.dumps(loaded_data, indent=2)
    
    # Create a folder if it doesn't exist
    if not os.path.exists(folder_path):
        os.makedirs(folder_path)

    # Determine the next available file number
    file_number = 1
    while os.path.exists(os.path.join(folder_path, f'tasks_{file_number}.json')):
        file_number += 1

    # Save to a new JSON file with a numbered filename
    json_filename = os.path.join(folder_path, f'tasks_{file_number}.json')
    with io.open(json_filename, 'w', encoding='utf8') as json_file:
        json_file.write(json_data)
return json_filename
```

Now, the `convert_yaml_to_json` function takes a YAML string, converts it to a JSON string, and saves the JSON data to a file with a numbered filename in the specified folder. This allows for the creation of multiple JSON files with incremental filenames, making it suitable for scenarios where you want to store different versions of the JSON data over time.

Let's add couple of line to see how it will work at the end of `main.py`

```python
json_filename = convert_yaml_to_json(yaml_dsl, 'modified_json_tasks')

print("Original YAML DSL:")
print(yaml_dsl)
print(f"\nConverted YAML to JSON saved to {json_filename}")
```

Now, When you run `main.py` file, we can see a folder `modified_json_task` with `task_1.json`, each time the python file is executed, we can see a new file with file\_number increased signifying the number of time the code has been executed.

**And, That's it!!! You have successfully created your first YAML-DSL in python! 🎉**