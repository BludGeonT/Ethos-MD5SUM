# Ethos-MD5SUM Project

# Read PLANNING.md First Before Any Thinking or Planning or Coding
**First, before AI starts to think about this all, a mandatory step is to go through the PLANNING.md file in this repository to set some guidelines and guardrails and it should all be considered when making decisions or starting a plan to build it all.  Second, The AI agent will also go through the UI.md file to establish guidelines and general UI formatting and layout for specific areas within the UI and project.**


Meant to be a supplemental modular package to a parent program by providing tools that allow for a channel owner or administrator of a channel to have an ethos_md5sum_bot deployed to their channel which will:
- Allow an admin to reply to a spam picture message which downloads the spam picture file, runs md5sum on it, stores that information into a database along with labels and dates to create a robust record for each spam picture md5sum and details.
- Allow for  ethos_md5sum_bot to watch a channel in real time and downloading any pictures that are posted by subscribers or commenters and will run md5sum on it and if that value matches what is in the spam database, then the message will be removed, the user kicked or banned automatically.

# Ethos MD5SUM project Meanings
- In this article, when I refer to ethos-md5sum-bot, I am referring to a Telegram bot that has been added into the Ethos MD5SUM Project by a user with permissions to be used by this project.
- When I refer to the "Project" I mean this module that we are building specifically to manage Telegram spam with our Ethos MD5SUM utility which will contain a web UI, user management, bot management, database management, configurations, etc


# Ethos-MD5SUM Project Overview

Telegram group chat channels allow users to post text, videos, pictures, and other media.  There is a big problem with spam on Telegram and one of the biggest problems are the bots that post spam in the form of a picture (JPG, BMP).  Inside of this spam they hope for people to read their spam and then manually go type in the name of the person or channel within the picture which ultimately ends up leading to being scammed.  The problem is that there are spam control bots, such as Rose (@missrosebot) that can filter on text to trigger actions related to spam, but they cannot detect spam within a picture and thus the spammer can successfully spam their post.

The only alternative to this is to turn off the ability to post pictures altogether unless you are in the approved users list for Rose for that channel.  This is not optimal and can frustrate subscribers as many like to post memes and engage in the conversation in a fun way.  Rose has a limitation of approved users set to 200 for a channel.  While that might seem like a lot it requires manual (for now) intervention to approve each user that shouldn't be affected by the non-picture posting rule in effect.

This Ethos-MD5SUM project will allow for a bot to monitor a channel that has pictures enabled.  Once a subscriber posts a picture, the bot will analyze this picture in one of two ways.  First, the bot can perform an md5sum on the picture and see the resulting hash.  If this hash matches a hash in the database of spammers, then the picture will be deleted from Telegram, archived in the Ethos-MD5SUM media database and the subscriber will either be kicked or banned.

## MD5SUM Online tool ideas to use
There are many online md5sum tools.  There might be an easier way for an agent to perform this task by using other workflows or MCP modules that minimize coding but also provide the quickest and most cost friendly results.  I will provide an example of a website that will do this type of conversion.  The critical component of this is that the bot agent should use the same tool to generate an md5sum hash so that we can be confident on the values generated.  
- Example Website: https://emn178.github.io/online-tools/md5_checksum.html
  - Has selectable input tile, being File or URL.  The bot should figure out the easist method to use one of thes choices.
  - Files can be dragged and dropped to the place on the screen or a file can be selected.

There might be ways to feed a picture or document into a website that has an API to access as opposed to manipulating a website through an emulator and I want the LLM to offer ideas related to the best solution for this.  It is important that the bot will use the same method for each observation of a picture to generate an md5sum hash and search the database for a match.  The agent should use the quickest and least resource intensive method to perform this as this will be happening very frequently in busy channels.  

In addition to downloading, archiving, labeling, and storing the offending picture being used by spam.  There should be the ability for the bot to extract text from the imagine using OCR technology.  This feature should be considered when AI designs things and will be implemented as additional features once the version iterations are operational.

## Requirements To Use Ths ethos_md5sum_bot trigger(s) Within Telegram by Human Users which MUST be Admins of the channel as well
We do not want everyone in the channel to be able to issue these commands to the ethos_md5sum_bot.  We only want to allow the admins of the channel that the ethos_md5sum_bot is in to be able to issue commands.  If a regular, **non-admin user**, tries to issue a `/md5add` command, this ethos_md5sum_bot should ignore any of their / commands.  Only ADMINS of the channel should be able to successfully issue these commands.
- This ethos_md5sum_bot should only listen in a Telegram channel for the following commands.  Once the bot sees someone issue one of the below commands, it should validate that the user is also an admin for that channel first and deny the operation which should be logged.

### The Only Valid base ethos_md5sum_bot commands as of now:
The ethos_md5sum_bot will only react and take actions on the base commands below if they are issued by a human being, WHICH IS AN ADMIN IN THE CHANNEL, as a reply to a picture spam Telegram message.  These require additional arguments or parameters but these are the base commands.  This specific ethos_md5sum_bot will ignore all other / (slash) commands **unless** they match in the list below.  Future iterations of this will be place into its own Markdown file named COMMANDS.md5 but for now, use these below available commands as the authoritative commands available.
- Command: `/md5add`
- Command: `/md5test`

Any future related ethos_md5sum_bot command should be added to the authoritative list above (or inside the `COMMANDS.md5` file.  This is to protect the bot from any malicious attempts to find a command that isn't locked down.

#### Error Conditions When Non-Admins Try Executing ethos_md5sum_bot Commands
- If someone that is NOT an admin type in a command that appears in the list above of valid commands they will get a reply back to them from the bot saying "**ERROR - You are not authorized to run this function**" and a log will be written in the overall ledger for the project in ethos_md5sum_bot and into the assigned Telegram logging channel.
- If an admin in the channel issues any of the commands shown above in the **Valid base ethos_md5sum_bot command list**, then the bot should proceed with taking the next actions.



## Methods and Functions For ethos_md5sum_bot to Use
There are several different functions that this bot should have access to.  These will allow for manually setting a new hash within the web interface that will be added to a persistent database.  This database record containing the hash should also have other fields that are configurable and will be used from other Ethos components to contribute to and share data for the entire suite of Ethos tools.

## ethos-md5sum-bot Run Levels
This designation will be a selectable item for each bot that has been added into the project using the GUI.  There should be a way to click on one, several, or all bots and change their ethos-md5sum-bot Run Levels.  Their run levels will let the system know what the bot will be doing.  Specifications for Run Levels are below.
### Run Level 1 - Reporting Pictures by Reply Only - Default
- *Run Level 1* means that one of the bots in the database that is assigned to a channel will be sitting idle in the channel but will not be monitoring every post from comments containing a picture.  This is probably the better choice for a very busy channel and is the default.
- When someone posts a spam picture, an admin will be able to type in the command `/md5add <arguments>` as a reply to the spam post.  This will tell the bot to save a copy of the picture from the replied-to post, and then will run an md5sum on it using the method already talked about in this doc, and then finally store this information in the hash database as mentioned below.
  - The bot should reply back to the admin that ran the `/md5add <arguments>` command that the operation to add this picture into the database was successful or if there was an error.  Results should be also sent to the designated Telegram logging channel.

Once a new hash is added, it will exist in the database but will not become live until the addition is reviewed by a human being.  The database view UI of all hashes and other information should have a section that shows all of the recent hash additions requiring approval.  When the user clicks one of those new hashes needing approval, they will see in a neat view the picture being added and all of the information that was submitted from the `/md5add` command and its arguments (which contain the description, etc (all detailed below)).  

Once a new hash is approved, it then becomes live so that any other bots that are currently running and running in Run Level 2 can take advantage of the newest md5sum hashes that are spam.  When looking at the main view and interface for visualizing and/or managing the database(s), it should be easy to see which hashes are Live and Approved, which ones are disabled and when clicking any of the bots on the list it will open a new page of containing many more details about the bot.

### Run Level 2 - Live monitoring
- *Run Level 2* means that one of the bots in the database will be in a channel, have admin permissions in that channel, and will sit and monitor all messages that come into the group chat.  It will ingore all messages that are text only and take no actions.  It is specifically looking for pictures (like JPG files) that are posted as a comment and may or may not contain any text.  When the bot sees a message like this then it will then act on it as follows:
  - Download the picture
  - Perform an md5sum on the picture with the best method but it must be the same method every time
  - If the md5sum hash matches a hash within the database of hashes, then actions need to be taken based on the field data in the record for that hash talked about below in a different section but also remembering to increment the time that hash was seen (also in the fields below).
    

## ethos-md5sum-bot Bot Management
This Bot Management part of project should be a section within the UI, on the main screen (and eventually it's own page), to allow for bots to be added into the system and then saved and stored so that there can be other interfaces and reports that can be generated from all of the information the bots will gather.
- **This must be its own Database or equivalent separate from the Main Media Database**
  - These Databases (Bot Management) and (Main Media Database) should be able to reference each other's data as needed and queries to be constructed to join them when needed, etc.

When logging into the UI, there will be a webpage UI that comes up with many different features and functions.  These will be described in the `UI.md` file but it's important that there is a user interface where there is a master admin account which can then add other users in so they can log onto the system.  Permission will be configured later in its own `PERMISSIONS.md` file.  

### Adding an ethos_md5sum_bot into the Project section for Bot Management
There should be something like a Google Sheets configured to store the information about the bots that will be registered into the Project by an authenticated user.  This database (or database equivalent) should be secure as it will store critical API Token information for Telegram.  The recommended field name and contents for each record in the database is below.

## Ethos-MDSum Project Bot Management Database Layout
After logging in, there needs to be a section that displays the current bots that are registered in the Ethos MD5Sum Utility that we are creating. New bots will be added by clicking an `Add Bot` button, which then will prompt for the bot's information (This should be presented as a section on the screen of the Ethos-MD5sum project UI after authenticating and being logged in:
- **DB Field Name:** `ethos_md5sum_bot_id`
  - **Description:** ID number of the bot account
- **DB Field Name:** `ethos_md5sum_bot_name`
  - **Description:** A name for the bot.  This can be the {username} of the bot or can be provided by the authenticated user adding the bot.  
- **DB Field Name:** `ethos_md5sum_bot_channels`
  - **Description:** The ID of the Channel or Channels that this bot will be idling in, listening in, and monitoring in
- **DB Field Name:** `ethos_md5sum_bot_API_token`
  - **Description:** The ethos_md5sum_bot's Telegram API Token value
- **DB Field Name:** `ethos_md5sum_bot_runlevel`
  - **Description:** The bot's Run Level (This should be set to 1 by default and should be auto filled in for the authenticated user adding a bot and not editable)
- **DB Field Name:** `ethos_md5sum_bot_logchan`
  - **Description:** The Channel ID of the log channel `LOGCHANNEL` so that confirmations can be sent for important bot actions and state changes
    - The logging channel is not critical to operation and the bot should be able to function if there is no logging channel specified
- **DB Field Name:** `ethos_md5sum_bot_state`
  - **Description:** `ACTIVE` or `NOTACTIVE`
    - When creating a new bot and adding in the information from the UI, the value of the ACTIVE or NOTACTIVE should be set to NOTACTIVE.  This will prevent a bot from running out of control due to data entry errors.  There should be a method to change the bot's state to ACTIVE or NOTACTIVE using a button on the UI.
      - A **ACTIVE** bot means that the bot is able to accept a RUN LEVEL and will function based on the RUN LEVEL
      - A **NOTACTIVE** This is the DEFAULT state for adding a new bot in with the UI as a safety precaution.  If a bot is set to be NOTACTIVE then it will do nothing and be paused until a user sets it back to ACTIVE again.

From there, there should be a way to select a specific bot, and press a button that says `ACTIVATE` which then tells the bot to begin to listen to the channel it has been assigned to.  A verification needs to be made that the bot is listening and can interact in the channel so when someone presses the `ACTIVATE` button, the bot needs to print out a message saying "Ethos md5sum monitoring enabled successfully.." after functionality validation has taken place.
- Reference the `UI.md` file for visualizations and mockup ideas.

### When a bot is changed to ACTIVE
Within the interface on the page where it shows and manages the added bots and their values (detailed above), there should be a way to select a single bot, multiple bots, or all bots and then followed by two buttons that say ACTIVE or NOTACTIVE and if the user clicks ACTIVE, then this section applies below:
- When a bot is changed to ACTIVE it then will begin listening in the channel that is specified in its record and watch for comments that contain a picture.
  - A log entry will be made in the main log audit file for this project detailing which bot was activated, when, etc
- When a bot is changed to ACTIVE it will post a message stating that the bot has been actived into the associated `LOGCHANNEL` which will be configured along with each bot added into the Project.

### When a bot is changed to NOTACTIVE
Sometimes a bot will need to be deactivated for various reasons.  The same method applies as activating bots.  The user will see a list of bots that have been added into the Project and can select one, many, or all and clicking the NOTACTIVE button from the UI.
- When a bot is NOTACTIVE, or deactivated, it then will stop listening in the channel that is specified in its bot record.
  - A log entry will be made in the main log audit file for this project detailing which bot was deactivated and now in a NOTACTIVE state, when, etc
- When a bot is deactivated (NOTACTIVE) it will post a message stating that the bot has been deactivated into the associated `LOGCHANNEL` which will be configured along with each bot added into the Project.

### Manually Adding A New Picture Hash
This part of the Ethos-md5sum should have a web UI that allows for a couple of different methods for adding a new hash to the database.
- Stressing again that the same method needs to be used by the Ethos-MD5SUM tool when generating md5sum hashes.
  - The most commonly used industry standard for generating md5sum hashes on files should be integrated into the Ethos-MD5SUM system so that it does not have to make Internet calls to generate and retrieve md5sums.  It should be a function contained within the Project that can be used in the ways needed.
#### Process of manually uploading a picture to generate a new hash and record in the database
- The user should have a place within the UI to upload or paste in a picture from the user's computer or from their clipboard.
  - The Ethos-MD5SUM poject tool should then perform an md5sum on the picture being added and populate the database field with this value automatically and display it to the user as an un-editable field as they go down the list of filling in the information about the picture.
- The user should also see a field to add a description to this photo
- The user should also see a way to attach labels to this photo *(eg: impersonator, spam, scam, crypto, terrorism, drugs, weapons, porn)*
- The user should also see selectable choices on the actions to perform once a hash match has been determined.
  - `Ban` - The spammer will be banned and their post should be deleted and this information is logged into the logging section and a message delivered to the Telegram logging channel.
  - `Kick` - The spammer will be kicked from the channel and their post should be deleted. this information is logged into the logging section and a message delivered to the Telegram logging channel.
  - `Nothing` - The spammer not be affected.  This can be used for training pourposes
#### Process of replying to a spam message containing a picture with `/md5add` followed by a few arguments
- The admin should be able to reply to a spam picture with the command `/md5add` which the ethos_md5sum_bot will listen for.
  - This will be done by using a set of arguments which the bot will parse and correctly add into the database
  - Arguments can be arranged in any order after the `/md5add` trigger.
    - **THE ARGUMENTS:**
      - **-d [DESCRIPTION]** - 1st Argument Example:  `/md5add -d Fake Trump post for gitmo_tv spam`
      - **-l <LABEL1, LABEL2, ...>** - 2nd Argument Example:  `/md5sum -l impersonator, spam`
        - Available LABELS to use will eventually be put into a `MEDIALABELS.md` file which will be authoritative as to what labels can be created and/or used on each media item.  For now, use the below LABELS LIST
        - Labels will be added into the record of the hash being added.  All labels should be converted into UPPER CASE before being stored in a section that will be populated as choices when a user manually adds a new hash.
        - Labels must end up being stored into the database as one of the below choices.  Any lables that a user adds during the process of adding a new hash by replying to a spam picture that don't match the below selections should be analyzed and converted into the closest label from the below list.
          - LABELS LIST - Labels will be and must be stored in UPPER CASE and will have colors assigned to them and, for now, must be one of the following selections and these seletions should be the items to choose from when manually adding in a new hash (later to be specified in the `MEDIALABELS.md` file:
            - `IMPERSONATOR`
            - `SPAM`
            - `SCAM`
            - `CRYPTO`
            - `TERRORISM`
            - `DRUGS`
            - `WEAPONS`
            - `PORN`
            - `COUNTERFEIT`
            - `CLONES`
            - `FAKEID`
            - `PASSPORTS`
            - `BANKACCT`
      - **-a <ban, kick, nothing>** - 3rd Argument Example:  `/md5sum -a ban`
  - As mentioned earlier, these arguments can be entered in any order.  For any argument that is not provided, the bot will fill in the field with information autmatically that will be added into the database.  If no data is provided for each of the now 3 arguments that are required upon seeing an admin trigger /`md5add` as a reply to a spam message, then either a DEFUALT will be used or a standard value will be added for that field in the database.  The defaults are shown below which should be used if the user provides not enough or no arguments at all to a `/md5add` command.
    - **ARGUMENT DEFAULTS**
      - **-d Argument** - DESCRPTION
        - If there is no `-d` argument at all, or if the `-d` argument is used but there is no argument data provided, then use the default value of `NEEDSDESCRPTION` to store into the database record for this new picture has.
          - There should be a small utility in the UI to search for records that have descriptions as `NEEDSDESCRIPTION` and can assist the user to go back and generate a report of these hashes and then go apply more meaningful descriptions manually.
      - **-l Argument** - LABEL
        - If there is no `-l` argument at all, or if the `-l` argument is used but there are no argument data provided, then use the default value of `NEEDSLABEL` to put into the hash record database for this picture.
          - There should be a small utility in the UI to search for records that have labels as `NEEDSLABEL` and can assist the user to go back and generate a report of these hashes and then go apply more meaningful labels manually.
        - If the user provided argument, after being converted to uppercase, is not exactly KICK, BAN, or NOTHING then their answer should be analyzed and checked for typos to determine what the user probably meant to type in.  If there is too much doubt about determinating, then go with the default value of `KICK`
      - **-a Argument** - ACTION TO TAKE
        - If there is no `-d` argument at all provided by the admin, or if the `-d` argument is used but there is no argument value data provided, then use the default value of `KICK` to put into the database.

#### Example of full /md5add commands with arguments that are replies to a picture from an admin
Here are a few examples of ways that Admins will reply to a picture to add a new hash record into the database.
- **Example 1 reply to a spam picture:**  `/md5add -d gitmo_tv generic spam picture -l spam -a ban`
  - Example 1 Actions
    - If the `-l` and `-a` data entered was not already upper case, then the bot will convert these fields to UPPER CASE:
      - `md5add -d gitmo_tv generic spam picture -l SPAM -a BAN`
    - Then the bot will go take the following actions:
      - It will download and save the picture being reported and save it into the new database record along with other information
      - It will use the same online md5sum tool every time to generate a md5sum hash
      - It will follow the steps below detailing all of the fields that need to go into the record for the newly stored has
      - It will be flagged in the UI that it needs approval to go live
      - It will reply back to the person issuing the reply with the bot trigger with a message like "{username} - This picture and its has has been stored in the system successfully" if the operation was successful.  If the operation was not successful, the bot will reply back to the person issuing the trigger with "{username} - There was a problem storing this new picture and hash, pleas notify an Adminstrator."
      - The bot will report this error to the Project and should log the error message, detailing what was being tried and which step failed so that troubleshooting the problem can be easier.

- **Example 2 reply to a spam picture:** `/md5add -a kick`
  - Example 2 Actions
    - The bot will recognize that there has been no `-d` specified by an admin from the Telegram channel for description so it will store `NEEDSDESCRIPTION` as this database record Description field data.
    - The bot will recognize that there has been no `-l` specified for label(s) so it will store `NEEDSLABEL` as this database record Labels field data.
    - The bot will store `KICK` into the database record Actions field data.
    - Then the bot will go take the following actions:
      - It will download and save the picture being reported and save it into the new database record along with other information
      - It will use the same online md5sum tool that was built within the Project every time to generate a md5sum hash
      - It will follow the steps below detailing all of the fields that need to go into the record for the newly stored has
      - It will be flagged in the UI that it needs approval to go live
      - It will reply back to the channel admin issuing command from Telegram with a message like "{username} - This picture and its has has been stored in the system successfully" if the operation was successful.  If the operation was not successful, the bot will reply back to the person issuing the trigger with "{username} - There was a problem storing this new picture and hash, pleas notify an Adminstrator."
      - {username} is a Telegram variable that will display the full username of the Telegram account.  Any time you see me reference something in curly braces, this means that these are Telegram Variables that we can leverage.  The available variables pertaining to an account should be easily findable online for the agent to learn and use and also suggest ideas.
      - The bot will report this error to the Project and should log the error message, detailing what was being tried and which step failed so that troubleshooting the problem can be easier.

## Database Structuring and Data Organization
### Database Ideas
- I Would like the database to contain several fields of numerical or alphanumeric contents but also a picture of the spam message needs to be saved directly from Telegram.
  - This might make things easier to store the downloaded media from Telegram when a user posts a picture to store the pictures and with their hash value in one area.
  - Then another database, keyed off the hash value as the primary fiend followed by the other data fields shown below in the **Main Media Database** section.

Looking for the most cost savings way of storing pictures and data into multiple databases, perhaps Google Sheets can do this effectively.  It needs to be figured out if two separate databases are better or if a single database can store the pictures (and eventually videos) as they are added into the Ethos md5sum system.



### Main Media Database

#### Each database record should be stamdardized and should contain fields for the following pieces of information (name the DB fields in the DB to be the following):
- **md5sum_hash** - the md5sum result using the same source and method to generate md5sum hashes
- **description** - A description of the photo and how it's being used, where
- **labels** - One or many labels will be stored in the record
- **action** - What action will be taken when the bot observes someone posting a picture that has a md5sum hash value that matches one in the database - kick, ban, or nothing
- **md5date** - The time and Date of hash record creation, dynamically added when a /md5add is issued or when a record is manually created
- **last_date_seen** - The last time and date of the has record being matched
- **total_times_seen** - The cumulative total of times the bot has observed this record
- **seen_in_channels** - As multiple bots start to monitor and observe multiple channels, we want to also keep track of the channels where this picture has been observed and this can be one to many channels and the channel ID should be recorded and stored.  This will help provide valuable data for other Ethos modules in the Telegram section.
- **privacy_filter** - This is a boolean value in the database.  If a picture or piece of media in the database is flagged as 1, or "enabled", then that picture or piece of media will be blurred out on the thumbnails that are generated and in order to view this picture without the Privacy Filter blurring, a button must be clicked to "Acknowledge" the risks of viewing this file and should be logged in the audit log of who the Project user was along with time and date.
- If this value is 0, or "disabled", then there are no restrictions on viewing the contents of the file or media item.
- The Project, when displaying thumbnails when browsing the Media Database, should obey this privacy_filter rule.
   
