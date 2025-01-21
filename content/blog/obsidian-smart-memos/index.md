+++
title = "Obsidian Automated AI Voice Workflows"
description = "Engineers hate him. Learn his one cheeky trick to run almost any workflow in Obsidian without extensions (mostly)."
date = 2025-01-18

[extra]
# banner = "TLP-Blog-Banner.webp"
draft = true

[taxonomies]
tags = ["Project", "Obsidian", "Software"]
+++

For a long time I've really wanted the ability to record voice notes on the go and access them later as text.
No, I'm not talking about dictation. I want something really robust and smart.

Obsidian Voice Notes.

TL;DR: I want to make a voice note workflow for Obsidian that works on mobile and desktop. The audio recording would be performed in Obsidian (Obsidian supports audio recording in the app with a built in extension). Upon saving the note (which results in a git push thanks to a third party extension ,fit) , a Github actions workflow runs. This workflow calls an Assembly AI API endpoint that extracts the data from the note and parses it with LeMUR to extract metadata and format content according to a template. That content would then be fed back into the original note the voice recording is embedded in. Finally, if enabled in the template, content may be sent to Eleven Labs AI for voice transcription.

Templates:

Templates inform LeMUR (chat based, takes instructions) and Eleven Labs AI.

Here's an example. Not sure how I feel about format or syntax, but doesn't exactly need to be json. Markdown is ideal really.

// Templates/Journal Entry

date: {{ Date}} 

author: {{ Author }}

tags : Journal

quoteAuthor: Kanye West

// Track Assembly AI templating status (ready, fail or success)

// If this field is not present, the overall workflow will not run

ProcessSTT: ready

// Track 11 Labs text to speech status (ready, fail or success)

// If this field is not present, no transcription will be run (disabled)

ProcessTTS: ready 

---

{{ I am a placeholder }}

// I am a comment

I am a static string

# {{ Title }}

{{ Tone/Emotion, 3 lines }}

{[ Context (when, where) , 2 lines }}

```dataviewjs 

const baseUrl = "https://inspirationalquotes.com/api"; 

const author = dv.current().quoteAuthor || "Unknown"; 

// Default to "Unknown" if no author is provided 

// Construct the API URL with the author query parameter const apiUrl = ${baseUrl}?author=${encodeURIComponent(author)}; 

try { const response = await fetch(apiUrl); 

if (!response.ok) throw new Error("Failed to fetch quote"); 

const data = await response.json(); 

// Assuming the API returns a JSON object with a "quote" field dv.paragraph(`"${data.quote}" - ${author}`); } catch (error) { dv.paragraph(`Failed to fetch a quote from ${author}. Please try again later.`); }

```

{{ TL;DR, paragraph }}

{{  Key points, 3 - 5 }}

--------------

Anyway,

On push, run a Github actions workflow that checks for unhandled voice notes.

Unhandled could be determined by a frontmatter variable in the markdown, or something else. 

In the workflow, send the info to Assembly AI, parse the tags, inject the template, commit the changes to the note and push. 

Maybe some of the other git plugins support Webhooks or some other method of forcing a sync remotely. 

Templates could be defined in the git repo and the workflow would use them. 