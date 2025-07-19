
## Data Types in Python
### **List**
* Ordered
* Mutable
* Allows Duplicate
* Store pointers to the objects in memory
* heterogeneous (can store string and integer and another list)
* `.append(x)`
* `.extend(iterable)`
* `.insert(i, x)` insert `x` at position `i`. `i = 0` inserts at the beginning
* `.remove(x)` removes the first occurrence
* `.pop(i)` removes and returns the item at the position `i`
* `.pop()` removes and returns the last item
* **Using a list as a stack (efficient)**
* **Using a list as a queue (highly inefficient)**
* 

```python
list_from_string = list("hello")
# ['h', 'e', 'l', 'l', 'o']

stack = []
stack.append(4)
last_item = stack.pop()

deque = collections.deque() # double ended queue O(1) in both directions

```


### Dictionary

* Organizer | Collection of `key-value pairs`
* database records, configurations, JSON from APIS
* **Insertion Ordered**
* NO Duplicate keys (Old value will be overwritten)
* **Key Must be Hashable (Immutable Type): strings, numbers, booleans, tuples, No list, sets, dicts** 
* Hash Table | Hash Map O(1) look up time on average


```python
empty_dict = {}

user = {
	"username": "alex",
	"id": 103,
	"is_active": True
}

# dict() Constructor
user = dict(username="alex", id=103, is_active=True)

# zip() trick
keys = ["fruit", "vegetable", "grain"] 
values = ["apple", "broccoli", "rice"]

food_map = dict(zip(keys, values))
# zip() returns a zip object

#################################

# Accessing
d[key] 
# raise KeyError if key does not exist

d.get(key, default) # safer way to access a potentially missing keys.

# EXAMPLE
config = {"retries": 3}
config["timeout"] # Raise KeyError

config.get("timeout", 30) # sets default (30) 

#################################

.keys() #dict_keys object
.values() # dict_values object
.items() # key-value tuple pair
```


### Set

* Unordered, Unique, Hashable Elements **ONLY** 
* Deduplication: The single most efficient way to remove duplicate elements from a list is to convert it to a set and then back to a list. This is a fundamental pattern in data cleaning.
* **Membership testing in faster in set than in list**. O(1) vs O(n)
* 


```python
empty_set = set()

vowels = {'a', 'e', 'i', 'o', 'u'}

user_ids = [_, _, _]
unique_user_ids = list(set(user_ids)) # order not guaranteed

```

## Data Manipulation Operations


> A list of dictionaries is a very common data structure, often resulting from reading a CSV file or parsing a JSON API response. Filtering this structure is a daily task for data analysts and engineers. Python offers several ways to accomplish this, with list comprehensions being the most idiomatic.

* ***List Comprehension (Preferred Method)** 

```python
employees = {}

engineers = [emp for emp in employees if emp['role'] == "Engineer"]

high_earning_engineers = [emp for emp in employees
	if emp['role'] == "Engineer" and emp['salary'] > 100000
]
```

* **Filtering a Dictionary** 

```python
grades = {'John': 85, 'Mary': 92, 'Matt': 78, 'Michael': 95, 'Laura': 88}

# get students with >= 90 score
top_performers = {name: score for name, score in grades.items() if score >= 90}
# new_dict = {key: value for key, value in dict.items() if value ... sth}

m_students = {name: score for name, score in grades.items() if name.startswith('M')}

```


### Sorting 

* `list.sort()` : in-place sorting; returns `None`
* `sorted(iterables)`: returns a new, sorted iterable

* **Sorting a list of dictionaries**

```python
employees = {sth}

sorted_by_salary_asc = sorted(employees, key=lambda emp: emp['salary'])

sorted_by_salary_desc = sorted(employees, key=lambda emp: emp['salary'], reverse=True)

# Complex Sort, Tie-breaking
sorted_complex = sorted(employees, key=lambda emp: (emp['role'], -emp['salary']))


```


## Aggregating, Grouping | from collections import defaultdict!!!!

> Grouping is a cornerstone of data aggregation and analysis. It is the process of taking a flat list of items and restructuring it into a nested data structure—typically a dictionary of lists—where items are categorized based on a common property or key.   

```python
transactions = [
    {'id': 't1', 'category': 'books', 'amount': 25},
    {'id': 't2', 'category': 'electronics', 'amount': 120},
    {'id': 't3', 'category': 'books', 'amount': 15},
    {'id': 't4', 'category': 'clothing', 'amount': 50},
    {'id': 't5', 'category': 'electronics', 'amount': 85},
]


# input: a list of dictionaries
# returns: a dictionary with key=category and value = list of transactions in the specific category


from collections import defaultdict

grouped_transactions = defaultdict(list)
for transaction in transactions:
	cat = transaction['category']
	grouped_transactions[cat].append(transaction)
	
```

sort(iterable, key, reverse)


## Taming Nested Data: From APIs and JSON to Python Objects

* Deserialization: Converting JSON to Python Object
* Serialization: Converting a Python Object to JSON

`json` Modules

* `json.loads(json_string)`: JSON-formatted string to Python object; deserialization
* `json.load(file_object)`: Reads from a file-like object (e.g., a file opened in read mode) containing JSON data and returns the corresponding Python object.
* `json.dumps(python_object, indent=None)`: Python object to JSON-formatted string; serialization
* `json.dump(python_object, file_object)`: Takes a Python object and writes it to a **file-like object** in JSON format.

```python
import json 

# load from file
with open("./data.json", mode='r') as file:
    read_as_dict = json.load(file)

# write to file
with open("./data.json", mode='w') as file:
	json.dump(data, file)

```

---

## API Response

```python
import json 
import requests

url = "something"
try:
	response = requests.get(url)
	response.raise_for_status()
	data = response.json() # python dict object 
	
except Exception as e:
	print(e)
```


---



## Miscellaneous 

```python

from collections import Counter

sentence = "the quick brown fox jumps over the lazy dog"
words = sentence.split()
word_counts = Counter(words)
most_common = word_counts.most_common(3)


from collections import deque #O(1) from either direction
task_queue = deque()

task_queue.append("Task 1")
task_queue.append("Task 2")

next_task = task_queue.popleft() # 'Task 1'

```




---

1. Think in Patterns: Recognize that most data manipulation tasks are variations of a few fundamental patterns: filtering, sorting, grouping, and transforming. By identifying the pattern, one can apply the appropriate and most Pythonic tool for the job.
2. Choose the Right Tool for the Job: Do not default to a list. Before writing a line of code, consider the access patterns the data requires. Does it need positional access? Fast key-based lookups? Uniqueness and set logic? A conscious choice between a list, dict, and set is the first step toward writing efficient and clean code.
3. Embrace Comprehensions: Make list, dictionary, and set comprehensions the default tool for creating new collections from existing iterables. They are more than just syntactic sugar; they are a core part of the Pythonic idiom, leading to more concise, readable, and often more performant code.
4. Master Nested Navigation: In an API-driven world, data is rarely flat. Practice safe and efficient navigation of nested dictionaries and lists. Make robust patterns like the .get() method and try-except blocks second nature to handle the inevitable inconsistencies of real-world data.





