# preparation for behaviour interview

## refresh work experience.
medical imaging software:
role: software project leader
- functional analysis and performance requirement
- design software architecture, modularize and guideline for software practice.
- prototyping, feasibility and build the essential modules
- define modular interfaces, ood design.
- assign modules to engineer according to the level and expertise.
- implementatition and check progress and following the guidelines
- unit test and integration test

work:
design and implement the core: multithreading
design and implement dsp communication module:
design config module
design and implement graphics system
design and implement file system
design and implement dicom system
design b mode processing
design and implement cfi data processing
design and implement pw data processing
design cw data processing
maintain 3d/4d/panaroma
design and implement serial port interface to microcontroller
bug fixing and streamlize the team work.


underwater imaging:
role: project manager
- requirement to functional and performance
- concept to modular
- prototyping signal processing, image processing
- supervise software, installation calibration
- design realtime schedule
- implement data processing 
- coordinate field testing, mechanic

adcp poduct line:
- early stage prototyping (modeling and simulation)
- implementation signal processing in dsp;
- firmware bring up and supporting utils
- coordinate with software engineer for the system.
- design, customize product line.
- customer support.
- bug fixing and maintenance.


## understanding interview questions and what they are looking for.

A behavioral interview focuses on asking questions to identify past behaviors and successes that closely relate to situations you’ll encounter in the job for which you have applied. This style of interviewing is used by most employers.

behavioral is generally story telling using STAR method.

Amazon 14 leadership principle is the most important guidelines.

Generally they are looking for team work, leadership, conflicts, problem solving. (Amazon emphasize also customer obsession)

### team work: demonstrate skills to interact with team member.
- communicate effectively and convey idea and image clearly
- work with people with different background, respect, tolerate and professional.
- be supportative
- open to new ideas
- commitment: responsive, support, make right decisions, earn trust.
- deal with conflict professionally (see conflict part)
- deliver results.
- leading role:
	mentoring
	- have a understanding of each one's skillset, strength and weakness
	- develope the best: more responsibility and visibility for top performers
	- assign work: challenging but not beyond
	- clear path of career improvement for each member.
	- each one work toward the same goal and informed.

related questions:
- how to work with engineers from other division, hardware, mechanics, business
	- have a big picture and also sufficient details
	- different perspective and convey the information of concern
	- different skill, familarity, expertise.
- how to work with engineers with different background
	- use languages they are familiar
	- make requirement, time, effort, interfaces clear.
	
- how to deal with customer or difficult customer
	- get customer feedback and work backward
	- analyze customer requirement and align to our product
	- negotiate and prioritize
	- get resources and get work done.
- how to mentor junior/senior level engineers? management style et al
	- give top performer more responsibility. given suggestions to improve.
	- development plan for junior engineer, assigning work challenging but not beyond their ability, build tr confidence, skills, trust and align to our team success.
	- for senior engineer: more abstract guidelines, for junior engineer, more concrete guides and help them achieve success.
	- evaluate performance and progress regularly and give feedback.

### leadership: probably the most frequently asked.
leadership is important for engineer role to ensure achieving the desired and and satisfactory results. To bring engineer with different specialities together and align with the same goal.
also management style.

How to develop leadership skills?
- meet all specifications (leaders shall have a big picture and sufficient details)
- deliver the project on time (commitment)
- stay on budget (time, resources are all budgets): frugal, get work done using minimal resources
- performance quality control (project management: insist the highest standard)
- make right decisions a lot (earn trust, learn from failure)
- ownership: go beyond your task.
- initiative: do work without asking or reminding
- insist highest standard: long term interst vs short term interest


make decisions:
- small technical decisions, make a call and move fast forward (need to stand a higher level and has a big picture and enough details)
- decisions will affect days, need to balance and trade off
- decisions will affect weeks and significant, need discuss with business leader.

relate question:
- management or leader style
- your strength or weakness
- mistakes you made
- biggest achievement
- go beyond your task 

### conflict
- avoid emotional to get in way of work, keep professional
- someone maybe emotional due to specific reason, try to solve the problem and help them.
- use data/proof to convince.
- look for 3rd party perspective and be more objective.
- conflict sometimes are just different perspective, respect their value input and make our thinking more sound.
- focus on behavior and events, not personality (do not take it personal and move on)
- listen carefully to other side's opinion
- identify points of agreement and disagreement
- prioritize the area of conflict
- develop a plan to work on each conflict.
- 

### problem solving: for engineers problem solving ability and methodology is important for the performance.

- systematic approach for a problem
- ethic and altitude: initiative, creativity, resource, analytical, persist, result oriented.
- open minded, open to new ideas, new skills and be able to make judgement.
- calculated risk and forseen work and possible outcome.

engineering: divide and conquer approach: divide into small tasks, and solve them and put them back.
- how to approach a problem with considerable complexity
- identify problem scope and constraints (resources available)
- dealing with tradeoffs.
- define problem scope and requirements
- define functional requirements
- define performance requirements
- break down into several parts and interaction of them 
- prototype and study for possible obstacles.


## stories from past working experience

story 1: (work under pressure, problem solving, beyond ability, out of comfort zone)
situation or task: asked to develop the rph installation offset to minimize tracking error.
situation: less experienced, complicated math and rigid body mechanics, optimization. Supervisor gives a lot of pressure.
action: learn rigid body mechanics, divide into different steps (coding the maths, optimization algorith, and data verification, each step ensure correctness). Ask questions frequently even with high pressure and clarify the goal and procedure)
result: struggle and get it correct.
lesson learned: divide and conquer, over pressure can slow down, task beyond ability is not good.


story 2: (management, align to same goal..)
situation: engineers with different skills and languages, experience constraints.
action: choose c++/MFC, define interface and modules, setup coding rules, tutoring, active code review
result: concise, reusable and clear architect, limit problem spreading, member grows fast

story 3: (open to new idea, conflict, creative)
task: develope touchscreen for our medical imaging software
situation: conventional approach: independent computer+software+IPC via serial port, novel approach: browser based web page + IPC
action: discuss the advantage/disadvantage, did some prototype, learn new skills
result: delivered high quality, easy to change, easy to implement product and learned new skills

story 4: (conflict solving)
task: develope 2d/3d graphics for our underwater side-scanner
conflict: opengl vs vtk: argument vtk could make 3d easier, learning is hard, opengl learning easier, but shall implement by ourselves
action: did some research and discussion: 3d task is not that heavy and opengl is chosen.
result: learned opengl and expert, vtk is also used in our medical software.

story 5: (emotional control)
situation: build a auto test script to test all combination for our medical system. A chip is burned. Hardware engineer blame my misuse and I insist that bad design of hardware.
action: step back and think that he might be correct in another perspective, also hardware engineer realized the defect and we decide to improve both the software test and hardware reliability.

story 6: 
situation: two members quarrels and are unhappy.
action: take initiative to stop it and try to understand the problem. analyze the disagreement and mentor the junior that be professional and both perspectives have valid information and the combination makes a more complete image. I advice both step back and be calm and they both appolgize and get my solution.

story 7: (leadership, mentoring, develop the best)
situation: touchscreen with hundreds of buttons. The top performer generate code for each button with thousands lines of code using tools.
action: I recommend: divide the buttons into categories and make one function for each type, which is easier for software interface and also reduce code. I pointed out that sometimes thinking more, organize, concise and reusable code could be more valuable
result: engineer grows fast with focusing more on thinking.

story 8: (initiative, go beyond, insist highest standard, ownership, thinking long term interest)
situation: complains about fpga bittrue, hard to get it right, takes a lot of time.
action: identify the problem: complexity, too many binary mysteries (tables, registers), bugs in performing fpga functions. 
decide the revise: each version could takes week and slow down process and waste resources
I volunte to revise it from scratch, and divide major steps, and collect snapshot of fpga configurations and dump as human-readable format, and dump each step data for auto comparison. And put the tool in git to track different version vs fpga version et al.
result: documented test, 1 minute with no effort

story 9: (customer obsession, work backward, go beyond)
situation: customer request to add a factor to adjust our discharge measurement results
action: 
communicate with customer and reason for this is lower and higher than actual. (biased)
asked data and did some research, deviated up to 20%
estimation comes from top layer, bottom layer and left and right which is not measurable
apply industrial often used models for the estimation
result: get stable results. and customer is satisfied without adding the factor.

story 10: (working against deadline, customer obsession)
situation: customer wants to bid using our discharge equipment, but far away. 
communication: major problem: quality problem, functional requirement on software
problems: sampling too slow and makes large deviation.
propose: eliminate average, single ping single output, minmal processing in equipment and move computation to software. - improve samping rate, - flexibility and easy debug - speed delivery speed
- divide task and ask resources, prioritize (core is the scheme change)
- ask customer to do test and feedback
- won the bid.

story 11: (ownership)
- clean up the hardware interface which spreads bugs
- tradeoff : think long term, quarantine bugs

story 12: (ownership, insist highest standard)
situation: repeated problem with configuration of our medical imaging modes
action: take action to build a tool to quickly locate the wrong configurations
result: speed up the problem location and greatly save the time and effort.

story 13: (mistakes, learn from failure)
situation: debugging and add features in xdma driver.
problems: a totally new field which requires tremendous learning and effort, progress not as expected.
lesson learned: estimation, calculated risk, more research in new field.

story 14: (biased for actions)
task: develope DICOM to interface with medical information network
situation: a complete standard with tremendous complexity (>7000 pages)
action: 
- roughly go over the standard
- search for 3rd party open source project
- priortize a tiny necessary part of functions
- start working with easiest functions and get more skilled
result: implement about 10 essential functions and a complete minimal DICOM in a short time

story 15: (disagree and commitment)
- discharge scheme change
situation: boss insist on multiple ping average, I propose single ping approach
action: provide data processing
result:

story 16: decision with no data or little data
once a customer doing experiment on the sea. And call in that they are having a problem with our instrument and waiting for our support. According to the behavior, my intuition is that the recent wave option added may be the cause. Since they are not using it, I suggest them to explicitly disable it and problem solved.

story 17: (self criticize, and make improvements)
situation: boss is OK with GUI, (two different approaches with different style)
action: use one single approach to implement GUI and graphics. A lot of effort to improve the appearance and efficiecy

story 18: 
color doppler imaging algorithm revision: good enough but wants to be better.
spectrum algorithm revision
- revisit every step and make sure its role and what will not work
- redesigned the changing parameters.
situation: works good for most cases, but sometimes works fair.

story 19: (challenges, achievement, calculated risk, hard decision)
situation: need someone to manage the whole software team and make a successful medical product
challenges: 
- not much professional software engineers 
- engineer with experiences is not good for the success and efficiency (communication, approach, thinking way, management)
- calculated risk: lack of experience in architect, lack of experence managing such a big project. 
strength: good learner, good intuition, successful management on previous product, good decison and excellent OOD skill
- take the risk and keep learning, working hard 
- result: highly performant, a successful team and a successful product.

story 20. (failure or mistake)
situation: architect for medical software using data collection, processing, graphics and gui threads
problem: data comes too frequently, processing and graphics a burden, data less frequently, GUI operation is not smooth
identify problem: GUI on graphics is bound to data and cause problem
action: remove graphics thread and update graphics regularly

story 21: (weakness)
situation: not agree with boss's assignment of work, did not give out opinion and hurt career
lesson learn: talk in calm and discuss to get the interest of both the company and myself. Also reminder me that assigning work is a big deal and is important for teamwork

questions to ask to interviewer
main purpose to discover:
- what the strategy of the company 
- what the future of the company
- what is the management style
- how does the company value the most of the employee
- what is the growth area expected on this role
- what is the leadership style
- company financial
- company organization
- what is your expectation on the role.
















