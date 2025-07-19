
**Syntax**

* Read Files
```python
with open("file.txt", mode='r') as file:
	data = file.read()

# file.read() -> reads entire file as a string
# file.readline() -> reads a single line
# file.readlines() -> reads all lines into a list
# for line in file:   -> iterate over lines: efficient for large files
```

* Write Files

```python
with open("file.txt", mode='w') as file:
	file.write("Hello\nWorld")
	
	lines = ["line1\n", "line2\n"]
	file.writelines(lines)
```


### Examples

```python
# count num lines in file
def count_lines(path: str) -> int:
	with open(path, mode='r') as file:
		return sum(1 for _ in f)

# copy contents from one file to another
def copy_file(src: file, dst: str):
	with open(src, mode='r') as fsrc, open(dst, mode='w') as fdst:
		for line in fsrc:
			fdst.write(line)

# read csv into list of dicts
import csv
def read_csv(path: str):
	with open(path, mode='r') as file:
		reader = csv.DictReader(file)
		return list(reader)

```


## Argparse

```python
import argparse

if __name__ == '__main__':
	parser =argparse.ArgumentParser(description="")
	parser.add_argument("input", help="")
	parser.add_argument("-o", "--output", help="" )
	parser.add_argument("-v", "--verbose", action="store_true", help="Show verbose output")

```

![[Pasted image 20250630115606.png]]

