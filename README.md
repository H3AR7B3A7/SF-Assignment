# Pseudo Code Assignment
The handout can be found [here](https://github.com/H3AR7B3A7/SF-Assignment/blob/master/assignment-handout.pdf).  
There were two assignment extensions as well: [1](https://github.com/H3AR7B3A7/SF-Assignment/blob/master/extension-1-handout.pdf) and [2](https://github.com/H3AR7B3A7/SF-Assignment/blob/master/extension-2-handout.pdf).

### Preface
To keep it simple we will take a look at a *single thread solution*. 
Then we will try to see what can be done to optimize the execution using multiple threads.
For brevity, we will just list some possibilities.

## Single thread
Given a growing list of mails (listOfMails):

    // To prevent data loss on power outage or shut downs,
    // we can fetch these from 'csv' (or any table/file we write to)
    // before the 'infinite' loop is started. 
    // (A reset could be asked through one of the args as a boolean.)
    
    recruitment = 0
    spam = 0
    sales = 0
    reception = 0
    
    spamReferenceList = null
    duplicate = 0
    
    get arg0 (reset): [default false]
    if arg0 = false
        recruitment = Read from CSV/file
        spam = Read from CSV/file
        sales = Read from CSV/file
        reception = Read from CSV/file
        duplicate = Read from CSV/file
        
    get arg1 (interval): [default 10]
        intervalInMin = INPUT("Interval in minutes?")
    
    WHILE liveSorting
        get listOfMails
        listOfMails -> stream
            if listOfMails != null
                FOREACH mail of listOfMails
                    // Using Apache commons string utils 'containsIgnorCase
                    IF contains "CV" (~ title / body / attachment ???)
                        forward to "recruitment@parkshark.com"
                        recruitment++
                    IF contains "sales" (~)
                        forward to "sales@parkshark.com"
                        sales++
                    IF contains "Promo" OR contains "advertising" (~)
                        IF NOT in spamReferenceList
                            forward to "spam@parkshark.com"
                            spam++
                            add to spamReferenceList
                        ELSE 
                            duplicate++
                    ELSE 
                        // devide work among 2 people (can use a modulus and switch case for more people)
                        if reception is even   
                            forward to "reception@parkshark.com"
                            reception++
                        else 
                            forward to "jobstudent@parkshark.com"
                            reception++
                    delete mail (* in info@parkshark mailbox)
            else
                liveSorting = false     // To stop program when there are no more new mails,
                                        // but having the program sleep for some time would make
                                        // more sense. This way it doesn't need to be restarted.
                // Execute some extra code before shut down
        print # of mails
        sleep (intervalInMin toMillis)
        
    getListOfMails
        listOfMails = (List) GET someApi // The single most simplified line in this writing
        return listOfMails
        
    deleteMail
        DELETE someApi
        
    print # of mails
        total = recruitment + spam + sales + reception
        print: total, recruitment, spam, sales, reception
        // Can print to csv file to save data 
        // This csv can be consumed by some other code and do more calc by adding collumns
        // For example (current - previous) for data on 1 loop
        
    print # of mails including duplicates
        total = recruitment + spam + sales + reception + duplicates
        print: total, recruitment, spam + duplicates, sales, reception

## Multiple threads
Things we could try to optimize:
- Use a **parallelStream** instead
  - List of mails has to be big enough to improve execution speed
- Introduce a **thread pool** and pass what comes after FOREACH to executors
- Have **different threads each take a list**, then filter for containing a string before FOREACH
  - There would be no future issues around the order in which the checks happen
  - Some lists could be made to have more / less importance

## Questions for 'Park Shark'
- Where does the list of mails come from? (We only have the address so far.)
- What is the average amount of mails received in the mailbox?
- What are the preferences regarding the interval to check the mailbox?
  - What should be the frequency?
  - Should it be changeable?
- Where will this program be running?
  - Should this program have an own interface? 
  - Or will it be integrated and run with command line arguments?
- ...

---
<div align="right">-- Steven D'Hondt</div>