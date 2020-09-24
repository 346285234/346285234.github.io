---
layout: post
title: Swift&OC&C++
category: Develop
---



## Swift

## Objective-C

## C++

### Variables

arithmetic type: bool, char, int,  float, double

signed, unsigned

extern

compound type: reference(&), pointer(*)

const

* int * const 指针不可变
* const int * = int const * 内容不可变

typedef (oldname) (newname)

decltype(a) b // b的类型和a一样

struct

preprocessor: #ifdef, #ifndef, #define, #endif

using: declare namespace

#### String

string is a custom class.

```
strlen(str);
strcmp(p1, p2); // 0 if p1 == p2, positive if p1 > p2, negative if p1 < p2
strcat(p1, p2); // append p2 to p1, return p1
strcpy(p1, p2); // copy p2 into p1, return p1
```

sizeof实际占用空间，

size, length, strlen字符串长度，到'\0'为止。  

```
const char *a = s.c_str(); 
```

#### Vector

```
vector<int> v = {1};
v.push_back(1); // append 1
v.empty(); // true if empty
v.size(); // return size
v[0]; // return element
v1 == v2; // true if all element same 
```

#### Array

Unlike vector, arry have fixed size.

```
int a[] = {0, 1};
int last = *(a+1); // a[1]
a[-1]; // a[0]
vector <int> v(begin(a), end(a)); // convert array to vector.
```

#### Iterator

```
auto b = v.begin(), e = v.end(); // get iterator from begin and end
*b; // return value
```

### Control flow

#### while

```
while (cin>>s) cout<<s;
do 
	i++;
while (i<3);
```



#### for

```
1. range for
for (auto c: str) cout<<c<<endl;
2. traditional for
for (int i = 0; i < 1; i++) {;}

```

#### if

```
if (a > 3) cout<<"bigger 3"; 
else if (a > 2) cout<<"bigger 2";
else
	cout<<a;
```

#### switch

```
switch (ch) {
	case 'a':
		break;
	default:
		break;
}
```

#### try

```
try {
	;
} catch (range_error err) {
	;
} catch (runtime_error err) {
	;
} catch(exception err) {
	;
}
```

### Fuction

```
void f() {
    static int a = 1;
}

int funcwithstaticvar() {
    static int a = 0;
    return a++;
}
```

#### argument passing

1. pass by value 

   ```
   void reset(int *ip) { // ip is a local var.
   	*ip = 0;
   	ip = 0; // change only the local pointer.
   }
   
   int i = 1;
   reset(&i); // copy address to new pointer.
   ```

   

2. pass by reference

   ```
   void reset(const int &p) {;} // use const reference to save memory
   ```

inline: avoid function call overhead

assert: preprocessor macro, if false, write a message and terminate program.

function pointer

```
void f();
void (*fp)() = f;
```

hidden name

```
int i = 0;
for (int i = 0; i < 2; i++) cout<<i; // inside i = 2 before leave scope
cout<<i; // outside i = 0
```

