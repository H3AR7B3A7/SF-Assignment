# Pseudo Code Assignment
To keep it simple we will take a look at a *single thread solution*. 
Then we will try to see what can be done to optimize the execution using multiple threads.
For brevity, we will just list some possibilities.

## Single thread
Given a growing list of mails (listOfMails):

    recruitment = 0
    spam = 0
    sales = 0
    reception = 0
    
    intervalInMin = INPUT("Interval in minutes?")
    
    WHILE liveSorting
        get listOfMails
        listOfMails -> stream
            FOREACH mail of listOfMails
                IF contains "CV" (~ title / body / attachment ???)
                    forward to "recruitment@parkshark.com"
                    recruitment++
                    delete mail (* in mailbox)
                ELSE IF contains "Promo" OR contains "advertising" (~)
                    forward to "spam@parkshark.com"
                    spam++
                    delete mail (*)
                ELSE IF contains "sales" (~)
                    forward to "sales@parkshark.com"
                    sales++
                    delete mail (*)
                ELSE 
                    forward to "reception@parkshark.com"
                    reception++
                    delete mail (*)
        print # of mails
        sleep (intervalInMin toMillis)
        
    getListOfMails
        listOfMails = (List) GET someApi
        return listOfMails
        
    deleteMail
        DELETE someApi
        
    print # of mails
        total = recruitment + spam + sales + reception
        print: total, recruitment, spam, sales, reception

## Multiple threads
Things we could try to optimize:
- Use **parallelStream** instead
  - List of mails has to be big enough to improve execution speed
- Have **different threads each take a list**, then filter for containing a string before FOREACH
  - Some lists could be made to have more / less importance
  
---
<div align="right">-- Steven D'Hondt</div>
