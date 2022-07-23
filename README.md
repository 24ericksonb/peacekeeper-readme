![image](https://user-images.githubusercontent.com/72327129/178548006-0255b086-4b47-4d62-89a9-3f0354f0189d.png)

# Peacekeeper Discord Bot
![image](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)
![image](https://img.shields.io/badge/Visual_Studio_Code-0078D4?style=for-the-badge&logo=visual%20studio%20code&logoColor=white)
![image](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![image](https://img.shields.io/badge/MySQL-005C84?style=for-the-badge&logo=mysql&logoColor=white)
![image](https://img.shields.io/badge/Amazon_AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![image](https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white)

### New Member and Verification
The player joins the server and has access to rules, and will receive a direct message from Peacekeeper asking them to verify their Tarkov username. The player must accept the rules and verify by direct messaging the bot using the `register` command to get the verified role (or if they are previously verified, they get it automatically upon joining the server). Once the player has the verified role, they now have access to the rest of the server.

### Match Queuing
Player types the `queue` command into their respective rank queue channel (one for each rank) to join the match queue. The bot adds them to the match queue and updates the members in the queue as players around their elo join. Once six players have joined the queue: Peacekeeper direct messages the players on the map, teams, captains (highest ELO players on each team), and the lobby creator (highest ELO player in the match). Once all members have joined the lobby and their respective voice chats, the lobby creator begins the raid.

### Match Reporting
Once the match ends (either everyone dies/extracts), the two team captains are instructed to get screenshots of the kill end screen from each of their teammates and then count the total number of points their team got (+5 points for enemy PMC kill, +1 for scav kill, +0 for no dogtag or friendly PMC kill). Next, each captain will report their team's score using the `score` command. If there is a discrepancy (one team believes the other team lied about points), an admin/staff will review and potentially change the match outcome using the `setscore` command. If there is no report from either side after a set interval, the match becomes voided.

### Rank and Elo Cutoffs

**Rank** | Unranked | E | D | C | B | A | S
--- | --- | --- | --- | --- | --- | --- | ---
**Elo**| 0-999 | 1000-1749 | 1750-2449 | 2500-3249 | 3250-3999 | 4000-4999 | 5000+

## [Database Design](https://github.com/24ericksonb/Peacekeeper/files/9126673/base.2.txt)
<p align="center"> <img src="https://user-images.githubusercontent.com/72327129/179373353-e0685473-e70e-4a86-a7be-1be3be621fc3.png" alt=""/> </p>

## Management Commands
:heavy_check_mark: `/derank`
- Drops everyone's rank to the lowerbound of that rank

:heavy_check_mark: `/namereset`
- Resets everyone's tarkov name linked to their discord

## Admin Commands
:heavy_check_mark: `/setelo <member> <new-elo>`
- _member_: discord.Member, discord member object of the user
- _new-elo_: int, the new elo to be assigned to the player
- Changes the elo associated with that Discord member
- **RESTRAINT:** elo must be positive
  
:heavy_check_mark: `/delstaff <member>`
- _member_: discord.Member, discord member object of the user
- Removes the staff role from the specified member
- **RESTRAINT:** specified member must have staff role

:heavy_check_mark: `/delwarn <member> <warn-id>`
- _member_: discord.Member, discord member object of the user
- _warn-id_: int, the id of the warn to be removed
- Removes the warn from the specified member
- **RESTRAINT:** specified member must have active warn with specified id

:heavy_check_mark: `/purge <number>`
- _number_: int, number of messages to be purged
- Purges the specified number of messages from the text channel
- **RESTRAINT:** number upper limit set to avoid long clear times

:heavy_check_mark: `/unban <member-id>`
- _member-id_: int, discord id of the user
- Unbans the specifed member
- **RESTRAINT:** specified discord id must be previously banned

## Staff Commands
:heavy_check_mark: `/ban <member-id> <reason>`
- _member-id_: int, discord id of the user
- _reason_: String, reason for the ban
- Bans the member for the specified reason

:heavy_check_mark: `/warn <member> <reason>`
- _member_: discord.Member, discord member object of the user
- _reason_: String, reason for the warn
- Warns the member for the specified reason

:heavy_check_mark: `/warnings <member>`
- _member_: discord.Member, discord member object of the user
- Lists the warns (and ids) of the specified user

:heavy_check_mark: `/mute <member> <reason> <seconds>`
- _member_: discord.Member, discord member object of the user
- _reason_: String, reason for the mute
- _seconds_: int, the seconds of the mute
- Mutes the member for the specified reason
- **RESTRAINT:** specified member must be current not muted

:heavy_check_mark: `/unmute <member>`
- _member_: discord.Member, discord member object of the user
- Unmutes the specified member
- **RESTRAINT:** specified member must be currently muted

:heavy_check_mark: `/close`
- Closes the ticket channel which the command was called in
- **RESTRAINT:** this command can only be called in the ticket sub text channels

:heavy_check_mark: `/setname <member> <new-name>`
- _member_: discord.Member, discord member object of the user
- _new-name_: String, the new tarkov name to be assigned to the player
- Changes the tarkov name associated with that Discord member

:heavy_check_mark: `/setscore <match-id> <team> <score>` 
- _match-id_: int, the match id
- _team_: boolean, 0 means team A, 1 means team B
- _score_: int: the new score for that team
- Updates the score for that team (eventually updating match results)

## Member Commands
:heavy_check_mark: `/register <name>`
- _name_: String, the tarkov name to be assigned to the player
- Assigns the tarkov in game name to the player
- **RESTRAINT:** Only can be done if the player has no register name

:heavy_check_mark: `/help`
- Request a direct message from bot with documentation of how to set up/join match

:heavy_check_mark: `/queue`
- Joins the queue to match against other players in same rank
- **RESTRAINT:** Only can queue if player has a resigistered name and is currently not queueing

:heavy_check_mark: `/leave`
- Leaves the queue if they are actively queuing
- **RESTRAINT:** Only players in queue (meaning not found a game) can leave the queue

:heavy_check_mark: `/status`
- Lists current status in the queue (ie how many slots filled)
- **RESTRAINT:** Only players in queue can find see the status of the queue

:heavy_check_mark: `/info [member]`
- _member_: discord.Member, discord member object of the user
- Lists the members info including creation date, date joined, warn number, elo, current rank, last five match outcomes
- **RESTRAINT:** Only staff members can call this command on members other than themselves

:heavy_check_mark: `/score <match-id> <score>`
- _match-id_: int, the match id
- _score_: int, the score their team recieved
- Reports the score their team earned within the match
- **RESTRAINT:** Only team captains of that match id can sucessfully report outcome

:heavy_check_mark: `/match <match-id>`
- _match-id_: int, the match id
- Returns information of the specifed match including players, teams, avg elo, date played, and outcome
