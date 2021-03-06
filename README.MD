**NOTICE**
These files and scripts are provided without any assurances that they
will work properly in your environment.  There is also no guarantee
that something won't get messed up in either PowerSchool or IA LDS
by using this project.  Use at your own risk, understand what the
commands will be doing, and use in a test environment if possible!

**Background**
This program was created by Jason Lester of Washingon County Schools.
It uses some query code originally supplied by Interactive Achievement,
but modified to fit our environment better.

**Description**
These scripts export out the attendance, grades, and discipline
from PowerSchool that are needed for incremental updates of the
Interactive Achievement Longitudinal Data System (now PowerSchool
Analytics and Assessment.)

**Release**
The latest version is always available on Github from:
  https://github.com/JLester73/ps2ia
  
**Requirements**
We run the scripts on a Debian Linux server.  It should theoretically work
on any system that supports Ruby, but we have not tested on anything else.
You will need to have Ruby Oracle's InstantClient all installed
and working properly.  Installing and configuring those is mostly outside
the scope of this document except where it applies specifically to these
scripts.

Dependencies:
- Ruby 1.9 or higher
- A Unix/Linux-like environment
- Oracle Instantclient
- PowerSchool v8 or higher (tested all the way to 10.0.3)

Ruby Dependencies:
- Log4r Gem
- OCI8 Ruby Library for Oracle (compiled against your instantclient)
- Spreadsheet Gem
- YAML Gem

**Setup**
- Unzip the package onto the sytem that will be running it.  We use a special
  user account called "data" on our server and place each script in that
  user's home directory.  This one would be in /home/data/ps2ia for
  example.
- Edit the scripts/ps2ia.example file and updated with the password,
  login, and IP address supplied to you by your IA support contact.
  Note that this file has plaintext passwords and is totally insecure
  unless you have your directory permissions setup correctly.  Make
  sure your paths are correct as well.
- For the Oracle instantclient, you will need to configure a tnsnames.ora
  file to tell the client how to locate your PowerSchool Oracle DB server.
  It will look something like this:
------
    PSPRODDB =
      (DESCRIPTION =
         (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.1)(PORT = 1521))
         (CONNECT_DATA =
           (SERVER = DEDICATED)
           (SERVICE_NAME = PSPRODDB)
         )
       )
------
  PSPRODDB is what we're calling the service when we call it in Ruby.
  Host should be set to your server's IP address and Service_Name should be
  set to the PowerSchool database's server instance (normally PSPRODDB).
- Edit the /config/ps2ia.yaml.example file to update your login and
  password information for your PowerSchool Oracle database.  We recommend
  using the psnavigator account since it has read-only access to your
  database.  Update the three lines to match your installation and save
  the file as ps2ia.yaml.

**Usage**
Make the ps2ia file in the scripts/ directory executable and then run it.
It will create the CSV files in the proper format and then upload them
into the LDS.  Login to the LDS Data Import Manager and watch the files
to ensure they import correctly.

Once you are satisfied that everything is working properly, you can use
cron to automate this to run each night.
  