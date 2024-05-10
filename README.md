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

*  void
    no data, used as return in functions
