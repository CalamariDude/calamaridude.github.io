---
title:  "Lechuck Writeup"
date:   2022-08-7 17:22:18 -0400
---
## from HackArmour

Lechuck

Visiting `https://challenges.hackrocks.com/lechuck`, checked all storage, cookies, network requests - all clean

guessed `https://challenges.hackrocks.com/lechuck/flag` - `404` are handled with a "Nothing to see here..." - nothing worthwhile

Read prompt and tried every word as an endpoint - all `404`

Dirbusted with gobuster:

`gobuster -fw -u challenges.hackrocks.com/lechuck/ -w ./common.txt dir | grep "(Status: 200"`

tried 5 different files , opened up two hints and hint #2 said use /user endpoint which yielded this message:

>To get info about a user, use the syntax: `/user/<username>`

username had to be lechuck from context of the problem which returned flag

