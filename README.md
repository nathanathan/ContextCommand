Context Command
===============
The goal is essentially to build the star trek computer, where everything you say to it is a valid command. The mechanism for building it is as follows, when someone tries a command that doesn't work, they have the ability to program the system to do what they intended. If enough people do this unknown commands will become rare.

Increasingly search engines are adding interactive widgets capable of responding directly to queries (e.g. "time in Seattle", "current weather"). Wolfram Alpha takes this to a new level providing thousands of widgets capable of responding to a wide range of commands. Duckduckgo has taken the step of enabling it's users to create customized Instant Answers by submitting them to their repository. Having a larger pool of widget creators will lead to a much richer experience.

I propose a command engine focused on these widgets in the vien of Wolfram Alpha. However, what I propose has two features that I believe are missing in similar systems.

1. It should be as easy for anyone who wants to build their own widgets and modify existing widgets. People should be able to add commands via other commands (e.g. "Add the command population of {{country}}"). 
2. Commands should take context into account. As I will demonstrate below this opens the door to a number of interesting possiblities. 
