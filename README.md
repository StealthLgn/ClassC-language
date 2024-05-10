C-like a general-purpose class-oriented coding language

//not programming language (programming == design, architecture)\
//not object-oriented (OOP it's about a messages [SMALLTALK], no types, no classes, no dots)

__________
Principles

    1.Encapsulation

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
```
class MyClass const {...};
struct MyStruct const {...};
```
Type is mutable in CONSTRUCTOR and DESTRUCTOR\
Between them no one can change they state
```
struct Minimal const { //const-type declaration

    unsigned int32 a,b; //can't be a const, becase need calc in constructor
    
    Minimal( unsigned int32 aa , unsigned int32 bb ) : a(aa) , b(bb) { //OK, initializator
        a = (a+b); //OK, mutation in constructor
    }
    
    unsigned int32 calculate()const {
        return( a < b ? a : b ); //read-only
    }
    
    Minimal update( unsigned int32 xx ) { //no const required
        return( Minimal( a+xx , b+xx ) ); //OK, create another copy
    }

    void set( unsigned int32 aa ) {
        a = aa; //ERROR! Minimal is a const (immutable) type, can't be changed
    }
};
void main() {

    Minimal min( 5 , 7 ); //OK, instance
    
    min.a = 10; //ERROR, Minimal is [const type], need to create a copy (another) object with different parameters
    
    Minimal othermin = min.update( min.calculate() ); //OK, create another Minimal with different parameters
}
```

__________
Mutability\
[mutable] keyword
"C++": Field (attribute) of class can be changed in [const] method //bad name! method can't be a const, it's a something different
but...
```
class A {

    mutable int32 m_value;

    void update()const {
        ++m_value; //ERROR! const method, m_value can't be changed! If method 'const' -- it's CONST
    }

    void check()const mutable {
        ++m_value; //OK, mutable method allow to change mutable attribute
    }

    bool valid()const {
        check(); //OK, can call const method
        return( m_value == 0 );
    }
};
```
```
[const_cast] keyword
!! REMOVED !!
```

____________________
Read only attributes\
[readonly] keyword

!! STOP USING getters/accessors !!
!! STOP calling method as get*... !!

```
class Point2D
{ public:

    readonly int32 posx,posy; //private attributes, but opened for read-only outside

    void move( int32 xx , int32 yy );
};
void main()
{
    Point2D coords( 10 , 20 );

    std::cout << coords.posx; //OK, public access; no 'get_position()' required; read-access to memory

    coords.posx = 11; //ERROR! read-only!

    coords.move( 11 , 21 ); //OK, public method; point will move itself
}
class Point3D :public Point2D
{public:

    readonly int32 posz;

    void move( int32 xx , int32 yy , int32 zz )
    {
        posx = 10; //ERROR! 'posx' is private for modification

        Point2D::move( xx , yy ); //OK, public method

        posz = zz;
    }
};
```

__________
Interfaces\
[interface] keyword
```
interface IReadable {

    virtual unsigned int64 read( void* , unsigned int64 size ) =0; //can contains only pure virtual methods

    //no constructors
    //IReadable() -- compile error
    //IReadable( const IReadable& ) -- compile error
    //IReadable( IReadable&& ) -- compile error
    //IReadable( ... ) -- compile error
    //template<> IReadable( ... ) -- compile error
    //interface can't be created -- inheritance only
    //the compiler does not generate a constructor (any one)

    //no destructor
    //compile error
    //interface can't be deleted
    //any template T->~T() or call IReadable->~IReadable() -- compile error
    //the compiler does not generate a destructor

    //no copy assignment operator
    //operator=
    //compile error

    bool is_ok()const; //OK, no 'virtual' required, it's pure virtual by default (because INTERFACE)

    virtual unsigned int64 peek( void* , unsigned int64 size ); //no '=0' required, it's pure virtual by default (because INTERFACE)
};
```
```
class StringReadable :public IReadable {...}; //read data from string\
class FileReadable :public IReadable {...}; //read data from file stream
```
```
new IReadable; //ERROR, interface can't be instantiated (even if it's an empty, without methods)
delete IReadable; //ERROR, interface can't be deleted
```
```
delete static_cast< FileReadable* >( &IReadable ); //ERROR! interface can't be cast to derived class
```
```
class EmptyReadable :public IReadable
{
    //IReadable method implementations

    //no virtual ~EmptyReadable destructor required, but allowed
};
delete EmptyReadable; //OK, delete class
```
```
static_cast< IReadable& >( EmptyReadable ); //OK, can cast derived 'class EmptyReadable' to base 'interface IReadable'
```
```
class FileStream :public IReadable, public IWritable {...}; //multi inheritance is OK
void copy( IReadable& , IWritable& ); //OK, use interfaces; pure abstractions
```
```
[dynamic_cast] keyword\
!! REMOVED !!
```

________
Override\
[override] keyword\
override virtual method from base class
```
class A {
    virtual void dump( ostream& )const;
};
class B :public A {

    override void dump( ostream& )const; //override at beginning, instead of virtual

    override dump( ostream& )const; //OK, no return value required, becase base class

};
```
__________
Exceptions\
!! REMOVED !!
```
[try] keyword removed
```
```
[catch] keyword removed
```
```
[throw] keyword removed
```
```
[nothrow] keyword removed
```
```
[noexcept] keyword removed
```

std::cout\
assert\
message box\
logfile\
console/terminal\
or something else to detect errors, but no 'goto'-style with jumps

____
goto\
```
[goto] keyword removed :)
```

________
Literals\
```
[constexpr] keyword
!! REMOVED !! replace by [literal] keyword
```
```
[constinit] keyword
!! REMOVED !!
```
```
[consteval] keyword
!! REMOVED !!
```
[literal] it's a static const readonly compile-time data
```
literal char Name[] = "My name";
```
```
void procedure( literal char* txt ); //literal only data in 'txt'
```
```
void func()literal; //literal only expressions in functions; prevent to use #define MACRO

literal int32 PlatformBits = 64; //x64 arch
literal int32 calculate_bytes()literal //return literal int32 and use only literal statements
{
    literal if ( PlatformBits == 16 )
    {
        std::cout << PlatformBits; //ERROR! 'operator <<' is not a literal function/operator
        return( 2 ); //OK, return literal
    }
    else literal if ( PlatformBits == 32 )
    {
        some_foo(); //ERROR! 'some_foo' is not a literal functions
        static const int32 Bits32 = 4; //OK, declara static const in function
        return( Bits32 ); //ERROR! 'Bits32' is not a literal
    }
    else
    if ( PlatformBits == 64 ) //no 'literal' keyword required, because all statement is literal
    {
        return( 8 ); //OK, return literal 8
    }
    else //last else is 'literal else' too!
    {
        static_assert; //stop compilation, if no valid statements
    }

    //?? what about 'literal switch' ??
}
```
```
literal int32 func() { //return literal-only in32 data
    return( 0 );
}
```
```
literal if( func() == 0 ) {...} //compile-time if check; if condition is false -- all this statement will be ignored by compiler
```
'literal if' CAN'T be mixed with normal(?) runtime 'if'\
example:
```
void some( int32 aa ) {
    if ( aa < 10 ) {
        //some stuff...
    }
    else
    literal if ( func() == aa ) { //ERROR! literal if combined with non-literal if
        //some stuff...
    }
}
```
solution:
```
void some( int32 aa ) {
    if ( aa < 10 ) { //OK, normal if
        //some stuff...
    }

    literal if( func() == 0 ) { //OK, another if; no mixing
        //some stuff...
    }
}
```

________________
Move constructor\

```
class A
{
    A( A&& other ); //!! move constructor is fantastic !!
};
```

________
Typedefs\
```
[typedef] keyword
```
same as 'C/C++', but...

template<class T> using type = .... //!! STOP USING 'using' aliases !!
```
typedef 
```
