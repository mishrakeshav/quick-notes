# Overview of Creational Design Patterns

Creational design patterns are concerned with the way of creating objects in software design. T

## Key Benefits of Creational Design Patterns

1. **Encapsulation of Object Creation**: They encapsulate the logic of object creation, allowing for the reuse of code and reducing complexity.
2. **Flexibility and Extensibility**: They enable the code to be more flexible and easier to extend, as new classes can be introduced without modifying existing code.
3. **Improved Code Maintainability**: By centralizing object creation, it becomes easier to manage changes and maintenance in the codebase.



## Common Creational Design Patterns
1. **Singleton Pattern**
   - Ensures a class has only one instance and provides a global point of access to that instance.

2. **Factory Method Pattern**
   - Defines an interface for creating an object but lets subclasses alter the type of objects that will be created.

3. **Abstract Factory Pattern**
   - Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

4. **Builder Pattern**
   - Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

5. **Prototype Pattern**
   - Allows for the creation of new objects by copying an existing object, known as the prototype.


### 1. Singleton Pattern
- The Singleton pattern ensures that a class has only one instance and provides a global point of access to that instance.

####  Example:
```python
class Logger:
    __instance = None 

    def __new__(cls, config : dict = None):
        if cls.__instance is None:
            cls.__instance = super(Logger, cls).__new__(cls)
        return cls.__instance

    def __init__(self, config) -> None:
        if not hasattr(self, 'logger_config'):
            self.logger_config = config
        print(self.logger_config)
```

### 2.  Factory Method Pattern 
- The Factory Method Pattern is like a system where instead of directly creating objects yourself, you ask a "factory" to make them for you. This way, if the way objects are made changes, you don’t need to worry about it — you just ask the factory to handle it.

#### Example : 
```python
class Localizer(Protocol):
    def localize(self, msg: str) -> str:
        pass


class GreekLocalizer:
    """A simple localizer a la gettext"""

    def __init__(self) -> None:
        self.translations = {"dog": "σκύλος", "cat": "γάτα"}

    def localize(self, msg: str) -> str:
        """We'll punt if we don't have a translation"""
        return self.translations.get(msg, msg)


class EnglishLocalizer:
    """Simply echoes the message"""

    def localize(self, msg: str) -> str:
        return msg


def get_localizer(language: str = "English") -> Localizer:
    """Factory"""
    localizers: Dict[str, Type[Localizer]] = {
        "English": EnglishLocalizer,
        "Greek": GreekLocalizer,
    }

    return localizers[language]()


def main():
    """
    # Create our localizers
    >>> e, g = get_localizer(language="English"), get_localizer(language="Greek")

    # Localize some text
    >>> for msg in "dog parrot cat bear".split():
    ...     print(e.localize(msg), g.localize(msg))
    dog σκύλος
    parrot parrot
    cat γάτα
    bear bear
    """
```


### Abstract Factory Pattern 
- used when you need to create families of related objects without specifying their exact class

```python
from abc import ABC, abstractmethod

# 1. Product Interfaces (Phone and Watch)
class Phone(ABC):
    @abstractmethod
    def specs(self) -> str:
        pass

class Watch(ABC):
    @abstractmethod
    def specs(self) -> str:
        pass

# 2. Concrete Products for Apple
class ApplePhone(Phone):
    def specs(self) -> str:
        return "Apple iPhone 14"

class AppleWatch(Watch):
    def specs(self) -> str:
        return "Apple Watch Series 8"

# 3. Concrete Products for Samsung
class SamsungPhone(Phone):
    def specs(self) -> str:
        return "Samsung Galaxy S23"

class SamsungWatch(Watch):
    def specs(self) -> str:
        return "Samsung Galaxy Watch 6"

# 4. Abstract Factory Interface
class GadgetFactory(ABC):
    @abstractmethod
    def create_phone(self) -> Phone:
        pass

    @abstractmethod
    def create_watch(self) -> Watch:
        pass

# 5. Concrete Factories for Apple and Samsung
class AppleFactory(GadgetFactory):
    def create_phone(self) -> Phone:
        return ApplePhone()

    def create_watch(self) -> Watch:
        return AppleWatch()

class SamsungFactory(GadgetFactory):
    def create_phone(self) -> Phone:
        return SamsungPhone()

    def create_watch(self) -> Watch:
        return SamsungWatch()

# 6. Client Code: Uses the factory to get gadgets
def get_gadgets(factory: GadgetFactory):
    phone = factory.create_phone()
    watch = factory.create_watch()
    print(f"Phone: {phone.specs()}, Watch: {watch.specs()}")

# Usage: Get gadgets from different factories
get_gadgets(AppleFactory())    # Output: Phone: Apple iPhone 14, Watch: Apple Watch Series 8
get_gadgets(SamsungFactory())  # Output: Phone: Samsung Galaxy S23, Watch: Samsung Galaxy Watch 6
```