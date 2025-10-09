---
title: "Socket Programming in C: Vigenere Cipher"
date: 2015-01-29 04:25:22
updated_at: 2020-03-28 09:20:33
tags:
- programming
---

*First posted 30/06/2012*

This was my project presentation for socket programming class. The code is basically a generic client-server model that will receive input from a client and prints a ciphered text returned from the server. I did the one on Vigenere cipher which I took the cipher source code on the internet and modified it so it can be combined with the socket template ha!

In this so-called guide, I will put comment to each functions and other necessary things in the code so, do follow carefully.

Vigenere cipher source code taken from: [http://rosettacode.org/wiki/Vigenere_cipher](http://rosettacode.org/wiki/Vigenere_cipher)

### How to compile in C

    gcc -o filename filename.c
    
    ./filename
    

### Client-Server Model

![clientserver](http://3.bp.blogspot.com/-b5tPsnMP4Ak/T-3uUIks2dI/AAAAAAAAAbE/pSPo2gtazpM/s320/model.png)

Above are basically the model but there are things in the server-side structure that has differences in this guide. But it does the same thing.

### Server-side

This will ensue some tl;dr remarks, but what the hell. Server-side is often be long and complicated because it needs to initialize some values and shits. So deal with it.

    // amnbhrmism.blogspot.com
    
    /* Header files that are necessary for compilation. This allows us to access socket, symbolic type and few required function */
    
    #include <sys/types.h>
    #include <sys/socket.h>
    #include <netinet/in.h>
    #include <arpa/inet.h>
    #include <stdio.h>
    #include <stdlib.h>
    #include <unistd.h>
    #include <errno.h>
    #include <string.h>
    #include <ctype.h>
    
    #define SERVER_PORT 5000 // Initialize to port 5000
    
    /*
    Vigenere cipher source code I took from rosettacode.com
    Pretty straight forward to understand the algorithm. I have stripped out the part where the decipher moves in because this program only deals with enciphering the text.
    */
    
    // This method will convert any lower cases string to upper case
    void upper_case(char *src){
        while (*src != '\0'){
            if (islower(*src)) *src &= ~0x20;
                src++;
        }
    }
     
    // This is where the enciphering job will be done
    char* encipher(char *src, char *key){
            int i, klen, slen;
            char *dest;
     
            dest = strdup(src); 
            upper_case(dest);
            upper_case(key);
     
            /* strip out non-letters */
            for (i = 0, slen = 0; dest[slen] != '\0'; slen++)
                    if (isupper(dest[slen]))
                            dest[i++] = dest[slen];
     
            dest[slen = i] = '\0'; /* null pad it, make it safe to use */
     
            klen = strlen(key);
            for (i = 0; i < slen; i++) {
                    if (!isupper(dest[i]))
                    	continue;
                    	
                    //dest[i] = 'A' + (is_encode
                    //		? dest[i] - 'A' + key[i % klen] - 'A'
                    //		: dest[i] - key[i % klen] + 26) % 26;
                    dest[i] = 'A' + (dest[i] - 'A' + key[i % klen] - 'A') % 26;
                    		//condition ? encode : decode
            }
     
            return dest;
    }
    
    
    
    int main(){
    	/*
    	Variable declarations
    	*/
            char *cod, *dec;
            char key[] = "VIGENERECIPHER";
    		
            int sock, connected, bytes_recieved , true = 1;  
            char send_data [1024], recv_data[1024];       
    
            struct sockaddr_in server_addr,client_addr;    
            int sin_size;
    		
            /*
    	Socket()
    	Create a socket with the following properties:
    	1. AF_INET: internet communications
    	2. SOCK_STREAM: TCP connection
    	*/
            if ((sock = socket(AF_INET, SOCK_STREAM, 0)) == -1) {
                perror("Socket");
                exit(1);
            }
    		
            if (setsockopt(sock,SOL_SOCKET,SO_REUSEADDR,&true,sizeof(int)) == -1) {
                perror("Setsockopt");
                exit(1);
            }
            
    		
    		
    	/*
    	Before a socket can be used, it needs to be initialized
    	*/
            server_addr.sin_family = AF_INET; // Define the family as AF_INET
            server_addr.sin_port = htons(SERVER_PORT); // Initalize to SERVER_PORT
            server_addr.sin_addr.s_addr = INADDR_ANY; // Define the incoming address, INADDR_ANY indicates that server will accept connections from any interface on the host
            bzero(&(server_addr.sin_zero),8); // Sets all values in a buffer to zero
    	
    	/*
    	Bind()
    	Configure socket based upon data initialized in the address structure earlier.
    	*/
            if (bind(sock, (struct sockaddr *)&server_addr, sizeof(struct sockaddr)) == -1) {
                perror("Unable to bind");
                exit(1);
            }
    	
    	/*
    	Listen()
    	Permits incoming connection and declare how many connection can be accepted
    	*/
            if (listen(sock, 5) == -1) {
                perror("Listen");
                exit(1);
            }
    		
    	printf("TCPServer Waiting for client on port 5000");
            fflush(stdout);
    
    	while(1){
    		sin_size = sizeof(struct sockaddr_in);
    		
    		/*
    		Accept()
    		When a client connects, the socket calls returns a new socket to handle the client connection
    		
    		Not that 'sock' is use solely to accept connection and is not used for data transfer
    		*/
    		connected = accept(sock, (struct sockaddr *)&client_addr, &sin_size);
    		
    		
    		/*
    		To get client's IP address, we use 'client_addr.sin_addr'
    		To get the port, we use 'client_addr.sin_port'
    		
    		inet_ntoa() used to convert dots-and-numbers string in a static string, in this case, IP address
    		ntohs() converts a port number in host byte order to a port number in network byte order
    		*/
    		printf("\n Connection from (%s , %d)", inet_ntoa(client_addr.sin_addr), ntohs(client_addr.sin_port));
    			    
    		/*
    		Recv()
    		This will receive string from the client side
    		First argument is the accepted client
    		Second argument is the buffer into which the message will be read
    		Third argument is the maximum number of bytes
    		Fourth argument is for flags.
    		*/
    		bytes_recieved = recv(connected, recv_data, 1024,0);
    		recv_data[bytes_recieved] = '\0'; // Null pad last index
    		
    		printf("\n Plaintext received = %s" , recv_data); // Print received string
    		printf("\n Using key = %s \n\n", key); // Print key used to encipher
    		
    		cod = encipher(recv_data, key); // Calls encipher() method to do its job and assign the return value to 'cod'
    			
    		/*
    		Send()
    		We send back the returned enciphered message to the client
    		*/
    		send(connected, cod, strlen(cod), 0);
    		
    		fflush(stdout);
    	}
    	
          	close(connected);
          	return 0;
    }
    

### Client-side

Client-side structure is simpler because it only needs to send and receive data from the server.

    // amnbhrmism [dot] blogspot [dot] com
    
    /* Header files that are necessary for compilation. This allows us to access socket, symbolic type and few required function */
    
    #include <sys/socket.h>
    #include <sys/types.h>
    #include <netinet/in.h>
    #include <netdb.h>
    #include <stdio.h>
    #include <string.h>
    #include <stdlib.h>
    #include <unistd.h>
    #include <errno.h>
    
    #define SERVER_PORT 5000 // Initialize to port 5000
    
    int main(){
    	/*
    	Variable declarations as usual
    	*/
            int sock, bytes_recieved;  
            char send_data[1024],recv_data[1024];
            struct hostent *host;
            struct sockaddr_in server_addr;  
    
    	/*
    	gethostbyname()
    		
    	Host that this client will connect to
    	*/
            host = gethostbyname("127.0.0.1"); 
    	
            /*
    	Socket()
    		
    	Create a socket with the following properties:
    	1. AF_INET: internet communications
    	2. SOCK_STREAM: TCP connection
    	*/
            if ((sock = socket(AF_INET, SOCK_STREAM, 0)) == -1) {
                perror("Socket");
                exit(1);
            }
    
    
    	/*
    	Before a socket can be used, it needs to be initialized
    	*/
            server_addr.sin_family = AF_INET;  // Define the family as AF_INET  
            server_addr.sin_port = htons(SERVER_PORT);  // Initalize to SERVER_PORT 
            server_addr.sin_addr = *((struct in_addr *)host->h_addr); // Define the IP address of the host
            bzero(&(server_addr.sin_zero),8);  // Sets all values in a buffer to zero
    	
    	/*
    	connect()
    	
    	To establish a connection to the server
    	First argument is the file descriptor (socket())
    	Second argument is the address of the host to which it wants to connect (incl the port number)
    	Third argument is the size of this address
    	*/
            if (connect(sock, (struct sockaddr *)&server_addr, sizeof(struct sockaddr)) == -1){
                perror("Connect");
                exit(1);
            }
            
    	/*
    	Send()
    	
    	Sends data to the server. This is receive input from user using gets()
    	*/
    	printf(" Enter a text : ");
            gets(send_data);
    	send(sock,send_data,strlen(send_data), 0);
    	
    	/*
    	Recv()
    		
    	This will receive string (enciphered text) from the server side
    	First argument is the file descriptor
    	Second argument is the buffer into which the message will be read
    	Third argument is the maximum number of bytes
    	Fourth argument is for flags.
    	*/
            bytes_recieved = recv(sock, recv_data, 1024, 0);
            recv_data[bytes_recieved] = '\0';
    	printf(" Enciphered text = %s \n\n" , recv_data); // Prints the enciphered text
    	fflush(stdout);
    
    	close(sock);
            
    	return 0;
    }
    
