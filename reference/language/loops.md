# Loops

There are two different Loops in VL:

* Repeat: a classic for-loop with an _Iteration Count_ input to specify the number of iterations the loop executes
* ForEach: executes for each slice of a spread entering the loop via a splicer. 

In the NodeBrowser you'll find different nodes named Repeat and ForEach. To get these primitive ones, choose the versions written in _italic_.

*Image:Choosing Repeat or ForEach from the NodeBrowser*

## Getting data into a loop
There are 3 different ways of getting data into a loop. All of them work for both the Repeat and the ForEach loop:

### Direct Connection

Data can be linked directly into a loop which results in each of the loops iterations receiving the same data.

*Image:A direct connection into a loop region*

### Splicer

Splicers allow you to access consecutive slices of a spread in consecutive iterations of a loop. Entering a loop with a link via the splicer-bar that shows up when starting a link, automatically leads to each iteration of the loop receiving one slice of the incoming spread.

*Image:A spread connects into a loop using a splicer*

Multiple spreads can go into the same loop via splicers. In case of a ForEach loop the number of its iterations is then determined by the lowest of the slice-counts of all incoming spreads! 

*Image:A foreach loop receives a spread with 20 slices and another one with 15 slices via splicers causing it to execute 15 times.*

In case of a Repeat Loop the iteration count determines the number of iterations ignoring the slice-counts of the spreads coming in via splicers. When the iteration count is higher than a spreads slice-count, slices of the spread are being repeatedly accessed with the loops index taken modulo the spreads slice-count.

*Image:A Repeat loop has its Iteration Count set to 5 and receives a spread with 2 slices and another one with 3 slices via splicers causing it to execute 5 times.*

By default splicers have no names. Sometimes it may help to label a splicer for clarity in which case you simply doubleclick the area right of it to enter a name. 

### Accumulator

Accumulators allow you to hand data over between iterations of a loop. Once initialized from outside of the region, the accumulator can be accessed and modified in each iteration and is then passed on to the next iteration of a loop. The final value is then available via the accumulator output.

An accumulator can thus be understood as a variable declared outside and then modified in each iteration of a loop. 

*Image:An accumulator is modified in each iteration.*

By default accumulators don't have a name. Only to distinguish multiple accumulators in the same loop from each other, they are automatically labeled using roman numbers. If you want to specify your own you can do so by doubleclicking the area right of an accumulator to change its name. 

## Getting data out of a loop

Since you can never link directly out of any region, there are only two different ways of getting data out of a loop. They work for both the Repeat and the ForEach loop:

Use an outgoing splicer to collect the results of all iterations and return them as one spread. 

Use an outgoing accumulator to receive the final value as modified by all iterations of a loop.

## Special Pins

There are three special Pins which you can create only inside loops via the NodeBrowser:

*Image:The Index, Break and Keep pins in a loop*

### Index 
Returns the current loop iteration number.

### Break
Set it to true to break out of the loop at any time before its actual iteration count is reached. Note that the breaking iteration is still fully executed, resulting in output splicers to include the result and accumulators to be modified by this iteration.

To find out if a loop executed to the end or it was interrupted by a break, the _Break_ output can be tested.

### Keep
Set it to true or false in each iteration to specify whether results of this iteration will be included in a spread returned by an outgoing splicer.

Note that the keep has no influence on accumulators, meaning that accumulators will still be changed for iterations that are not 'kept'.

# Other Loops
## While 
Use a Repeat loop with its _Iteration Count_ set to a very high number which in this case you can consider as the maximum iteration count you specify in order to make sure your patch can never hang indefinitely. Inside the loop you specify the condition that needs to be met for the loop to continue to execute and connect its negation to the loops _Break_ output.

*Image:Simulating a while loop using a Repeat loop*


