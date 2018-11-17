+++
title = "Gotime"
date = 2018-04-18T08:23:19+01:00
type = "post"
draft = false
+++

So I thought it is about time I started blogging. I hear it is a good way to force yourself to properly learn new things by trying to explain them to others. 

So the first thing i want to learn is Time. Specifically how the programming lanagauge deals with time. So you could say its Go Time.

<h2>What is time</h2>

What exactly is time. Usually I go to wikipedia and see what it says.

Time is the indefinite continued progress of existence and events that occur in apparently irreversible succession from the past through the present to the future.

Not that helpful but most people think of time as one of two things. Either a specific point in time such as I was born 1'st Januray 1982 at 12:03. They also think of time as a duration of time like Usain bolt ran 100m in 9.58 seconds. 

Like most things it really is a bit more complicated then that though, Depending on where you are in the world 1'st of January 1982 at 12:03 is actully a differnt time. We need a bit more information that that to be precise you will need a location. So If I said I was born in london at that time you would now be able to tell exactly what time I was born.

<h3>Timezones</h3>

So as most people know time is differnt around the world. They celebrate new years in Australia first and it slowly makes it way west. This is becase the earth is round and one half is facing the sun and has daylight and the other is facing away so it is dark. We all want to be up during the day and asleep at night (mostly) So we based the strt and end of day on this.

So some bright spark came up with the idea of timezones. These are standard offsets of time from a central point. The central point was called Greenwich Mean Time (GMT) and was considered +0 hours from itself. If you go East from this point you add an hour and east you deduct an hour. Apprently the reason for this central point is GMT was widely used by mariners so had spread through the world. I assume it's also the reason most maps have england at the center.

//Need image of timezones
//https://upload.wikimedia.org/wikipedia/commons/e/e8/Standard_World_Time_Zones.png

So if I saw i was Born in london on 1'st of January 1982 you should now have enough information to know exactly when I was born.

So the central timezone has three names that are commonly used GMT ,Universal Time Coordinated (UTC) and Zulu time. For most purposes they are pretty much interchangable. UTC technically isn't a timezone but it doesnt seem to hurt to think of it as one. It is a properly standardised version of GMT and now tends to be used more in computing then GMT.
//https://en.wikipedia.org/wiki/Coordinated_Universal_Time

Zulu time is called this because zulu is the word used in the phonetic alphapbet for Z and Z is the letter given to GMT timezone by the military. You will often see dates with a Z in now you know what it means. 
//https://en.wikipedia.org/wiki/List_of_military_time_zones

<h3>oddities</h3>

Isn't it lucky we have a system that works exactly for time so there are 24 hours in a day which is one rotation of the earth and that one year is exactly 365 days long or one rotation of the earth around the sun. Well it seems we are not that lucky. 

It turns out that a day is really 23 hours, 56 minutes and 4 seconds. Also a year is really 365 and a 1/4 days long. So roughly every 4 years we insert a new day into the calendar to make up for this. 



Leap seconds, monotonic
A year rotation around the sun.
So a day has 24 hours right.

Leap seconds, monotonic


Russian julian calendare 1950, islamic calender.

https://www.iana.org/time-zones
https://golang.org/pkg/time/

<h2>How Go deals with time</h2>

https://golang.org/pkg/time/
<h3>duration</h3>
<h3>Missing</h3>



