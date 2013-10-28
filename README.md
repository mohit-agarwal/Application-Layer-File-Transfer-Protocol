Application-Layer-File-Transfer-Protocol
========================================

Application Level file sharing protocol with support for upload/download and indexed searching.


**How to Use:**

1)Copy the code onto two different machines or two different folders on the same machine

2)Start two different sessions, compile the code ( gcc Protocol.c -o client), and run it (./client)

3) The program asks you to enter the port on which it will listen, enter a value (eg: 5000)

4) Choose the mode (O for TCP and 1 for UDP)

5) Enter the server address to which you want to connect ( Eg: 6000)

Note: Now the program tries to connect with the server listening on this port and will wait till it establishes a connection

6) Once connection is established, the program will work and the 2 network clients can start listening for requests waiting to share files.

-------------------------------------------------------

The Protocol enables two clients to:
-Know list of files on each other's machines in designate shared folders

-Upload a file to each other

-Download a file to each other

-Periodically check for changes in the shared folder

-Application level error checking using MD5 checksum

-Enable both TCP and UDP based transport of files as per client requests

----------------------------------------------------------

The file sharing protocol implements the following features:

- An **"IndexGet" request** which can request different styles of the same index of the shared folder on the other client as listed below. The history of requests made by either clients is maintained at each of the clients respectively.
	
	
	a. A **"ShortList" request** indicating that the requesting client wants to know only the names files chosen from the time-stamps specified by the requesting client, i.e., the client only wishes to learn about a few files.
	
	E.g., Sample request: IndexGet ShortList starting-time-stamp ending-time-stamp
	
	The response includes the "names", "sizes", "last modified" time-stamp and "type" of files (if available)
	
	
	b. A **"LongList" request** indicating that the requesting client wants to know the entire listing of the directory of the shared folder  including the "names", "sizes", "last modified timestamp" and "type"(if available)
	
	E.g., is similar to above with necessary changes.
	
	
	c. A **"RegEx" request** indicating that the requesting client wants to know the list of files that contain the regular expression pattern in their file names. The response includes all the file names which "contain" the regular expression pattern in their names, the sizes of the files and "type" (if available).
	
	E.g., IndexGet RegEx "*mp3"
	
	
	
	
- A **"FileHash" request** indicates that the client to enable the client to check if any file's content has changed. Two types of "FileHash" are supported:
	
	a.  A **"Verify" request** which gives the name of the file that the client wants the hash for. The response contains the MD5 hash of the file and the name of the file and last modified time stamp.
	
	E.g., FileHash Verify Name-of-file
	
	
	b.  A **"Check All" request** which is used to periodically check for modifications in the file. The response includes the hashes of all the files, their names and the last modified time stamp.
	
	E.g.,  FileHash CheckAll
	
	
- A **"FileDownload" request**, which includes the name of the file that the client wants to download. The response includes the File, the file name, the file size, the MD5 hash and the time-stamp when the file was last modified. The "file size" parameter is used by the requesting client to allocate memory and receive the file in the allocated memory. 
	
	E.g., FileDownload Name-of-file 
	
	
- A **"FileUpload" request**, which includes the name and size of the file that the client wants to  upload. The other client can either send  a "FileUpload Deny" or "FileUpload Allow"  response. The other client uses the file size parameter to allocate memory as done in "FileUpload" request. The client can upload the file, its md5 hash and the time-stamp if it  receives a "FileUpload Allow" response. If it gets a "FileUpload Deny" then the client goes back to listening for other requests. 
	
	E.g., FileUpload Name-of-file
	

- A **"FileUpload" or "FileDownload" requests** can be serviced through a UDP or TCP based connection. If such a socket is not available, it is created and both clients use that socket to exchange the file.
