# C++ constructor
* is a method which is invoked automatically at the time of object creation
* is used to initialize the data/variable members of new object
* A derived class constructor always calls a base class constructor first
* if no constructors are declared in a class, the compiler provides a default constructor
    * you can call it like Class A; Class A{};
    * default constructors have no parameters

# C++ this pointer
* this pointer is an implicit parameter to all non-static member functions. Therefore, this may be used to refer to the invoking object.
* this pointer is a constant pointer that holds the memory address of the current object
