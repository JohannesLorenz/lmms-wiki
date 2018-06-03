Everybody has their own preferred style for writing code, however, in the interest of making the codebase easier to maintain, we request that the following conventions be observed. Code that is lifted from other projects does not need to be modified to match these rules – no need to fix what isn't broken.

**• All indentation is performed using tabs.**  
Mixing tabs and spaces is evil. Please, please, please make sure your editor is configured to use tabs.  Instructions for QtCreator can be [found here](https://github.com/LMMS/lmms/pull/2033#issuecomment-98895801).

**• Try to limit line lengths to 80 characters.**  
Not an absolute requirement – sometimes longer lines can't be avoided. But it is a friendly thing to do.  

**• Add a space after `if`, `else if`, `else`, `for`, `do`, `while` and `switch`.**

**• Do not add a space after opening brackets or a space before closing brackets.**

**• Do not add a space before semicolons.**

**• Do not prefix header guard macros with an underscore.**  
Header guards should not begin with an underscore. Identifiers that begin with an underscore + capital letter are reserved identifiers in C++ and their usage should be avoided. If you edit an older file which contains an improper header guard, please fix it to comply with guidelines.  

**• Include file lists should be grouped by type (system include or local include) and then sorted by name.**  
The exception is that in a source file, the first included file should always be its header.  
Example:
```c++
MySourceFile.cpp:
#include "MySourceFile.h"

#include <QMap>
#include <QString>

#include "DataFile.h"
#include "Engine.h"
#include "GuiApplication.h"
```

**• All kind of types (class names, enums, structs) begin with an upper case letter.**  
Example:

```c++
	class ResourcesDB;
	enum MyEnum
	{
	   ListItem1,
	   ListItem2,
	   ...
	} ;
	typedef QList<AutomatableModel *> AutoModelList;
```

**• Variable and method names begin with a lower case letter.**  
Example:
```c++
	void doThis(int a);
	int myLocalVariable;
```

**• Member variables are prefixed with "m_".**  
**• Static variables are prefixed with "s_".**  
Examples:
```c++
	float m_tension;
```
```c++
	static int s_quantization;
```

**• Function parameters are _not_ prefixed with '_' anymore.**  
Example:
```c++
	void clearS16Buffer(outputSampleType* outbuf, Uint32 frames)
```

**• Infix operators ('=', '+', '-', '*', '/', etc.) should have a space before and after them.**  
Example:
```c++
	sub_note_key_base = base_note_key + octave_cnt * NOTES_PER_OCTAVE;
```

**• `if`, `else if`,`else`, `for`, `do`,`while` and `switch` should use explicit blocking.**  
Example:
```c++
	if (m_sample > 0)
	{
		--m_sample;
	}
```

**If you can comfortably fit the block on one line, then it's acceptable to format it like this:**  
```c++
	if (m_sample > 0) {--m_sample;}
```
but note that the braces must always be included.

**• Return without parenthesis.**  
Example:
```c++
	int foo()
	{
		return bar;
	}
```

**• Standard true/false constants instead of C-style one.**  
Example:
```c++
	if (a == 1)
	{
		b = true; // OK
		c = FALSE; // use false instead
	}
```

**• Ternary operators: generally, you should only use them when it makes sense, when it makes the code more streamlined or more readable. If you have to use long ternary expressions that don't fit on one line, they should be formatted like this:**
```c++
	a == condition
		? value
		: otherValue;
```
However, if the expressions are very long or convoluted, consider using if/else blocks instead.

In general, please make a bit of an effort to try to keep things looking the way they already do. Individual creativity is good, but coordinated creativity gets things done faster.