# Programming Challenge

## Instructions
Fork this repository and implement the simulation scenario as described below. Try to show your process and write clean, robust, tested code. You may complete the challenge using Java or Javascript, but please describe your approach and anything we may need to know in the `results.md` file. 

## Scenario
Acme Lottery Co. is developing a new Lottery Machine that will dispense different types of lottery tickets to customers. Your job is to write code to simulate one complete sale cycle and drawing of the Lottery Machine's functionality to provide insight for company executives.

## The Lottery Machine
- The Lottery Machine is capable of producing Pick 3, Pick 4, or Pick 5 tickets. See below for a description of the different ticket types. However, it is possible that different ticket types may be introduced in the future.
- Assume that all ticket prices are equal.
- Assume that the Lottery Machine starts out completely full.
- Once the Lottery Machine is empty, a drawing will be held. See below for drawing rules.

## Customers
- Customers may want to purchase between 1 and 5 lottery tickets. 
- A customer is equally likely to intend to purchase any type of lottery ticket.
- If the ticket type that a customer intended to purchase is sold out, they will attempt to purchase another type at random.
- Customers will continue to show up until the machine is empty.
- All customers will be available at the drawing.
- Each customer can be uniquely identified. (e.g., name or ID)

## Drawing 
- A drawing consists of picking a winning ticket for each ticket type the Lottery Machine is capable of producing. 
- For each ticket type, the Lottery Machine randomly picks a number it has generated with the appropriate amount of digits. This is used to declare the winning ticket.
- A customer is a winner if they have a winning ticket.
- A customer may be the winner for more than one ticket type.

## Lottery Ticket Types & Rules
### Pick 3
- A customer buys a ticket with a random 3 digit number (e.g. 345) chosen by the Lottery Machine. 
- Valid digits are 0-9. 
- No two tickets will have the same 3 digit number.
- The Lottery Machine is capable of holding 50 Pick 3 tickets.

### Pick 4
- A customer buys a ticket with a random 4 digit number (e.g. 1927) chosen by the Lottery Machine. 
- Valid digits are 0-9.
- No two tickets will have the same 4 digit number.
- The Lottery Machine is capable of holding 40 Pick 4 tickets.

### Pick 5
- A customer buys a ticket with a random 5 digit number (e.g. 03456) chosen by the Lottery Machine. 
- Valid digits are 0-9.
- No two tickets will have the same 5 digit number.
- The Lottery Machine is capable of holding 60 Pick 5 tickets.

## Deliverables

### Simulation Integrity
- The Lottery Machine should guard against producing invalid ticket data. 
- The Lottery Machine should always choose a valid winner during drawings.

### Simulation Reporting
You may want to have the simulation report the following:
- How many customers purchased tickets?
- What type of tickets did each customer purchase?
- Did customers attempt to purchase sold out ticket types?
- Which customers won the drawing for each ticket type?
- Which numbers were selected during the drawing?

### results.md
When you are finished with the challenge, include instructions on how to run your program in this file. Submissions that do not compile and run may not be considered. Please include instructions on how to test and verify your results.
