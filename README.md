
# PIDtoolbox

PIDtoolbox is a GUI-based Matlab tool for analyzing Betaflight blackbox log files.

![](images/PIDtoolboxGUIexample.png)

The motivation behind the development of this tool was to create a user-friendly, objective, method for comparing between flights. This came after a realization of the way we often troubleshoot an issue with flight performance (e.g., vibrations, mid-throttle oscillation, propwash oscillation, etc). A typical scenario goes something like "run test flight 'A', make subjective assessment of performance, tweak parameters, run test flight 'B', make subjective comparison between A and B, re-tweak", and so on. The problem with this approach from a scientific standpoint is that it lacks objectivity and reliability. Subjective bias becomes particularly problematic when the differences between A and B are subtle. PIDtoolbox was designed with this in mind.

I am not an engineer, and what I know about control theory has only come from my experience with miniquads over that past 3-4 years. So for engineer types out there, PIDtoolbox may seem like a slightly unconventional approach to PID analysis. That said, I am a Neuroscientist with about ~15ys experience and publishing on the neural basis of oculomotor control <a href="https://scholar.google.com/citations?user=xbMsWGcAAAAJ&hl=en" target="blank">(Google Scholar)</a>, which believe it or not shares some resemblance to the flight characteristics and data associated with miniquads <a href="https://www.youtube.com/watch?v=UEqraYeP7Qs" target="blank">(eye movements)</a>. I also study neuronal oscillations, hence the focus on spectrograms. PIDtoolbox was partly inspired by Plasmatree PID-analyzer (https://github.com/Plasmatree/PID-Analyzer), and the goal was to extend some of that type of functionality here in a GUI-based program.

Over the past year or so, there has been increasing emphasis on advanced gyro and dterm filtering, particularly in the +150Hz range, and this has led to amazing improvments in flight characteristics. During that time I have been analyzing many blackbox log files in Matlab and noticed a consistent difference between copters that fly well and copters that do not, specifically in the 30-90Hz spectrum, which can often be seen in |PID error| during steadly-state flight. Most commanded copter motion is below ~20Hz (even if you're a Mr. Steele on the sticks, at best you might push this to 25-30Hz!), but from my analyses, activity between 30-90Hz tends to correlate with the kind of vibes we SEE during flight and in HD video (things like mid-throttle oscillation, propwash, jello, etc.). Although these frequencies cannot be filtered out, the goal of PIDtoolbox is to use it as an objective comparative measure between A/B testing to help decide whether changes between A-B tests (in software settings, hardware, mechanical, or otherwise) were effective. I will show some examples that support this idea.

That said, below is everything you need to begin using PIDtoolbox. I hope you find it useful, and I would really appreciate feedback from the FPV community.

Cheers! -Brian White

# Download

There are a couple of ways you can download PIDtoolbox. 

**1)** If you have Matlab, you can simply download the repository of mfiles, place them in a folder, and run PIDtoolbox.m from the Matlab command prompt. You will need Signal Processing Toolbox installed for this to work.

**2)** If you do not have Matlab, you can download a standalone Windows version. You must first install Matlab runtime (MATLAB® Compiler Runtime, MCR), which then allows you to run any standalone Matlab program.

**Steps:**

**(i)** Install Matlab runtime <a href="https://www.mathworks.com/products/compiler/matlab-runtime.html" target="blank">(Matlab-runtime)</a> for Windows (64bit only, version 9.3 R2017b). Unfortunately, this will likely not work for Windows 32bit users, but you could try the next most appropiate version for your Windows machine, at your own risk. I have not tested this extensively across multiple versions of Windows, so your feedback here will be very helpful. I will continue to provide updates in this regard so stay tuned. The good news is this is a one-time installation, after which you can run any Matlab standalone!

**(ii)** Download PIDtoolbox for Windows here:
<a href="https://github.com/bw1129/PIDtoolbox/releases" target="blank">(PIDtoolbox.zip)</a> Unzip and place entire folder in your preferred location on your computer. For Mac/Linux/others, if you have a bonafide copy of Matlab and can create your own standalone version of PIDtoolbox, and I'd be greatly appreciated if you could upload it here!

**(iii)** Run 'PIDtoolbox.exe', found in folder: 
'PIDtoolbox\for_redistribution_files_only\PIDtoolbox.exe'
NOTE: PIDtoolbox.exe and other files must remain in this folder.

If you have issues installing Matlab runtime, or running PIDtoolbox, please give feedback here:
<a href="https://github.com/bw1129/PIDtoolbox/issues" target="blank">(issues)</a>
or drop a post to the Betaflight BlackBox Log Review Facebook group: <a href="https://www.facebook.com/groups/291745494678694/?ref=bookmarks" target="blank">(Betaflight BlackBox Log Review Facebook group)</a>

# How to use:

PIDtoolbox will support .bbl, .bfl, or .csv files, but there are a few things you need to know: 

**(i)** If you wish to use .bbl or .bfl files directly, you will need to have a copy of 'blackbox_decode.exe' in the same directory as your .bbl/.bfl files. 'blackbox_decode.exe' is part of Betaflight/cleanflight blackbox-tools, and can be downloaded here:
<a href="https://www.github.com/betaflight/blackbox-tools" target="blank">(Betaflight Blackbox-tools)</a>

**(ii)** My recommendation is use .csv files! BUT NOTE, you MUST convert your .bbl or .bfl file to a .csv file using Betaflight Blackbox-log-viewer. Save as .csv from there please! Betaflight Blackbox-log-viewer can be downloaded here: 
<a href="https://www.github.com/betaflight/blackbox-log-viewer/releases" target="blank">(Betaflight Blackbox-log-viewer)</a>

If you have issues installing Matlab runtime, or running PIDtoolbox, please give feedback here:
<a href="https://github.com/bw1129/PIDtoolbox/issues" target="blank">(issues)</a>
or drop a post to the Betaflight BlackBox Log Review Facebook group: <a href="https://www.facebook.com/groups/291745494678694/?ref=bookmarks" target="blank">(Betaflight BlackBox Log Review Facebook group)</a>

Now, let's get on with a few examples:

### E.G. (i) Mid-throttle oscillation
'Glen' posted a log file (A) to the Betaflight BlackBox Log Review Facebook group: <a href="https://www.facebook.com/groups/291745494678694/?ref=bookmarks" target="blank">(Betaflight BlackBox Log Review Facebook group)</a>
describing a vibration issue with his copter. He then made some changes to the PIDs and filters and reposted a second log file (B). 
At a glance (see below), the post filtered Gyros (right panels) looked clean in test flights A (upper) and B (lower). 
![](images/PIDtoolboxGUIexample2b.png)

I then selected the specrograms to compute sub-100Hz throttle x freq plots for set point and PID error, then hit 'refresh' (see below). I know, you're thinking "why are you plotting set point?" Since Set point is RC input in deg/s, the freq spectrum of set point is a good measure of the range of frequencies associated with commanded motion. Theoretically then, anything above this is uncommanded motion (e.g., mid-throttle oscillations, propwash, etc.). The horizontal dashed lines on the spectrograms delineate this region (which, incedentally corresponds with the cutoff used in <a href="https://www.youtube.com/channel/UCI2MZOaHJFMAmW5ni7vuAQg" target="blank">UAVtech's</a> <a href="https://drive.google.com/drive/folders/1jCIJ2FKL7t-ZADcErNrZOcWfWtkgSVdr" target="blank">filter Calc Tool</a>. Now, when we look at the PID error spectrum (right panels), we see a clear band of activity around 40-50Hz in the mid-to-upper throttle range, mainly on roll and pitch for test A, and less so for test B (despite the fact that the full spectrum looked clean for both!). To corroborate this, we can see bursts of oscillation in the Gyro and PID error traces blown up on the left, with peaks ~22ms apart (which is 45Hz!). I would argue then that the degree of activity within ~30-90Hz range should be a good indicator of a copter's actual performance, though more testing is required.  
![](images/PIDtoolboxGUIexample2c.png)

To go a little further, we can plot the mean absolute value of PID error as a function of throttle (see below). For these plots, we constrain analysis to a portion of the data in which RC input was at a minimum (see max stick window in GUI), since we know PID error gets much larger during fast rolls/flips. Again, the results corroborate our previous observations. From these results, we can conclude that the adjustments made in test B are a significant improvement from test A. We are on the right track! 
![](images/PIDtoolboxGUIexample2d.png)


