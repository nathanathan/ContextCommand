## Context Command
### A Proposal for a Natural Language Programming System

The goal of Context Command is essentially to become the Star Trek computer where everything the user says to it is a valid command. The mechanism for building it is as follows, whenever someone tries a command that doesn't work they have the ability to program the system to do what they intended. If enough people do this, unknown commands will become rare.

Increasingly search engines are adding widgets capable of responding directly to queries (e.g. googling "time in Seattle" or "current weather"). Wolfram Alpha brought this to a new level by building a "computational knowledge engine" with thousands of widgets capable of responding to a wide range of commands. Duck Duck Go has taken the step of enabling its users to create their own Instant Answer widgets by submitting them to a github repository. By crowdsourcing their community Duck Duck Go has been able to build up [an impressive collection of Instant Answers](https://duckduckgo.com/goodies).

I propose a "command engine" focused on these widgets in the vein of Wolfram Alpha. However, what I propose has two features that I believe are missing in similar systems.

1. It is easy as easy as possible for anyone to build their own widgets and modify existing widgets. For example, people are be able to add commands via other commands (e.g. "Add a command triggered by the phrase coffee near me").
2. Commands take context into account. The context is the set of previous commands the user invoked leading up to the current command.

I will demonstrate some of the interesting capabilities these features enable in the mockup below.

### Mockup

The user begins by entering the command "Get data on mass of known stars" into an input box, for which they receive a response like this:

![](cc_1.png)

By showing the most popular commands for the current context, we are able to generate a de facto UI that allows the user to discover some of the available commands and perform them with the press of a button.

The user follows up by typing "make a histogram." Since the previous commands are used to infer the meaning of the follow-up command the user is able to use very terse commands that would otherwise be ambiguous.

![](cc_2.png)

I believe *"Make what I just did into a JavaScript."* is one of the most interesting commands that Context Command could have. It allows users to compile natural language into code. Since all the commands are just JavaScript programs, to create a program they just need to be sequentially executed. (However, there are some details that complicate things, such as determining when programs have finished executing and dealing with interactive widgets.)

After that, the user creates a new command from their previous session (in theory the code generation step doesn't need to be explicitly called to do this). *Using this approach, users without any programming expertise are able to program new commands using only natural language by building off of other commands.*

![](cc_3.png)

### Implementation details

#### Defining commands

To add a command users need to define three components, a trigger which is the string the user has to type in order to invoke it, a program that implements the command's behavior, and optionally the command's context, which is a set of commands that the command can only be invoked subsequent to.

The trigger can contain variables, such as "city" in "Population of {{city}}," that will be made available to the program. (I believe that variables will cover most use cases and that regexes add unnecessary complexity.)

The program is JavaScript that gets evaluated on the Context Command webpage. This certainly has security risks, but I propose a way to overcome them via community moderation below. The JavaScript should follow conventions, such as only doing DOM manipulation within an output element. To the greatest extent possible these conventions are defined by community moderation rather than the Context Command framework.

Commands may match multiple triggers. If the previous command has a matching trigger that one will take precedence, otherwise the system will search for triggers further up the context chain until the top-level context is reached. The context chain is made up of the command that has the last command in its context and the command that has that command in its context and so on.

I will illustrate the precedence rules with the following series of commands:

1. Show a map of Seattle
2. Who is the mayor?
3. tea, earl grey, hot

When a user enters command 2 they probably mean “Who is the mayor of Seattle?” However, we could also interpret it as "Who is the mayor of the city where I am currently located?". According to the precedence rules, if someone has defined a trigger for "who is the mayor?" in the context of Command 1 we will use that, otherwise we will look in the top-level context where "who is the mayor?" probably means "Who is the mayor of the city where I am currently located?". None of the previous commands are likely to have triggers for command 3 in their context, so it will be treated as a general command in the top-level context and the computer will make tea.

There is no automated cleanup of the previous program. This enables subsequent commands to interoperate with previous commands by using left over data, without requiring any effort on the part of the initial command's developer. Requiring that each command store its data in a standardized location would probably have numerous benefits but that is outside of the scope of this proposal. It is left to community moderators to decide what kind of interoperation to allow.

#### Extensions to context

The concept of context could be expanded beyond just being the set of previous commands. Commands could use url of webpages as part of their context. Then, by using a bookmarklet to inject a context command prompt into the current webpage, users could issue commands specifically designed for that webpage. For example, on Wikipedia they could say "highlight text that was recently modified" or on their webmail client they could say "show me the previous message from the person who sent me this".

#### Command moderation

Commands are open source and any registered user can add or edit them. A review process will be required in order to filter out low quality code and malicious code. Whenever a new command is added, users who trigger it will see a prompt like this:
"This code was not fully reviewed and may be hazardous, do you want to proceed?" If a function is modified the previous version will be used until the new one is reviewed, and a passive notification will be shown indicating that there is a newer version that needs review. Users will also be given the option to view the code, and qualified users (i.e. users with enough reputation that they can be assumed to be trustworthy) can review it, accepting for inclusion or rejecting it.

A Stack Overflow style reputation system could be used to encourage creation, editing and moderation activity. This could also be driven by monetary bounties. Context Command affords a clear articulation of the user's wishes. When an user hits a dead end where the command they used lacks the appropriate action, there could be a button they click to post their transcript to an issue tracker along with a reward for making a command that performs an intuitive action.

-----------------------------------------------------------------------------------------------------------

My hope is that this proposal sparks discussion that I am able to learn new things from, and if there truly is nothing like Context Command in existence, I hope this proposal inspires some coders out there to build a similar system. I would be happy to help with the implementation.
