Everybody has their own favorite "style" for writing code, however, in the interest of making the codebase easier to maintain, we request that the following conventions be observed. Code that is lifted from other projects does not need to be modified to match these rules – no need to fix what isn't broken.

**All indentation is performed using tabs.**  
Mixing tabs and spaces is evil. Please, please, please make sure your editor is configured to use tabs.

**Try to limit line lengths to 80 characters**  
Not an absolute requirement – sometimes longer lines can't be avoided. But it is a friendly thing to do.

**Do not prefix header guard macros with an underscore** 
Header guards should not begin with an underscore. Identifiers that begin with an underscore + capital letter are reserved identifiers in C++ and their usage should be avoided. If you edit an older file which contains an improper header guard, please fix it to comply with guidelines.

**All kind of types (class names, enums, structs) begin with an upper case letter**  
Example:

```
	class ResourcesDB;
	enum MyEnum
	{
	   ListItem1,
	   ListItem2,
	   ...
	} ;
	typedef QList<AutomatableModel *> AutoModelList;
```

**Variable and method names begin with a lower case letter**  
Example:
```
	void doThis( int a );
	int myLocalVariable;
```

**Member variables are prefixed with "m_"**  
Example:
```
	Knob * m_chordRangeKnob;
```
**Function parameters are _not_ prefixed with "_" anymore**  
Example:
```
	void clearS16Buffer( outputSampleType * outbuf, Uint32 frames )
```
**Infix operators (=, +, -, *, /, etc.) should have a space before and after**  
Example:
```
	sub_note_key_base = base_note_key + octave_cnt * NOTES_PER_OCTAVE;
```
**Open parenthesis "(" should be followed by a space and close parenthesis ")" should be preceded by a space, except for function calls without parameters**  
Example:
```
	if( ( _n->baseNote() && m_arpDirection == OFF ) || _n->arpNote() )
```
**`if`, `else`, `for`, and `while` should use explicit blocking**  
Example:
```
	if( m_sample > 0 )
	{
	       --m_sample;
	}
```
**Make sure, there're 4 lines of space between functions declarations**  
Example:
```
	void foo( int bar )
	{
		...
	}
	
	
	
	
	void bar( int foo )
	{
		...
	}
```
**Return without paranthesis (NEW)**  
Example:
```
	int foo()
	{
		return bar;
	}
```

**Standard true/false constants (NEW)**  
Example:
```
	if( a == true )
	{
		b = false;
	}
```

In general, please make a bit of an effort to try to keep things looking the way they already do. Individual creativity is good, but coordinated creativity gets things done faster.