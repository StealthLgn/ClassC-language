```
in-progress
```

C-style a general-purpose class-oriented coding language

//not programming language (programming == design, architecture,patterns)\
//not object-oriented (OOP it's about a messages [SMALLTALK], no types, no classes, no dots)\
//not a 'C++ killer' or some
__________
## Principles

    1.Encapsulation
    //prevent change state outside of object
    //only object knows how it's changed
    //the object must have an idea and purpose
    
    2.Composition
    //~40% of code should be a constructors

!! STOP FLEXIBILITY !!
INFLEXIBLE LANGUAGE
```
flexibility does not equal opportunity
```
```
more 'discipline code'
```
____
## Encapsulation
hold data/state and behavior together inside an object
```
class X
{
    public:

        void set_data( int32 i )
        {
            m_encapsulation = i; //!! encapsulation is over now !!
        }

    private:
        int32 m_encapsulation;
};
//same
void main()
{
    X xx;
    xx.m_encapsulation = 10; //or 'set_data' it doesn't matter
}
```
each 'set_data', 'set_name', 'set_*' broke encapsulation\
this is not an object - it's a data structure\
if object have no-state, he don't need encapsulation -- only state+behavior require encapsulation
```
std::vector, std::map, std::list - this is all data 'struct', it's not an objects; they are not independent

class Widget
{
    virtual void draw( Context& ); //check visibility, prepare state and draw: only widget itself knows how to draw -- no external control is needed

    virtual void move( const Point2D& ); //check point, check state (is movable), check clamp to screen and then move

    virtual void show( bool ); //check state, check already shown and then toggle visibility + maybe other staff
};
```
______
## Composition
make attributes as objects
```
inheritance vs. composition

bad idea, no 'versus'
inheritance AND composition
```
```
interface allocator
{
    virtual void* allocate( memory_size ) =0;
};
class HeapAllocator :public allocator
{
    override allocate( memory_size sz )
    {
        return( Platform::allocate( sz ) );
    }
};
class LoggerAllocator :public allocator
{
    allocator& Parent;
    LoggerAllocator( allocator& );

    override allocate( memory_size sz )
    {
        log( "allocate size" );
        return( Parent.allocate( sz ) );
    }
};
void main()
{
    app_run( LoggerAllocator( HeapAllocator() ) );
}
```
```
void main()
{
    Engine( EmptySystems() ).run();

    Engine( Systems( Loop( EmptyProgram() ) ) ).run();

    Engine( MySystems( Loop( Program( User( "folder_path" , Settings() ) , Database("path") , Offline() ) ) ) ).run();

    Engine( MySystems( Loop( DebugProgram( DebugUser( "folder_path" , Settings() ) , MySqlDatabase("path") , Online("address:port") ) ) ) ).run();
}
```
_____________________________
## The One Definition Rule (ODR)
```
"cannot have more than one definition by translation unit"
enum
class
interface
struct
typedef
```
```
//golang
type MyStruct struct
{
};
func (me*MyStruct) function()
{
    //add method 'function' to struct 'MyStruct'

    //anytime

    //anywhere
}
//in ClassC lang not allowed
```
___
## Header files
definitions
```
<myheader.ch>
```
___
## Module files
translation units\
?? can we change idea of translation units ?? how ??
```
<mymodule.cc>
```
___
## Comments
### C-style
```
/**
 * Multi line documentations
 */
```
### C++ style
```
//inline comments
//try to minimize comments in your code
```
____
## Preprocessor
```
format __CLASSC_[XXX]__
```
* Language version
```
__CLASSC_VERSION__ //language version by compiler; 2022, 2025, 2030 (no bit mask)
```
* File name
```
__FILE__ //at least filename (module name); very important for testing/debugging/error handling
__CLASSC_FILE__
```
* Line number
```
__LINE__ //line number; very important for testing/debugging/error handling
__CLASSC_LINE__
```
* Define keywords
```
#define true  false //ERROR! no language 'keywords' in #define
#undef true //ERROR! no language 'keywords' in #undef
```
* Includes
```
#include <memory> //only <> breckets

#include "file.ch" //ERROR! use <file.ch> instead

#include once //include this file only once in translation unit
```
* Compiler extensions
```
#pragma //some compiler extensions

__CLASSC_NOEXTENSIONS__ //if extensions are disabled by compiler
```
* Detect architecture
```
__CLASSC_ARCH__ == 32 //x32
__CLASSC_ARCH__ == 64 //x64
```
* Detect compiler name
```
__CLASSC_COMPILER_[name]__ == version //#ifdef compiler 'GCC', 'MS', 'CLANG' ...
```
* Detect target cpu
```
__CLASSC_CPU_[name]__ //#ifdef target machine cpu 'X86', 'X64', 'X86X64' ...
```
* Target cpu extensions
```
__CLASSC_CPUEXT_[name]__ //#ifdef target cpu feature enabled 'SSE', 'SSE2', 'SSSE3', 'AVX', 'RDRND' ...
```
___
## Data types
*  bool
```
8 bits
only 1(true) or 0(false) -- only one bit used
```
*  char
```
8 bits
[−127, +127]
```
*  signed char
```
8 bits char, force signed
[−127, +127]
```
*  unsigned char
```
8 bits char, force unsigned
[0, +255]
```
*  wchar
```
16 bits unsigned, unicode char
[0, +65535]
```
*  int8
```
8 bit integer
[−127, +127]
```
*  signed int8
```
8 bit integer, force signed
[−127, +127]
```
*  unsigned int8
```
8 bit integer, force unsigned
[0, +255]
```
*  int16
```
16 bit integer
//keyword [short] for back-campat with 'C'
[−32767, +32767]
```
*  signed int16
```
16 bit integer, force signed
[−32767, +32767]
```
*  unsigned int16
```
16 bit integer, force unsigned
[0, 65535]
```
*  int32
```
32 bit integer
//keyword [int] for back-campat with 'C'
[−2147483647, +2147483647]
```
*  signed int32
```
32 bit integer, force signed
[−2147483647, +2147483647]
```
*  unsigned int32
```
32 bit integer, force unsigned
[0, 4294967295]
```
*  int64
```
64 bit integer
[−9223372036854775807, +9223372036854775807]
//keyword [long long] for back-campat with 'C'
```
*  signed int64
```
64 bit integer, force signed
[−9223372036854775807, +9223372036854775807]
```
*  unsigned int64
```
64 bit integer, force unsigned
[0, 18446744073709551615]
```
```
//in future int128...maybe...
```
*  float16
```
16 bit float
[0.xxx, 1.xxx] floating-point
```
*  float32
```
32 bit float
//keyword [float] for back-campat with 'C'
```
*  float64
```
64 bit float
```
*  double32
```
32 bit double floating-point
```
*  double64
```
64 bit double floating-point
```
*  double128
```
128 bit double floating-point
keyword [long double] for back-campat with 'C'
```
*  void
```
no data, used as return in functions
```
```
//!! no [long double], [long long float], [unsigned int float] or something else in non-C code !!
//!! this keywords for backward compatibility with 'C' libraries and API !!
```
___
## extern "C"
```
extern "C"
{
    //pure 'C' code

    #include <stdio.h>
    #include <stdlib.h>

    #include "yourlib.h" //in pure-C include with "" brackets is allowed for compat

    enum {};

    struct {};

    void function();

    const int GVar;

    const float GSome;

    //all types should be marked as 'extern_c' or 'pure_c' when compile
}
```
___
## Declarations
```
[category] [name] [specifiers] {  } ;
```
```
[category]
enum
struct
interface
class
namespace
```
```
[specifiers]
const
static
true
```
___
## Structures
```
[struct] keyword
```
same a 'C/C++', but...
*  inheritance
```
only other struct can be a base class (no classes, no abstract, no interface...)
struct is just simple data in memory, it's not an OBJECT

struct A
{
};
struct B :public A //OK
{
};
class CC :public B //ERROR! 'struct' as base type is not allowed (stop mixing classes with data structures, use composition)
{
    public readonly B data; //OK, composition
};
struct D :public CC //ERROR! CC is a 'class', only 'struct' allowed
{
    std::string Name;

    struct sort_name { //OK, nested types
        bool operator()( const D& , const D& )const;
    };
};
```
*  no-virtual
```
can't contains any virtual code:
no virtual destructor //compiler error
no virtual base class (like classes, interfaces)
no virtual/pure virtual methods //compiler error
struct A
{
    virtual ~A(); //ERROR! no virtual methods/destructors
};
struct B :public A
{
};
struct C :public virtual A //ERROR! not virtual base types
{
};
struct D :protected virtual B, protected virtual C //ERROR! not virtual base types
{
};
```
*  immutability
```
struct S const //[const] after struct name
{
};
can be an immutability type
```
___
## Classes
```
[class] keyword
```
same a 'C++', but...
*  inheritance
```
only [classes] or [interfaces] can be a base types for classes
no struct are allowed
```
*  immutability
```
class C const //[const] after class name
{
};
can be an immutability type
```
*  virtual
```
allow to use virtual base classes
allow to use virtual methods, pure virtual methods
allow to use virtual destructors
```
___
## Access Specifiers
```
[public] keyword
[protected] keyword
[private] keyword
```
same as C++, but...
```
struct A
{
    public: //OK, struct can contains access specifiers
    protected:
    private:
};
struct B :public A //OK, public inheritance
{
    public:

        int32 a,b,c; //OK, public attributes (fields)

        private int32 d; //OK, keep public section, but attribute 'd' is private (private without ':')

        int32 len,count; //OK, 'len' and 'count' is public

    private:

        ~B(); //OK, private destructor
};
class AA
{
    public:

        AA();

        protected AA( const AA& ); //OK, protected copy constructor
};
```
___
## Immutability
```
[const] keyword
```

All user-types (struct,class) are mutable by default\
They can change state

Immutability prevent this changes
```
class MyClass const {...};
struct MyStruct const {...};
```
Type is still mutable in CONSTRUCTOR and DESTRUCTOR\
Between them no one can change they state (@see UNION section)
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
    
    min.a = 10; //ERROR! Minimal is [const type], need to create a copy/another object with different parameters
    
    Minimal othermin = min.update( min.calculate() ); //OK, create another Minimal with different parameters
}
```
___
## Mutability
```
[mutable] keyword
```
"C++": Field (attribute) of class can be changed in [const] method //bad name! method can't be a const, it's a something different
```
class A
{
    mutable int32 m_value;
    const int32 m_const;
    bool m_flag;

    void update()const
    {
        ++m_value; //ERROR! const method, m_value can't be changed! If method 'const' -- it's CONST

        ++m_const; //ERROR! 'm_const' declared as const

        m_flag = true; //ERROR! const method
    }

    void check()const mutable
    {
        ++m_value; //OK, mutable method allow to change mutable attribute

        ++m_const; //ERROR! 'm_const' declared as const

        m_flag = true; //ERROR! const method
    }

    bool valid()const
    {
        check(); //OK, can call const method

        ++m_const; //ERROR! 'm_const' declared as const

        return( m_value == 0 );
    }
};
```
```
[const_cast] keyword
!! REMOVED !!
```
```
disable C-style cast ((type)(other))
allowed only in extern "C" {...} section for back-compat with C
```
```
struct A const mutable //ERROR! const type can't be a mutable
{
};
```
___
## Readonly fields
```
[readonly] keyword
```
!! STOP USING getters/accessors !!\
!! STOP calling(naming) methods as get*... !!
```
class Point2D
{public:

    readonly int32 posx,posy; //private attributes, but opened for read-only outside; do not use 'get_posx()'

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
        posx = 10; //ERROR! 'posx' is a private

        Point2D::move( xx , yy ); //OK, public method

        posz = zz;
    }
};
void func()
{
    Point2D coords( 10 , 20 );

    const int32& crefxx = coords.posx; //OK, const reference

    int32& refxx = coords.posx; //ERROR! posx is const (because readonly)

    int32 xx = coords.posx; //OK, copy

    const int32* cptrxx = &coords.posx; //OK, const address

    (*cptrxx) = 10; //ERROR! cptrxx is const

    const_cast<int32*>(cptrxx); //ERROR! no 'const_cast'

    ((int32*)cptrxx); //ERROR! no 'C-style' type cast

    int32* ptrxx = &coords.posx; //ERROR! posx is const (because readonly)

    (*c_const_cast( cptrxx )) = 10; //OK, cast 'readonly' witch extern "C" function; so bad with 'C' back-compat, see 'void*' section
}
extern "C"
{
    int32* c_const_cast( const int32* p )
    {
        return( (int32*)p ); //OK, 'C' type cast allowed in extern "C" block
    }
}
```
```
struct UserDeclaration
{
    readonly const std::string name; //ERROR! 'const' with 'readonly' is not allowed

    const std::string nickname; //OK, public const 'nickname' attribute

    readonly int32 ping; //OK, public readonly 'ping' attribute

    UserDeclaration( const std::string& strname , int32 pp ); //primary constructor

    protected void update_ping( int32 );
};
struct DebugUserDeclaration :public UserDeclaration //OK, public struct inheritance
{
    DebugUserDeclaration( int32 pp )
    {
        UserDeclaration::ping = pp; //ERROR! readonly attributes is private

        update_ping( pp ); //OK, call protected mutator-method to change attribute
    }

    DebugUserDeclaration( const std::string& strname , int32 pp ) :UserDeclaration( strname , pp ) {;} //OK, use primary constructor of base type
};
```
___
## Interfaces
```
[interface] keyword
```
```
interface IReadable
{
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
    //interface can't be deleted
    //any template T->~T() or call IReadable->~IReadable() -- compile error
    //the compiler does not generate a destructor

    //no copy assignment operator=
    //the compiler does not generate a copy assignment operator
    //no any operators

    bool is_ok()const; //OK, no 'virtual' required, it's pure virtual by default (because INTERFACE)

    virtual unsigned int64 peek( void* , unsigned int64 size ); //OK, no '=0' required, it's pure virtual by default (because INTERFACE)

    protected: virtual void hidden(); //ERROR! no access specifiers are allowed! interface is always public
};
```
```
interface IEmpty
{
    //ERROR! Interface can't be empty! At least one method

    struct NestedStruct {}; //ERROR! no nested types
};
```
```
extern "C"
{
    #include <libc_function.h>

    typedef interface IFace { //ERROR! no 'interfaces' in 'C', unknown keyword
    } IFace;

    void* libc_function( void* userdata )
    {
        //try to cast
        OtherInterface* ptr = ((OtherInterface*)userdata); //ERROR! 'interface OtherInterface' is not 'extern C'

        typedef OtherInterface CInterface; //ERROR! 'interface OtherInterface' is not 'extern C'

        return( userdata ); //OK
    }
}
void main()
{
    OtherInterface* ptr;

    libc_function( ptr ); //OK, void* in 'C'

    ptr = static_cast< OtherInterface* >( libc_function(ptr) ); //OK, so bad with void* :(
}
```
```
class StringReadable :public IReadable {...}; //read data from string
class FileReadable :public IReadable {...}; //read data from file stream
```
```
IReadable* ptr = new IReadable(); //ERROR! interface can't be instantiated (even if it's an empty, without methods)
delete ptr; //ERROR! interface can't be deleted
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
EmptyReadable* someptr;
delete someptr; //OK, delete class
```
```
static_cast< IReadable& >( EmptyReadable ); //OK, cast derived 'class EmptyReadable' to base 'interface IReadable'
```
```
class FileStream :public IReadable, public IWritable {...}; //OK, multi inheritance
void copy( IReadable& , IWritable& ); //OK, use interfaces; pure abstractions
copy( FileStream() , MemoryStream() ); //OK, create temp objects on stack (no 'const&' required)
```
```
[dynamic_cast] keyword
!! REMOVED !!
no down casting
```
___
## Override
```
[override] keyword
```
override virtual method from base class
same as C++, but...
```
class A
{
    virtual void dump( ostream& )const;
};
class B :public A
{
    override void dump( ostream& )const; //override at beginning, instead of virtual
};
class C :public B
{
    override dump( ostream& )const; //OK, no return value required, because base class
};
```
___
## Exceptions
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
## goto
```
[goto] keyword removed :)
```
```
but allowed in 'extern "C" {...}' for back-compat :(
```
___
## operator ? :
```
return( condition ? if_true : if_false );
```
```
return( condition ?: if_false ); //same as ( condition-value == true ? condition-value : if_false ), like 'elvis operator in Kotlin language'
```
________
## Literals
```
[constexpr] keyword
!! REMOVED !!
replaced by [literal] keyword
```
```
[constinit] keyword
!! REMOVED !!
```
```
[consteval] keyword
!! REMOVED !!
make compiler more smarter with operators '<', '==', '!='...
```
[literal] it's a static const readonly compile-time data
```
literal char Name[] = "My name"; //OK, fixed size const string
literal char* OtherName = nullptr; //OK, null address
```
```
void procedure( literal char* txt ); //literal only data in 'txt'
```
```
void func()literal; //literal only expressions in functions; replace #define MACRO

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

        //static const <= literal; //OK
        //literal <= static const; //ERROR
    }
    else
    if ( PlatformBits == 64 ) //no 'literal' keyword required, because all statement is literal
    {
        return( 8 ); //OK, return literal 8
    }
    else //last 'else' is 'literal else' too!
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
literal int32 func() {
    int32 a = 32;
    return( a ); //ERROR! 'a' is not a literal

    literal int32 a = 32;
    return( a ); //OK
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
    else
    {
        //some non-literal stuff too...
    }

    literal if( func() == 0 ) { //OK, another if; no mixing
        //some stuff...
    }
    else
    {
        //some literal stuff too...
    }
}
```
___________________
## Literal expressions
```
template< typename A , typename B > void some()
{
    literal if (A == B) //check type(A) equal to type(B), WE NEED SMART COMPILER
    {
        literal char name[] = typename(A); //use 'typename()' keyword to GET typename as literal string by compiler
    }
    if (A == B) //ERROR! only compile-time if allowed with literals
    { ... }
    literal if (A < B) //check if type(A) is a base class for type(B)
    { ... }
    literal if (static(A)) //check if type(A) is a static
    { ... }
    literal if (virtual(A::~A)) //check if virtual destructor of type(A) (no std::super_puper_is_virtual_destrucotr< auto(typename..) something-something >::value)
    { ... }
    literal if (return(A::empty) == bool) //check if function A::empty return 'bool' type
    { ... }
    literal if (return(const(A::size))) //check if function 'A::empty' return a const type
    { ... }
    literal if (literal(some)) //check if function 'some' is literal
    { ... }
    literal if (class(B)) //check if type(B) is a class (not struct, not int, not bool, not interface)
    { ... }
    literal if (interface(A)) //check if type(A) is an interface
    { ... }
    literal if (! return(A::size)) //check if function (A::size) return no value
    literal if (return(A::size) == void) //same
    { ... }
    literal if (typename(A) == typename(B))
    { ... }
    literal if (A == variableA) //ERROR! type to value checking
    literal if (A == typedef(variableA)) //OK, use 'typedef()' keyword to deduce type of 'variableA'
    { ... }
}
```
____
## Static assertion
```
[static_assert] keyword
Performs compile-time assertion checking.
```
```
static_assert( [bool] );
check 'bool' expression and if 'false' - compilation error
```
```
static_assert( [bool] , [string] );
check 'bool' expression and if 'false' - compilation error with literal 'string' message
```
```
static_assert; //without parameters
always compilation error
can be used only inside 'literal if' statements
```
___
## Move constructor
```
class A
{
    A( A&& other ); //!! move constructor is fantastic !!

    this& operator= ( A&& ); //move assignment operator
};
void main()
{
    A somea;

    //do stuff

    A othera &&= somea; // 'operator &&=' call a move constructor (no 'static_cast< TYPE&& >' is needed); move constructor should be defined

    othera &&= A(); // 'operator &&=' call move assignment operator 'operator=(&&)'; 'move assignment operator' should be defined
}
```
___
## Typedef and decltype
```
[typedef] keyword
[decltype] keyword removed
```
same as 'C/C++', but...

template<class T> using type = .... //!! STOP USING 'using' aliases !!
```
typedef unsigned int32  size_type;

typedef template<typename T> MyTemplate<T,size_type>  MySizeTemplate; //OK, allow to 'typedef' use templates<T>

MySizeTemplate<bool> // MyTemplate< bool , size_type >
```
```
global scope
{
    namespace scope
    {
        typedef ...; //only visible in this namespace scope

        class scope
        {
            typedef ...; //only visible in class scope

            function scope
            {
                typedef ...; //only visible in function scope

                statemenet scope
                {
                    typedef ...; //only visible in statemenet scope
                }
            }
        }
    }
}
```
```
template<typename T> void some( T xx , T yy )
{
    std::vector< typedef(xx) > arr; //OK, use 'typedef()' keyword in template parameters to deduce type

    std::vector< typedef(xx::method) > vec; //OK, use 'typedef()' keyword to deduce return type from 'xx::method'

    std::vector< typedef(&xx::method) > pointers; //OK, use 'typedef()' keyword to declare pointer-to-member type

    std::vector< typedef(yy::attribute) > attrs; //OK, use 'typedef()' keyword to deduce return type from 'yy::attribute'

    std::vector< typedef(&yy::attribute) > attrs; //OK, use 'typedef()' keyword to declare pointer-to-field type
}
```
```
void func( typedef int32 p ); //ERROR!

typedef struct MyStruct {}; //ERROR! no 'typedefs' in struct declarations
struct MyStruct {}; typedef MyStruct  data_type; //OK

typedef class MyClass {} OtherClass; //ERROR! no 'typedefs' in class declarations
class MyClass {}; typedef class MyClass  OtherClass, *OtherClassPtr; //OK

typedef interface IFace {}; //ERROR! no 'typedefs' in interface declarations
interface IFace {}; typedef IFace  some_interface; //OK

extern "C"
{
    //OK, for 'C' back-compat
    typedef struct MyStruct {
    } MyStruct;
}
```
___
## Static cast
```
[static_cast] keyword
```
enums
```
enum MyEnum : int32
{
    //constants...
    E1=0,E2,E3,E4=4,
};
void main()
{
    MyEnum e = int32(0); //OK

    MyEnum e = int16(0); //OK

    MyEnum e = int64(0); //OK, but truncation from 'int64' to 'enum MyEnum:int32'

    e = static_cast<MyEnum>(0); //OK

    literal MyEnum e = int32(0); //ERROR! literal only
    literal MyEnum e = literal int32(0); //OK

    literal MyEnum e = literal int32(5); //ERROR! 'enum MyEnum' does not contains 'value 5' (4 is max)
    literal MyEnum e = literal int32(4); //OK, (E4 == '4')
}
```
no down casting
```
class A
{
};
class B :public A
{
};
void main()
{
    B bb;
    A& aa = bb; //OK, derived 'B' to base 'A'

    B& other = static_cast<B&>(aa); //ERROR! no down-casting
}
```
___
## Auto
```
[auto] keyword
```
```
no auto as return value
[auto function] with another [auto function] with another [auto function] with another [auto function]...horrible!!
```
```
no auto in function parameters
it's completely broke an idea of templates
```
but...
```
template<class T>
bool func( const T& container )
{
    auto count = some_count(); //but inside a function it's good!
    auto iter = container.cbegin(); //no horrible 'typedef typename T::iterator  iterator' or something like this
}
```
```
//C++:
auto (*p)() -> int; // declares p as pointer to function returning int
auto (*q)() -> auto = p; // declares q as pointer to function returning T, where T is deduced from the type of p

//now:
int32 (*p)();
typedef(p) (*q)(); //use [typedef()] keyword (instead of 'decltype') like normal function without ->[&(*...)]<-
```
```
function(...) => ...; //ERROR! =>
function(...) -> ...; //ERROR! ->
All types BEFORE function name
```
___
## Parameters type
```
void point( int32 xx , yy , zz ); // 'xx,yy,zz' is an <int32>
```
```
template< struct TIter , typename TPred >
void sort( TIter it , itend , TPred predic ); // 'it,itend' is an iterator <TIter>
```
```
//no ugly ( a:int32 , b:int32 )
```
___
## Namespaces
```
[namespace] keyword
```
! great for code organization !

```
namespace std
{
    //standart language library
    //any user extensions in this namespace is !! NOT ALLOWED !!
}
```
```
namespace stdext
{
    using namespace std;

    //custom namespace for user extensions; like 'boost' in C++
}
```
access specifiers in namespaces
```
namespace my
{
    public: //not required --public by default

        enum access_mode //enum in public section, any one can use 'my::access_mode'
        {
            read_only=0, write_only, read_write, num_modes,
        };

    private:

        template< typename A , typename B> void some(); //internal use only, only types in 'my' namespace can use this functions
        //any 'using namespace my' do not inject this function to your scope -- only public

    public: namespace foo //namespace always public, can't hide namespaces
    {
        class Other
        {
            Other()
            {
                my::access_mode; //OK, public access

                my::some(); //ERROR, function 'some' is private

                my::Range; //OK, 'class Range' is protected and 'class Other' inside namespace my
            }
        };
    }

    protected:

        class Range {...}; //class 'Range' by any type declared inside 'my' namespace or sub-namespaces
}
```
! namespace aliases is great !
```
namespace AA {
    namespace BB {
        namespace CC {
            void doit();
        }
    }
}
namespace SomeCC = AA::BB::CC;
void main()
{
    SomeCC::doit(); //call 'AA::BB::CC::doit()' function
}
```
```
inline namespaces
namespace //no name
{
}
```
___
## Static
```
[static] keyword
```
static classes/structures/interfaces
```
class NonDynamic static
{
    //prevent to create object dynamicly (operator new)

    //can't containt 'operator new/delete'
    void* operator new( memory_size ); //ERROR! static class
    void operator delete( void* ); //ERROR! 'delete' req 'operator new', but 'new' is not allowed because 'static class'

    //all derived types is static too
};

NonDynamic* obj = new NonDynamic(); //ERROR! 'class NonDynamic' is static

NonDynamic* obj = new (mem_placement) NonDynamic(); //ERROR! 'class NonDynamic' is static

NonDynamic obj; //OK, no dynamic

NonDynamic* ptr = &obj; //?? what about pointers to static classes/structs/interfaces ??
delete ptr; //ERROR! 'class NonDynamic' is a static; it can't be created dynamicly with 'operator new'
```
static methods\
bad name - method can't be a 'static', it's a function\
!! MINIMIZE STATIC 'METHODS' AS POSIBLE !! DONT WRITE 'C'-PROCEDURE CODE !!
```
class File
{
    static void read( File& f ); //ERROR! can't use class type as parameter inside this class

    void swap( File& f ); //OK, operator=, swap(), merge()
};
```
____
## Non-Copyable
```
?? 'noncopyable' keyword ?? less keywords
use 'true' keyword instead
```
prevent copy objects -- singleton only
```
//class X is non-copyable now
class X true
{
    X( const X& ); //ERROR! copy constructor is not allowed -- class X is non-copyable
    X( X&& ); //ERROR! move constructor is not allowed
    X& operator= ( const X& ); //ERROR! copy assignment operator is not allowed
    X& operator= ( X&& ); //ERROR! same
};

//struct F is non-copyable now
struct F true { ... };

//ERROR! not allowed in interfaces -- interface can't be copy
interface I true;

//class M is non-copyable now too!
class M :public X
{
    M( const M& ); //ERROR! 'class M' is non-copyable because base 'class X'

    M( X& other ); //OK, aggregation
    X& m_aggr;
};

void main()
{
    M obj; //OK

    M otherobj = obj; //ERROR! copy
}
M func()
{
    return( M() ); //ERROR! copy
}
```
____
## THIS
```
[this] keyword
```
[this] is now reference (not a pointer)
```
class A
{
    readonly std::string Username;
    A()
    {
        this.Username; //OK, no -> pointers

        this->Username; //ERROR! 'this' is not a pointer;

        //no 'delete this'
    }
};
```
secondary constructors
```
class My
{
    //primary constructor
    My( unsigned int32 , unsigned int32 )
    {
    }
    //secondary constructor
    My() :this(0,0) //use 'this' in initializators (Java style)
    {
    }
    //more secondary constructors (and more!!)
    My( const Minimal& mm ) :this( mm.calculate() , mm.calculate() )
    {
        //do not write 2500 functions
        //write 250 classes instead
        //more files - yes, but less code lines
    }
};
```
'this&' as return value
```
class Output
{
    Output( IWritable& );

    Output& operator<< (bool); //operator<<
    Output& operator<< (float32); //OK, operator overloading (static polymorphism)

    this& operator<< (int32) //OK, return 'this' type
    {
        //some print IWritable:write( int32 );

        //no return required, because 'this&' (no UB)
        return( this ); //but you can ('this' is reference, not a pointer)
    }
};
class MyOutput :public Output
{
    this& operator<< (const Class&); //OK, operator overloading, return 'this' type

    virtual this& write( const void* , stream_size ); //ERROR! no 'this&' allowed in virtual functions (yet)

    static this& get(); //ERROR! 'function get' is static, no object instance -- 'this' is not allowed
    static this& get( MyOutput& ); //ERROR! same
    static MyOutput& get( MyOutput& ); //OK
};
void main()
{
    MyOutput cout = some_writable_interface;

    cout << true; //OK, Output::operator<< (bool), return Output&

    cout << 5; //OK, Output::operator<< (int32), return MyOutput& because return 'this', but this is a 'class MyOutput'
    //equivalent to (static_cast<MyOutput&>( Output::operator<<( int32(5) ) ))
    //operator<<(int) called from cout, which 'class MyOutput' so 'this&' is a 'MyOutput&'

    cout << Class(...); //OK, MyOutput::operator<< (const Class&)
}
```
___
## Thread
```
[thread] keyword
dangerous keyword
```
```
thread(int32) PerThreadValue; //thread-local-storage variable
```
```
?? or use std::thread_local<int32> PerThreadValue ??
```
```
void main()
{
    thread
    {
        //corutine scope

        parallel();
    }
}
void parallel()thread //corutine function
{
}
```
____
## Enum
```
[enum] keyword
```
! great constant declaration !
```
//some enum
enum WindowMode //by default enum size equal to sizeof(void*) pointer (16 bits, 32, 64...)
{
    Fullscreen = 0, Borderless, Windowed,
};
//some enum with data type
enum FileMode : int32 //force to int32
{
    Create,Open,Append,Undefined,
};
void main()
{
    literal char name[] = enum(FileMode::Create); //OK, use 'enum' keyword to get literal string (table with strings generated by compiler)
}
void foo( FileMode n )
{
    literal char name[] = enum(n); //ERROR! 'n' is not a literal -- now 'enum' is a runtime-function to find string (if not found -- [""] empty string will be returned)

    literal FileMode otherenum = FileMode::Append;
    literal char name[] = enum(otherenum); //OK, 'otherenum' is a literal now

    std::string othername = "Append";
    othername = enum(FileMode::Append); //OK, literal string to std::string is allowed
    n = enum<FileMode>( othername.c_str() , FileMode::Undefined ); //OK, try to cast string 'othername' to 'enum FileMode'; if not found -- use Undefined as default

    //allow to use 'FileMode::Undefined' with scope '::'
}
```
```
enum Color
{
    ColorA,ColorB,ColorC,
};
enum Mode
{
    ModeA,ModeB,ModeC
};
void main()
{
    if (ColorA == Color::ColorB) //OK, compare two colors, enum scope 'Color::' is OK too
    {
        std::string = enum(ColorA); //OK, convert to string "ColorA"

        std::string = enum(Color::ColorA); //OK, convert to string "Color::ColorA"
    }

    if (Mode::ModeA == ColorB) //ERROR! different enum types
    {
    }

    if (static_cast<int32>(Mode::ModeA) == static_cast<int32>(ColorB)) //OK, check integers
    {
    }

    literal int32 DefaultMode = Mode::ModeC; //OK, enum constants is literals
}
enum class Other //ERROR! 'enum class' is removed
{
};
enum Condition :bool //OK
{
};
enum Value :unsigned int8 //OK, clamp to int8, int16, int32, int64
{
    MaxValue = 998811, //ERROR! value '998811' is too big for 'unsigned int8'
};
enum Range :float32 //ERROR! integers only
{
};
enum Custom :MyClass //ERROR! base integer types only
{
};
```
___
## Final
```
[final] keyword
!! REMOVED !!
```
```
Class-oriented language: we need to extends functionality of each class
This keyword is broke our idea
```
___
## Volatile
```
[volatile] keyword
!! REMOVED !!
```
___
## Template annotations
```
template< typename T , class C , struct S , enum E , enum EI:int32 , interface I , class CD:C >
void some()
{
    //type T - any

    //type C - any, but classes only

    //type S - any, but structures only

    //type E - any, but enums only

    //type EI - any, but enums only with 'int32' base type

    //type I - any, but interfaces only

    //type CD - any, but classes only with base class of 'C' (static_cast<C>( CB ))
}
```
```
template< typename T:std::vector >
void some( const T& )
{
    //type T is any type, but same as 'std::vector'
}
void main()
{
    std::vector<int32> arr;
    some( arr ); //OK, 'arr' is a vector type, call 'some< std::vector >()' overloaded function

    std::list<float64> ls;
    some( ls ); //ERROR! only 'std::vector' or 'std::map' based types

    std::map<char,void*> automap;
    some( automap ); //OK, call 'some< std::map >()' overloaded function
}
template< typename T:std::map >
void some( const T& )
{
}
```
literal template functions
```
template< class A , interface B >
literal bool some()literal
{
    return( B < A ); //check if interface 'B' is based type for class 'A'
}
```
mutable template parameters
```
template< typename T >
void some( mutable T& v ) //non-const, non-literal, non-readonly T&
{
}
```
___
## Auto deduce template types
```
no [auto] in return value
no [auto] in function parameters
need to teach compiler detect parameters
```
```
template< class A >
struct Command
{
    readonly A& Action;
    Command( A& act ) :Action(act) {;}
    Command();
};
template< class T >
void doit( Command<T>& cmd )
{
    cmd.Action.doit();
}
void other_doit( const Command<MyAction>& cmd )
{
}
template< class T >
void next_doit( Command<T>& cmd )
{
    doit( cmd );
}
void main()
{
    doit( Command<MyAction>( MyAction() ) ); //OK, 1) explicit type <MyAction>

    doit( Command( MyAction() ) ); //OK, if no explicit type, try to check constructor; 2) found typename in constructor Command<MyAction>::Command( MyAction& )

    other_doit( Command() ); //OK, if no constructor, try to check 'lvalue' in function parameter; 3) found const Command<MyAction>& in function other_doit;

    next_doit( Command() ); //ERROR! 1) no explicit params, 2) no constructor type, 3) 'function next_doit' don't know about parameter T == couldn't decuce parameter 'A' for 'struct Command'
}
```
```
std::vector<int32> collect();

void main()
{
    std::vector collection = collect(); //OK, 'collection' will be 'std::vector<int32>' as return value from 'collect()'

    std::vector otherarr; //ERROR! couldn't deduce template parameter 'std::vector<T>'

    std::vector vec = { 0 , 1 , 2 }; //OK, use typename from constructor (initializator list with 'int32') -- no 'vec' is a 'std::vector<int32>'

    //1) explicit parameters: high priority

    //2) second chance to deduce parameters from constructor

    //3) last chance to deduce parameters from 'l-value' type

    update( std::vector() ); //OK, deduce parameter <T> from 'update' function argument (create empty temp 'std::vector' on a stack)

    update( std::vector<float32>() ); //ERROR! different types 'std::vector<float32>', but function 'update' req 'std::vector<int32>'
}

void update( std::vector<int32>& );
```
```
template< sturct TCont >
std::map< int32 , std::string > filter( const TCont& container );

void main()
{
    std::map ff = filter( std::list({"apple" , "avocado" , "banana"}) );
}
```
___
## Lambda
no lambda functions/expressions\
no [lambda] keyword\
it's right-way to procedure code\
lambda inside lambda inside lambda inside lambda...\
this is a functional programming, not a class-based
___
## VOID pointers
Pandora's Box in class-based coding
```
libc/unix use void*
winapi use void*
other c-libs use void* (script lua)
```
?? how to protect object memory (const/readonly/static) from [void*] abueses ??\
and we can't break 'C' compatibility :(
___
## Friend
```
[friend] keyword
!! MINIMIZE FRIENDS AS POSIBLE !!
```
problem:
```
class A
{
    friend class B; //now all stuff from 'class A' opened for 'class B', but need only one method for 'class B'
};
class B;
```
solution:
```
class A
{
    friend(class B) inline void friend_method()const mutable; //only this method is 'friend' for 'class B'
};
```
literal expressions
```
void some()
{
    literal if (friend(A)) //ERROR! 'friend' check is not allowed; this is private :)
}
```
___
## Reflection
```
!! completely breaks encapsulation !!
!! NO REFLECTION !!
```
___
## Union
```
[union] keyword
```
It's destroy objects idea -- with union our objects it's just a memory :(
```
union
{
    int8,int16,int32,int64; //OK

    float16,float32,float64; //OK

    char[]; //OK

    void*; //OK

    interface*; //ERROR! no 'interface' allowed

    class*; //??ERROR?? if you need data struct, use 'struct' keyword, not a 'class'

    virtual(class); //ERROR! virtual classes (virtual destructor, with interfaces or virtual functions) is not allowed

    struct*; //OK, structs allowed

    struct const; //ERROR! const immutable type - not allowed
};
```
```
struct X const
{
    readonly int32 Value;
    X( int32 v ) :Value(v) {;}
};
//and now
union
{
    int32 B;
    X xx; //ERRO! constructor in union
};
//ok, then
union
{
    int32 B;
    char data[sizeof(X)]; //for placement new
};
X* ptr = new (data) X( 10 );
//now 'union:B' at same address as 'X:Value'
//'B' == 10
//'X:Value' == 10
B = 32; //OK, int32 is non-const, non-readonly
//now 'X:Value' == 32 !! READONLY value changed !! outside of 'const struct' :(
```
```
?? what about ??
extern "C"
{
    union //?? 'union' can be used only in 'extern "C"' sections ??
    {
    };
}
```
______________
## std::allocator
```
namespace std
{
    typedef unsigned int32  streamsize; //for x32, yes 'unsigned'
    typedef unsigned int64  streamsize; //for x64

    typedef streamsize  memorysize;

    interface allocator
    {
        //allocate memory block
        virtual void* allocate( memorysize ) =0;

        //free memory block
        virtual bool deallocate( void* , memorysize ) =0;

        //reallocate memory block from 'allocate' method
        virtual void* reallocate( void* , memorysize ) =0;

        //try to calculate size of block
        virtual memorysize size( const void* )const =0;
    };

    static allocator& malloc; //global heap allocation
}
```
____
## While loop
```
[while] keyword
```
same as C, but...
```
void method()const
{
    while //OK, no condition and no brackets; same as 'while( true )'
    {
        ++infinite;
    }

    do
    {
    }
    while(); //ERROR! with brackets req a condition

    do
    {
    }
    while; //OK

    //no 'while(true)', but allowed

    //no 'for(;;)', but allowed
}
```
___
## Size OF
```
[sizeof] keyword
```
same as 'C', but...
```
struct MyStruct sizeof(32) //'struct MyStruct' should be a 32 bytes; if less/greather -- compiler error
{
};
class MyClass sizeof(16)
{
};
```
```
enum sizeof(MyEnum); //ERROR! not allowed

interface sizeof(MyInterface); //ERROR! not allowed
```
```
void main()
{
    const std::memorysize total = sizeof(MyStruct); //should be 32 bytes; return literal unsigned int32
}
```
___
## Align OF
```
[alignof] keyword
```
same as 'C++', but...
```
struct MyStruct alignof(4) //'struct MyStruct' aligned by 4 bytes
{
};
class MyClass alignof(8)
{
};
```
```
enum alignof(MyEnum); //ERROR! not allowed

interface alignof(MyInterface); //ERROR! not allowed
```
```
void main()
{
    const std::memorysize total = alignof(MyStruct); //should be 4 bytes; return literal unsigned int32
}
```
```
struct MyStruct sizeof(32) alignof(4) //OK, if 'sizeof' is valid for 'alignof'
{
};
struct BadStruct const sizeof(11) alignof(5) //ERROR! imposible achive 'sizeof' 11 with 'alignof' 5 -- change 'sizeof' or 'alignof'
{
};
```
___
## Static arrays
```
int32 numbers[] = { 1,2,3 };
```
same as C++, but...
```
void some( int32 limit )
{
    int32 numbers[limit] = {0}; //allow to create static arrays from variable like C; !! ONLY IN FUNCTIONS/METHODS !!
}
struct Data
{
    int32 Max;
    char Value[Max]; //ERROR! fixed literal size only

    char Other[sizeof(int32)]; //OK
};
std::memorysize calc( int32 limit )
{
    char text[limit] = {0};
    limit = 0;
    return( sizeof(text) ); //ERROR! 'text' is not a static array, because use variable 'limit'
}
std::memorysize calc_correct( literal int32 limit )
{
    char text[limit] = {0};
    limit = 0; //ERROR! 'limit' is literal
    return( sizeof(text) ); //OK
}
```
___
## Return
```
[return] keyword
```
same as C/C++, but...
```
class MyClass
{
    this& print() {
        //do stuff...
        //return is not req
    }
};
int32 main() :return(int32 exitcode = 0) //declare auto variable
{
    //do stuff

    //will return 'exitcode' variable, no UB
}
float32 func( float32 v ) :return(v) //by default return value 'v'
{
    v = std::sqrt(v);

    if (v == 0)
    {
        return; //stop here and return 'v' parameter
    }

    v *= v;

    //will return 'v' parameter, no UB
}
int32 other()
{
    if (false)
    {
        return( 0 ); //OK, function return int32
    }

    //ERROR! function must return value
}
```
```
literal if (return(Class::method))
{
}
else literal if (return(Class::method) == void)
{
}
else literal if (! return(Class::method))
{
}
else literal if (return(Class::method)) == const int&)
{
}
else literal if (sizeof(return(Class::method)) == 4)
{
}
```
___
## Operator delete
```
Class* p;
delete p; //call destructor ~Class(), and free memory
```
but now...
```
0) ?? what about (p != nullptr) ?? auto check
1) check type (no interfaces)
2) call destructor ~Class();
3) free memory
4) write 'nullptr' to 'p' (p == nullptr after delete); memory is lost now, no address
```
___
## Functions annotations/attributes
```
[[nodiscard]] [[deprecated]] [[noreturn]] [[maybe_unused]] and other else are UGLY!
!! no attrubutes/annotations !!
```
___
## Removed keywords
```
alignas

and

and_eq

atomic_cancel

atomic_commit

atomic_noexcept

bitand

bitor

char8_t

char16_t

char32_t

compl

concept

consteval

constexpr

constinit

const_cast

co_await //use std::objects + std::functions

co_return //how corutine implemented ? what about corutine scope ?

co_yield

decltype

dynamic_cast

export

goto

noexcept

not

not_eq

or

or_eq

reflexpr

register

requires

synchronized

thread_local

throw

try

typeid

volatile

wchar_t //replace by 'wchar'

xor

xor_eq

final

transaction_safe

transaction_safe_dynamic

import

module
```
___
## namespace std
```
remove std::locale
```
