# singleton

1. Singleton should be considered only if all three of the following criteria are satisfied:
    1. Ownership of the single instance cannot be reasonably assigned
    2. Lazy initialization is desirable
    3. Global access is not otherwise provided for

2. Ensure a class has only one instance, and provide a global point of access to it
    1. how to: Make the class of the single instance responsible for access and "initialization on first use". 
    2. The single instance is a private static attribute. The accessor function is a public static method.

3. how to make a singleton
    1. Define a private static attribute in the "single instance" class.
    2. Define a public static accessor function in the class.
    3. Do "lazy initialization" (creation on first use) in the accessor function.
    4. Define all constructors to be protected or private.
    5. Clients may only use the accessor function to manipulate the Singleton.
    6. Inheritance can be supported, but static functions may not be overridden. The base class must be declared a friend of the derived class (in order to access the protected constructor). ????
```
# include<iostream>
using namespace std;
class CSingleton {
 private:
   // constructor
   CSingleton() {
     cout << "constructor" << endl;
   }
   static Singleton *m_pInstance;
 public:
   static CSingleton* getInstance() {
     // lazy initialize
     if (m_pInstance == NULL) {
       m_pInstance = new CSingleton();
     }
     return m_pInstance;
   }
};
// must initialize the static member outside of the class declaration
CSingleton* CSingleton::m_pInstance = NULL;
int main() {
  CSingleton::getInstance();
  CSingleton::getInstance();
  return 0;
}
```
### tips:
1. The single instance is a private static attribute. The accessor function is a public static method. Why?
    1. because you access the public static method without a object, if in this static method, you use a non-static member, this member must be called with this pointer (or a object), but static method doesn't have one.
    2. also this static variable is shared by all objects of the class, it need static

2. why must define the static member outside of the class declaration?
    1. static members exist as members of the class rather than as an instance in each object of the class.
    2. if you initialize the static variable inside the class declaration, as a concept it will be re-initialized (not the actual behaviour) on every creation of an object/instance of the class
