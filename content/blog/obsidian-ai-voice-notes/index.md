+++
title = "Obsidian Automated AI Voice Workflows"
description = "Engineers hate him. Learn his one cheeky trick to run almost any workflow in Obsidian without extensions (mostly)."
date = 2025-01-22

[extra]
banner = "Obsidian Gem Illustration.webp"

[taxonomies]
tags = ["Project", "Obsidian", "Software"]
+++


## The Problem 🤔
For a long time, I've wanted the ability to record voice notes on the go and access them later as text.

No, I'm not talking about simple dictation, I want something that cuts down on my distracted "umms", "ahhs", flight-of-ideas, and other incoherent vocalised nonsense as I drive like the incapable-of-multitasking man I am.

For a while, I used [AudioPen](https://audiopen.ai/), and it was pretty fantastic at condensing everything import into neat little summaries. The features have grown over the years, but I'm still a little wary of paying a subscription. 

I dreamed a little as I entertained the thought of building something out:
- It would be open-source
- Ideally extendable
- Loosely-coupled to work with a multitude of tools
- What if it could run ***any*** workflow?

I couldn't find anything like that yet...


**So I started building my own thing, again.**
(As if I didn't start enough projects over my parental leave break 🤦‍♂️👶)

---

## The Idea 💡

And so ***ThinkAloud.md*** was born!

(And at time of writing, is very much still a baby)

🫱 [Check it out!](https://github.com/in03/ThinkAloud) 🫲

Here's the gist:

1. **Audio Recording**:
   - Utilise Obsidian's built-in audio recording feature to capture voice notes directly within the app.

2. **Git Sync Backend**:
   - using `fit` or `git` extensions for Obisidian, git push is triggered to GitHub.

3. **GitHub Actions Workflow**:
   - The push initiates a GitHub Actions workflow that processes the new voice note.

4. **Speech-to-Text Conversion**:
   - The workflow sends the audio file to AssemblyAI's API for transcription.

5. **Content Parsing and Formatting**:
   - Assembly AI's LeMUR parses the transcribed text to extract metadata and formats the content according to predefined templates.

6. **Text-to-Speech (Optional)**:
   - If specified in the template, the content is sent to Eleven Labs AI for voice synthesis.

7. **Update Obsidian Note**:
   - The processed content is fed back into the original note containing the voice recording.

## The Doing It 🏃

Granted, it's a little async... I'm a bit worried that it'll be too slow. But it's very powerful. Anything that can run in a GitHub workflow can influence what ends up in the notes. 

My personal uses cases are pretty much all while I'm commuting. I like to "think out" long form ideas on-the-go and review my notes quite a bit later, so granted the CI/CD workflow doesn't fail, it should be fine for me.

If you're after similar and are happy with less flexibility, I'd recommend AudioPen or Obsidian Scribe.

## The Templates 🍽️
Templates guide LeMUR and Eleven Labs AI in processing and formatting the content. Here's an example of a "journal entry" template:

```markdown
---
author: {{ Author }}
date: {{ Date }} 
tags: journal, diary, log

match_phrase: captains log
process_stt: ready
process_tts: ready 
---

# {{ Title }}

## In a Nutshell

*Today I was feeling...*

{{ Tone/Emotion, 3 lines }}

**Context**: {{ Context. When, where, etc. 50 words }}

{{ Provide a synopsis on the journal entry, roughly a paragraph }}

## Key points 

{{ Key points, 3 - 5 }}

## Full text 

{{ Provide verbatim transcript lightly cleaned up: "umms", "ahhs", stutters, grammatical mistakes removed etc. }}
```

In a nutshell, the syntax is pretty much this:

```markdown
I am a **static** ~~potato~~ *markdown* string, I will be completely ignored by the LLM for context's sake.

## Big Ol' MD Heading

{{ I am a placeholder string. Put instructions in me for the LLM to follow}}

// I am a comment. I will be stripped out of the final output.
```

{% alert(note=true) %} 
Not really sure about the comment syntax. Markdown is funny with comments. They're not really part of the "spec".
{% end %}

## The Power ⚡️

Since the templates are processed by a GPT, you can get really creative 🎨 You can even chain further workflows to automate further:

- **Clean up ad-lib artefacts**: "umms" and "ahhs",
mistakes, stutters, silences.

- **Re-record**: Swap your noisy car voice-note with a professional studio-quality voice-over using [ElevenLabs](https://elevenlabs.io/).

- **Speaker diarisation**: Title each reader uniquely for conversational dialogue, like for a podcast.

- **Journal**: Summarise your whole day as a structured journal entry with time and place context, emotional analysis, key points, etc.

- **Populate Dataviews**: Populate Obsidian dataview fields, autofill tables, queries and kick-off Javascript tasks.

- **Integrate with Workflow**: [n8n](https://n8n.io/) workflows.

- **Blogging**: Create draft posts that match the structure and tone of your personal blog, or even publish directly to GitHub pages afterwards.

- **Social Cross-Posting**: Publish to multiple social media platforms at once, respecting their various algorithms.

- **BYD**: Generate and publish proof-of-concept apps from spur-of-the-moment app ideas.

---

## Existing Solutions

While developing this workflow, I explored existing plugins that offer similar functionalities:

- **[Obsidian Vox](https://github.com/vincentbavitz/obsidian-vox)**:
  - Automatically transcribes audio notes, extracting metadata, categories, and tags.
  - Frontmatter and verbatim text only, no templating or workflows.

- **[Obsidian Transcription](https://github.com/djmango/obsidian-transcription)**:
  - Creates high-quality text transcriptions from media files using OpenAI's Whisper.
  - Verbatim text only, no templating or workflows.

- **[Obsidian Scribe](https://github.com/Mikodin/obsidian-scribe)**:
  - Records voice notes, transcribes, summarises, and enriches them with AI.
  - Ask questions mid recording, automatically filled in.
  - Still no workflows.

These plugins offer valuable features, and I could have settled with Vox or Scribe. But while I really enjoy Obsidian, I love the idea of being able to use this for any markdown notes, anywhere. 

Better yet is the flexibility of being able to kick off all sorts of actions with CI/CD workflows!



---

Stay tuned for updates as this project progresses! ✨
