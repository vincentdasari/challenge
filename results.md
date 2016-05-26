Your implementation information goes here.
Design: 
Three classes are present- two data definition classes and an implementation class. The user input will be passed from the implementation class to the data definition class and that's where the instance variable validation will take place. 
There's an aggregation relationship between a customer and the ticket. It's aggregation because a ticket can be present theoretically based on the given challenge instructions, even before a ticket has been bought by a customer.
The first data definition class is for the customer. This class contains the private instance variables, name and ID, and the static variables, number of customers and the static ID variables. A private ticket object reference is present within this class, denoting aggregation relationship between a customer and a ticket.
The second data definition class is for the ticket. This class contains the private instance variables, ticket type and the ticket number. A static array of counters for the different ticket types is also present. A special purpose method, addTicket, increments the corresponding ticket type counter in the array mentioned earlier. The equals( object) methods has been overridden to make sure that no two tickets will have the same ticket number.
The implementation class basically gathers all the user inputs, adds a new customer, generates the simulation reports.
Static constants have been used whereever it seemed appropriate to declare a constant and only one copy of it is required. For instance, number of ticket types, becuase it can change at a later point of time.

Instructions on how to run the submission:
Since I see that the java files themselves can't be attached, I'm copy-pasting my submission for all the 3 java files. Save each of the class in a seperate java file, with in the same package and execute the implementation class.
Additional comments have been added inbetween code and at the start of important methods in all 3 classes.
Test input to test the submission:
Name:a
number of tickets:6 (will be re-prompted). Now it's 5
ticket type:2 (re-prompted), asdfa(re-prompted), 3,4,5,3,4
Name:(blank input-> re-prompted). Now it's b
number of tickets:2
ticket type:4,5,..... and so on. Once all the tickets of all types have been finished, the machine will commence the draw.


Sample output: The draw winners' details will be printed. An intermediary report showing what each customer has purchased will be displayed as well. Finally, the final report with the detials, number of customers and if any of them tried to access finished ticket types, will be printed.

Code:
import javax.swing.JOptionPane;

public class Lottery{
   public static void main(String[] args){
      final int MAX_CUSTOMERS = 150;
      Customer customer[] = new Customer[MAX_CUSTOMERS];
      int customerIds[]= new int[MAX_CUSTOMERS];
      int currentCustomer=0;
      int numberOfTickets=0;
      boolean availabilityFlag=false;
     //The outer loop that manages taking in all the 150 customers       
      while(currentCustomer<MAX_CUSTOMERS){
         String report= "";
         customer[currentCustomer]= new Customer();
         boolean nameFlag= false;
         String name="";
         //Loop to gether a valid(anything but empty) customer name
         do{
            name= JOptionPane.showInputDialog("Enter your name"+(currentCustomer+1));
            if(!customer[currentCustomer].setCustomerName(name)) nameFlag=false;
            else nameFlag=true;
         }while(!nameFlag);
         //Loop that makes sure that there are enough tickets left in the lottery machine
         do{
            try{
               numberOfTickets=Integer.parseInt(JOptionPane.showInputDialog("How many tickets would you like to buy?"));
            }
            catch(NumberFormatException e){
               JOptionPane.showMessageDialog(null,"Please enter a number");  
            }
            if(!customer[currentCustomer].setNumberTickets(numberOfTickets)) JOptionPane.showMessageDialog(null, "Invalid input! Only 1-5 tickets can be bought!");
               if((currentCustomer+numberOfTickets-1)<MAX_CUSTOMERS) customer[currentCustomer].setNumberTickets(numberOfTickets);
               else JOptionPane.showMessageDialog(null, "Not enough tickets available!");

         }while(!customer[currentCustomer].setNumberTickets(numberOfTickets)||((currentCustomer+numberOfTickets-1)>=MAX_CUSTOMERS));
         //Loop to process one customer- generates the lottery tickets for a given customer
         for(int i=currentCustomer; i<currentCustomer+numberOfTickets;i++){
         
            customer[i]= new Customer(customer[currentCustomer].getCustomerID());
            customer[i].setCustomerName(name);

            int ticketType= 0;
            ticketType= getTicketType(customer[i]);
            customer[i].getTicket().setTicketType(ticketType);
            int number= 0;
            boolean IDflag=false;
            //Since the ticket numbers are generated at random, this loop makes sure that the machine doesn't
            //generate two tickets with the same number
            do{
               boolean available= true;
               IDflag=false;
                  //Loop that makes sure that enough lottery tickets of a certain type are present
                  //once the customer selects a ticket type
                  do{
                     if(available==false) ticketType= getTicketType(customer[i]);
                     if(ticketType==3){
                        number=(int)(Math.random()*1000);
                     }
                     else if(ticketType==4){
                        number= (int)(Math.random()*10000);
                     }   
                     else{
                        number= (int)(Math.random()*100000);
                     }
            
                     available= checkAvailability(ticketType);
                     if(available){
                        customer[i].getTicket().setTicketNumber(number);
                        System.out.println("Customer ID:"+customer[i].getCustomerID());
                        for(int j=0; j<customer[i].getCustomerID()-1;j++){
                           if(customer[i].getTicket().equals(customer[j].getTicket())) IDflag = true;
                        }
                     if(IDflag)JOptionPane.showMessageDialog(null, "Generating new ticket number...");
                     }
                     else{
                        availabilityFlag=true;
                        JOptionPane.showMessageDialog(null, "Tickets of this type are sold out! Please choose a different ticket type");
                     }
                  }while(available==false);
               System.out.println(number);
               }while(IDflag);
              customer[i].getTicket().addTicket(ticketType);
              customerIds[i]=customer[i].getCustomerID();
              JOptionPane.showMessageDialog(null, customer[i].toString());
              report+= reportHelper(customer[i]);
            }
         
         customerReport(report, customer[currentCustomer]);
         currentCustomer+=numberOfTickets;
        
       }
      int winners[]=drawWinner(customer);
      String winnerReport="";
      //Printing the draw winners
      for(int winner=0; winner<winners.length;winner++){
         winnerReport+="\nType"+(winner+3)+" Winner's details:"+customer[winners[winner]].toString()+
         "\n"+customer[winners[winner]].getTicket().toString();
      }
      JOptionPane.showMessageDialog(null,winnerReport);
      lotteryReport(availabilityFlag);
   }
   
   /*
   Method: drawWinner- method that calls helper methods to generate the lottery draw winners
   Parameters: Array of customer objects
   Return: Array of integers- the customer indices who won the lottery draw
   */
   public static int[] drawWinner(Customer[] customers){
      int winners[]= new int[3];
      winners[0]= drawType3(customers);
      winners[1]= drawType4(customers);
      winners[2]= drawType5(customers);
      return winners;
   }
   
   /*
   Method: drawType3- method that generates the type3 ticket winner
   Parameters: Array of customer objects
   Return: int- generated at random
   */
   public static int drawType3(Customer[] customers){
      int winner=-1;
      
      do{
         winner= (int)(Math.random()*150);
         if(customers[winner].getTicket().getTicketType()==3) return winner;
         else winner=-1;
      }while(winner==-1);
      return winner;
   }
   
    /*
   Method: drawType4- method that generates the type4 ticket winner
   Parameters: Array of customer objects
   Return: int- generated at random
   */
   public static int drawType4(Customer[] customers){
      int winner=-1;
      
      do{
         winner= (int)(Math.random()*150);
         if(customers[winner].getTicket().getTicketType()==4) return winner;
         else winner=-1;
      }while(winner==-1);
      return winner;
   }
   
    /*
   Method: drawType5- method that generates the type5 ticket winner
   Parameters: Array of customer objects
   Return: int- generated at random
   */
   public static int drawType5(Customer[] customers){
      int winner=-1;
      
      do{
         winner= (int)(Math.random()*150);
         if(customers[winner].getTicket().getTicketType()==5) return winner;
         else winner=-1;
      }while(winner==-1);
      return winner;
   }
   
   /*
   Method: getTicketType- this method validates the ticket type entered by the customer
   Parameters: Current customer object
   Return: int- type of the ticket
   */
   public static int getTicketType(Customer customer){
   int ticketType=0;
      do{
         try{
            ticketType= Integer.parseInt(JOptionPane.showInputDialog("Enter the ticket type"));
         }
         catch(NumberFormatException e){
            ticketType=0;
         }
         if(!customer.getTicket().setTicketType(ticketType)) JOptionPane.showMessageDialog(null, "Invalid Ticket Type");  
      }while(!customer.getTicket().setTicketType(ticketType));
   return ticketType;
   }
   
   /*
   Method: checkAvailability- this method checks if there are enough tickets of a type left to be purchased
   Parameters: ticketType- ticket type choosen by the current customer
   Return: boolean- true if enough tickets are left, false, otherwise
   */
   public static boolean checkAvailability(int ticketType){
   
      if(ticketType==3){
         if(TicketType.getTicketCounter()[0]>=50) return false;
      }
      else if(ticketType==4){
         if(TicketType.getTicketCounter()[1]>=40) return false;
      }
      else{
         if(TicketType.getTicketCounter()[2]>=60) return false;
      }
      return true;
   }
   
   /*
   Method: reportHelper- helper method to gather the lottery ticket details
   Parameters: current customer object
   Return: String- intermediary report for the current customer
   */
   public static String reportHelper(Customer customer){
      String report="";
      report+="\n Ticket Number:"+customer.getTicket().getTicketNumber()+"\n Ticket Type:"+customer.getTicket().getTicketType();
      return report;
   }
   
   /*
   Method: customerReport- method to generate the current customer's receipt with the customers details
   and the tickets' details
   Parameter:String (return from the reportHelper method) and current customer object
   Return: void
   */
   public static void customerReport(String report, Customer customer){
      String customerReport=customer.toString()+"\n"+report;
      JOptionPane.showMessageDialog(null, customerReport);
   }
   
   public static void lotteryReport(boolean flag){
      String report="";
      report+="Total number of customers:"+Customer.getNumberOfCustomers()+"\n Did the customers try to purchase sold out ticket types?";
      if(flag){
         report+="YES";
      }   
      else
         report+="NO";
      JOptionPane.showMessageDialog(null,report);
   }
}

public class Customer{
   private String customerName;
   private int customerID;
   private static int ID=0;
   public static final int MAX_TICKETS= 5;
   public static final int MIN_TICKETS=1;
   private int numTickets;
   private static int numCustomers;
   private TicketType ticket;
   
   /*
   Default constructor- increments the ID and sets it to the customer
   */
   public Customer(){
      this.customerID=ID++;
      numCustomers++;
      ticket= new TicketType();
   }
   
   /*
   Specific constructor- to instantiate a new customer object when a customer wishes to buy multiple tickets
   */
   public Customer(int customerID){
      ticket=new TicketType();
      this.customerID=customerID;
   }
   
   //start of accessors for the private instance variables and the static variables
   public String getCustomerName(){
      return this.customerName;
   }
   
   public static int getNumberOfCustomers(){
      return numCustomers;
   }
   
   public int getCustomerID(){
      return this.customerID;
   }
   
   public static int getID(){
      return ID;
   }
   
   public int getNumTickets(){
      return this.numTickets;
   }
   
   public TicketType getTicket(){
      return this.ticket;
   }
   
   //end of accessors
   
   /*
   Method: setCustomerName- to set the customer's name. Validates it to be anything but blank input
   Parameter: name of the customer
   Return: boolean- true if valid name, false, otherwise
   */
   public boolean setCustomerName(String name){
      if(name.equals("")){
         return false;
      }
      else{
         this.customerName=name;
      }
      return true;
   }
   
   /*
   Method: setNumberTickets- to set the number of tickets a customer wishes to buy. validates it to be with in 1-5
   Parameter: number of tickets
   Return: boolean- true if valid number of tickets, false, otherwise
   */
   public boolean setNumberTickets(int numTickets){
      if(numTickets<MIN_TICKETS || numTickets>MAX_TICKETS){
         return false;
      }
      else{
         this.numTickets=numTickets;
         return true;
      }
   }
   
   /*
   toString to give String representation of the customer object
   */
   public String toString(){
      String output= "Customer Name:"+getCustomerName()+"\n Customer ID:"+getCustomerID();
      return output;
   }
}

public class TicketType{
   private int ticketNumber;
   private int ticketType;
   public static final int MAX_TYPE_TICKETS=3;
   private static int ticketTypeCounter[]= new int[MAX_TYPE_TICKETS];
   
   //start of accessors for the instance and static variables for a ticket
   public int getTicketNumber(){
      return this.ticketNumber;
   }
   
   public int getTicketType(){
      return this.ticketType;
   }
   
   public static int[] getTicketCounter(){
      return ticketTypeCounter;
   }
   //end of accessors
   
   /*
   Method: setTicketNumber- method to set the ticket number generated at random when a ticket has been bought
   Parameter: int- ticketNumber generated at random
   Return: void
   */
   public void setTicketNumber(int ticketNumber){
      this.ticketNumber=ticketNumber;
   }
   
   public boolean setTicketType(int ticketType){
      if(ticketType<3||ticketType>5){
         return false;
      }
      else{
         this.ticketType=ticketType;
         return true;
      }
   }
   
   /*
   Method to increment the corresponding ticket type counters when a ticket has been bought by a customer
   */
   public void addTicket(int ticketType){
      if(ticketType==3) ticketTypeCounter[0]++;
      else if(ticketType==4) ticketTypeCounter[1]++;
      else ticketTypeCounter[2]++;
   }
   
   /*
   Overridden equals method to make sure that two tickets generated are not the same
   */
   public boolean equals(Object ticket){
      TicketType oldTicket= (TicketType)ticket;
      if(oldTicket.getTicketNumber()==(this.getTicketNumber())) return true;
      else  return false;
   }
   
   public String toString(){
      return "Tikcet Number:"+getTicketNumber()+"\n Ticket Type:"+getTicketType();
   }
}
