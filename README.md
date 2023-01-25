# server_client_chat
A simple client server chat inface made using Multiple threading concept in Java
                                                  
                                                  IMPLEMENTATION

In this model, we implement thread since we work with not just a single client to connect with the server at a specific time but many clients working on it simultaneously. For this reason, we use the thread concept on the server side so that whenever a client request comes in a separate thread can be assigned for handling each request.
SERVER-SIDE IMPLEMENTATION:
The Serverview.java is a java file where the main class Serveriew extends to JFrame (to create the GUI). This file will additionally also have another thread class called ClientAccept which further calls upon other sub-thread classes-MsgRead() and PrepareClientList() 
1.	class Server view-
•	create a ConcurrentHashMap called allUserList which maps all users with their respective socket connections. We also create a HashSet called activeuserset to store active users.
•	Further we declare all the JFrame components accordingly
•	In the run() method and object windows are created for the class and this is used to set the frame to visible.
•	In the class constructor the socket object is created(serversocket) for the server; in the messageBoard of the Server  “Server started…..” is printed and a new object for ClientAccept is created. (new thread for each request is assigned)
•	Serversocket is then connected with the default port
                                
![image](https://user-images.githubusercontent.com/121189145/214596932-20116e1c-a04e-4e08-b5a3-13185e07dd89.png)


2.	  Class ClientAccept-
•	It is a thread class, clientsocket is initialized concerning serversocket.
•	uName is a variable that holds the I/O stream from the login module which gets the username
•	coutstream holds the O/P stream from the server.
•	If the username is already present then (“Existing user”) is printed else the username is added to allUserList and activeuserset(additionally we have also previously declared 2 collections  set to carry a copy of each of the above-mentioned ones specifically to DISPLAY the lists to the JFrame)
•	We print (Client: Name, connected) on the server messageBoard.
•	Create a thread for MsgRead and PrepareCllientList thread classes respectively
   ![image](https://user-images.githubusercontent.com/121189145/214597052-26d61f58-59c6-49a2-993f-3cbda02baa5d.png)
    

3.	Class MsgRead-
•	Create a non-parameterized constructor and assign the currently used object(client) to the different variables.
•	If allUserList is not empty proceed to read the message from the client using the variable message.
•	Message is then split by the “:”  to create an array (msglist)consisting of action_to_be_taken, Client_for_receiving, message
•	Now if msglist[0] ie the action_to_be_taken is “multicast” then we need to send the list of the clients to send the message to, now we will also recheck if the clients in the above list are active and then add a message to the server output stream which will be picked up by their input streams.
•	Else if msglist[0] ie the action_to_be_taken is “broadcast”, we create an iterator itr1 which will iterate over allUserList while also checking if they are active and then send to the output stream of the server to be picked up by ALL the clients’ input stream.

•	Else if msglist[0] ie the action_to_be_taken is “exit”, remove the current client (using id) from userList and also print on the server message board “ (name) disconnected”
•	Now we need to create a new instance of the Thread class PrepareClientList and use the .start() to run it
     ![image](https://user-images.githubusercontent.com/121189145/214597153-ef91aed6-e9e9-4ccc-9d11-af63aad9fc77.png)
     ![image](https://user-images.githubusercontent.com/121189145/214597172-d72449f2-5969-4cde-980e-7b790eb5bc3a.png)
![image](https://user-images.githubusercontent.com/121189145/214597214-dbb20829-d882-4f57-86c4-86c74a2dffb3.png)

                    

                     

               

4.	Class PrepareClientList-
•	This class aims to always maintain a recently updated client list
•	Create an iterator itr for activeUserset
•	We  then create a String with the names of all the clients separated by commas


•	Then as we iterate over the activeUserset we send the list to the output stream of the server
![image](https://user-images.githubusercontent.com/121189145/214597299-db159123-139b-41ef-b1f7-9a30607f7bd8.png)


         
Then we initialize and designate the various components of the JFrame.
![image](https://user-images.githubusercontent.com/121189145/214597342-f5120b89-021d-4766-89bc-cb5a498087b0.png)

                     
CLIENT-SIDE IMPLEMENTATION-
In the clientview.java module, we don’t create the main method as we will not run this program rather it will run automatically from the login module. The methods in this class are being used by other modules and hence have only the requirement of facilitating those needs.
1.	clientView class-
•	We create a non-parameterized constructor and in it, we only specify initialize() which helps to initialize the JFrame UI.
•	We then create a constructor with the parameter String for client ID and s with datatype Socket to specify the socket connection involved.
•	We again initialize the UI components, setting the title of the UI and also creating dm which DefaultListModel for showing active users this time for the client side.
•	Then we assign a list component to display on the UI with the above-mentioned dm



•	We create an input stream for the input stream from the server and the output stream for the output stream of the client.
•	We then create a new instance of the Run thread class and call for it using the .start() method.

 ![image](https://user-images.githubusercontent.com/121189145/214597449-6d0e84a3-5f81-48be-ba5f-a2fe6034c9ba.png)



2.	Read class-
•	We create a String m which holds a copy of the input stream
•	We check if the string m contains the “:;.,/=”( in the previously mentioned PrepareClientList class of the Server implementation we had arbitrarily separated certain parts of the message using “:;.,/=” hence we will search for this in the client inputstream). if present, then we split the message by the “’,” (since all active user id was separated by commas).
•	We clear dm using the .clear() method and then we insert this list of all the active users to all the clients’ user pane (for them to know who is active or not)
•	If “:;.,/=” is absent then that means the input stream of the client contains only the message and nothing else. hence we just need to print it on the message board of the client.

![image](https://user-images.githubusercontent.com/121189145/214597483-593fc4e1-7412-4b66-bdfa-c108badff153.png)
3.	Initializing all the JFrame components-
![image](https://user-images.githubusercontent.com/121189145/214597539-bdf6ef9d-a634-4d33-8887-3c279068c48d.png)

 
Finally, the following is the final state of the server output with 4 clients-
       ![image](https://user-images.githubusercontent.com/121189145/214597567-396bc810-5759-4864-b2e9-2644597d3dab.png)
       
                                          OUTPUT
                                                   
                                                   
SERVER VIEW AND CLIENT VIEW-

  ![image](https://user-images.githubusercontent.com/121189145/214598162-bf425dcd-da43-4767-87fd-afdd5086be0f.png)
  ![image](https://user-images.githubusercontent.com/121189145/214598198-6cd12fe4-f0f2-4685-bbe1-1a589f0e4637.png)


 


     
 
	
