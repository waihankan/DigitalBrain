
**Unittest**


```python
import unittest

from another_file import function

class CustomTestCase(unittest.TestCase):
	def test1(self):
		self.assertEqual(function(x), "Expected Output")
	def test2(self):
		self.assertEqual(function(x), "Expected Output")


# running test in the file

if __name__ == '__main__':
	unittest.main(verbosity=2)

```

To run from the terminal

```bash
python3 -m unittest test_file.py -v
```

**unittest has teardown and setup functions**


