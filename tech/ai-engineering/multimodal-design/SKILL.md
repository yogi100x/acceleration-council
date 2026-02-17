# Multimodal Design

Design systems that process and combine text, images, audio, and documents.

## Research Protocol
### Web Search
- "multimodal AI architecture patterns [current year]"
- "vision language model best practices [current year]"
### WebFetch
- https://docs.anthropic.com/en/docs/build-with-claude/vision

## Decision Tree: Modality Handling

```
What modality do you need?
├── Image understanding (describe, extract, classify)
│   └── Vision LLM (Claude Vision, GPT-4V)
│       → Send image + prompt, get text response
├── OCR / Document extraction
│   └── Vision LLM for complex layouts, dedicated OCR for simple
│       → Vision: invoices, forms. OCR: receipts, IDs
├── Audio transcription
│   └── Whisper (open source) or cloud STT (Deepgram, AssemblyAI)
├── Text-to-speech
│   └── ElevenLabs, OpenAI TTS, or browser SpeechSynthesis
├── Image generation
│   └── DALL-E, Midjourney, Stable Diffusion
├── Combined (text + image → analysis)
│   └── Multi-modal prompt with both inputs
│       → Single request with text + image to vision model
└── Video understanding
    └── Frame extraction + vision model on key frames
        → Extract 1 frame/sec, analyze with vision model
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Model selection** | Right model per modality, benchmarked | Standard models | Random selection |
| **Preprocessing** | Optimized input (resize images, compress audio) | Basic preprocessing | Raw input |
| **Error handling** | Graceful fallback for unsupported content | Basic errors | Crashes on bad input |
| **Cost** | Optimized image sizes, audio chunking | Some optimization | Full resolution everything |
| **Latency** | Parallel processing, appropriate timeouts | Sequential | Unbounded processing time |
| **Quality** | Output validated against ground truth | Basic validation | No quality checks |
| **Accessibility** | Alt text for images, transcripts for audio | Some accessibility | Not accessible |

**28+ = Effective multimodal | 21-27 = Functional | <21 = Poor quality or wasteful**
