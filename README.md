# Quickrundown

# Quickrundown

## Table of Contents
1. Summary and Methodology
2. Walkthrough
3. Under the Hood


## Summary and Methodology

Quickrundown (QRD) at its core is just an overlay for the **bps** command packaged with Cobalstrike; this overlay incorporates elements from @r3dQu1nn's processcolor.cna aggressor script to provide enhancements. Utilizing QRD will allow an operator to quickly characterize what processes are both known and unknown on a host through the use of colors and notes about the processes displayed. Built into the script are capabilities to add new processes or update processes that already have characterizations with them, letting the operator do characterization that remains permanent (for the life of their cna file, that is). 

The methodology behind QRD came from sitting a frustration with not knowing the "so what" about processes on a host. Yes I had a rough idea about some of them, but when it came down to it I really only truly knew a handful. Another problem was that I had no idea how to really utilize Sleep 2.1. I had grown accustom to running Python code in an IDE so that I could set breakpoints, analyze values, etc. With Sleep 2.1 I did not see anything readily available that could mimic that. Eventually I bit the bullet and emailed Raphael Mudge @armitagehacker) directly to start asking questions. Thankfully he was nice enough to put up with me until I could get the hang of things, and I do want to send him a big thank you

I tried to put as many comments as I could throughout this script to help aspiring operators. If you do not like all of the comments, I have included a **sed** script that will remove them. Please feel free to message me (@1c3be4r) if you have any questions about the script, or need help getting started with Sleep.

## Walkthrough

Finally: the "just tell me how to turn it on" portion of the README. 

### 1) Load the script

First, you will need to pull down Quickrundown.cna and place it on your filesystem. Make sure you remember where on the filesystem you will be loading this script (/home/ubuntu/Quickrundown.cna, for example). 

Open the script and at the top you will see a variable labeled *$filesystem*. Edit this variable to say where *Quickrundown.cna* is being loaded from. Ensure that Cobaltstrike can **write** to this file

![alt text](https://raw.githubusercontent.com/1c3be4r/stash/master/step1.png "Step 1")

### 2) Once the script is loaded, pull up a beacon console and run the command **qrd**

The command **qrd** will launch the quickrundown function. You can find this function in the script at *alias qrd*

![alt text](https://raw.githubusercontent.com/1c3be4r/stash/master/step2_.png "Step 2")

### 3) Examine the data

The list and array packaged with this build are not extensive. I couldn't spend a week or more jamming processes into this thing, but it does come with a few already preloaded. Feel free to share your lists once you get them matured. 

Known processes will simply be standard white text with a note off to the right. If one of the **known** processes also hits a category from the processcolor.cna script, then it will be *colorized()*. The lists for the color categories are found at the bottom of the script.

![alt text](https://raw.githubusercontent.com/1c3be4r/stash/master/step4_more.png "Step 3")

### 4) Document new processes or update old one

If and when you discover an *++UNKNOWN PROCESS++* (as seen above), this is where the script gets good. Right click on the beacon to bring up the context menu, and select **Quickrundown** at the bottom. 

![alt text](https://raw.githubusercontent.com/1c3be4r/stash/master/step3_.png "Step 4 also")

Place the full process name into the top box (*newprocess.exe*) and the note you would like for the processes in the box below it. Hit **create/update**.

Continue these steps until you are satisfied with your research progress

![alt text](https://raw.githubusercontent.com/1c3be4r/stash/master/step4.png "Step 4")

To **update** an existing processes, do everything that you would do to add in a new process: same process name, new note. 

*NOTE:* New or updated processes will not show characterization by **qrd** until you complete the next step

### 5) Reload *Quickrundown.cna* via the script console

The processes you have researched are written to the *Quickrundown.cna* file, yes. However, they are not loaded into memory within Cobaltstrike just yet. Go to the *Script Console* within Cobaltstrike and reload the script

![alt text](https://raw.githubusercontent.com/1c3be4r/stash/master/step5.png "Step 5")

### 6) Verify and repeat

With the script reloaded, run **qrd** again to see your work displayed. Known processes will be displayed in certain colors if they match specific categories (that you can add to, located at the bottom of the script). 

![alt text](https://raw.githubusercontent.com/1c3be4r/stash/master/step6.png "Step 6")

## Under the Hood

How does this thing work? 

First, QRD is taking data returned from **bps** and manipulating it in certain ways. It uses a variety of loops, lists, and arrays to place things where they need to go. I tried to comment the loops and variables as best as I could to help you follow along.

Second, to add a new process the script takes a different route. The *Quickrundown.cna* file is first opened in readonly mode and iterated through via a **while** loop. The loop will look for certain strings in the code, and take action once those strings are found. It will add the new process to a list, and the note to an array that is keyed to the process name; if the process name is already in the list or array, it will just overwrite the note instead. 

As each line is read it is placed into a new list, and once the keywords are hit they are placed into the same list as well. The *Quickrundown.cna* file is closed and reopened with the **>** flag: this flag means that it will overwrite the entire file. The list holding our hold file is then iterated through, line by line, and pushed into the file. The file is closed with your new data in it. 

TL;DR: I rewrite the entire file with your new data in it. This is why it needs reloaded. 

