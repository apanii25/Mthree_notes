# Object-Oriented Programming (OOP) in Python

## 1️⃣ OOP Concepts in Python
Object-Oriented Programming (OOP) is a programming paradigm that uses objects and classes to structure code. The key principles of OOP are:

- **Encapsulation** → Binding data and methods together.
- **Abstraction** → Hiding implementation details.
- **Inheritance** → Reusing code from parent classes.
- **Polymorphism** → Using the same method name with different implementations.

---

## 2️⃣ Method Overloading and Overriding

### **Method Overloading (Polymorphism)**
Python does **not** support method overloading like Java/C++, but we can achieve it using **default parameters**.

```python
class Dog:
    def speak(self, age=0):
        return f"Dog is barking and is {age} years old"

d = Dog()
print(d.speak())      # Output: Dog is barking and is 0 years old
print(d.speak(5))     # Output: Dog is barking and is 5 years old
```

### **Method Overriding (Inheritance)**
Method overriding allows a child class to **provide a specific implementation** of a method already defined in the parent class.

```python
class Animal:
    def speak(self):
        return "Animal is making a sound"

class Dog(Animal):
    def speak(self):  # Overriding
        return "Dog is barking"

d = Dog()
print(d.speak())  # Output: Dog is barking
```

---

## 3️⃣ Constructor & Destructor

### **Constructor (`__init__`)**
A constructor is called automatically when an object is created.

```python
class Animal:
    def __init__(self, name):
        self.name = name
        print(f"{self.name} is created")

obj = Animal("Dog")  # Output: Dog is created
```

### **Destructor (`__del__`)**
A destructor is called when an object is deleted.

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def __del__(self):
        print(f"{self.name} is deleted")

obj = Animal("Dog")
del obj  # Output: Dog is deleted
```

---

## 4️⃣ Shallow Copy & Deep Copy

### **Shallow Copy**
Copies references to the same objects, so changes in one affect the other.

```python
import copy

class Address:
    def __init__(self, city):
        self.city = city

class Person:
    def __init__(self, name, address):
        self.name = name
        self.address = address

address = Address("New York")
p1 = Person("Alice", address)
p2 = copy.copy(p1)

p1.address.city = "Boston"  # Changes reflect in p2
print(p2.address.city)  # Output: Boston
```

### **Deep Copy**
Creates a new copy, so changes in one object don’t affect the other.

```python
dp2 = copy.deepcopy(p1)
p1.address.city = "Chicago"
print(dp2.address.city)  # Output: Boston (original value is preserved)
```

---

## 5️⃣ Class Method, Static Method, and Class Variable

### **Class Variables**
Variables shared by all instances of a class.

```python
class Employee:
    company = "TechCorp"  # Class variable

def display(cls):
        return f"Company Name: {cls.company}"
```

### **Class Method** (`@classmethod`)
A method that operates on class variables rather than instance variables.

```python
class Employee:
    company = "TechCorp"
    
    @classmethod
    def change_company(cls, new_name):
        cls.company = new_name

Employee.change_company("NewTech")
print(Employee.company)  # Output: NewTech
```

### **Static Method** (`@staticmethod`)
Does not depend on class or instance variables.

```python
class Math:
    @staticmethod
    def add(a, b):
        return a + b

print(Math.add(5, 10))  # Output: 15
```

---

## 6️⃣ Exception Handling (`try-except-finally`)

### **Basic Try-Except**
```python
try:
    x = 1 / 0
except ZeroDivisionError as e:
    print("Cannot divide by zero!", e)
finally:
    print("Execution completed!")
```

### **Custom Exception**
```python
class CustomException(Exception):
    def __init__(self, message):
        self.message = message
        super().__init__(self.message)

try:
    raise CustomException("This is a custom exception")
except CustomException as e:
    print(e)
finally:
    print("Finally block executed")
```

---

## 📌 Summary
| Concept               | Description |
|-----------------------|-------------|
| **Encapsulation**     | Hides data to prevent direct access |
| **Abstraction**       | Hides complex implementation details |
| **Inheritance**       | Allows one class to inherit from another |
| **Polymorphism**      | Methods with the same name but different behaviors |
| **Constructor**       | Initializes an object (`__init__`) |
| **Destructor**        | Cleans up when an object is deleted (`__del__`) |
| **Shallow Copy**      | Copies object references (changes affect both) |
| **Deep Copy**         | Creates a completely independent copy |
| **Class Method**      | Operates on the class level (`@classmethod`) |
| **Static Method**     | Independent method (`@staticmethod`) |
| **Exception Handling** | Handles runtime errors using `try-except-finally` |

