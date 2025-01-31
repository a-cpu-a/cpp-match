# cpp-match
Simple match expression for c++, since it hasnt been added yet.

To add to your project, just copy the file into some folder like "ext/"

```cpp

#include <iostream>

inline void test()
{
	std::variant<int, double, float> v{ 0.0f };

	ezmatch(v)(// note that this isnt a curly brace!

		varcase(int) { std::cout<<"Int "<< var <<"\n"; },// note the comma
		varcase(double) { std::cout<<"Double "<< var <<"\n";},
		ezcase(varName,float) { std::cout<<"Float "<< varName <<"\n";}
		// Note the missing comma!
	);
}

```
```cpp

#include <iostream>

namespace MyEnumType
{
	using INT = int;
	using F64 = double;
	struct STRNUM {std::string str; int num;};
}
using MyEnum = std::variant<
	MyEnumType::INT,
	MyEnumType::F64,
	MyEnumType::STRNUM
>;

inline void test2()
{
	MyEnum v = MyEnumType::STRNUM("aa",0);

	ezmatch(v)(

		varcase(MyEnumType::INT) { std::cout<<"Int "<< var <<"\n"; },
		varcase(double) { std::cout<<"Double "<< var <<"\n";},

		varcase(MyEnumType::STRNUM) {
			std::cout <<"Strnum "<< var.str 
				<<" N: "<< var.num <<"\n";
		}

	);
}

```
The return types of the cases must be identical.
The compiler will add the return type itself, so using "-> Type" is optional, but required in some cases
```cpp

#include <iostream>

consteval int test3()
{
	std::variant<int, bool> v{ 0 };

	return ezmatch(v)(
		varcase(int) -> char { return 1; },
		varcase(bool) -> char { return 0;}
	);
}

```
