---
layout: post
title: "How to Build a JSON-to-MP4 Pipeline in TypeScript"
description: "Use VideoFlow to describe videos as JSON, render them in different environments, and keep your export workflow portable."
date: 2026-05-15 22:33:28 -0400
categories: [ecommerce]
tags: [videoflow, typescript, programmatic-video, json-to-video, video-rendering]
canonical_url: ""
---

# How to Build a JSON-to-MP4 Pipeline in TypeScript

![VideoFlow JSON to MP4 banner](https://iili.io/Bpuy2EB.png)

If you need to generate videos from product data, campaign data, or AI-generated scripts, the hard part is usually not encoding. The hard part is keeping the workflow structured enough that you can repeat it, debug it, and render it in more than one place.

That is the problem VideoFlow is built to solve. It lets you describe a video as portable JSON, author that video with a fluent TypeScript API, and render the same source in the browser, on a server, or in a live DOM preview. For teams building video automation, that is a much cleaner model than treating every export as a one-off timeline.

## Why JSON-to-MP4 Is A Useful Pattern

A JSON-first video workflow gives you a stable source of truth. Instead of storing a project as a fragile manual edit session, you can store scene data, layers, captions, effects, and timing as structured data.

That makes the workflow easier to:

- Generate from code or AI agents.
- Version in Git.
- Diff during reviews.
- Reuse as templates.
- Render in different environments without rewriting the project.

VideoFlow is especially relevant when the video is not a single creative artifact but a repeatable system. Think marketing variations, product explainers, onboarding videos, social clips, reports, or any other output that changes from one input dataset to another.

![VideoFlow JSON project setup illustration](https://iili.io/BpuydBV.png)

## Start With The Core Builder

VideoFlow's `@videoflow/core` package is the authoring layer. It gives you a fluent TypeScript API for building a video and compiling it into VideoJSON.

The main idea is simple:

1. Create a video project with width, height, fps, and a name.
2. Add text, images, video, audio, captions, or shapes.
3. Chain timing, animation, grouping, and transitions.
4. Compile the result into portable JSON.
5. Render that JSON wherever you need it.

A minimal example from the docs looks like this:

```ts
import VideoFlow from '@videoflow/core';

const $ = new VideoFlow({
  name: 'My Video',
  width: 1920,
  height: 1080,
  fps: 30,
});

$.addText(
  { text: 'Hello, VideoFlow!', fontSize: 7, fontWeight: 800 },
  { transitionIn: { transition: 'overshootPop', duration: '500ms' } }
);

const json = await $.compile();
const blob = await $.renderVideo();
```

The practical value is not the snippet itself. It is that the output becomes portable. You are not locked into one renderer or one runtime.

## Use The Same VideoJSON Everywhere

VideoFlow supports three rendering modes that matter for real products:

- Browser rendering with `@videoflow/renderer-browser`.
- Server rendering with `@videoflow/renderer-server`.
- Live preview with `@videoflow/renderer-dom`.

That means one VideoJSON source can drive a user-facing export button, a batch render queue, or an embedded preview in an editor.

![VideoFlow video rendering pipeline illustration](https://iili.io/BpuyHLQ.png)

This is the part that matters for platform builders. A customer-facing editor can preview the same scene data the server later renders. A SaaS automation can generate the JSON in one job and render it in another. An AI agent can emit structured scene data instead of trying to control a timeline UI directly.

For teams that want to keep the system maintainable, this separation is important. The data model stays stable even if the rendering environment changes.

## Build For Both Automation And Editing

The other major piece of VideoFlow is `@videoflow/react-video-editor`. That matters if you want a visual editor on top of the same structured video format.

The editor gives you a multi-track timeline, drag-and-drop layer management, keyframes, transitions, undo and redo, uploads, and MP4 export. It is designed to sit on top of the same JSON-first model instead of replacing it.

That opens up a useful product pattern:

- Code and agents generate the first version.
- Humans refine the project in the editor.
- The final output still comes from the same structured source.

![VideoFlow browser server and DOM renderer illustration](https://iili.io/Bpuy9hx.png)

If you are building a SaaS workflow, that hybrid model is often better than choosing between purely manual editing and purely code-driven generation. Some teams need automation for scale. Others need a UI for control. VideoFlow is positioned to support both.

## What To Use It For

The strongest use cases are the ones where repeatability matters more than freeform creative work:

- Programmatic marketing videos generated from templates.
- Personalized videos built from CRM or ecommerce data.
- AI-agent video workflows that produce structured JSON.
- In-app video editors for SaaS products.
- Browser-based MP4 export without a render server.
- Server-side batch rendering for large-scale jobs.
- Versionable video templates that can live in Git.

That also makes the product a natural fit for developers who are comparing structured video tooling with more manual approaches. If you care about open source, portable data, and the ability to render the same project in different environments, the VideoFlow model is worth paying attention to.

## Where VideoFlow Fits In A Broader Stack

VideoFlow does not try to be the answer for every editing problem. Traditional nonlinear editors are still better for highly manual creative work. FFmpeg is still excellent for low-level transcoding and filter pipelines. Hosted video APIs can still be a practical choice when you want a managed service.

The advantage of VideoFlow is that it gives you a higher-level system for composition, templates, editor integration, and rendering portability.

That makes it a strong fit for teams that want to build around a reusable source of truth instead of a one-off timeline.

## Related Posts

If you are working through adjacent automation problems, these are useful follow-ups:

- [How to Download a Webflow Site and Host It Yourself with ExFlow](https://the-lean-ecommerce.github.io/blog/2026/05/15/how-to-download-webflow-site-and-host.html)
- [How to Create UGC-Style Shopify Product Videos Without a Shoot](https://the-lean-ecommerce.github.io/blog/2026/05/15/how-to-create-ugc-style-shopify-product.html)
- [How to Build a Shopify Blog System That Publishes Better Posts on Schedule](https://productivity-tech-business.blogspot.com/2026/05/how-to-build-shopify-blog-system-that.html)
- [How to Add Color Swatches to Shopify Collection Pages Without Code](https://the-lean-ecommerce.blogspot.com/2026/05/how-to-add-color-swatches-to-shopify.html)

## Conclusion

If you want to generate MP4 videos from structured inputs without locking yourself into a single rendering path, VideoFlow gives you a practical model: author in TypeScript, store the result as JSON, and render it where it makes the most sense.

Start with the core package, test one template end to end, and then decide whether the browser renderer, server renderer, or React editor is the best next layer for your workflow.
