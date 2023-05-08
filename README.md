Download Link: https://assignmentchef.com/product/solved-simulation-of-a-very-simple-computer
<br>
Introduction

For this assignment, you will build a complete simulation of a very simple computer. Being simple, our computer only has 100 slots for both memory and instructions and contains just a single register (also known as an <em>accumulator</em>). However, in spite of this, we shall see that our simple computer is fairly capable.

Commands are sent to our computer using <em>words</em>. A word is a four-digit number, e.g. 0000, 0001, 0002, etc. Each word has two components. The first component, contained within the first two digits, indicates the word’s operation code (e.g. read, write, etc.). The 3rd and 4th digit represent the 2nd component, which specifies a specific memory address, thereby giving us 100 slots to store words. Execution of a program is guided by an <em>instruction pointer </em>that “points” to the next word to be executed. Unless a JUMP command is encountered, execution continues in linear order, just like a C++ program. Below are all of the possible operations that our simple computer can perform:

<strong>Operation Code Explanation </strong>




<strong>00 – </strong><strong>NULL </strong>

Does nothing (null instruction).




<strong>10 – READ </strong>

<strong>20 – </strong><strong>LOAD </strong>

<strong>30 – </strong><strong>ADD </strong>

<strong>32 – </strong><strong>DIVIDE </strong>

<strong>40 – JUMP </strong>

<strong>42 – JUMPZERO </strong>

Task

Your task for thissuccessfully interprets our simple computer’s machine code. Second, you must implement an interactive debugger that allows programmers to step through the execution of a program. I illustrate how each of these tasks work in the screenshots below.

Read a word from the command line into a specific location in memory.

Writes a word from a specific location in memory to the command line.

Load a word from a specific memory location into the accumulator register.

Stores a word from the accumulator into a specific location in memory.

Adds a word from a specific location in memory to the word presently in the accumulator. The result is stored in the accumulator.

Divides the value presently stored in the accumulator by the word located in a specific memory location. The result is stored in the accumulator.

Alters the value of the INSTRUCTION POINTER to the specified memory address.

If the accumulator’s value is presently zero, alters the value of the INSTRUCTION POINTER to the specified memory address.

<strong>11 – </strong><strong>WRITE </strong>

<strong>21 – </strong><strong>STORE </strong>




<strong>31 </strong><strong>– </strong><strong>SUBTRACT </strong>

Subtracts a word located in a specific location in memory from the value presently stored in the accumulator. The result is stored in the accumulator.




<strong>33 – MULTIPLY </strong>

Multiplies a word located in a specific location in memory by the value presently stored in the accumulator. The result is stored in the accumulator.




<strong>41 – JUMPNEG </strong>

If the accumulator’s value is presently negative, alters the value of the INSTRUCTION POINTER to the specified memory address.




<strong>43 – </strong><strong>HALT </strong>

Indicates the end of a program (execution terminates).

assignment are twofold. First you must write an emulator for our computer that

Sample Output – Loading a Program from a File

Below, I’ve run the FindLargest code (provided in a section below). This program prompts the user for two integers and displays the largest of the two. In this example, I input the values 3 and 4 and have the value 4 returned to me.

Sample Output – Interactive Console

In these screenshots, I enter the “Find Largest” into the interactive console.










Sample Implementation

My implementation contains a Computer class and several supporting Instruction classes (one for each instruction supported by the Computer). Below are my UML diagrams and an explanation of key functionality:




I use Instruction to represent an instruction supported by the computer. As such, I’ve created a child class for each instruction (approximately 13 sub classes) and a factory to handle new object creation. Also note that I’ve created an enum called Operation_t that acts as an easier way to work with operations (i.e. using Read, Write, Load, etc. instead of 10, 11, etc.). Other than that, it’s mostly getters and setters with the exception of the virtual execute method:

virtual int execute(int &amp;accumulator, Indexed&lt;Instruction *&gt; &amp;memory)

This function executes the given instruction. It’s virtual because different instructions will execute differently. For example, store will store the value in the accumulator into the memory address property of the Instruction. My execute method returns the location of the next instruction to the caller.

Computer Class

The Computer class is a little more straightforward. It has a vector of Instructions that act as main memory (note that instructions are stored in memory), a flag to determine if the program has finished (_has_halted), an integer to indicate the next instruction to execute (_instruction_pointer), and a single accumulator. The most interesting aspect of Computer is the executeNextInstruction method:

void executeNextInstruction()

This function uses the instruction pointer to pull out the next Instruction to execute and calls that Instruction’s virtual execute method. Note that Computer passes in a reference to both its accumulator and memory to execute(). Also note that I use the integer returned by execute() to update the Computer’s instruction pointer.




Sample Programs

To give you a better sense of how our computer executes programs, I’ve included the following basic programs:

Program #1: Add Two Numbers

The following code will ask the user for two numbers, add them together, and then output the result to the screen.

01 Declare variable B

Read from terminal into A

03 Read from terminal into B

Load A into accumulator

05  Add B to accumulator value

Store accumulator into A

07     Write result (A) to the screen

Program #2: Find Largest

This program will output the largest of two integer values obtained from the user

01 Declare variable B

Read from terminal into A

03 Read from terminal into B

Load A into accumulator

05      Subtract B from accumulator value

07 Write variable A back to the screen

Jump to instruction 10

09     Write variable B back to the screen

<strong>Memory Command Location </strong>

<strong>Comment </strong>




00

0000

Declare variable A




0000




02

1000




1001




04

2001

3001

06

2100

1100




08

4300

End of program




<strong>Memory Command Location </strong>

<strong>Comment </strong>




00

0000

0000

Declare variable A




02

1000

1001

04

2001

3101




06

4109

Jump to instruction at location 9 if accumulator is negative




1100

08

4010




1101




10

4300

End of program




Program #3: Fibonacci Sequence

This program calculates the Nth Fibonacci number. Note that this program assumes that the user is interested in at least the 3rd Fibonacci number.

<strong>Memory Command Comment Location </strong>

00 0002 Declare variable A – used to store N-2 value




01 0001 03 0000

05 0001 07 2003

Declare variable B – used to store N-1 value

Declare variable D – used to determine fib value to find

Will always represent the value 1 Load fib value into accumulator




02    0001  Declare variable C – used to store N value




04    0000  Declare variable E – used as a loop counter




06    1003  Get fib value from the user




08 3100 Subtract 2 from accumulator to account for assumption that we are getting at least 3rd fib

number.




09 2103   Store result back into memory

11 2001   Load N-1 value into accumulator

10  4223 While we still have values to calculate…




12    2100  Store accumulator into N-2 value

13 2002

15 3000

Load N value into accumulator

Add N-1 value to accumulator




14    2101  Store accumulator into N-1 value




16   2102   Store accumulator into N value

17  2004 Load counter into accumulator

19  2104 Store updated value back into counter

21  3104 Subtract loop counter

18   3005   Add one to accumulator (i.e. counter)

20  2003 Load fib value into accumulator




22  4010 Jump to start of loop

.      23  1102.      24  4300Write current fib value (N) to the screen Program complete.

Header Comment, and Formatting

<ol>

 <li>Be sure to modify the file header comment at the top of your script to indicate your name, student ID, completion time, and the names of any individuals that you collaborated with on the assignment.</li>

 <li>Remember to follow the basic coding style guide. For a list of basic rules, see my website or examine my example files from previous assignments and labs.</li>

</ol>

Reflection Essay

In addition to the programming tasks listed above, your submission must include an essay that reflects on your experiences with this homework. This essay must be at least 350 words long. Note that the focus of this paper should be on your reflection, <strong><em>not </em></strong>on structure (e.g. introductory paragraph,

conclusion, etc.). The essay is graded on content (i.e. it shows deep though) rather than syntax (e.g. spelling) and structure. Below are some prompts that can be used to get you thinking. Feel free to use these or to make up your own.

Describe a particular struggle that you overcame when working on this programming assignment.Conversely, describe an issue with your assignment that you were unable to resolve.Provide advice to a future student on how he or she might succeed on this assignment.Describe the most fun aspect of the assignment.Describe the most challenging aspect of the assignment.Describe the most difficult aspect of the assignment to understand.Provide any suggestions for improving the assignment in the future.

=============================================================Lab 2 Programming ChallengesFor lab 2, you must write computer instructions to solve the following challenges:

<ol>

 <li>Write a program that computes the average of seven numbers.</li>

 <li>Write a program that continually prompts the user for numbers until the user enters a negativevalue. The program then outputs the sum of these numbers.</li>

 <li>Write a program that uses a loop to reverse a list of ten numbers. The program then outputsthe reversed list to the screen.</li>

</ol>