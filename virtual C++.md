# virtual
## virtual member
* A virtual member is a member function that can be redefined (Overriden) in a derived class, while preserving its calling properties through references.
* Non-virtual members can also be redefined in derived classes, but non-virtual members of derived classes cannot be accessed through a reference of the base class
* Must be declared in public section of class, should be accessed using pointer or reference of base class type to achieve run time polymorphism.
* virtual functions are called only for objects of class types

## call virtual function
When calling a function using pointers or references, the following rules apply:
* A call to a virtual function is resolved according to the underlying type of object for which it is called.
* A call to a nonvirtual function is resolved according to the type of the pointer or reference.

## pure virtual function
* virtual int area () =0;
* Classes that contain at least one pure virtual function are known as abstract base classes.
  * Abstract base classes cannot be used to instantiate objects
  * a member of the abstract base class can use the special pointer this to access the proper virtual members, even though itself has no implementation for this function(没实现，但是可以用this指针调用该方法，会找到合适的实现函数)

## how virtual works?
* vtable (the class, adresses of all virtual functions) & vptr (in a object as a member variable, points to vtable of the class)
* Once vptr is fetched, vtable of derived class can be accessed. Using vtable, address of derived class function is accessed and called.

## virtual destructor
* A destructor can be virtual. We may want to call appropriate destructor when a base class pointer points to a derived class object and we delete the object. If destructor is not virtual, then only the base class destructor may be called. 
* Since the destructor is vitual, the derived class destructor is called which in turn calls base class destructor.

## example
```
#include <iostream>
using namespace std;
class Box {
 public:
	Box (int a, int b, int c): l(a), w(b), h(c) {};
	void setBox (int a, int b, int c){
		l = a;
		w = b;
		h = c;
	}
	virtual int volume () {
		return 0;
	};
	void printV () {
		cout << this->volume() << endl;
	}
	virtual void print() {
		cout << "BOX" << endl;
	}
 protected:
	int l, w, h;
};
class CuboidBox : public Box{
    public:
	CuboidBox(int a, int b, int c) : Box(a, b, c){};
	int volume () {
		return l * w * h;
	}
	void print () {
		cout << "CuboidBox" << endl;
	}
};
class PyramidBox : public Box{
    public:
	PyramidBox(int a, int b, int c) : Box(a, b, c){};
	int volume () {
		return l * w * h / 3;
	}
	void print() {
		cout << "PyramidBox" << endl;
	}
};
int main() {
	CuboidBox cb(2,3,4);
	PyramidBox pb(2,3,4);
	//cb.setBox(2,3,4);
	//pb.setBox(2,3,4);
	Box *c = &cb;
	Box *p = &pb;
	cout << c->volume() << endl;
	cout << p->volume() << endl;
	c->printV();
	p->printV();

	Box* b1 = new CuboidBox(1,2,3);
	Box* b2 = new PyramidBox(1,2,3);
	Box* b = new Box(1,2,3);
	b1->Box::print();
	b1->print();
	b2->print();
	return 0;
}
```
