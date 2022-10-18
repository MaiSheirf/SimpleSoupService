# SimpleSoupService

Task steps 

1- creating Mw wsdl with two message( get customer , fault ) and two operations (get customer Request , get Customer Response )
 and attributes ( id , name  faultdata) in iib .
 
2- creating Back wsdl with two nessage (get customer , fault ) and two operations ( get customer Request , get Customer Response )
 and attributes ( id , name , status , faultdata) in iib.
 
3- edit port number in requests

4-create 3 flows 

5- first flow ---> soapinput ---- compute node ---- mqoutput
	soapinput --> mw wsdl ( choose the binding operation )
	compute node --> copy soapheader , transform mw request to back request
	mqoutput --> put message on specified queue
	
6- second flow ---> mqinput --- compute node --- souprequest --- soupextract ---compute node ---mqoutput --- trace 
	mqinput --> listen on specified queue 
	compute node --> copy entire message , copy mqrfh2 in environment variable 
	souprequest --> back wsdl ( set url to mockservice url )
	soupextract --> deleteing envelope as to be xmlnsc parser 
	compute node --> copy headers ( properties , mqrfh2 , xmlnsc )
	mqoutput --> put message on specified queue 
	trace --> logging
	
7- third flow ---> mqinput --- compute node , exception handling --- soup reply 
	mqinput --> listen on specified queue 
	compute node --> get soupheader , transform back response to mw response 
	exception handling --> catch the exceptions
	soup reply --> reply with final response ( mw response ).
	

take care of : 
	1-casting from character to blob 
	2- takecare of needed headers 
	3-the scope of sending ( message , environment , local environment )
	4-queues
	5-namespaces
	6-depugging
