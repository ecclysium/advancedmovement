ADVANCED MOVEMENT
by GrayArea/Eccylsium
https://github.com/ecclysium/advancedmovement
Please be certain to check the link above frequently for SAORI updates. 

Last Update: 5/5/2022
        -First official release


This SAORI allows movement tracks to be specified for any number of characters in a ghost. They can be set to then hover, fall, or loop. Another file is included, am_utility.txt (thanks to Zichqec for the error function in there), that is full of functions that will streamline usage of this SAORI. Most importantly, it contains a parser that allows movement to be called from SakuraScript in a shorthand. It also contains an initialization function, easy access functions for each action, and a function to set up arrays to keep track of information about created objects. 


Objects may be created, points of movement may be added, objects may be paused, started, paths cleared, and objects destroyed. At this time, the SAORI supports up to 15 objects to be animated at once, but I would only recommend using up to 5. Each item you add costs a notable amount of CPU power. I will be working on optimizing this to the best of my ability, but it may be a while, as this SAORI is being released right before the Ghost Jam 2022. 


It currently creates three events: 
	OnAMPathComplete - when there is no where else they are set to travel
		* reference0: The tag number (# in \p[#]) of the object that fired this event
	OnAMRedirect - when they have completed one line in their movement
		* reference0: The tag number (# in \p[#]) of the object that fired this event
	OnAMHover - when a ghost set to hover begins to hover, such as after finishing a line of movement
		* reference0: The tag number (# in \p[#]) of the object that fired this event
	OnAMError - returns a string explaining what went wrong when something went wrong
		* reference0: A string explaining the error.


It should not be difficult for me to add more references and data to these events, if someone is interested in more. 


This SAORI is in a perpetual state of development and was built with easy expansion in mind. As such, I request that if there is anything you want to be added, to please let me know at GrayArea #5561 on Discord, or through the Ukagaka Dream Team Discord. There are some things I won’t be able to do, however, I can appreciate a good challenge. I’d like to be able to accept any request that is possible to do. 


Some objects I would like to code are a bit out of reach, as math and physics aren’t my strongest skill. However, if you can explain a way to break down certain equations so that I can code them, I would very much be interested in working with you to expand this SAORI. 


Thank you very much! I hope you enjoy. 





-------------------------------------------------------------SAORI Functions---------------------------------------------------------

Functions are displayed in the order of:

-----“name”
Description

FUNCTIONEX format
SakuraScript parser format*, if applicable
Function Format*

Parameters

* These options require the utility code to be fully installed.

----------------------------------------------------------------------------------------------------------------------------------

-----“start”
Start the SAORI. This sets up the interface that you can then call. THIS MUST BE RAN BEFORE ANY OTHER FUNCTION CALL.

FUNCTIONEX(“advancedmovement.dll”,sakurahwnd,”start”,processwait,gravityspeed)
Not available to call in SakuraScript
AMSTART(worldspeed,gravityspeed)

Parameters:
* sakurahwnd - A tag for the window of the Sakura. See bottom of file or utility file on how to acquire.
* proccesswait - How often the SAORI will update in ms. It is recommended to keep this within 5-15. 
* gravityspeed - The speed at which objects of the Gravity type will fall. If not specified, will be set automatically to 98. 




-----“make”
Set up a character to move. There are a few different types of movement, that require different parameters. 

FUNCTIONEX(“advancedmovement.dll”,sakurahwnd,”make”,objecttype,hwndofmovingitem,tagnumber,speed,bonus parameter(s))
|MK,objecttype,tagnumber,speed,bonus parameters| or |make,objecttype,tagnumber,speed,bonus parameters|
AMMAKE(objecttype, tagnumber,speed,bonus parameters)

Parameters:
* sakurahwnd - A tag for the window of the Sakura. See bottom of file or utility file on how to acquire.
* objecttype - How this object will behave. 
   * Default “D”
         * The character will move along points to specified paths, and then stop.
   * Looping “L”
         * The character will move to the location specified, and then move to where it began, and play the path again.
   * Hover “H”
         * The character will bob up and down and very occasionally side to side. No coordinates need to be given. An interrupt signal (see later) needs to be put in for when the character is dragged.
   * Gravity “G”
         * When it is above/below a given point, it will return to that y coordinate.
* hwndofmovingitem - A tag for the window of this item. See bottom of file or utility file on how to acquire.
* tagnumber - the number associated with this item. For sakura, this is 0, and for kero, this is 1, but it can be any number you specify with \p[#]
* speed - Relatively how fast you want the ghost to go. 
* Bonus Parameters
* Default “D”
      * None
* Looping “L”
      * None
* Hover ”H”
      * magnitude - how much the ghost will go up and down. The exact frame of measurement is uncertain, but larger is more dramatic, and smaller gets you close to vibrations— As this is beta, I am looking for outside commentary on how you would expect this to function.
* Gravity “G”
      * floor - the y-coordinate that the ghost will tend to return to when not on a specified movement path. It will move up or down as necessary to return to here while its x-coordinate will stay the same.





-----“addpoint”
Add a pair of coordinates to the given tag number to move to.

FUNCTIONEX(“advancedmovement.dll”,sakurahwnd,”addpoint”,tagnumber,xcoord,ycoord,edgetype)
|AD,tagnum,xcoord,ycoord,edgetype| or |addpoint,tagnum,xcoord,ycoord,edgetype| 

Parameters:
* sakurahwnd - A tag for the window of the Sakura. See bottom of file or utility file on how to acquire.
* tagnumber - the number associated with this item. For sakura, this is 0, and for kero, this is 1, but it can be any number you would specify with \p[#]
* xcoord - X coordinate
* ycoord - Y coordinate
* edgetype
   * “L” - a straight line with no changes in speed.
   * In future versions, one will be created that slowly speeds up then slows down again. However, I am bad at math, and need help completing it. But it will happen at some point and sooner if you are good enough at math and come help.




-----“activate”
Send the signal to the given tag/character number to begin moving along the specified track.

FUNCTIONEX(“advancedmovement.dll”,sakurahwnd,”activate”,tagnumber)
|A,tagnumber| or |activate,tagnumber|
AMACTIVATE(tagnumber)

Parameters:
* sakurahwnd -  A tag for the window of the Sakura. See bottom of file or utility file on how to acquire.
* tagnumber - the number associated with this item. For sakura, this is 0, and for kero, this is 1, but it can be any number you would specify with \p[#]




-----“activateall”
Make all created items begin moving along their respective specified tracks, or to begin their special behavior (i.e., Gravity Items fall). 

FUNCTIONEX(“advancedmovement.dll”,sakurahwnd,”activateall”)
|AA| or |activateall|
AMACTIVATEALL

Parameters:
* sakurahwnd -  A tag for the window of the Sakura. See bottom of file or utility file on how to acquire.




-----“pause”
Send the signal to the given tag/character number to halt movement temporarily, including special object behavior.

FUNCTIONEX(“advancedmovement.dll”,sakurahwnd,”pause”,tagnumber)
|P,tagnumber| or |pause,tagnumber|
AMPAUSE(tagnumber)

Parameters:
* sakurahwnd -  A tag for the window of the Sakura. See bottom of file or utility file on how to acquire.
* tagnumber - the number associated with this item. For sakura, this is 0, and for kero, this is 1, but it can be any number you would specify with \p[#]





-----“pauseall”
Send the signal to make everything stop moving.

FUNCTIONEX(“advancedmovement.dll”,sakurahwnd,”pauseall”)
|PA| or |pauseall|
AMPAUSEALL

Parameters:
* sakurahwnd -  A tag for the window of the Sakura. See bottom of file or utility file on how to acquire.




-----“kill”
Stop the SAORI and all movement.

FUNCTIONEX(“advancedmovement.dll”,sakurahwnd,”kill”)
|K| or |kill|
AMKILL

Parameters:
* sakurahwnd - A tag for the window of the Sakura. See bottom of file or utility file on how to acquire.




-----“clear”
Clear the movement queue of a given tag. As a side effect, will stop current movement.

FUNCTIONEX(“advancedmovement.dll”,sakurahwnd,”clear”,tagnumber)
|C,tagnumber| or |clear,tagnumber|
AMCLEAR(tagnumber)

Parameters:
* sakurahwnd - A tag for the window of the Sakura. See bottom of file or utility file on how to acquire.
* tagnumber - the number associated with this item. For sakura, this is 0, and for kero, this is 1, but it can be any number you would specify with \p[#]









--------------------------------------------------------------TIPS AND COMMON PROBLEMS----------------------------------------------------------------

1. CLICK AND DRAGGING: 
   * Objects will remain in motion unless paused. If you want to allow users to move the ghost, in OnMouseDragStart, pause the object’s movement (AMPAUSE(reference3)). In OnMouseDragEnd, you can reactivate it (AMACTIVATE(reference3)), and the ghost will go back to where they were indicated, fall, or hover in place, depending on type. 
2. BASEWARE MOVEMENT: 
   * Similarly to the above, if you want to use SSP baseware movement functions, you may, but if the ghost is in the middle of moving, you will want to pause or clear it first. 
3. SAKURA WON’T ANIMATE: 
   * If you can’t get the Sakura to animate, put the parameters sent to the SAORI in quotations, then it should work. The utility functions should implement measures to make sure arguments are interpreted correctly. 
4. ACTIVATING OBJECTS:
   * You only need to make/MK an object once when you are setting it up for use. After its been made, you can replace it in the system by making it again, deleting its content. This is useful if you need to change speed, magnitude, where the floor is (you might want to do this when an object changes monitors), or an object type entirely.
5. RAISE CHAINING: 
   * The order SSP executes things in may catch you off guard. The parser works by chaining a message through many raise functions. Mixing multiple methods of talking to the SAORI can cause problems, so choose either functions (TMMAKE(args)) or SakuraScript (|MK,args|) and keep it consistent to minimize hard-to-catch bugs.
6. ON HWNDS: 
   * You'll need to have the hwnd of anything you'd like to move. This means you have to show that character before you make the object. Here's a function to set things up (included in am_utility):
//Do this before any talking occurs. Even before starting the SAORI.
INITHWNDS : all {
        objecttypes = IARRAY
        //any bonus tracking arrays should be initialized here
        
        //Change the 2 to a 1 if you want the kero to be included, but remain invisible.
        for _i = 2 ; _i <= ARRAYSIZE(BODYHWND) ; _i++ 
        {
                objecttypes[_i] = IARRAY        //to include this formally, when you make an object, store its type here (D, L, H, or G)
                "\p[%_i]\s[-1]"
        }
}