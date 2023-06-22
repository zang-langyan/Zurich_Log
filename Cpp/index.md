# C++ basic

\toc

## Literals and Types

| Interger | Character | Floating-point | String | Boolean | Pointer |
| -------- | --------  | --------       | ------ | --------| --------|
| int      | char      | float      |  | true    | std::nullpter_t nullptr|
| signed/unsigned | signed char  | double      | | false   | |
| short(16)/long(32)/long long(64) | unsigned char  | long double |  | | |

## Declarations and Definitions

```cpp
#include <string>

int f(int i); // forward declaration

int main()
{
    const double pi = 3.14; //OK
    int i = f(2); //OK. f is forward-declared
    C obj; // error! C not yet declared.
    std::string str; // OK std::string is declared in <string> header
    j = 0; // error! No type specified.
    auto k = 0; // OK. type inferred as int by compiler.
}

int f(int i)
{
    return i + 42;
}

namespace N {
   class C{/*...*/};
}
```

Static class members

## Typedefs and using statements

```cpp
#include <vector>

/*
 * typedef
 */ 
// template <typename T>
// typedef std::vector<T> VECTOR

/*
 * using
 */ 
template <typename T>
using VECTOR = std::vector<T>
```

## Storage classes

`static` keyword can be used to declare `variables`, `functions` at global scope, namespace scope and class scope; `variables` at local scope.

1. When you declare a variable or function at file scope (global and/or namespace scope), the static keyword specifies that the variable or function has internal linkage. When you declare a variable, the variable has static duration and the compiler initializes it to 0 unless you specify another value.
2. When you declare a variable in a function, the static keyword specifies that the variable retains its state between calls to that function.
3. When you declare a data member in a class declaration, the static keyword specifies that one copy of the member is `shared by all instances of the class`. A static data member must be defined at file scope. An integral data member that you declare as const static can have an initializer.
4. When you declare a member function in a class declaration, the static keyword specifies that the function is shared by all instances of the class. A static member function can't access an instance member because the function doesn't have an implicit this pointer. To access an instance member, declare the function with a parameter that's an instance pointer or reference.
5. You can't declare the members of a union as static. However, a globally declared anonymous union must be explicitly declared static.

### static variable in a function (2.)

```cpp
// static1.cpp
// compile with: /EHsc
#include <iostream>

using namespace std;
void showstat( int curr ) {
   static int nStatic;    // Value of nStatic is retained
                          // between each function call
   nStatic += curr;
   cout << "nStatic is " << nStatic << endl;
}

int main() {
   for ( int i = 0; i < 5; i++ )
      showstat( i );
}
```

### static in a class (3.)

```cpp
// static2.cpp
// compile with: /EHsc
#include <iostream>

using namespace std;
class CMyClass {
public:
   static int m_i;
};

int CMyClass::m_i = 0;
CMyClass myObject1;
CMyClass myObject2;

int main() {
   cout << myObject1.m_i << endl; // 0
   cout << myObject2.m_i << endl; // 0

   myObject1.m_i = 1;
   cout << myObject1.m_i << endl; // 1
   cout << myObject2.m_i << endl; // 1

   myObject2.m_i = 2;
   cout << myObject1.m_i << endl; // 2
   cout << myObject2.m_i << endl; // 2

   CMyClass::m_i = 3;
   cout << myObject1.m_i << endl; // 3
   cout << myObject2.m_i << endl; // 3
}
```

```cpp
// static3.cpp
// compile with: /EHsc
#include <iostream>
using namespace std;
struct C {
   void Test(int value) {
      static int var = 0;
      if (var == value)
         cout << "var == value" << endl;
      else
         cout << "var != value" << endl;

      var = value;
   }
};

int main() {
   C c1;
   C c2;
   c1.Test(100);
   c2.Test(100);
}

/* Output
var != value
var == value
*/
```

In short, the `static` variable can be tranferred and retained through the process

## `const` member functions

Declaring a member function with the const keyword specifies that the function is a "read-only" function that doesn't modify the object for which it's called. A constant member function can't modify any non-static data members or call any member functions that aren't constant. To declare a constant member function, place the const keyword after the closing parenthesis of the argument list. The const keyword is required in both the declaration and the definition.

```cpp
// constant_member_function.cpp
class Date
{
public:
   Date( int mn, int dy, int yr );
   int getMonth() const;     // A read-only function
   void setMonth( int mn );   // A write function; can't be const
private:
   int month;
};

int Date::getMonth() const
{
   return month;        // Doesn't modify anything
}
void Date::setMonth( int mn )
{
   month = mn;          // Modifies data member
}
int main()
{
   Date MyDate( 7, 4, 1998 );
   const Date BirthDate( 1, 18, 1953 );
   MyDate.setMonth( 4 );    // Okay
   BirthDate.getMonth();    // Okay
   BirthDate.setMonth( 4 ); // C2662 Error
}
```

## `constexpr`

The primary difference between `const` and `constexpr` variables is that the initialization of a `const` variable can be deferred until run time. A `constexpr` variable must be initialized at compile time. All `constexpr` variables are `const`.

```cpp
constexpr float x = 42.0;
constexpr float y{108};
constexpr float z = exp(5, 3);
constexpr int i; // Error! Not initialized
int j = 0;
constexpr int k = j + 1; //Error! j not a constant expression

constexpr float exp(float x, int n)
{
    return n == 0 ? 1 :
        n % 2 == 0 ? exp(x * x, n / 2) :
        exp(x * x, (n - 1) / 2) * x;
}
```

## Alias and typedef

```cpp
typedef type identifier
using identifier = type;
```

```cpp
#include <stdlib.h>
#include <new>

template <typename T> struct MyAlloc {
    typedef T value_type;

    MyAlloc() { }
    template <typename U> MyAlloc(const MyAlloc<U>&) { }

    bool operator==(const MyAlloc&) const { return true; }
    bool operator!=(const MyAlloc&) const { return false; }

    T * allocate(const size_t n) const {
        if (n == 0) {
            return nullptr;
        }

        if (n > static_cast<size_t>(-1) / sizeof(T)) {
            throw std::bad_array_new_length();
        }

        void * const pv = malloc(n * sizeof(T));

        if (!pv) {
            throw std::bad_alloc();
        }

        return static_cast<T *>(pv);
    }

    void deallocate(T * const p, size_t) const {
        free(p);
    }
};

#include <vector>
using MyIntVector = std::vector<int, MyAlloc<int>>;

#include <iostream>

int main ()
{
    MyIntVector foov = { 1701, 1764, 1664 };

    for (auto a: foov) std::cout << a << " ";
    std::cout << "\n";

    return 0;
}
```

`typedef` is often combined with struct to declare and name user-defined types:

```cpp
// typedef_specifier2.cpp
#include <stdio.h>

typedef struct mystructtag
{
    int   i;
    double f;
} mystruct;

int main()
{
    mystruct ms;
    ms.i = 10;
    ms.f = 0.99;
    printf_s("%d   %f\n", ms.i, ms.f);
}

/* Output
10   0.990000
*/
```

## `decltype`

The decltype type specifier yields the type of a specified expression. The decltype type specifier, together with the auto keyword, is useful primarily to developers who write template libraries. Use auto and decltype to declare a template function whose return type depends on the types of its template arguments. Or, use auto and decltype to declare a template function that wraps a call to another function, and then returns the return type of the wrapped function.

