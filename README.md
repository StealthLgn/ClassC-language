C-like a general-purpose class-oriented coding language

//not programming language (programming == design, architecture)\
//not object-oriented (OOP it's about a messages [SMALLTALK], no types, no classes, no dots)

___________________________\
"Header" files, definitions\
<myheader.ch>

_________________________________\
"Module" files, translation units\
<mymodule.cc>

__________
Data types\

*  bool\
    8 bits\
    accept only 1(true), 0(false) -- only one bit used

*  char\
    8 bits
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
  
*  signed int8\
  
*  unsigned int8\

*  int16\

*  signed int16\

*  unsigned int16\

  int32\
  signed int32\
  unsigned int32\

  int64\
  signed int64\
  unsigned int64\

  //in future int128...

  float16 //

  float32

  float64

  double32

  double64

  void //no data, used as return in functions
