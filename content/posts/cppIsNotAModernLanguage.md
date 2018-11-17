---
title: "cpp is not a modern langauge"
date: 2018-04-14T09:30:32+01:00
anchor: "cppIsNotAModernLanguage"
weight:
draft: false
---

I have read alot about C++ recently as I have been involved in trying to port a medium sized collection of c++ apps from Windows to Linux. From everything I have read there are two types of c++ code, pre c++11 and post c++11 which everyone seems to refer to as modern c++. From what i have found this is partially true.

My normal roll is team lead for our cloud team so I spend a fair bit of time on a Linux command line, in fact im writing this blog post in vim :-) so porting something from windows to Linux would make my life easier as Linux is much easier to manage then windows so thought i would pitch in.

<h3>Build systems</h3>

I'm used to writing go/php/rust code normally and have had to build things from source previously with ./configure && make && make install so how hard can this be right.

C++ is painful when it comes to including third party libraries and I mean really painful, Everything uses it's own build tools there is no standard like go get or cargo. To get a few 3rd party pretty standard libs building like openssl, curl, boost, libmysql and v8 I ended up having to learn 3 different build systems. Make, cmake and ninja. Being the good cloud engineer i even tried to automate everything so I can just grab the latest tar.gz stick it in the right folder update a Makefile to the new version and it will build. This step took 3 weeks for me and im still not done as i need to setup static,shared,debug and release versions of all of them :-(


To give you an idea of what a modern build system looks like, i use hugo for this blog. To get the latest version of hugo i did this and it produced the hugo binary in about 5 mins on my rasperberry pi.

mkdir $HOME/src
cd $HOME/src
git clone https://github.com/gohugoio/hugo.git
cd hugo
go install

null has often been called the billion dollar mistake, I dread to think how much time and money has been wasted with every c++ project writing their own build system to sticky tape together something so they can do builds.

<h3>unicode support</h3>

I was trying to learn a bit more C++ so was looking for ways dip my feet in with small tasks so when we had a issue parsing an email address as defined by RFC 5322 i thought that should be pretty easy right. So i did what i normally do in this case and found that go has this abilty in the standard library so see how they do it and borrow their unit tests and maybe a few others to prove it works as expected. It wasnt long before I had to loop over a string and get the utf8 codepoints from it. Now in go or rust this is trivial so shouldn't be too hard in c++ right, you would be wrong.

There is currently no unicode support for strings in the langauge at all that I can see. So how would i do this? The answer seems to be to include a third party library called ICU. I really really dont want to include any more third party libs it is just so painful to get it building I just gave up at this point.

<h3>let it bleed</h3>

I started to not like c++ when I learnt about Macros. I like it even less now i have been bitten more than once by macros being defined in a library that gets transitively included. Clearing up the code base and removing theses meant i get failures as something was defined and is no longer there :-(

includes are done by name only so got bitten again where for some reason an old version of mysql.h was included in a 3rd party lib and by libmysql but it was picking up the old version and one of the structs had changed. Random crashes ensue and it took about a week to debug to find the issue.

Even more fun where you have 2 names clash in this case sha2.h from libmysql and another with the same name in one of our company libs. This meant the build breaks for me but strangly for no one else. I have no idea why but again more wasted time.

<h3>Use muliple compilers</h3>

I have often read you should really build your code with multiple compilers that way your code has more chance of being standards compliant. We are cross compiling for both Windows and Linux so are using msvc and gcc so where forced to do this and again its painful.

Code that builds perfectly in one refuses in another :-( then the game of working out why this is the case begins. Is it a compiler bug in one or the other, is it a compiler extension in one that makes it work, Is it some platform specific code or is my code just wrong.

I was testing out boost beast for a webserver and got it working on GCC but there is a bug in msvc where generic lambda's cannot be used within a namespace. This is fixed in the not yet released version but when i upgrade I have to rebuild all the 3rd party libs again which is alot of work and not fun in any way. Any one else seeing a pattern here of one issue that causing pain everywhere else.

<h3>cpp is complex</h3>

So no one would be surprised by me saying c++ is a complex language. But the lie being pushed about its fine if you just stick to modern c++ really does annoy me. From what I understand the biggest change in c++11 was the inclusion of move semantics. It all sounds good on the surface but when you get to the difference between lvalues and rvalues I get lost. I have read multiple blog posts on the subject and then you read the comments saying the article is actully wrong it makes you think does anyone really understand this. 

This is the foundation of everything on top of it and pretending it's easy or not complex is a big lie. Tell this to any programmer when they get their first compiler error mentioning rvalues and down this rabbit hole they go.

One of cpp's main strengths is the amount of code already out there being used which is going to be a majority old pre c++11 code so to read it you have to learn the old nasty stuff as well anyway.

<h3>Compile times</h3>

We have all seen the xkcd comic before havn't we ( https://xkcd.com/303/ ) and it is very true. I make a small change in one of our core libs and eveything has to be recompiled. On my 8 core machine this takes about 25 minutes. I dont know about you but i have forgotten what i was doing after 25 minutes. incremental builds of smaller sections of code are better but can still be longer than 2 minutes for me.

Rob Pike did a write up for what descisions where made about go and why https://talks.golang.org/2012/splash.article section 5 is very interesting. It basically says the reason for the slow compiles are the include system having to parse the same files over and over again which is causing the slowness.

<h3>Dirty little secret</h3>

I have been reading a fair bit of cpp recently and the more i read the more im convinced that cpp is still really c with classes. The way to include more code is identical and the benefit of this is you get to include the huge volume of c code silently and pretend its cpp. 

I recently had to write code to get the list of DNS server so trying to make sure this works across platforms I googled the standard way to do this. Surely asio would have a way to get this list. Turns out no so have to write platfrom specfic code. 

I ended up writing this on Linux and notices this is not actully cpp its plain c to do the work.

#include <arpa/inet.h>
#include <resolv.h>
#include <iostream>
int main(){
	res_init();
	std::cout << _res.nscount << std::endl;
    
	for ( int i = 0; i < _res.nscount ; i++){
		auto a = _res.nsaddr_list[i].sin_addr;
		char buffer[INET_ADDRSTRLEN];
		const char* result = inet_ntop(AF_INET,&a, buffer,INET_ADDRSTRLEN);
		if (result == 0){
			std::cout << "ERROR" << std::endl;
		}
			std::cout << buffer << std::endl;
	}
	return 0;
}


<h3>standard</h>
 
So are they going to fix any of this ? Not sure it going to be any time soon.
*modules other langauges have them quick to get reproducable builds
*its really just c with classes and some nice algo thrown on top still. c libs still make up core of langauge
*no unicode support - made worse modules are shit
*namespacing, includes macros oh my.
*multiple compilers good + bad
*still complex langauge hard to learn easy to make it crash.
*compile times :-(
*next train in 3 years.
