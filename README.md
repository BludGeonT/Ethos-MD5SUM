# Ethos-md5sum
A unique way of performing an MD5SUM on files posted in a Telegram chat, specifically pictures.

# Ethos-md5sum Project Overview

Telegram group chat channels allow users to post text, videos, pictures, and other media.  There is a big problem with spam on Telegram and one of the biggest problems with it are the bots that post spam in the form of a picture (JPG, BMP).  Inside of this spam they hope for people to read their spam and then manually go type in the name of the person or channel which ends up leading to being scammed.  The problem is that there are spam control bots, such as Rose (@missrosebot) that can filter on text to trigger actions related to spam, but they cannot detect spam within a picture and thus the spammer can successfully spam their post.

The only alternative to this is to turn of the ability to post pictures altogether unless you are in the approved users list for Rose for that channel.  This is not optimal and can frustrate subscribers as many like to post memes and engage in the coversation in a fun way.  Rose has a limitation of approved users set to 200 for a channel.  While that might seem like a lot it requires manual (for now) intervention to approve each user that shouldn't be affected by the non-picture posting rule in effect.

This Ethos-md5sum project will allow for a bot to monitor a channel that has pictures enabled.  Once a subscriber posts a picture, the bot will analyze this picture in one of two ways.  First, the bot can perform an md5sum on the picture and see the resulting hash.  If this hash matches a hash in the database of spammers, then the picture will be removed and the subscriber can either be kicked or banned.

## MD5SUM Online tool to use
There are many online md5sum tools.  There might be an easier way for an agent to perform this task.  I will provide an example of a website that will do this type of conversion.  The critical component of this is that the bot agent should use the same tool to generate an md5sum hash so that we can be confident on the values generated.  
- Website: https://emn178.github.io/online-tools/md5_checksum.html
  - Has selectable input tile, being File or URL.  The bot should figure out the easist method to use one of thes choices.
  - Files can be dragged and dropped to the place on the screen or a file can be selected.

There might be ways to feed a picture or document into a website that has an API to access as opposed to manipulating a website through an emulator.  It is important that the bot will use the same method for each observation of a picture to generate an md5sum hash and search the database for a match.  The agent should use the quickest and least resource intensive method to perform this as this will be happening very frequently in busy channels.  

## Requirements To Use Ths ethos_md5sum_bot trigger(s)
We do not want everyone in the channel to be able to issue these commands to the ethos_md5sum_bot.  We only want to allow the admins of the channel that the ethos_md5sum_bot is in to be able to issue commands.  If a regular, non-admin user, tries to issue a /md5add command, this ethos_md5sum_bot should ignore any of their / commands.  Only ADMINS of the channel should be able to successfully issue these commands.
- This ethos_md5sum_bot should only listen in a Telegram channel for the following commands.  Once the bot sees someone issue one of the below commands, it should validate that the user is also an admin for that channel first and deny the operation which should be logged.
  - **Valid ethos_md5sum_bot commands as of now:**
    - `/md5add`
- The ethos_md5sum_bot should be an admin in the channel that it will be monitoring and listening in.  If the bot is not an admin, and if someone tries to issue one of its commands, the bot should reply back saying "ERROR:  I MUST BE AN ADMIN TO EXECUTE THAT COMMAND".

Any future related ethos_md5sum_bot command should be added to the authoritative list above.  This is to protect the bot from any malicious attempts to find a command that isn't locked down.

## Methods and Functions For ethos_md5sum_bot to Use
There are several different functions that this bot should have access to.  These will allow for manually setting a new hash within the web interface that will be added to a persistent database.  This database record containing the hash should also have other fields that are configurable and will be used from other Ethos components to contribute to and share data for the entire suite of Ethos tools.

### Manually Adding A New Hash
This part of the Ethos-md5sum should have a web UI that allows for a couple of different methods for adding a new hash to the database.
#### Process of manually ploading a picture to generate a new hash and record in the database
- The user should have a place within the UI to upload or paste in a picture from the user's computer or from their clipboard.
- The user should also see a field to add a description to this photo
- The user should also see a way to attach labels to this photo *(eg: impersonator, spam, scam, crypto, terrorism, drugs, weapons, porn)*
- The user should also see selectable choices on the actions to perform once a hash match has been determined.
  - `Ban` - The spammer will be banned and their post should be deleted
  - `Kick` - The spammer will be kicked from the channel and their post should be deleted
  - `Nothing` - The spammer not be affected.  This can be used for training pourposes
#### Process of replying to a spam message containing a picture with `/md5add` followed by a few arugments
- The admin should be able to reply to a spam picture with the command `/md5add` which the ethos_md5sum_bot will listen for.
  - This will be done by using a set of arguments which the bot will parse and correctly add into the database
  - Arguments can be arranged in any order after the `/md5add` trigger.
    - **The Arguments:**
      - **-d [DESCRIPTION]** - 1st Argument Example:  `/md5add -d Fake Trump post for gitmo_tv spam`
      - **-l <LABEL1, LABEL2, ...>** - 2nd Argument Example:  `/md5sum -l impersonator, spam`
        - Labels will be added into the record of the hash being added.  All labels should be converted into UPPER CASE before being stored in a section that will be populated as choices when a user manually adds a new hash.
        - Labels must end up being stored as one of the below names.  Any lables that a user adds during the process of adding a new hash by replying to a spam picture that don't match the below selections should be analyzed and converted into the closest label from the below list.
          - Labels will be and must be stored in UPPER CASE and must be one of the following selections and these seletions should be the items to choose from when manually adding in a new hash:
            - IMPERSONATOR
            - SPAM
            - SCAM
            - CRYPTO
            - TERRORISM
            - DRUGS
            - WEAPONS
            - PORN
            - COUNTERFEIT
            - CLONES
            - FAKEID
            - PASSPORTS
            - BANKACCT
      - **-a <ban, kick, nothing>** - 3rd Argument Example:  `/md5sum -a ban`
  - As mentioned earlier, these arguments can be entered in any order.  For any argument that is no provided, the bot will fill in the field with information autmatically that will be added into the database.  If no data is provided for each of the now 3 arguments that are required upon seeing someone trigger /`md5add` as a reply to a spam message, then either a default will be used or a standard value will be added for that field in the database.  The defaults are shown below which should be used if the user provides not enough or no arguments at all to a `/md5add` command.
    - **ARGUMENT DEFAULTS**
      - **-d Argument** - DESCRPTION
        - If there is no `-d` argument at all, or if the `-d` argument is used but there is no argument data provided, then use the default value of `NEEDSDESCRPTION` to store into the database record for this new picture has.
          - There should be a small utility in the UI to search for records that have descriptions as `NEEDSDESCRIPTION` and can assist the user to go back and generate a report of these hashes and then go apply more meaningful descriptions manually.
      - **-l Argument** - LABEL
        - If there is no `-l` argument at all, or if the `-l` argument is used but there are no argument data provided, then use the default value of `NEEDSLABEL` to put into the hash record database for this picture.
          - There should be a small utility in the UI to search for records that have labels as `NEEDSLABEL` and can assist the user to go back and generate a report of these hashes and then go apply more meaningful labels manually.
        - If the user provided argument, after being converted to uppercase, is not exactly KICK, BAN, or NOTHING then their answer should be analyzed and checked for typos to determine what the user probably meant to type in.  If there is too much doubt about determinating, then go with the default value of `KICK`
      - **-a Argument** - ACTION TO TAKE
        - If there is no `-d` argument at all, or if the `-d` argument is used but there is no argument value data provided, then use the default value of `KICK` to put into the database.

#### Example of full /md5add commands that are replies to a picture from an admin
Here are a few examples of ways that Admins will reply to a picture to add a new hash record into the database.
- Example 1:  /md5add -d gitmo_tv generic spam picture -l spam -a ban
  - Example 1 Actions
    - Bot converts this user submitted command into the following (noticing the upper casing of -l and -a):
      - /md5add -d gitmo_tv generic spam picture -l SPAM -a BAN
    - What will happen by the ethos_md5sum_bot as a result:
      - It will download the picture being reported
      - It will use the same online md5sum 

## Saving a copy of the picture
In addition to adding into the database the hash value of the picture or photo being added by replying to a spam message, the bot should save a copy of the actual picture file and also added into the database.  

Each database record then should contain fields for:
- md5sum Hash - the md5sum result using the same source and method to generate md5sum hashes
- Description - A description of the photo and how it's being used, where
- Labels - One or many labels will be stored in the record
- Action - What action will be taken when the bot observes someone posting a picture that has a md5sum hash value that matches one in the database - kick, ban, or nothing
- Date - The time and Date of hash record creation, dynamically added when a /md5add is issued or when a record is manually created
- Last Date Seen - The last time and date of the has record being matched
- Total Times Seen - The cumulative total of times the bot has observed this record
- Seen In Channels - As multiple bots start to monitor and observe multiple channels, we want to also keep track of the channels where this picture has been observed and this can be one to many channels and the channel ID should be recorded and stored.  This will help provide valuable data for other Ethos modules in the Telegram section.
   
