# Policy for use-case awareness
* 10-15 minutes
* highly technical, less narrative

## Presentation
https://github.com/sweeneyb/policy-presentation-revealjs

The presentation is built in markdown, using pandoc to compile into revealjs.  Run locally, as a website.

The overview is that I SRE a large platform, and a power user wants to know when their operators go off the "happy path."  Using policy, specified in Rego and run via OPA, we empower the user to manage their own estate and define guardrails.  In the demo, we add a call-out in the node/typescript backend to send back violated rules when appropriate.

I also talk about OPA and being able to pull in data from 3rd party providers.

### Demos
https://github.com/sweeneyb/policy-demos

# What Sump Pumps taught me about Reliability
* target length 20 minutes
* less technical, more narrative

### Outline
* Moved into house
* Sump pump alarm blares late at night; couldn't figure out what it was for 30 minutes
* State assessment
* Replace the main?  Trial of the radon vent
* Replace the backup
* ** Trial of the connectors and batter replacement
* ** Tangent into the notifications
* *** Build vs buy; trade-offs.  This won't work when power is out because neither does my internet
* Testing monthly