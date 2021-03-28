# Spidering-Modeling-and-Visualizing-Email-Data
The idea is that we have an API out there that will give us a mailing list. Given a URL that we just hit over and over again changing this little number and then we're going to be pulling this raw data and then we'll have analysis cleanup phase and then we're going to visualize this data in a couple of different ways.

# <h2>Data Flow </h2>

The first step is to spider the gmane repository.  The base URL  is hard-coded in the gmane.py and is hard-coded to the Sakai developer list.  You can spider another repository by changing that base url.   Make sure to delete the content.sqlite file if you switch the base url.  The gmane.py file operates as a spider in that it runs slowly and retrieves one mail message per second so as to avoid getting throttled by gmane.org.   It stores all of its data in a database and can be interrupted and re-started as often as needed.   It may take many hours to pull all the datadown.  So you may need to restart several times.

The program scans content.sqlite from 1 up to the first message number not already spidered and starts spidering at that message.  It continues spidering until it has spidered the desired number of messages or it reaches a page that does not appear to be a properly formatted message.

One nice thing is that once you have spidered all of the messages and have them in content.sqlite, you can run gmane.py again to get new messages as they get sent to the list.  gmane.py will quickly scan to the end of the already-spidered pages and check  if there are new messages and then quickly retrieve those messages and add them to content.sqlite.

The second process is running the program gmodel.py.  gmodel.py reads the rough/raw data from content.sqlite and produces a cleaned-up and well-modeled version of the data in the file index.sqlite.  The file index.sqlite will be much smaller (often 10X smaller) than content.sqlite because it also compresses the header and body text.

Each time gmodel.py runs - it completely wipes out and re-builds index.sqlite, allowing you to adjust its parameters and edit the mapping tables in content.sqlite to tweak the data cleaning process.The gmodel.py program does a number of data cleaing steps.

You can re-run the gmodel.py over and over as you look at the data, and add mappings to make the data cleaner and cleaner.   When you are done, you will have a nicely indexed version of the email in index.sqlite.   This is the file to use to do data analysis.   With this file, data analysis will be really quick.

The first, simplest data analysis is to do a "who does the most" and "which organzation does the most"?  This is done using gbasic.py

There is a simple vizualization of the word frequence in the subject lines in the file gword.py.This produces the file gword.js which you can visualize using the file gword.htm.

A second visualization is in gline.py.  It visualizes email participation by organizations over time.Its output is written to gline.js which is visualized using gline.htm.


