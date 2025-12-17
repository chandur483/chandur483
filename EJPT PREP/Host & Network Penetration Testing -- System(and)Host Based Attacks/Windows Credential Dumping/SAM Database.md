SAM (Security Account Manager) is a database file that is responsible for managing user accounts and passwords on Windows. **All user account passwords stored in the SAM database are hashed

**NOTE : The SAM database file cannot be copied while the operating system is running

The Windows NT kernel keeps the SAM database file locked and as a result, attackers typically utilize in-memory techniques and tools to dump SAM hashes from the LSASS process.

In modern versions of Windows, the SAM database is encrypted with a syskey.

**Note: Elevated/Administrative privileges are required in order to access and interact with the LSASS process.



