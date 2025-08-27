# Gemini Image Generator - n8n Workflow

This n8n workflow allows you to generate AI images using the Gemini API by sending a text input via chat trigger. It fetches both text and image response and converts the base64 image into binary format usable in various outputs (Telegram, Airtable, etc).

## Features
- Chat Trigger (LangChain or custom)
- Gemini API integration
- Image + Text response
- Fully binary-ready output

## Setup

1. Replace `YOUR_GEMINI_API_KEY` in HTTP Request node
2. Import workflow in n8n
3. Connect it with Telegram, Airtable, or any output node


⚠️ Replace `YOUR_GEMINI_API_KEY_HERE` in the HTTP Request node with your own Gemini API key.
You can get it from: https://makersuite.google.com/app/apikey

<img width="624" height="608" alt="API" src="https://github.com/user-attachments/assets/311137e2-54ab-486e-839b-4b62b60388bc" />

## Preview

<img width="1361" height="690" alt="workflow pic" src="https://github.com/user-attachments/assets/5ac138bd-a6d6-41cb-9f34-2952dd702f01" />

## Code Node (JavaScript)

```javascript
return items.map(item => {
  const responseData = item.json.data;
  if (responseData && responseData.candidates && responseData.candidates.length > 0) {
    const imageData = responseData.candidates[0].content.parts[0].inlineData.data;
    const mimeType = responseData.candidates[0].content.parts[0].inlineData.mimeType;

    item.binary = item.binary || {};
    item.binary.image = {
      data: imageData,
      mimeType: mimeType,
      fileName: "gemini-generated-image.png",
    };

    const textPart = responseData.candidates[0].content.parts.find(part => part.text);
    if (textPart) {
      item.json.generatedText = textPart.text;
    }

    return item;
  } else {
    console.warn("No image data found in the response.");
    return item;
  }
});
```
## Files

- `gemini-image-generator-workflow.json` – The full n8n workflow
- `README.md` – This file 😄

