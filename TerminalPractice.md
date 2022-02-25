- A **command line (terminal)**: is a text based interface to the system. You are able to enter commands by typing them on the keyboard and feedback will 
  be given to you similarly as text.   
-  When we entered a command, Typically a **command** is always the first thing you type. After that we have what are referred to as command line **arguments**.
-  The command and the arguments must be separated by spaces.  
-  **Options** are typically used to modify the behaviour of the command. Options are usually listed before other arguments and typically start with a dash ( - ).  
-  Within a terminal you have what is known as a **shell**. This is a part of the operating system that defines how the terminal will behave and looks after 
   running (or executing) commands for you.  
- There are various shells available but the most common one is called **bash** which stands for **Bourne again shell**.  
- Some commands and what they are doing:   
  1. **pwd** : stand for **Print Working Directory**, result the path of the currently directory.  
  2. **ls** : List the contents of a directory.  
  3. **cd** : stand for **Change Directories**, move to another directory.  
  4. **file** : obtain information about what type of file a file or directory is.  
  5. **ls -a** : List the contents of a directory, including hidden files.  
  6. **man <command to look up>** : The manual pages are a set of pages that explain every command available on your system including what they do, the specifics of how you run them and what command line arguments they accept.  
  7. **man -k <search term>** : Do a keyword search for all manual pages containing the given search term.  
  8. **/<term>** : Within a manual page, perform a search for 'term'.  
  9. **n** : After performing a search within a manual page, select the next found item.  
  10. **mkdir [options]<directory>** : stand for **make a directory**, to create a directory.  
  11. **rmdir [options]<directory>** : stand for **Remove Directory**, to Delete a directory(the directory must be empty before it may be removed).   
  12. **touch [options]<file>** : Create a blank file.  
  13. **cp [options]<source file> <destination file>** : stand for **Copy**, to Copy a file or directory(it will create a copy of the source but with differnt name we specified in the destination).    
  14. **mv [options]<source file> <destination file>** : stand for **Move** to Move a file or directory (can also be used to rename).  
  15. **rm** : stand for **Remove** to Delete a file, and if we want to remove a directory we use **-r** option after **rm** command, it will remove the directory and the sub directories.    
- **Absolute paths** specify a location (file or directory) in relation to the root directory. You can identify them easily as they always begin with a forward slash ( / ).  
- **Relative paths** specify a location (file or directory) in relation to where we currently are in the system. They will not begin with a slash.  
- Linux is an **extensionless system** : Under Linux the system actually ignores the extension and looks inside the file to determine what type of file it is.  
- Linux is **case sensitive** : As such it is possible to have two or more files and directories with the same name but letters of different case.  
