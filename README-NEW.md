# Ethos-MD5SUM Project

# Read PLANNING.md First Before Any Thinking or Planning or Coding
**First, before AI starts to think about this all, a mandatory step is to go through the PLANNING.md file in this repository to set some guidelines and guardrails and it should all be considered when making decisions or starting a plan to build it all.  Second, The AI agent will also go through the UI.md file to establish guidelines and general UI formatting and layout for specific areas within the UI and project.  Then, the AI agent must go through and analyze any other markdown files before starting any coding so that a big plan is in place.**

# Ethos-MD5SUM Use Cases
- Many Telegram channels and group chats that offer the ability to post pictures regularly get attacked by pictures that contain spam.  This bypasses normal text base spam filters allowing the picture to be posted with the hopes that someone viewing this picture will go to the channel name being spammed or to a website.  Typically these spammers always use the same exact picture which is why we are going to md5sum them.
- Many Telegram channels and group chats will see spam messages from spammers, hackers, and dark web illegal channels which include a picture.

# Ethos-MD5SUM Workflow Explained
EthosProject, using the Ethos-MD5SUM MODULE, will contain a DATABASE of records which are entered by EthosProject Administrators.  This is a service that EthosProject provides to combat spam and scam messages and to disallow picture content that matches records in the database.

EthosProject Administrators will (Through the GUI) populate a database with picture information including the MD5SUM of the pictures, the actual image, a thumbnail of the image, a description of the image, LABELS or TAGS for the image, the record creation date, the last time the picture was seen in any channel that has the Ethos-MD5SUM service account in it, the total amount of times seen, the channels that the picture has been seen in, the ACTIONS that the Ethos-MD5SUM SERVICE ACCOUNT bot should take when it sees it, and whether or not the ACTION is DISABLED or ENABLED.  
- This needed Database will be explained in the section below titled MEDIA DATABASE

In addition, any channel that subscribes to, or invites the Ethos-MD5SUM SERVICE ACCOUNT into, any Telegram Administrator in the channel using this service will have the ability to REPORT a picture to the Ethos-MD5SUM SERVICE ACCOUNT.  When a Telegram Channel Administrator replies to a spam picture with `/md5report`, then the Ethos-MD5SUM SERVICE ACCOUNT will examine the post that was reported (or replied to with `/md5report`) and perform the following actions by **creating** a new database record and populating a few fields:
- Downloads a copy of the photo into a database field 
- Runs an md5sum on the picture using Python's md5hash library to generate a hash value into a database
  - Then runs a check to see if this md5sum hash value already exists in another database record.
    - If it does exist in another record, then we don't need to create a new record and can abort the process
    - If it does NOT exist in another record, then we continue on
- Generates a thumbnail of the photo into a database field
- Adds a "REVIEW USER SUBMISSION" description to the description field in the database
- Adds a DISABLED by default into the MD5_STATUS field as before it can be ENABLED it will require an EthosProject operator to examine, approve, and mark ENABLED to begin monitoring and tracking of this information.
- The DATABASE that this will go into is described in the MEDIA DATABASE section below in more detail.



# Ethos-MD5SUM Requirements

### SERVICE ACCOUNT DATABASE (Global and Accessible by all of EthosProject and its MODULES)
  - There should be a global database created, which will be used by other modules in the EthosProject, which contains SERVICE ACCOUNTS.  SERVICE ACCOUNTS will be used by different modules, including this Ethos-MD5SUM project.  SERVICE ACCOUNTS are in their own small DATABASE and are different from bots within other modules but can be referenced by other modules.
    - SERVICE ACCOUNTS are our Telegram bots that are created with @botfather on Telegram which will be used by EthosProject MODULES.  SERVICE ACCOUNTS are bots that will be pulled into one to many Telegram group chats so they are shared in a way where one single Telegram bot account will be used by multiple channels.
    - The stand-alone SERVICE ACCOUNTS database should be easily referenceable by other EthosProject MODULES.
    - The database should contain the following fields:
      - **BOT_NAME:**  The name of the bot created using @botfather on Telegram
        - This will be manually entered by an EthosProject operator
      - **BOT_ID:**  The ID of the bot created using @botfather on Telegram
        - This will be manually entered by an EthosProject operator
      - **ETHOS_MODULES:**  The EthosProject MODULE name that this SERVICE ACCOUNT will be tied to.  (EG: Ethos-MD5SUM)
        - In future iterations, a SERVICE ACCOUNT will be able to be tied to multiple EthosProject MODULES.  This will allow for a single SERVICE ACCOUNT bot to be able to perform multiple functions (or MODULES) so take this into consideration when creating this field.  (EG: Ethos-MD5SUM, Ethos-Telegram-SwarmDetect)
      - **MD5_STATUS:**  By default when a SERVICE ACCOUNT is added into this database, it should default to the DISABLED status.  Within the UI for Ethos-MD5SUM, there will be a way to ENABLE or DISABLE this SERVICE ACCOUNT.
      - **MD5CHANNELS:**  The channels field will be used to display in the GUI a numerical value of the number of channels that the Ethos-MD5SUM SERVICE ACCOUNT has joined as is monitoring.
      - As the main EthosProject grows, there might be more fields that need to be added to this database which other MODULES will refer to or designate.  These fields in the future should be added when requested.
        - Future EthosProject MODULES will not need to create this database as they are introduced.  This global SERVICE ACCOUNT database should only be created once and I'm including this task into this Ethos-MD5SUM MODULE.  Future projects will leverage this database and potentially add new fields to the database if needed.
       
# Ethos-MD5SUM MEDIA DATABASE
This will be the main database that holds all of the information about images.  It will more more READ heavy than WRITE heavy by orders of magnitude.  

### MEDIA DATABASE STRUCTURE
This database will contain:
- Alpha-numeric data
- Images
- Thumbnail images
- Static Labels (one to many)  (EG:  SPAM, CRYPTO, DRUGS, WEAPONS, PII)
- Nuerical only data
- Boolean Data

### MEDIA DATABASE FIELDS
This database should contain the following fields for each record:
- **MD5HASH** - m5dsum hash values will be stored in this field
- **THUMBNAIL** - A thumbnail version of the picture being added (generated from RAW_IMAGE)
- **RAW_IMAGE** - A copy of the raw image of the picture being added
- **LABELS** - One to many LABELS - comma delimited
- **CREATION_DATE** - The date the record was created and saved by an EthosProject operator
- **LAST_SEEN** - A date that this image was last seen in any groupchat that the Ethos-MD5SUM SERVICE ACCOUNT is indling in
- **TIMES_USED** - A numerical value of the amount of times that this picture has been seen from any groupchat that the Ethos-MD5SUM SERVICE ACCOUNT is indling in
- **CHANNELS_SEEN** - A list of the IDs of channels where the Ethos-MD5SUM SERVICE ACCOUNT has seen this picture
- **ACTION** - The action to take when this picture is seen in any channel where the Ethos-MD5SUM SERVICE ACCOUNT is indling in (BAN, KICK, IGNORE)
- **STATUS** - Whether or not the record is ENABLED or DISABLED
       

