# Dropping all tables in database

In SSMS:

Right click the database
Go to "Tasks"
Click "Generate Scripts"
In the "Choose Objects" section, select "Script entire database and all database objects"
In the "Set Scripting Options" section, click the "Advanced" button
On "Script DROP and CREATE" switch "Script CREATE" to "Script DROP" and press OK
Then, either save to file, clipboard, or new query window.
Run script.

Now, this will drop everything, including the database.
Make sure to remove the code for the items you don't want dropped. Alternatively, 
in the "Choose Objects" section, instead of selecting to script entire database just select the items you want to remove.
