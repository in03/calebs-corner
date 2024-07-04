---
title: How DRY should I be?
description: Quick thoughts on honesty and battling abstraction
slug: how-dry-should-I-be
date: 2024-06-20 00:00:00+0000
image: desert_dev.jpg
categories:
    - Journal
tags:
    - AI
    - Software dev
    - Quick thoughts

---

## The Problem

I love writing abstractions.

It appeases my "ducks in a row" desires. It makes my code tidy and I feel like a pro calling my nifty functions.
The problem is, it doesn't take too long before I get backed into a corner and the tidiness unravels into spaghetti.

We’ve all done this.

It feels great to make magic, but magic gets messy when you need to fix it.
If you’re like me, you’ll claim you’re not doing it for the magic by hiding behind the intelligent cause of staying DRY.

For the unfamiliar, DRY stands for Don’t Repeat Yourself. It's a software development principle designed to reduce redudancy and maintainability issues.

In a nutshell:

1. **Productivity**: Writing logic once and reusing it reduces typing.
2. **Readability**: Less code means less to read. And we read a lot more code than we write.
3. **Maintainability**: Less code to read means less to fix when things go wrong.

At least that's how I understand it.

The term was first introduced in the book *The Pragmatic Programmer*, advocating:

> “Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.”

---

I don't know how, but maybe it's just because its so simple... This is the one thing I just innately *over*do. When I see those capital letters online, I feel like I'm around my people. They **get** me. It resonates with the same immature hype that the minimalist movement had. Clearly important elements of truth that people have either drastically under or over-actioned with no clear markers in place to measure effectiveness.

I’m going to be honest. I have not read the book... maybe my understanding of DRY is incomplete or immature, but I’m going to hazard a guess that most people who "practice" DRY haven't read it either, probably not even you.

I imagine if there is "true" nuanced DRY, most developers aren't familiar with it.

So here's my hot take.

**I don’t think programmers need any encouragement to be more DRY.**

Us programmers don’t like repeating ourselves. We work out pretty early that it’s easier to use functions and variables than it is to copy and paste code all over the place like 5 year old scrap-bookers.

We want elegance and flow; we like watching code dance for us. The problem is, if we don’t control that obsession, we end up dancing for the code.

## Examples

As soon as something becomes repetitive, we have the choice of leaving it in or reaching for an *abstraction*.

Abstraction #1: The Humble Variable.

```Python

# Not very DRY
print(9 + 10)
log.debug(9 + 10)
>> 21
```

```Python

# Variables!
message = 9 + 10
print(a)
log.debug(a)
>> 21

```

Nice. It works. But eww. If we know we want to print and log for all sorts of code throughout the project, that's gonna get repetitive. And it's not very future proof if we want to send that output to more "sinks" in the future. Say I have 26 `prints` to stdout throughout my code. What if I now want to add the feature to send a fax every time I print? (A perfectly reasonable goal).

I would have to update the code **26 times**. DRY says no!

I guess we could use a function?

```Python

def wuphf (message):
"""
Notify on all the services!
"""
 print(message)
 log.debug(message)
 send_email(message)
 tweet(message)
 fax(message)

wuphf(9 + 10)
>> 21

```

{{% videogif src="wuphf.webm" %}}

Much better! Now my action is a single line function call that wraps five statements. I just saved myself 121 lines of code! Let the minimalist addiction kick in. Concise and maintainable.

But what if I need a little more flexibility? Maybe I don't want to fax or tweet *every* time. Just most of the time. I don't want to have to create another function called `wuphf_without_fax_and_tweet`. That's clearly dumb. Let's make our existing function more flexible.

```Python

def wuphf (m, do_print=true, do_log=true, do_send_email=true, do_tweet=true, do_fax=true)
"""
Now customisable!
"""
 if (do_print):
  print(m)
 if (do_log):
  log.debug(m)
 if (do_send_email)
  send_email(m)
 if (do_tweet)
  tweet(m)
 if (do_fax)
  fax(m)

wuphf(9 + 10)
>> 21
```

Now I'm really solving problems. Look at all the work I've moved into the function ready for repeat use! My function only needs one mandatory argument. That'll keep my function calls concise.

But *ahem* as "useful" as our new function is, it's a bit wordy itself. If we write a lot of functions like this, our file might be getting pretty chunky. I don't want to see these utility functions shadowing out the rest of the business logic, now do I??

We need to get organised.

Let's put it in another file out of the way somewhere and import it. We're bound to add additional functions of a similar nature, they can just live in that module too. We may as well start moving any other wordy functions to their own modules and just have a tidy-up while we're at it.

```Python
from notifications import wuphf

wuphf(9+10)
>> 21
```

Done! Easy. Works great. Rinse-and-repeat as you please and your main script is going to look so tidy 👌

```Python
from notifications import wuphf
from cli import app
from users import new_user

# Prompt user
username = app.get_username()
new_user(username)
wuphf(f"New user added! '{username}'")


```

### It's a Trap!

This is obviously a silly example, but it illustrates a trap I honestly fall into:

1. I abstract for repeat use
2. I add more functionality to the abstraction (uh, oh, function is no longer single responsibility)
3. I move similar functions into a module
4. I need more functionality but have to dance around to implement it.
5. I think I've fixed it, but one fix somewhere causes a break somewhere else since there are so many calls to my now overcomplicated, fragile function.

{{% videogif src="leak.webm"%}}

If I knew I needed the flexibility of notifying to different services, I wouldn't have done the abstraction like this to begin with. Functionality that should exist as statements in the main script now exists two layers deep.

Not that I'd wish *Notepad* on someone as an IDE, but... good luck navigating without Intellisense.
We haven't even touched classes.

### Stuck with the Cheque

Potential downsides of abstraction:

- **Complexity**. Abstraction can turn simple code into a navigation nightmare. A code block becomes a function, then a file, then a module. Code functionality can become hidden deep in the codebase.

- **Brittle**. Abstractions can create unnecessary hoops to jump through in order to anticipate future needs that may never materialise. These hoops constrain the way we write code that interfaces with it. If the code that needs it can't change, we end up having to change the abstraction and potentially any other code that's already used it.

- **Maintainability**. Code that's complex and brittle is already hard to work with. What about anyone in the future who has to work on that code? Can they learn it quickly? What about yourself in a weeks time?

Premature abstraction is basically blind refactoring. We don't want to refactor later when we have the facts, let's refactor now without them!

## Mitigation

Alright, so too much DRY can lead to brittle abstractions. How do we find a good balance?
Well, being aware is a good first step. Hopefully, you can analyse your own dev journey and see where you lie on that spectrum.
You might be doing great and this is all for someone else.

But if this is for you, read on. Here's a few things that have helped me.

### Consider Abstraction Usefulness

Be honest with yourself. Ask yourself these questions before you get your hands dirty:

- How likely are you to need this later?
- Is there something more generic that does the job?
  - If so, exhaust those options before rolling your own!

- How nested is your code?
- What do you gain in making a code block a function?
- What do you gain in moving a set of functions into a class?
- What do you gain in moving a set of functions/classes to a separate file (module, script, etc.)?

### File Length

Is your file getting too long?
Are you tired of scrolling up and down so much?

To be honest, it probably isn't too long.

- Are you using [code folding shortcuts](https://code.visualstudio.com/docs/editor/codebasics#_folding) effectively? 
  You can collapse by different levels of indentation. E.g. If you have a set of test functions defined at one indentation level in, you can fold all the function definitions into their titles with `ctrl/cmd + k, ctrl/cmd 1`. Each indentation level deeper is a higher number.

- Are you using ["Peek"](https://code.visualstudio.com/docs/editor/editingevolved#_peek)? 
  VS Code has an option to jump to function definitions from references to a function elsewhere (Go to Definition). It also allows the inverse, listing references from the definition itself (Go to References). Use `ctrl/cmd + left click` or `shift + F12` and `option + F12` respectively. Using Peek to perform these navigations gives you a little window to work with, so you can update a reference (or several) and not have the mental overhead of finding you way back to deal with.

- Are you using [cursor history navigation](https://stackoverflow.com/questions/35424367/how-can-i-navigate-back-to-the-last-cursor-position-in-visual-studio-code)?
  VS Code has a "Go forward" and "Go back" functionality for the cursor. If the Peek window isn't enough and you just need more visual context, or hindsight is 20/20 and you need to go back somewhere that wasn't a linked function definition/reference, you can navigate to where your cursor was last and back again.

- Are you using ["Rename Symbol"](https://code.visualstudio.com/docs/editor/editingevolved#_rename-symbol)?
  This one's a stretch, but if you find the possibility of similar names causing rename conflict has you wanting to split your file, consider using Rename Symbol. If you need to change the name of a function or variable *everywhere* you might not want to change every instance of the word too. Say you have a function named, "file", but you realise a little late that `file` is actually a built-in variable in your language that shouldn't be overriden. If you run "find and replace", every instance of that word will change, including instances of that built-in variable and any time you used the word "file" in inline comments or as part of a name in any other variables, function calls or methods, etc. Using Rename Symbol ensures you don't mess with that other stuff.

Learning how to navigate in your IDE efficiently will help prevent you from segregating your code out of navigation frustration. When you think you've maxed out your ability to grasp navigation tools, if it feels like there's too much going on in a file, then consider moving stuff.

If after some time you can see sections that have discrete responsibilities that you have not edited in a while, you can try moving them, but consider what you will lose and what you will gain carefully!

There's plenty more to consider. Use the IDE's outline, breadcrumbs, etc.

### Code Complexity

[Cyclomatic complexity](https://jellyfish.co/library/cyclomatic-complexity/) is a metric that measures the complexity of a program by counting the number of linearly independent paths through a program's source code.

Essentially, it'll help you keep yourself honest with stats that tell you when you're a bad boy. Too many nested if statements in a single function, that sort of thing. It won't help you with [Separation of Concerns](https://www.geeksforgeeks.org/separation-of-concerns-soc/) ([if that's a principle you follow](https://grugbrain.dev/#grug-on-soc)), but it will help you with keeping complexity low. Unnecessary complexity is what makes a brittle abstraction.

If you're using Python, consider using [Ruff as your linter](https://docs.astral.sh/ruff/configuration/) (and formatter). It enables McCabe's complexity rule for linting by default (c901). It's written in Rust, is very fast and includes support for a lot of linting rules.

### Snippets

Snippets are blocks of text that you can inject at your cursor.
If we're talking code, snippets are a great way to inject common code patterns.
They offer a template for a common pattern, like looping or conditional statements.

I've never much used snippets until recently.

I've had to push through some mental friction in using them.
Part of me feels if I use snippets, I'll forget how to write the code patterns they inject.
It's like trading memory-space; if I need to remember the name of a snippet and a keyboard shortcut for it, I'm going to forget the code pattern.

Fortunately, this isn't true. How you get your code into the IDE matters a lot less than understanding what it does.
Sometimes, I find myself staring at the flashing cursor with 10 different for-loop implementations fighting for my attention.
Snippets help take this load off a bit.

Visual Studo Code has a built-in snippet feature that you can use to create snippets.
There are also a heap of snippets published on the VSCode marketplace as extensions.
When you insert a snippet in VSCode, you can tab through the required variables to quickly fill them out.

Other IDEs have similar implementations.

if you find youself needing snippets across IDEs, code editors in browsers or quickly in a terminal, consider using [RayCast](https://www.raycast.com) if you're on a Mac – for a bunch of reasons, snippets being one. Raycast allows snippet injection globally, anywhere you can type. It also includes support for dynamic placeholders (variables).
It goes beyond code. It's handy for templating emails, Zendesk tickets, whatever you can think of.

I've also used [Espanso](https://espanso.org/install/), [KeyboardMaestro](https://www.keyboardmaestro.com/main/), and [AutoHotkey](https://www.autohotkey.com) on Windows.
They all have their own unique features, but support snippets at a minimum.

### AI Assistance

AI Autocomplete is different. It often completes what I'm going to type and of course, I get to accept or deny the suggestion.
This is one less cognitive hurdle compared to snippets, I'm not trading any mental real-estate to remember a trigger.
It comes with the huge added benefit of being intelligent:

- You don't need to add any snippets manually
- Completions are fully dynamic, using the context of your file, repo or even whole codebase
- Cycle through multiple suggestions if the first one doesn't work

I've been using Sourcegraph's Cody as an alternative to the non-free Github Copilot.
I did some work on a frontend with react-form-json-schema and PowerShell as a backend (WILD stuff – see the amazing [Powershell Universal](https://docs.powershelluniversal.com) platform). Because the LLM was familiar with the larger react-form-json-schema project, it had no trouble translating that into my niche PowerShell codebase. All the form validation is in native PowerShell with very consistent `Test` functions for each field:

```PowerShell
function Test-RequesterEmail {
    param (
        [Parameter(mandatory = $True)]
        [String]$RequesterEmail
    )

    # Get AD user
    $User = Get-ADUser -Filter {EmailAddress -eq $RequesterEmail} -Properties EmailAddress, DistinguishedName
    Write-Host "DEBUG: AD User info: $User"
    
    # Test domain address
    If (-not $User) { 
        Write-Host "ERROR: Invalid! User '$($RequesterEmail)' not registered in Active Directory."
        return New-ValidationResult -Name "Requester Email" -Status $False -Message "'$RequesterEmail' is not a valid XCompany email address."
    }

    # Test staff address
    If (-not ($User.DistinguishedName -match "OU=XCompany Staff")){
        Write-Host "ERROR: Invalid! User '$($RequesterEmail)' is not in ""Staff"" OU."
        return New-ValidationResult -Name "Requester Email" -Status $False -Message "'$RequesterEmail' is not a valid staff email address."
    }
    return New-ValidationResult -Name "Requester Email" -Status $True -Message "'$RequesterEmail' is a valid XCompany staff email address."
}
```

After writing a couple of these validation functions, Cody pretty much did the rest. All my fields were defined in the schema. Cody pre-empted each line I needed. Even the comments.
It even replicated my weird `Write-Host` stuff with "ERROR" and "INFO" prefixes (PU sends `Write-Host` to the browser console, but not `Write-Debug/Info/Error`).

If your code is predictable enough, you may find AI autocomplete does a fine job of anticipating your structure. This in a way is a good marker of your consistency as a programmer. If you find it anticipating your code well, you might find that the mental strain of yourself anticipating your code is also lowered.

AI isn't always going to have the completions you want, but it is a step up from snippets for cutting through the tedium.
Just be aware that when you don't want a suggestion it can be a bit of a nuisance and suggest a completiion if you pause for too long.
In my opinion, this is more annoying than you'd think. If you struggle with maintaining a train-of-thought, unwanted completions will derail you quicker.

Regarding DRY itself, if you find a block of code you think could be more concise, ask Cody to rewrite it. See what it suggests.

## AI prompting

We're all familiar with ChatGPT by now.

The most recent model GPT-4O has really impressed me. I've received some great working code examples from it that just worked out-of-the-box. The Powershell Universal project above, it's all in one file, 400 lines of code. I pasted that into ChatGPT and asked for a few changes and it sent it right back, changes applied. It actually became more convenient to ask for changes I knew how to make directly within ChatGPT itself. By the time I could make the necessary modifications to the `FormSchema`, `UISchema` and "Test" functions myself, ChatGPT would've finished writing the whole script out again from scratch.

I'm excited to see more convenient coding workflows pop up.

Consider [Cody](https://sourcegraph.com/cody), [ChatGPT](https://chatgpt.com), [GitHub Copilot](https://github.com/features/copilot), [Groq](https://groq.com), [Ollama](https://ollama.com) and [Mods](https://github.com/charmbracelet/mods) to name a few AI tools... and [Cursor](https://www.trycursor.com), for an AI first IDE. This seems to be getting better by the day.

### Environments

Consider your production and development environments. How will it deal with your abstractions? Is it going to play with a modular codebase nicely?
Environments can be the difference between a project building successfully or not. If manual changes to the code are required to deploy in common environments, merging upstream changes from git gets messy. This is not a fun experience for your users.

- Portainer has a great GitOps style integration for its Stacks feature. It works great with a `docker-compose.yaml` and `.env`, but it won't easily handle sidecar yaml configs or a `docker-compose.override.yaml`. Does the configuration need to be abstracted into multiple files?

- Powershell Universal has an internal repo that is not designed for manual updates because of the way file-watching works to reload downstream services. Creating and importing modules can get a bit prickly without proper planning.

- Application hosting services like Fly.io expect a certain file structure. Keeping your code in line with recommendations might be important to prevent refactoring later.

- Can your planned abstractions be used later in this codebase? How about in future projects?

- Can you release code in a way to make abstractions more compatible?

### Frameworks, Libraries and other Dependencies

This one takes good judgement. Both of the follwing statements are true:

- The **best** kind of abstractions to work with are ones you didn't write
- The **worst** kind of abstractions to work with are ones you didn't write

Provided you are careful about the dependencies you choose, you can find yourself with a featureful set of abstractions that are well scoped, well planned, maintained, secure, rarely include breaking changes and have a healthy community happy to support.

Nothing lasts forever, but good dependencies should outlast your own projects.
When choosing dependencies, check:

- It doesn't do too much.
  - Depedencies that do too much become a greater risk to your code if they become unmaintained.
  - Poor scoping may also be indicative of poor planning and more difficult maintenance.

- Maintainer engagement.
  - Are issues responded to in a timely manner?
  - Are pull requests reviewed?
  - Are new features added?
  
- It has a healthy community.
  - Are users asking questions and getting answers from the community?
  - Is there a dedicated place for discussion?
  - Is the community engaged with the direction of the project?

- It's mature.
  - Does it use semantic versioning?
  - Are they in Alpha/Beta or Release Candidate stages?
  - Have they released a 1.0.0 version yet?
  - Is the project prone to breaking changes?
  - Linters? Formatters? CI/CD workflows?
  - What sort of tooling is used for development?

- Documentation.
  - Is the code well documented?
  - Is the code well commented?
  - Is there lag between the main branch and docs?

- Source Code.
  - Is the code well structured?
  - Do you understand it?
  - Could you confidently make contributions to it if you needed to?
  - How much code is there?
  - How abstract is it? 😏

Rolling-your-own has maintainability implications, but at the end of the day, it's code you wrote and understand.
Vulnerabilities in that code won't go away unless you fix them, but you'll be safe from [supply chain attacks](https://attack.mitre.org/techniques/T1195/).

Dependencies come with whatever their maintainers decide to put in their code. A healthy open-source community may detect and mitigate security vulnerabilities, fix bugs, add docs and maybe even introduce new features. An unhealthy community can lead to stagnant, vulnerable, out-of-date code that may even stop working with other dependencies or language versions.

Choose wisely!