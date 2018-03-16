# Multithreading

### 5 different ways to construct a thread
* p1 & p4: As static functions are not associated with any object of class. So, we can directly pass the static member function of class as thread function without passing any pointer to object.
* p3: Function Pointer
* p5: Function Objects 

### join() & detach()
Before return, we must join() or detach() current thread objects. Before do it, check if this object joinable.

### get_id()

```
// This is a test of how to construct a thread and how to join
#include<iostream>
#include<thread>
using namespace std;

class TestThread {
  public:
	void display(int n) {
		for (int i = 0; i < n; ++i) {
			cout << "class display: " << i << endl;
		}
		cout << endl;
	}
	static void displays(int n) {
		for (int i = 0; i < n; ++i) {
			cout << "class static display: " << i << endl;
		}
		cout << endl;
	}
};

class Obj{
  public:
	void operator() () {
		cout << "create thread with object" << endl;
	}
};

struct C {
	void display(int n) {
		for (int i = 0; i < n; ++i) {
			cout << "struct display: " << i + 'A' << endl;
		}
		cout << endl;
	}
};

void display(int n) {
	for (int i = 0; i < n; ++i) {
		cout << "func display: " << i + 'a' << endl;
	}
	cout << endl;
}

int main() {
	int n = 5;
	thread p1(TestThread::display, TestThread(), n);
	C var;
	thread p2(&C::display, var, n);
	thread p3(display, n);
	thread p4(&TestThread::displays, n);
	thread p5((Obj()));
	for (int i = 0; i < n; ++i) {
		cout << "main func: " << i << endl;
	}
	cout << "Wait..." << endl;
	auto id = p1.get_id();
	cout << "p1.id " << id << endl;
  if (p1.joinable()) p1.join();
	p2.join();
	p3.join();
	p4.join();
	p5.join();
	cout << "finished!" << endl;
	return 0;
}
```

### race condition
When two or more threads perform a set of operations in parallel, that access the same memory location.  Also, one or more thread out of them modifies the data in that memory location, then this can lead to an unexpected results some times.

### mutex (
lock(), unlock(), try_lock()

### lock_guard & unique_lock
* pretty same
* lock_guard will be locked only once on construction, and unlocked on destruction
* unique_lock supports unlock and lock again (with condition_variable, during wait())


### condition_variable 
* wait(lck) 
* notify_all() notify_one()

```
// This is the test of race condition
#include<iostream>
#include<vector>
#include<thread>
#include<mutex>
using namespace std;

class Wallet {
        int money;
        mutex mtx;
  public:
        Wallet() {
                money = 0;
        }
        int getMoney() {
                return money;
        }
        void addMoney(int x) {
                for (int i = 0; i < x; ++i) {
                        mtx.lock();
                        money++;
                        mtx.unlock();
                }
        }
};

int test() {
        Wallet w;
        vector<thread> v;
        for (int i = 0; i < 10; ++i) {
                v.push_back(thread(&Wallet::addMoney, &w, 100));
        }
        for (auto &th : v) {
                th.join();
        }
        return w.getMoney();
}

int main() {
        for (int i = 0; i < 100; ++i) {
                int val = test();
                if (val != 1000) {
                        cout << "Error: count " << i << " is " << val << endl;
                }
        }
        return 0;
}
```
