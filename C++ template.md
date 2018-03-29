## 1. template
  The simple idea is to pass data type as a parameter so that we don’t need to write same code for different data types.

## 2. What is the difference between function overloading and templates?
  Both function overloading and templates are examples of polymorphism feature of OOP. 
  * Templates provide an advantage when you want to perform the same action on types that can be different.
  * You can use overloading when you want to apply different operations depending on the type

## 3. What is template specialization?
  Template specialization allows us to have different code for a particular data type.

## 4. Function templates
  * template <class identifier> function_declaration;
  * the function call: function_name <type> (parameters);
  * When the compiler encounters this call to a template function, it uses the template to automatically generate a function replacing each appearance of myType by the type passed as the actual template parameter and then calls it. This process is automatically performed by the compiler and is invisible to the programmer.
  * The compiler has instantiated and then called each time the appropriate version of the function.？？？先实例化出几个不同的版本然后调用么？还是每次调用时替换好？
  * can just call：function_name (parameters); without <type> the compiler can determine the appropriate instantiation

## 5. Class templates
  a class can have members that use template parameters as types.
  
```
#include<iostream>
#include<string>
using namespace std;

template <class T>
T getMin(T a, T b) {
  T ret;
  ret = (a > b)? b : a;
  return ret;
}
template <class T, class S>
T getMax(T a, S b) {
  return (a > b)? a : b;
}

template <class T>
class Q {
  T a, b;
 public:
  Q (T x, T y) {
    a = x;
    b = y;
  }
  T getMax();
};
template <class T>
T Q<T>::getMax() {
  T ret;
  ret = (a > b)? a : b;
  return ret;
}

int main() {
  int a = 10, b = 21;
  double c = 14.2, d = 11.54;
  string s = "aaa", t = "aab";
  cout << getMin<int>(a, b) << endl;
  cout << getMin(c, d) << endl;
  cout << getMin(s, t) << endl;
  cout << getMax(a, c) << endl;
  Q<int> q(a, b);
  cout << q.getMax() << endl;
  return 0;
}
```
