C-style a general-purpose class-oriented coding language

//not programming language (programming == design, architecture)\
//not object-oriented (OOP it's about a messages [SMALLTALK], no types, no classes, no dots)

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

_____________________________
## The One Definition Rule (ODR)
```
"cannot have more than one definition by translation unit"
enum
function
class
interface
struct
typedef
```

_______________
## Header files
definitions
```
<myheader.ch>
```

_______________
## Module files
translation units
```
<mymodule.cc>
```

__________
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
```
//in future int128...maybe...
```

__________
## Structures
```
[struct] keyword
```
same a 'C/C++', but...

*  inheritance
```
only other struct can be a base class (no classes, no abstract, no interface...)
struct is just simple data in memory, it's not an OBJECT
```
*  no-virtual
```
can't contains any virtual code:
no virtual destructor //compiler error
no virtual base class (like classes, interfaces)
no virtual/pure virtual methods //compiler error
```
*  immutability
```
struct S const {...}; //[const] after struct name
can be an immutability type
```

_______
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
class C const {...}; //[const] after class name
can be an immutability type
```
*  virtual
```
allow to use virtual base classes
allow to use virtual methods, pure virtual methods
allow to use virtual destructors
```

____________
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

__________
## Mutability
```
[mutable] keyword
```
"C++": Field (attribute) of class can be changed in [const] method //bad name! method can't be a const, it's a something different
but...
```
class A
{
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
## Read only attributes
```
[readonly] keyword
```
!! STOP USING getters/accessors !!\
!! STOP calling(naming) methods as get*... !!

```
class Point2D
{public:

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
        posx = 10; //ERROR! 'posx' is a private

        Point2D::move( xx , yy ); //OK, public method

        posz = zz;
    }
};
```

__________
## Interfaces
```
[interface] keyword
```
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
    //interface can't be deleted
    //any template T->~T() or call IReadable->~IReadable() -- compile error
    //the compiler does not generate a destructor

    //no copy assignment operator=
    //the compiler does not generate a copy assignment operator

    bool is_ok()const; //OK, no 'virtual' required, it's pure virtual by default (because INTERFACE)

    virtual unsigned int64 peek( void* , unsigned int64 size ); //OK, no '=0' required, it's pure virtual by default (because INTERFACE)
};
```
```
interface IEmpty
{
    //ERROR! Interface can't be empty! At least one method
};
```
```
class StringReadable :public IReadable {...}; //read data from string
class FileReadable :public IReadable {...}; //read data from file stream
```
```
new IReadable; //ERROR! interface can't be instantiated (even if it's an empty, without methods)
delete IReadable; //ERROR! interface can't be deleted
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
class FileStream :public IReadable, public IWritable {...}; //OK, multi inheritance
void copy( IReadable& , IWritable& ); //OK, use interfaces; pure abstractions
copy( FileStream() , MemoryStream() ); //OK, create temp objects (no 'const&' required)
```
```
[dynamic_cast] keyword
!! REMOVED !!
dynamic_cast searching a dervied class...
how ?? by vtable ?? dont touch vtable !!
no down casting
```

________
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
__________
## Exceptions\
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
## goto
```
[goto] keyword removed :)
```
```
but allowed in 'extern "C" {...}' for back-compat :(
```

________
## Literals
```
[constexpr] keyword
!! REMOVED !! replaced by [literal] keyword
```
```
[constinit] keyword
!! REMOVED !!
make compiler more smarter with operators '<', '==', '!='...
```
```
[consteval] keyword
!! REMOVED !!
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

        //static const <= literal; //OK
        //literal <= static const; //ERROR
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
    literal if (A == B) //check type(A) equal to type(B), make compiler some smarter
    {
        literal char name[] = typename(A); //use 'typename()' keyword to GET typename as literal string by compiler
    }
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
}
```

________________
## Move constructor
```
class A
{
    A( A&& other ); //!! move constructor is fantastic !!
};
```

________
## Typedef and decltype
```
[typedef] keyword
```
same as 'C/C++', but...

template<class T> using type = .... //!! STOP USING 'using' aliases !!
```
typedef unsigned int32  size_type;
typedef template<typename T> MyTemplate<T,size_type>  MySizeTemplate;
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
    std::vector< typedef(xx) > arr; //OK, use [typedef()] keyword in template parameters to deduce type

    std::vector< typedef(xx::method) > vec; //OK, use [typedef()] keyword to detect return value from 'xx::method'

    std::vector< typedef(&xx::method) > pointers; //OK, use [typedef()] keyword to declare pointer-to-member typename
}
```
```
[decltype] keyword removed
```

____
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

__________
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

______
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
!! MINIMIZE STATIC METHODS AS POSIBLE !! DONT WRITE 'C'-PROCEDURE CODE !!
```
class File
{
    static void read( File& f ); //ERROR! can't use class type as parameter inside this class

    void read( File& f ); //OK, operator=, swap(), merge()

    void read(); //OK
};
```

____________
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

        //prevent 'delete this'
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

______
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

_____
## Final
```
[final] keyword
!! REMOVED !!
```
```
Class-oriented language: we need to extends functionality of each class
This keyword is broke our idea
```

________
## Volatile
```
[volatile] keyword
!! REMOVED !!
```

____________________
## Template annotations
```
template< typename T , class C , struct S , enum E , interface I >
void some()
{
    //type T - any

    //type C - any, but classes only

    //type S - any, but structures only

    //type E - any, but enums only

    //type I - any, but interfaces only
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

________________________
## Auto template parameters\
no [auto] in return value, no [auto] in function parameters\
need to teach compiler detect parameters
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

______
## Lambda\
no lambda functions/expressions\
no [lambda] keyword\
it's right-way to procedure code\
lambda inside lambda inside lambda inside lambda...\
this is a functional programming, not a class-based

_____________
## VOID pointers\
Pandora's Box in class-based coding
```
libc/unix use void*
winapi use void*
other c-libs use void* (script lua)
```
?? how to protect object memory (const/readonly/static) from [void*] abueses ??\
and we can't break 'C' compatibility :(

______
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

__________
## Reflection
```
!! completely breaks encapsulation !!
!! NO REFLECTION !!
```

_____
## Union
```
[union] keyword
```
It's destroy objects idea -- with union our object it's just a memory :(
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

    const(struct); //ERROR! const immutable type - not allowed
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
    union
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
__________
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
}
```

_______
## Size OF
```
[sizeof] keyword
```
same as 'C', but...
```
struct sizeof(MyStruct,32) //'struct MyStruct' should be a 32 bytes; if less/greather -- compiler error
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
    const std::memorysize total = sizeof(MyStruct); //should be 32 bytes
}
```

________
## Align OF
```
[alignof] keyword
```
same as 'C++', but...
```
struct alignof(MyStruct,4) //'struct MyStruct' aligned by 4 bytes
{
};
```
```
enum alignof(MyEnum); //ERROR! not allowed

interface alignof(MyInterface); //ERROR! not allowed
```
void main()
{
    const std::memorysize total = alignof(MyStruct); //should be 4 bytes
}
```

___
## Functions annotations
```
[[nodiscard]] [[deprecated]] and other else is UGLY!
```

___
## Remove other keywords
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

co_return

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
