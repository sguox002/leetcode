briefs

- ood design pattern
creation:
abstract factory: an abstract class for each family, example furniture with different style
factory method: a virtual constructor, so derived class can alt the behaviour
singleton: return instance or the reference
clone (prototype)
builder: move complex constructor to a class, for example house builder.

structure:
adapter: from one data structure to another data structure
bridge: multiple different hierarchy and bridge (use one as member in another)
composite: tree structure of objects
decorator: add extra function 
facade: a simple interface to a complicated library
flyweight: share of common member to allow much more small objects
proxy: a service (cache et al) to implement original object's function

behaviour:
chain of responsibility: 
iterator
memento 
state
template
command
mediator
observer
strategy
visitor

- c++11/14 new features
c++11
- move semantics and rvalue
- smart pointers: unique_ptr, shared_ptr, weak_ptr
- initializer
- auto
- lambda function
- nullptr, constexpr
- range based for loop
- array and hashmap stls
- threads and supporting library
c++14:
- binary literals
- lamda capture initializer
- return type auto 
- chrono
c++17
- parallel stl.

job specific knowledges:
agile:
- short sprint,fast iteration
- scrum meeting: planning, daily, retrospective

IPC:
shared memory using lock, semaphore et al

multithreading:
mutex, condition-variable

memory safe:
- buffer overflow
- dangling pointers
- race condtion
- invalid page fault
- use after free
- uninitialized pointers (wild pointer)
- memory leak
- stack overflow
- double free or mismatched free

API and ABI compatibility
com, dll rule
ABI: inner parameter passing mechanism, variable naming, function naming, class et al.

ROS: maps, navigation, computer vision, artificial intelligiencc

TDD/BDD:

linux kernel
- concurrent
- device management
- runtime policy
- ipc + synchronization
- file system
- IO
- communication protocols

software practice (all stages)
design & architecture
requirement, concept, modular, interaction..

module development and deployment
- module function requirements
- its role in the system and added interactions.

improving performance
- better compiler, algorithm, library
- better data structure
- concurrency
- optimize memory management

benchmark

code size vs performance (for loop unrolling)

understand object construction destruction, reference passing et al
(use initializer instead assignment)

exceptions are expensive.

avoid runtime type check












