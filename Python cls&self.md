# cls & self

```
class MyClass:
    def method(self):
        return 'instance method called', self

    @classmethod
    def classmethod(cls):
        return 'class method called', cls

    @staticmethod
    def staticmethod():
        return 'static method called'
```

## Instance Method
* The parameter 'self' points to an instance of MyClass when the method is called.
* Through the self parameter, instance methods can modify attributes and other methods on the same object.

## Class Method
* Marked with '@classmethod' decorator.
* Class methods take a 'cls' parameter that points to the class when the method is called.
* Class methods can modify class state that applies across all instances of the class.
* Avoid the overhead of instantiating a class.
* Can help add alternative constructors.

## Static Methods
* Marked with '@staticmethod' decorator.
* Can't modify neither class state nor object state

[Reference](https://realpython.com/blog/python/instance-class-and-static-methods-demystified/)
