Context Command
===============
The goal of Context Command is essentially to build a Star Trek computer, where everything the user says to it is a valid command. The mechanism for building it is as follows, when someone tries a command that doesn't work, they have the ability to program the system to do what they intended. If enough people do this unknown commands will become rare.

Increasingly search engines are adding content widgets capable of responding directly to queries (e.g. googling "time in Seattle" or "current weather"). Wolfram Alpha takes this to a new level providing thousands of widgets capable of responding to a wide range of commands. Duck Duck Go has taken the step of enabling its users to create customized Instant Answers by submitting them to a github repository. By crowdsourcing their community Duck Duck Go has been able to build up [an impressive collection of functions](https://duckduckgo.com/goodies).

I propose a command engine focused on these widgets in the vein of Wolfram Alpha. However, what I propose has two features that I believe are missing in similar systems.

1. It is easy as easy as possible for anyone to build their own widgets and modify existing widgets. Fore example, people are be able to add commands via other commands (e.g. "Add a command triggered by the phrase coffee near me").
2. Commands take context into account. The context is the set of previous commands the user typed during a session. I will demonstrate below how this opens the door to a number of interesting possiblities in the mockup below.

The user begins by entering the command "Get data on mass of known stars" into their action bar, and recieves the following responce:


By showing the most popular commands for the current context, we are able to gererate a defacto UI that allows the user to discover some of the available commands and perform them without needing to type.

The user follows up by typing "make a histogram." Since the previous commands, are used to infer the meaning of the follow-up command the user is able to use very terse and simple commands.





*"Make what I just did into a JavaScript."* I believe this command is the most interesting part of my proposal. It essentially makes it into a natural language compiler. This command will probalby be fairly complex. (One issue is that without placing requirements on how programs are structured it is difficult to tell when they finish executing. Perhaps this could still be done by tracing the commands execution and placing a done callback after the last executed statement. There is also the issue of commands that return interactive content. I don't see a way these could be compiled without the programmer adding some funtionality for recording how it is used.)

Finally, the user creates a new commmand from their previous session (in theory the code generation step doesn't need to be explicitlly called to do this). This enables users without any programming expertise to create new commands by building off of other commands. 




To add a command users need to define three coponents, a trigger which is the string the user has to type in order to invoke it, a program that implements the command's behavoir, and optionally the command's context which is a set of commands that the command can only be invoked subsequent to. The trigger can contain variables like "city" in "Population of {{city}}" that will be made available to the program. (I believe that variables will cover most use cases and that regexes add unnecessairy complexity.) The JavaScript has almost no hard requirements other than it run without error when it is injected into the page. This certainly has security risks, but I propose a way to overcome them via community moderation below. The JavaScript should follow conventions, such as only doing DOM manipulation within the output element. Avoiding limiations on the JavaScript provides freedom for a variety of ways for commands to build off eachother to evolve.

When a command is evaluated the previous commands are searched in reverse chronological order for matching triggers in their context, and the command is parsed with the first trigger found.

There is no automated clean up of the previous command. This enables subsequent commands to interoperate with previous commands by scraping left over DOM elements and global state, without requiring any effort on the part of the initial command's developer. 

Commands are open source and any registered user can add or edit them. A review process will be required in order to filter out low quality and malicious code. Whenever a new command is added, users who trigger it will see a prompt like this:
"This code was not fully reviewed and may be hazardous, do you want to proceed?" If a funciton is modified the previous version will be used until the new one is reviewed, and a passive notification will be shown indicating that there is a newer version that needs review. Users will also be given the option to view the code, and qualified users (i.e. users with enough reputation that they can be assumed to be trustworth) can review it.

A Stack Overflow style reputation system could be used to encourage creation, editing and moderation activity. This could also be driven by monitary bounties. Context Command affords a clear articulation of the user's wishes. When an user hits a dead end where the command they used is lacking a trigger there could be a button they click to post their transcript to an issue tracker, along with a reward.

