# OOD design patterns

## Creational patterns

### abstract factory

Abstract Factory is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

polymorphism.
generally it includes:
- abstract factory class which defines the interfaces
- concrete factory (to implement one concrete type factory)
- use it when you deal with a family of related objects
example: furniture factory: sofa, char, desk
modern, victorial, classical.

similar design in our sonoquest:
- abstract interface (bmfm_render)
- concrete type (rect,fan fanfh)
- use the same abstract type.

### builder:
move the constructor to a builder class
to avoid: a lot of subclasses, or constructor with a long list (some may be unused at all) of parameters
The builder will provide a set of functions to build each part and return the constructed object.
Builder can have inheritance to include multiple different implementations.

similar design in our sonoquest:
bdwin to support multiple layout and components (for example, agc bar, ruler,...)
use a builder to get the different view on each instance.

such as build a house, it may have many configurations
- use a buildClass which implements all the house build functions
- use a director class to how to configure the house
idea: avoid complicated logic and hard-to-understand initializer.
for example the bdwin with many configurations.

### singleton:
only one instance of a class is allowed.

- private the default constructor to avoid new
- a static creation method as the constructor. (it calls the default constructor and saves in a static field. If the instance is there, just return it)

similar design in our sonoquest:
ethernet, xdma, serial port, usb et al all allows only one instance.
use GetXXX to return he new or old instance.
use lock to guarantee single access.

windows app generally has this mechanism to avoid more than one instance.

### prototype (also known as clone)
copy one object without knowing its data members.
delegate this work to object's method clone.
for example the render in our software. same render can be used in different window, clone then is used.
especially when an object is passed using base pointer and concrete type is unknown.

## Structural patterns

### adapter:
design a intermediate adapter class to convert output to another's input.
allowing two incompatible class to work together.

or convert format for different lib/class inputs.
for example stack, deque, queue et al.

### bridge:
split a large class or a set of closely related classes into two separate hierarchies: abstraction and implementation.

for example shape with color, there would be m x n combinations
but we can define shape class with m subclasses, and color class with n classes
abstraction layer, implementation class, this reduces to m + n classes.

another example: GUI and backend tasks on different platform.
generally split into two different hierarchy and use one as data member (composition instead of inheritance)
composition vs inheritance are often seen in ood design. Inheritance is important but sometime composition could be a better choice.
the connection between the two hierarchy via composition is called bridge.

### composite (also known as object tree)
tree structure.
a composite class will pass function to each of its object members which recursively forms a tree structure.
an order may contain products, boxes, a box may contain box and product...
tree structure and traverse to get information.
client only goes through all its data members, data member will recursively does the work (avoid coupling to complicated data structures)

### decortator (also known as wrapper.)
add some behavior to objects in wrapper class.
when you need alter an object's behaviour.
similar to person wearing multiple clothes, clothes are not part of the person and can take off any time.
The wrapper shall implement the same interface as the base.
for example:
data can be from:
file
serial IO
enet IO
...
and they can be all treated the same using wrapper.

### facade:
a simple interface to a series moving objects
only provide the necessary information where the customer cares.

add a interface class to interact with the sophisticated library.

### flyweight (also known cache)
save common data in different objects instead to keep each data in each object.

### proxy (代理）
Proxy is a structural design pattern that lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.
a credit card is a proxy on payment.
a lazy operation can also add a proxy
a proxy will have the same interface as the original and perform similar action and return to caller. The proxy will contact the original object later.

especially for services from time to time, the server is up all the time, wasting a lot of resouces (like cache to avoid repeatedly download the same)

access control: for example give acess control to a special group.

local execuation of a remote service.

Proxy is sometimes an important OOD.

## behaviour patterns
behaviour patterns are concerned with algorithms and the assignment of responsibilitites among objects.


### chain of responsibilitites
a list of chains - classes
MFC windows message processing using this mechanism
The request is either processed or passing to its base.
the handler even can convert it to another request and pass to its base.

chained handler:
each handler contains a method to process the data and linked list to the next handler.
each handler pass the processed data to next handler.

### iterator:
traverse elements of a collection without exposing its underlying representations.
similar in sonoquest: using list::iterator to traverse the list of objects.

### memento (snapshot)
save and restore the previous state of an objec without revealing the details of its implementation.

each object can store its state called momento (create the meta data of the object). We only interact with these momentos instead of original object (to protect the privacy).

### state: finite state which works differently
there are quite a few such state machines, such as the usb communication protocol.
to implement finite state machine, we use some condition to switch. If more states to be added, there need a lot of revisions.
create new classes for all possible state of an object and extract all state-specific behaviour into these classes (context classes)
Implementing state-specific behavior directly within a class is inflexible because it commits the class to a particular behavior and makes it impossible to add a new state or change the behavior of an existing state later independently from (without changing) the class. In this, the pattern describes two solutions:

Define separate (state) objects that encapsulate state-specific behavior for each state. That is, define an interface (state) for performing state-specific behavior, and define classes that implement the interface for each state.
A class delegates state-specific behavior to its current state object instead of implementing state-specific behavior directly.
not very clear about it yet.

define a base class state (its action virtual, a pointer to context)
context class: has a reference (pointer) to concrete state object.
for example the power saving mode for sq860 software.
context using polymorphism to complete the task by the state.

a 5s query will drive the state change.

```
class PMM_context;
class PMM_state{
	PMM_context* context;
public:
	void set_context(PMM_context* c){context=c;}
	virtual int action1()=0;
	virtual int action2()=0;
};

class PMM_context{
	PMM_state* state;
public:
	PMM_context():state(0){}
	PMM_context(PMM_state* s):state(0){transition_to(s);}
	~PMM_context(){delete state;}
	int action1(){return state->action1();}
	int action2(){return state->action2();}
	void transition_to(PMM_state* s){
		if(state) delete state;
		state=s;
		state->set_context(this);
	}
};

//implement a class for a concrete state
class PMM_state1:public PMM_state{ //
public:
	int action1(){}
	int action2(){}
};

class PMM_state2:public PMM_state{ //
public:
	int action1(){}
	int action2(){}
};
```


### template method:

defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.
this is very similar to polymorphism.
matrix for different type of data.

### command (action, transaction)

turns a request into a standalone object, letting you parameterize methods with different requests, delay, queue, undoable requests.
Always include these information: command, receiver, invoker, client
for example the UI and business logic, separate them.
example in sonoquest:
the touchscreen and controlpanel separate with the business logic after the UI.
the commands are stored in a queue and is processed accordingly.
resterant ordering procedure is a command pattern.

In this pattern, an abstract Command class is declared as an interface for executing operations. This Command class defines a method, named execute, which must be implemented in each concrete command. This execute method is a bridge between a Receiver object and an action. The Receiver knows how to perform the operations associated with a request (any class may be a Receiver). Another relevant component in this pattern is the Invoker class, which asks for the command that must be executed.

### mediator: intermediary, controller.
reduce chaotic dependencies between objects. The pattern restricts direct communication between objects and force them collborate via mediator.
for example: a dialog has multiple components, they all talk to the dialog instead they communicate each other.

example: air traffic controller.

### observer: event subscriber, listener
subscription mechanism so that an event can be notified to all subscribers.

### strategy
Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.
for example: find the route using car, public transportation, walker, bicycle et al.

### visitor
separate algorithm from the objects on which they operate
new behaviour into a separate class (visitors) and pass the request and the object to the visitor class.

design pattern focuses on good style design and implementation
- inherit good code instead revise
- separate and divide into layers
- design to satisfy restrictions
- hiding implementation details..


