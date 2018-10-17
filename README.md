October 17st, 2016
Author: Daryoush Joobbani


This is a web multi-threaded application in Java that was developed in Eclipse IDE.
It was developed using “Dynamic Web Project” in Eclipse.
It has a Servlet called CrackDigestsServlet.java and two JSP files:
Index.jsp and messages.jsp. The index.jsp is used for the Upload File page and messages.jsp is used to display the status message.
The web page is set so that it does not timeout.
It uses Tomcat V9.0. 

The algorithm that was used for the hashing function is based on FNV1 128-bit:

hash = FNV_offset_basis
   for each byte_of_data to be hashed
   	hash = hash × FNV_prime
   	hash = hash XOR byte_of_data
   return hash

File FNV1.java performs the above algorithm for a given password.
However, this application is supposed to do the reverse, which is,  given a list of hash digests,
Determine the password which is impossible. The only possible way is to come up with a list of passwords based on some pattern and then use those passwords to see if their output digest matches what’s in the original input list of digests.

This app attempts to generate a list of passwords given a-z letters in length of 1-7 (based on the given requirement). This is a huge list of passwords: way over 26! (factorial), this is 100 of billions, since it has to take into consideration, repeating letters.
So it builds the list in 7 iterations: 1 letter passwords, 2 letters passwords, 3 letters passwords, ….. 7 letters passwords.
The file that generates all the passwords is GeneratePasswords.java.

This program is multi-threaded and uses a pool of 10 threads to distribute the work of having to go through each digest list of over 3900. File related to that are ScheduleTasks.java and DoTask.java


The program is hosted by Tomcat on port 8080: URL http://localhost:8080/CrackDigests


This program has many existing issues that needs to be investigated and fixed but due to the time constraints, couldn’t do it currently. One of the problem is that after running more than 30 minutes it crashes (at least on my pc) due to memory usage by the JVM garbage collector:

Exception in thread "pool-1-thread-1" java.lang.OutOfMemoryError: GC overhead limit exceeded

The following are the two screen snapshots: One is used for the user to upload their given file and then other one displays status messages to the user.




 






 


The FNV1 algorithm used in this application has been tested with most of the provieded (password,digest) pairs.

One of the changes could be made was to use some kind of database to store all the password pattern records.

The given zip file is basically the entire Eclipse project which can be used to import into your Eclipse.




Thanks,
Daryoush
