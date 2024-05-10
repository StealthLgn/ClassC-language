C-like a general-purpose class-oriented coding language

//not programming language (programming == design, architecture)\
//not object-oriented (OOP it's about a messages [SMALLTALK], no types, no classes, no dots)

___________________________
"Header" files, definitions

<myheader.ch>

_________________________________
"Module" files, translation units

<mymodule.cc>

__________
Data types

*  bool\
    8 bits\
    accept only 1(true), 0(false) -- only one bit used

*  char\
    8 bits
    [−127, +127]
   
*  signed char\
    8 bits char, force signed\
    [−127, +127]
   
*  unsigned char\
    8 bits char, force unsigned\
    [0, +255]

*  wchar\
    16 bits unsigned, unicode char\
    [0, +65535]

*  int8\
    8 bit integer\
    [−127, +127]
  
*  signed int8\
    8 bit integer, force signed\
    [−127, +127]
  
*  unsigned int8\
    8 bit integer, force unsigned\
    [0, +255]

*  int16\
    16 bit integer\
    //keyword [short] for back-campat with 'C'\
    [−32767, +32767]

*  signed int16\
    16 bit integer, force signed\
    [−32767, +32767]

*  unsigned int16\
    16 bit integer, force unsigned\
    [0, 65535]

*  int32\
    32 bit integer
    //keyword [int] for back-campat with 'C'\
    [−2147483647, +2147483647]
  
*  signed int32\
    32 bit integer, force signed\
    [−2147483647, +2147483647]
  
*  unsigned int32\
    32 bit integer, force unsigned\
    [0, 4294967295]

*  int64\
    64 bit integer\
    //keyword [long long] for back-campat with 'C'\
    [−9223372036854775807, +9223372036854775807]
  
*  signed int64\
    64 bit integer, force signed\
    [−9223372036854775807, +9223372036854775807]
  
*  unsigned int64\
    64 bit integer, force unsigned\
    [0, 18446744073709551615] 

//in future int128...maybe...

*  float16\
    16 bit float\
    [0.xxx, 1.xxx] with floating-point

*  float32\
    32 bit float

*  float64\
    64 bit float

*  double32\
    32 bit double floating-point

*  double64\
    64 bit double floating-point

*  double128\
    //keyword [long double] for back-campat with 'C'\

//!! no [long double], [long long float], [unsigned int float] or something else in non-C code !!

*  void\
    no data, used as return in functions

__________
Structures\
[struct] keyword

same a 'C/C++', but...

*  inheritance\
    only other struct can be a base class (no classes, no abstract, no interface...)\
    struct is just simple data in memory, it's not an OBJECT

*  no-virtual\
    can't contains any virtual code:\
    no virtual destructor //compiler error\
    no virtual base class (like classes, interfaces)\
    no virtual/pure virtual methods //compiler error

*  immutability\
    struct S const {...}; //[const] after struct name\
    can be an immutability type

____________
Immutability
[const] keyword

All user-types (struct,class) is mutable by default\
They can change state

Immutability prevent this changes

class MyClass const {...};\
struct MyStruct const {...};

Type is mutable in CONSTRUCTOR and DESTRUCTOR\
Between them no one can change they state

struct Minimal const { //const-type declaration\
    unsigned int32 a,b; //can't be a const, becase need calc in constructor\
    Minimal( unsigned int aa , unsigned int bb ) : a(aa) , b(bb) { //OK, initializator\
        a = (a+b); //OK, mutation in constructor\
    }\
    unsigned int32 calculate()const {\
        return( a < b ? a : b ); //read-only\
    }\
    Minimal update( unsigned int xx ) { //no const required\
        return( Minimal( a+xx , b+xx ) ); //OK, create another copy\
    }\
};\
void main() {\
    Minimal min( 5 , 7 ); //OK, instance\
    min.a = 10; //ERROR, Minimal is [const type], need to create a copy (another) object with different parameters\
    Minimal othermin = min.update( min.calculate() ); //OK, create another Minimal with different parameters\
}

__________
Interfaces\
[interface] keyword

interface IReadable {\

    virtual unsigned int64 read( void* , unsigned int64 size ) =0; //can contains only pure virtual methods

    //no constructors\
    //IReadable() -- compile error
    //IReadable( const IReadable& ) -- compile error
    //IReadable( IReadable&& ) -- compile error
    //IReadable( ... ) -- compile error
    //template<> IReadable( ... ) -- compile error
    //interface can't be created -- inheritance only
    //the compiler does not generate a constructor (any one)

    //no destructors\
    //compile error
    //interface can't be deleted
    //any template T->~T() or call IReadable->~IReadable() -- compile error
    //the compiler does not generate a destructor

    //no copy assignment operator
    //operator= -- compile error
};
