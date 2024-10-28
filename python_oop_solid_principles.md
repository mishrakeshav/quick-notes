# Python, OOP, SOLID, Design Patterns 

### OOP 

1. Class and Objects 
2. Encapsulation
  - hides the internal state of an object and allows modification only through methods.
  - Use private variables with a single underscore _ (partial protected) or double underscores __ (private) to restrict access.
3. Abstraction ( from abc import ABC, abstractmethod)
4. Inhertance (multiple,multilevel,hybrid)
5. Polymorphism (overriding,overloading,operator overloading)



### SOLID PRINCIPLES 

1. Single Responsibility Principle (SRP)
- A class should have only one reason to change, meaning it should perform a single task or responsibility.

2. Open/Closed Principle (OCP)
- Software entities (classes, modules, functions) should be open for extension, but closed for modification.

3. Liskov Substitution Principle (LSP)
- A subclass should be substitutable for its parent class without altering the correctness of the program.

4. Interface Segregation Principle (ISP)
- A class should not be forced to implement interfaces it doesn't use
- It’s better to have many small, specific interfaces rather than one large one.

5. Dependency Inversion Principle (DIP)
- High-level modules should not depend on low-level modules; both should depend on abstractions.
- This reduces tight coupling between modules.

Example : (DIP Voilation)
```py
class MySQLDatabase:
    def connect(self):
        print("Connecting to MySQL")

class Application:
    def __init__(self):
        self.db = MySQLDatabase()  # Tightly coupled to MySQL

    def run(self):
        self.db.connect()
```
Example : DIP Compliant Version
```py
class Database:
    def connect(self):
        pass

class MySQLDatabase(Database):
    def connect(self):
        print("Connecting to MySQL")

class PostgreSQLDatabase(Database):
    def connect(self):
        print("Connecting to PostgreSQL")

class Application:
    def __init__(self, db: Database):
        self.db = db  # Depends on abstraction

    def run(self):
        self.db.connect()

# Usage
app = Application(MySQLDatabase())
app.run()  # Output: Connecting to MySQL
```
