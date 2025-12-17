

LM is the default hashing algorithm that was implemented in Windows operating systems prior to NT4.0. 

 The protocol is used to hash user passwords, and the hashing process can be broken down into the following steps: 
+ The password is broken into two seven-character chunks. 
+ All characters are then converted into uppercase. 
+ Each chunk is then hashed separately with the DES algorithm. 
 
LM hashing is generally considered to be a weak protocol and can easily be cracked, primarily because the password hash does not include salts, consequently making brute-force and rainbow table attacks effective against LM hashes