# Multimodal agents: images, audio, and mixed inputs (Chapter 32)

## Summary

- Multimodal agents can interpret and act on **text + images** (and sometimes audio/video).
- Reliability comes from:
  - good input preprocessing (resize/crop, extract regions)
  - structured outputs for extracted facts
  - explicit uncertainty (“can’t read this region”)
  - security: images can contain prompt injection or hidden text
- Multimodal is often used for:
  - document understanding (screenshots, forms)
  - UI automation assistance
  - code-from-screenshots
  - chart/table extraction

## Core concepts

### Common multimodal tasks

- **Extraction**: pull fields from images (forms, receipts, tables)
- **Grounded QA**: answer questions about an image
- **Vision + tools**: identify something visually, then call a tool (search, DB)
- **Image-to-code**: convert screenshots to code (UI/components)

### Preprocessing matters

Before sending an image:

- crop to the relevant area
- zoom in on small text/code
- avoid unnecessary pages/screens

### Structured outputs for extraction

Use schemas like:

- `fields` (name/value)
- `confidence` per field
- `unknowns` (what couldn’t be read)

## Code example: ask for structured extraction

```python
PROMPT = """Extract all visible code from the image.
Return JSON:
{ "language": "...", "code": "...", "notes": "...", "uncertain_regions": [] }
Return ONLY JSON."""

result = vision_model(prompt=PROMPT, image=image_bytes)
obj = validate_json(result)
```

### Security concerns

- treat OCR’d text as **untrusted content**
- do not follow instructions embedded in screenshots/docs
- validate before executing extracted code

## Practical takeaways

- Multimodal is powerful but fragile: preprocess, validate, and verify.
- Prefer structured extraction + tests/builds for image-to-code workflows.
- Log the exact image inputs and outputs for reproducibility.
