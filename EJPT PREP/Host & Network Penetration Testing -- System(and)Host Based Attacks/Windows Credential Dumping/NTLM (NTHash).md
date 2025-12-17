NTLM is a collection of authentication protocols that are utilized in Windows to facilitate authentication between computers. The authentication process involves using a valid username and password to authenticate successfully.


From Windows Vista onwards, Windows disables LM hashing and utilizes NTLM hashing.

When a user account is created, it is encrypted using the MD4 hashing algorithm, while the original password is disposed of.

NTLM improves upon LM in the following ways: 
+ Does not split the hash in to two chunks. 
+ Case sensitive. 
+ Allows the use of symbols and unicode characters.

