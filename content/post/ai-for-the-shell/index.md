---
title: AI for the Shell
description: Cross-platform, "pipable" AI directly in your terminal. Bring the power of LLMs to your local environment. ⚡️
slug: ai-for-the-shell
date: 2024-05-30 00:00:00+0000
image: cover.jpg
categories:
    - Showcase
tags:
    - AI
    - Tool
    - OpenAI
    - Groq

# weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

## What is it?

AI integration is becoming ubiquitous. Warp is pioneering "the terminal of the future" with AI capabilities built-in. VS Code supports extensions like Github Copilot and Cody, while Zed includes Zed AI. But what if you need a simple, versatile tool to bring AI into any conversation? Checkout **Mods**. Charm's dev team and community are on a roll, producing some awesome new apps and libraries, and this is no exception. 👀

![Credit: Charmbracelet](mods_img.png)

Mods takes the concept of prompting a GPT and brings it to the command line. It even supports piping stdin and stdout!
Any output from any command on your terminal, think `ls`, `cat`, `curl` and the like, can become input for Mods. Prompt it with some instructions on how to handle that data and you can either get rich feedback in your terminal or pipe the output for further processing.

Curious? Here are some examples!

---

## Examples

### Rich Markdown Output

Mods supports rich colour and formatting out-of-the-box. Install Charm's Glow, set the default output format to markdown and every response will be pretty. ✨

![Disclaimer: I don't know anything about Pokemon 😬](pokemon.gif)

### Evaluate Script Security

How about those inherently-unsafe one-liner shell script installers? Now finally safe with Mods! Here's an example in WSL Ubuntu:
![Easily get a second opinion on shell script safety.](oneliner.gif)

### Piping GitHub API Data

Feed it raw JSON data from an API. Here it is running in good old Powershell 5 on Windows 10:
![Shameless plug for my cheeky repo.](rate-repo.gif)

### Piping to Text-to-Speech

What about some text to speech? We can pipe stdout to a text to speech engine. Here's a little example with Wsay, a Windows equivalent to "Say" on MacOS: [https://github.com/p-groarke/wsay](https://github.com/p-groarke/wsay)
![Duck haikus for life](tts.gif)

> 📝 **Note...**
>
> It might be worth considering something a little more natural if you want it to read you bedtime stories. [Google text-to-speech](https://github.com/pndurette/gTTS) might be a better option.

<audio controls preload="auto">
    <source src="tts_limerick.wav">
</audio>

### Calling in a Loop

You can even call it in a loop! This will ask for 10 fresh duck haikus. Each iteration is a new conversation. No conversational context; guaranteed freshness. 🍃

```powershell
# 10 🦆 haikus straight to your speakers!

1..10 | % { "Tell me a haiku about ducks" | mods | wsay -o duck_haiku$_.wav } 
```

And in Bash, like so:

```bash
    for i in {1..10}
    do
        echo "Haiku time!" | mods | gtts-cli - --output duck_haiku$i.mp3
    done
```

For more examples check out the official ones from [the Mods repo](https://github.com/charmbracelet/mods/blob/main/examples.md)

## Multi LLM Support

Mods supports multiple Large Language Models (LLMs) through multiple providers. Providing an OpenAI API key gets you a pretty quick out-of-box experience. It's already set up to use OpenAI's GPT-4 by default.

Pop open the settings with:

`mods --settings`

Note, you'll have to define a default `$EDITOR` in your environment variables.

You'll be greeted with the *mods.yml* file. Check out the APIs section. Mods supports:

* **OpenAI**: GPT-4O, GPT-4, GPT-3.5-Turbo, etc.

* **Azure**: GPT-4O, GPT-4, GPT-3.5-Turbo, etc.

* **Perplexity AI**: CodeLlama, Mistral, Claude 3, Sonar, etc.

* **Groq**: Mixtral, Llama 2, Llama 3, etc.

* **RunPod**: OpenChat, Ollama, etc (IaaS/SaaS)

* **Local**: Llama.cpp, GPT4ALL.cpp, etc. (local - GPU optional!)

> 📝 **Note...**
>
> Until today, I'd never tried Groq. Boy is it fast! Evidently, Groq provides [significantly faster inference](https://www.semianalysis.com/p/groq-inference-tokenomics-speed-but) through next-gen hardware: Language Processing Units (LPUs).
>
> API access is free, at least for now. I couldn't find any official pricing anywhere. My guess is they're relying on the free access to spread the word so they sell their chips 🍟💲

Check it out: [https://groq.com](https://groq.com/?ref=ghost.notablegravity.com)

```yaml
# Default model (gpt-3.5-turbo, gpt-4, ggml-gpt4all-j...).
default-model: gpt-4
...

apis:
  openai:
    base-url: https://api.openai.com/v1
    api-key:
    api-key-env: OPENAI_API_KEY
    models:
      gpt-4:
        aliases: ["4"]
        max-input-chars: 24500
        fallback: gpt-3.5-turbo
      gpt-4-1106-preview:
        aliases: ["128k"]
        max-input-chars: 392000
        fallback: gpt-4
...
```

Configuration is the same for all providers. Provide the base url, desired models, and specify aliases for those models. If you want to use a specific model, call it like this:

`"What is gravity?" | mods --model="Mixtral"`

You can also supply `-M` or `--Ask-Model` to interactively choose a model (though this seems to be ignored if you pass a prompt non-interactively).

## Conversational Memory

Nice! Now when I was generating haikus I hard-sold amneisa as a feature for conversational freshness. What if we wanted to remember conversations across commands? Maybe with something more complicated... Something that can't be done "zero-shot"?

Well thankfully Mods has some handy parameters that allow resuming and managing conversations:

```yaml
  -c, --continue       Continue from the last response or a given save title.
  -C, --continue-last  Continue from the last response.
  -l, --list           Lists saved conversations.
  -t, --title          Saves the current conversation with the given title.
  -d, --delete         Deletes a saved conversation with the given title or ID.

  -s, --show           Show a saved conversation with the given title or ID.
  -S, --show-last      Show the last saved conversation.
```

### A Really Heavy Example

Here's a PowerShell commandlet I made (along with 59 other functions necessary to make it work). I use it to provision temporary user accounts for student exams at my workplace.

`New-Exam -D 10/03 -FT 8 -TT 11 -Alloc 105 -U 5 -CC "10SCI"`

My bespoke exam provisioning commandlet

For the sake of your screen real-estate I've used the parameter aliases, but I will explain in a moment!

Essentially this commandlet:

* Sets Active Directory account logon hours

* Sets account expiration time

* Sets group membership (for exam conditions)

* Validates users by name or student ID and fetches AD attributes

* Populates an HTML login sheet template with user data and exam details

* Merges the sheets into a PDF

And it saves me **HOURS.**

But one thing remains that sucks like a parasite at my remaining time and brain power:

**The exam login requests themselves.**

The problem is that login reqeuests aren't consistent. Requests come in all shapes and sizes. Some come with multiple updates across a chain of emails. Some come in concise bullet points. Some say "next Wednesday". Others say "19/05/2024".

What an excellent opportunity for AI – and for Mods!

#### Interfacing with the Commandlet

So first I told Mods how to commandlet. works and the inputs and outputs I expect:

```powershell

$Instructions = "New-Exam is a powershell commandlet.
> It is used to create temporary Active Directory accounts for exams.
>
> Here's an example:
> New-Exam -D 10/03 -FT 8 -TT 11 -U 5 -S "10SCI" -C
>
> -D is "date"
> -FT is "from time"
> -TT is "to time"
> -U is "user count"
> -S is "subject"
> -C is "conditions" (do not include)
>
> Follow the format of the example.
> Output the exact command to run, no extra information or context.
> If data is missing or invalid, output:
> 'echo "Invalid data: ___" 
> where blank is the invalid or missing data.

$Instructions | mods --title="New Exam"
```

> 📝 **Note...**
>
> "Conditions" above I asked it not to include. The conditions aren't difficult to pass, but for the sake of experimentation I left them out. Fortunately, if a mandatory parameter is left out of a PowerShell commandlet it will be prompted at runtime. Good to know!

#### Pushing Jobs

Then I gave it an exam login request email.

```powershell
$Message = "Hi IT team!
>
> We have a science exam on tomorrow.
> 
> Please can we have four logins?
>
> - Disable internet
> - No USB access
> - They can have spellcheck
>
> Start: 10:30, Finish: 12PM
>
> Kind regards,
>
> Dane Joe"

$ Message | mods --continue="New Exam" | Invoke-Expression
```

#### Retrospect

Running the commandlet is now as easy as "continuing" the conversation with the previous context (the instructions) and piping it to PowerShell's Invoke-Expression. Super easy!

I actually have a lot of validation built into the commandlet. Like what if the exam is on a Saturday? Shouldn't happen. What if the subject is only offered in grade 12 and a grade 10 student is taking it? Shouldn't happen.

I'm a little wonky about trusting validation to the mysterious *Latent Space*. Getting different answers to the same question asked twice makes me a little twitchy... But technically all of those validation rules could be applied at the prompt context level instead.

Could this all be solved with a simple HTML form? Definitely! Yes.

Should it? Mmhmm. Probably!

As a general rule, if you want automation to produce consistently reliable outputs, you need consistently reliable inputs. AI "magics" away some of this, but it still requires some massaging and testing.

In a situation like this, if a user form was not an option, it would be a great candidate for an AI flow in LangChain or Flowise. I'm hoping to take a look at these soon!

## Wrapping Up

Mods is a great bridge between AI LLMs great demo of some cheap ad-hoc glue to try things out. Great for protyping, proof-of-concept, or ad-hoc formatting/parsing/sorting and the like. Perfect for someone like me who frequently gets caught up on the implementation details. With mods I can rapidly prototype in English, adjusting for desired behaviour with additional prompts. Once I've got a proof of concept, I can lay down something more solid.

If an HTTP form This is actually a great use case for LangChain or Flowise. Hoping to take a look at these soon!

So "Don't be shy, give Mods a try."
