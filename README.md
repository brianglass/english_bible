# English Bible
This package provides a simple Go-based tool for looking up passages of scripture. 
It presently uses the King James Version, otherwise known as the Authorized Version, 
and includes books often referred to as deuterocanonical or apocryphal. The books names
are expected to follow the Orthodox and Catholic naming conventions. For instance, 
1 Samuel is 1 Kings and 1 Kings is 3 Kings.

```golang
    if bibledb, e = sql.Open("sqlite3", "english.db"); e != nil {
        log.Printf("Got error opening database: %#v. Exiting.", e)
        os.Exit(1)
    }
    defer bibledb.Close()

    bible := english_bible.NewBible(bibledb)
  
    passage := bible.Lookup("Matt 1.1-25")
    
    // passage is a list of verses
    // Each verse has Chapter, Verse, Book, and Content attributes
    for _, verse := range passage) {
        fmt.Printf("%d. %s\n", verse.Verse, verse.Content)
    }
```

A reference includes a book name and a verse specification.  A specification 
can include multiple verse ranges separated by commas, where a range is a 
collection of 1 or more contiguous verses. Chapter specification can be implicit. 
A reference can look like any of the following:

* Matt 1.1-25
* Matt 4.25-5.13
* Matt 10.32-36, 11.1
* Matt 6.31-34, 7.9-11
* Matt 10.1, 5-8
* Mark 15.22, 25, 33-41
* 1 John 2.7-17
* Jude 1-10
* 1 Cor 5.6-8; Gal 3.13-14
* Prov 10, 3, 8
* Matt 26:2-20; John 13:3-17; Matt 26:21-39; Luke 22:43-45; Matt 26:40-27:2
* Baruch 3.35-4.4
* Wis 3.1-9

**Warning**: this function directly interpolates values from the reference into SQL. 
This is safe as long as the provided reference is coming from the database. 
In other words, this method might be unsafe when used to lookup user-provided 
references.
