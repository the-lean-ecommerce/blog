---
layout: post
title: "How to Build a Portable JSON-to-Video Pipeline with VideoFlow"
description: "A practical way to turn structured JSON into reusable MP4 workflows with VideoFlow."
date: 2026-05-17 14:36:16 +0000
categories: [ecommerce]
tags: [javascript, typescript, video, automation, open-source]
canonical_url: ""
image: "/assets/img/posts/2026-05-17-how-to-build-a-portable-json-to-video-pipeline-with-videoflow/cover-1503e1c411d7.png"
---

![Browser and code-driven video rendering scene](/assets/img/posts/2026-05-17-how-to-build-a-portable-json-to-video-pipeline-with-videoflow/image-01-1503e1c411d7.png)

If you need to ship video without turning every change into a manual edit session, the hard part is usually not encoding. It is keeping the project structured enough that you can reuse it, diff it, and render it in more than one place.

That is the problem VideoFlow is built for.

VideoFlow treats a video as portable JSON, with a TypeScript builder on top and multiple renderers underneath. In practice, that means you can author a scene once, store it as data, then render the same project in the browser, on the server, or inside a live preview. For teams building automation tools, product demos, SaaS editors, or LLM-driven video flows, that is a much better shape than a one-off timeline file.

## Start With The Shape Of The Video

The easiest way to think about VideoFlow is as a source-of-truth layer for videos. You define the scene in `@videoflow/core`, compile it into VideoJSON, and then decide where rendering happens later.

![JSON cards and timeline pipeline diagram](/assets/img/posts/2026-05-17-how-to-build-a-portable-json-to-video-pipeline-with-videoflow/image-02-6dc46b11a3ce.png)

That design is useful for a few reasons:

- You can version a video template in Git.
- You can generate video scenes from code or an AI agent.
- You can render the same project in different environments without rewriting the composition.
- You can keep editor state and export state aligned because both can point at the same JSON structure.

Here is the kind of flow I reach for when I want repeatable output:

1. Build a scene in TypeScript.
2. Compile to VideoJSON.
3. Preview it in the DOM renderer.
4. Export it in the browser or on the server.
5. Reuse the same template for variants.

That is much closer to software engineering than to hand-editing video.

## What The Core API Gives You

The `@videoflow/core` package is the part that turns a composition into something maintainable. It gives you a fluent API for text, image, video, audio, captions, and shape layers, plus grouping, keyframes, transitions, and parallel animation.

A minimal example looks like this:

```ts
import VideoFlow from '@videoflow/core';

const $ = new VideoFlow({
  name: 'Product teaser',
  width: 1920,
  height: 1080,
  fps: 30,
});

$.addText(
  { text: 'New product launch', fontSize: 7, fontWeight: 800 },
  { transitionIn: { transition: 'overshootPop', duration: '500ms' } }
);

const json = await $.compile();
const blob = await $.renderVideo();
```

The important part is not the sample itself. It is the fact that the project becomes portable data after compilation. That makes it easier to generate from templates, store in a database, or hand off to a renderer later.

For reference, the main docs are here:

- [VideoFlow site](https://videoflow.dev/)
- [Docs](https://videoflow.dev/docs)
- [Core docs](https://videoflow.dev/core)
- [Renderer docs](https://videoflow.dev/renderers)
- [Playground](https://videoflow.dev/playground)
- [Examples](https://videoflow.dev/examples)
- [GitHub repo](https://github.com/ybouane/VideoFlow)

## Browser Export Or Server Jobs

This is where VideoFlow gets more interesting than a simple composition helper. The same JSON can drive browser rendering, server-side rendering, and live DOM preview.

![JSON data flow into reusable video layers](/assets/img/posts/2026-05-17-how-to-build-a-portable-json-to-video-pipeline-with-videoflow/image-03-d7a366a92da2.png)

That lets you choose the export path based on the actual product requirement:

- Use browser rendering when you want the user to export locally, avoid uploads, or keep cost down.
- Use server rendering when you need queues, scheduled jobs, batch processing, or API-driven generation.
- Use the DOM renderer when you need a live preview that stays close to the final output.

For a SaaS app, that separation matters. The editor can stay interactive while the renderer stays isolated. For an automation workflow, it means an LLM or backend job can generate the scene data and hand it off without touching a manual timeline.

If you are comparing patterns, I would also read these earlier notes:

- [How to Generate MP4 Videos from JSON with TypeScript](https://the-lean-ecommerce.blogspot.com/2026/05/how-to-generate-mp4-videos-from-json.html)
- [How to Build a Browser-Based MP4 Export Pipeline with VideoFlow](https://the-lean-ecommerce.blogspot.com/2026/05/how-to-build-browser-based-mp4-export.html)
- [How to Turn JSON Into MP4 Videos in TypeScript with VideoFlow](https://the-lean-ecommerce.blogspot.com/2026/05/how-to-turn-json-into-mp4-videos-in.html)
- [How to Create UGC-Style Shopify Product Videos Without a Shoot](https://the-lean-ecommerce.blogspot.com/2026/05/how-to-create-ugc-style-shopify-product.html)

## When The React Editor Helps

If you need non-technical users to adjust the output, the React video editor is the other half of the story. It adds a multi-track timeline, drag-and-drop editing, keyframes, transitions, preview, and MP4 export on top of the same VideoFlow model.

![Split browser and server rendering pipeline](/assets/img/posts/2026-05-17-how-to-build-a-portable-json-to-video-pipeline-with-videoflow/image-04-458164a40c9f.png)

That is a cleaner architecture than trying to maintain one data model for engineering and another one for the editor. The same JSON can be the store format, the preview input, and the export payload.

I like that because it reduces the number of translation layers. Less translation usually means fewer bugs.

## Where VideoFlow Fits Best

I would use VideoFlow when the output needs to be repeatable, versioned, and generated from data. Good fits include:

- Social clips built from templates.
- Personalized marketing videos.
- Product demos generated from catalogs or CRM rows.
- In-app video editors for SaaS products.
- Agent workflows where an LLM produces structured scene data.
- Batch rendering pipelines for repeatable jobs.

I would not use it as a replacement for a full nonlinear editor when the job is highly manual and artistic. That is not the point. VideoFlow is for structured video systems where code is the right control surface.

## The Part I Would Build First

If I were wiring this into a real product, I would start with one template, one preview path, and one export path. Then I would make sure the scene definition stayed stable enough to diff in Git.

That gives you a foundation you can extend later with variants, localization, and automated generation.

If you want the next step, start with the [VideoFlow docs](https://videoflow.dev/docs), open the [playground](https://videoflow.dev/playground), and wire one JSON template into a real render path.

VideoFlow works best when you treat video like software: composable, repeatable, and built from a source of truth instead of a sequence of ad hoc edits.
