+++
title = 'Narro'
date = 2026-10-23
summary = 'A Chrome extension that reads web pages aloud with natural AI voices, in the cloud or entirely in your browser. Supports Kindle Reader too.'
draft = false
image = 'player.png'
+++

I wanted a read-aloud extension that could read from the [Kindle Online Reader](https://read.amazon.in/). The ones I tried used
voices I did not enjoy listening to or had a UX that was too cluttered or annoying. A good enough option that I found is [CastReader](https://castreader.com/) but it requires a subscription and the UI is somewhat buggy. I waned something that runs locally or with a preferred cloud provider.

Now since I have some AI subscriptions, I decided to vibe code my own. In about 2 evenings I have a tool that is fairly usable and serves my purpose. I call it [**Narro**](https://swrdfish.github.io/Narro/). It reads the page you're on, highlights each sentence as it's spoken, and works with most modern AI models. It supports both local and cloud
hosted models - the local models run in browser using WebGPU for improved performance.

{{< youtube F1HC9DOluug >}}



## What it does

- Highlights each sentence as it's read, and scrolls to follow along.
- Sends several sentences at a time, so the voice keeps a natural rhythm instead of resetting at every full stop.
- Lets you start anywhere: click **Start from** and pick a sentence, or select text and right-click to start reading from there.
- Remembers where you stopped on each page.
- Skips half-sentences cut off at the edge of a page, and joins a sentence that continues on the next page.
- Can be paused or stopped from any tab. The toolbar icon shows a red ▶︎ while it's playing.
- Reads Kindle Cloud Reader books using on-device text recognition (uses [Tesseract.js](https://github.com/naptha/tesseract.js/))
- It can turn pages inside the Kindle Cloud Reader as it goes.


{{< figures >}}
{{< figure src="player.png" width="68%" alt="..." caption="Sentence highlighting and the floating player" >}}
{{< figure src="popup.png" width="32%" alt="..." caption="The popup: controls and settings" >}}
{{< /figures >}}

## Voices

- **In browser:** Kokoro (the default), Supertonic and Piper, Kitten TTS. Models are downloaded on the fly and then cached. Everything works locally.
- **Cloud:** Supports OpenAI, ElevenLabs, Gemini, or OpenRouter. The user needs to provide the respective api key for the cloud models to work.
- **Other:** It supports OpenAI-compatible speech server running locally. I have not tested this.

I have open sourced the tool, have a look: [GitHub](https://github.com/swrdfish/Narro)