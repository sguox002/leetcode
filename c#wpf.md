C#

- similar to java using .net as the virtual machine

- syntax similar to c++

- using packets as library

- can be used for desktop, mobile, web, games database

- no pointers, no memory management

- no .h file

- compiled instead of script

using: to import library

## language
- namespace: container for data and method.
- class

data type:
int, double, char, string, bool, float
long
in c#, char has two bytes.
bool has one bit

type casting: 
smaller to larger: implicit
larger to smaller: explicit

Convert.ToString/ToDouble/ToInt32/ToString.

IO:
console:
read/write
ReadLine/WriteLine

Operators:
arithmetic: +,-,*,/,%,++,--
assignment: =,+=,-=,*=,/=,%=,&=,|=,^=,>>=,<<=
comparison: ==,!=,>,<,>=,<=
logical: &&, ||, !,

math: 
min,max,sqrt,abs,round...

string:
.Length
ToUpper();
ToLower()
+
.Concat
Interpolation: $ {firstname}
[] to access element
IndexOf()
Substring()

statements
if...else
switch
while
for
break/continue
?:
array: string[]
loop through: foreach
Array.sort(input)
Linq: Min, Max,Sum...

methods:
static: class functions (belong to class not object)
deafult parameter:
overloading:

OOP: very similar to c++

The "Don't Repeat Yourself" (DRY) principle is about reducing the repetition of code. You should extract out the codes that are common for the application, and place them at a single place and reuse them instead of repeating it.

- default is private
- create an object using new
- public/protected.. applies to each member
- constructor (no destructor)
- internal: only accessible in internal class.
- property: set/get (much simplified even can use auto set;get;

Inheritance:
only public inheritance
sealed: no inheritance is allowed.

Polymorphism:
use the same base class type but new a derived class object.
similar to c++ but not using pointers

abstraction: similar to virtual in c++
abstract: to define an abstract method
override: to implement an abstract method in derived class.

interface: pure virtual class in c++
enum: similar to c++
file: using System.IO
AppendText, copy, create, delete, exists, readAllText, replace, writeAllText

exception: similar to c++

try...catch...finally
throw
ArithmeticException, FileNotFoundException, IndexOutOfRangeException, TimeOutException

WPF: windows presentation form
wpf is based on .net and used to create windows GUI
MFC: is just a wrapper of win32
WPF: in not just a wrapper, it contains managed and unmanaged code
It uses directX to render.
WPF separate the GUI and the business logics.

WPF features:
control inside a control
data binding
media services
templates
animations
alternative input
direct3d.

XAML:
in xml format
You can also create the GUI without xaml using c#
GUI elements in tree structure:
grid, stackpanel, button, listBox
logical tree and visual tree.

























