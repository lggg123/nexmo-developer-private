---
title: Speech Recognition
description: Capture user input by converting user speech to text form during a call.
navigation_weight: 6
---

# Speech Recognition (ASR)

## Overview

Automatic Speech Recognition (ASR) enables apps to support voice input for such use cases as IVR, identification and different kinds of voice bots/assistants. Using this feature, the app gets transcribed user speech (in the text form) once it expects the user to answer some question by saying it rather than entering digits (DTMF); and then may continue the call flow according to its business logic based on what the user said.

## How it works

```sequence_diagram
participant U as User
participant V as Vonage
participant A as App

activate U
U->>V: phone call

activate V
V->>A: answer callback

activate A
A->>V: NCCO
note left of A: talk/stream\ninput

V->>U: audio message

U->>V: speech

V->>A: text

A->>V: NCCO
note left of A: next action(s)

V-->U: next action(s)
```

Typically, ASR is used in conjunction with an audio message playing to the user. The message might be an [audio file](/voice/voice-api/code-snippets/play-an-audio-stream-into-a-call) or [Text-to-Speech](/voice/voice-api/guides/text-to-speech), or a combination of both played sequentially. To activate the ASR, NCCO should contain an `input` action with speech parameters specified.

### NCCO Example

```json
[
  {
    "action": "talk",
    "text": "Hello!"
  },
  {
    "action": "talk",
    "text": "Please tell us, how can we help you today?",
    "bargeIn": true
  },
  {
    "eventUrl": [
      "https://api.example.com/callbacks/events"
    ],
    "eventMethod": "POST",
    "action": "input",
    "type": [ "speech" ],
    "speech": {
      "language": "en-gb",
      "context": [
        "support",
        "buy",
        "credit",
        "account"
      ],
      "endOnSilence": 0.5,
      "saveAudio": true      
    }
  }
]
```

The [NCCO Reference Guide](/voice/voice-api/ncco-reference#speech-recognition-settings) contains information on all the possible parameters that can be used in conjunction with the ASR `input` NCCO action.

### Input Type
Set `type` as `speech` for speech input only, or `[ "dtmf", "speech" ]` to accept both speech or [DTMF](/voice/voice-api/guides/dtmf).

### Language

The expected language of user speech should be specified as the `language` parameter (`en-US` by default).

#### Supported languages

| Language | Code
| ---- | ----
| Afrikaans (South Africa) | `af-ZA`
| Albanian (Albania) | `sq-AL`
| Amharic (Ethiopia) | `am-ET`
| Arabic (Algeria) | `ar-DZ`
| Arabic (Bahrain) | `ar-BH`
| Arabic (Egypt) | `ar-EG`
| Arabic (Iraq) | `ar-IQ`
| Arabic (Israel) | `ar-IL`
| Arabic (Jordan) | `ar-JO`
| Arabic (Kuwait) | `ar-KW`
| Arabic (Lebanon) | `ar-LB`
| Arabic (Morocco) | `ar-MA`
| Arabic (Oman) | `ar-OM`
| Arabic (Qatar) | `ar-QA`
| Arabic (Saudi Arabia) | `ar-SA`
| Arabic (State of Palestine) | `ar-PS`
| Arabic (Tunisia) | `ar-TN`
| Arabic (United Arab Emirates) | `ar-AE`
| Armenian (Armenia) | `hy-AM`
| Azerbaijani (Azerbaijan) | `az-AZ`
| Basque (Spain) | `eu-ES`
| Bengali (Bangladesh) | `bn-BD`
| Bengali (India) | `bn-IN`
| Bulgarian (Bulgaria) | `bg-BG`
| Burmese (Myanmar) | `my-MM`
| Catalan (Spain) | `ca-ES`
| Chinese, Cantonese (Traditional, Hong Kong) | `yue-hant-HK`
| Chinese, Mandarin (Simplified, China) | `zh` (`cmn-hans-cn`)
| Chinese, Mandarin (Traditional, Taiwan) | `zh-TW` (`cmn-hant-TW`)
| Croatian (Croatia) | `hr-HR`
| Czech (Czech Republic) | `cs-CZ`
| Danish (Denmark) | `da-DK`
| Dutch (Belgium) | `nl-BE`
| Dutch (Netherlands) | `nl-NL`
| English (Australia) | `en-AU`
| English (Canada) | `en-CA`
| English (Ghana) | `en-GH`
| English (India) | `en-IN`
| English (Ireland) | `en-IE`
| English (Kenya) | `en-KE`
| English (New Zealand) | `en-NZ`
| English (Nigeria) | `en-NG`
| English (Philippines) | `en-PH`
| English (Singapore) | `en-SG`
| English (South Africa) | `en-ZA`
| English (Tanzania) | `en-TZ`
| English (United Kingdom) | `en-GB`
| English (United States) | `en-US`
| Estonian (Estonia) | `et-EE`
| Filipino | `fil-PH`
| Finnish (Finland) | `fi-FI`
| French (Canada) | `fr-CA`
| French (France) | `fr-FR`
| Galician (Spain) | `gl-ES`
| Georgian (Georgia) | `ka-GE`
| German (Germany) | `de-DE`
| Greek (Greece) | `el-GR`
| Gujarati (India) | `gu-IN`
| Hebrew (Israel) | `he-IL`
| Hindi (India) | `hi-IN`
| Hungarian (Hungary) | `hu-HU`
| Icelandic (Iceland) | `is-IS`
| Indonesian (Indonesia) | `id-ID`
| Italian (Italy) | `it-IT`
| Japanese (Japan) | `ja-JP`
| Javanese (Indonesia) | `jv-ID`
| Kannada (India) | `kn-IN`
| Khmer (Cambodia) | `km-KH`
| Korean (South Korea) | `ko-KR`
| Lao (Laos) | `lo-LA`
| Latvian (Latvia) | `lv-LV`
| Lithuanian (Lithuania) | `lt-LT`
| Macedonian (North Macedonia) | `mk-MK`
| Malay (Malaysia) | `ms-MY`
| Malayalam (India) | `ml-IN`
| Marathi (India) | `mr-IN`
| Mongolian (Mongolia) | `mn-MN`
| Nepali (Nepal) | `ne-NP`
| Norwegian Bokmål (Norway) | `nb-NO`
| Persian (Iran) | `fa-IR`
| Polish (Poland) | `pl-PL`
| Portuguese (Brazil) | `pt-BR`
| Portuguese (Portugal) | `pt-PT`
| Punjabi (Gurmukhi, India) | `pa-guru-IN`
| Romanian (Romania) | `ro-RO`
| Russian (Russia) | `ru-RU`
| Serbian (Serbia) | `sr-RS`
| Sinhala (Sri Lanka) | `si-LK`
| Slovak (Slovakia) | `sk-SK`
| Slovenian (Slovenia) | `sl-SI`
| Spanish (Argentina) | `es-AR`
| Spanish (Bolivia) | `es-BO`
| Spanish (Chile) | `es-CL`
| Spanish (Colombia) | `es-CO`
| Spanish (Costa Rica) | `es-CR`
| Spanish (Dominican Republic) | `es-DO`
| Spanish (Ecuador) | `es-EC`
| Spanish (El Salvador) | `es-SV`
| Spanish (Guatemala) | `es-GT`
| Spanish (Honduras) | `es-HN`
| Spanish (Mexico) | `es-MX`
| Spanish (Nicaragua) | `es-NI`
| Spanish (Panama) | `es-PA`
| Spanish (Paraguay) | `es-PY`
| Spanish (Peru) | `es-PE`
| Spanish (Puerto Rico) | `es-PR`
| Spanish (Spain) | `es-ES`
| Spanish (United States) | `es-US`
| Spanish (Uruguay) | `es-UY`
| Spanish (Venezuela) | `es-VE`
| Sundanese (Indonesia) | `su-ID`
| Swahili (Kenya) | `sw-KE`
| Swahili (Tanzania) | `sw-TZ`
| Swedish (Sweden) | `sv-SE`
| Tamil (India) | `ta-IN`
| Tamil (Malaysia) | `ta-MY`
| Tamil (Singapore) | `ta-SG`
| Tamil (Sri Lanka) | `ta-LK`
| Telugu (India) | `te-IN`
| Thai (Thailand) | `th-TH`
| Turkish (Turkey) | `tr-TR`
| Ukrainian (Ukraine) | `uk-UA`
| Urdu (India) | `ur-IN`
| Urdu (Pakistan) | `ur-PK`
| Uzbek (Uzbekistan) | `uz-UZ`
| Vietnamese (Vietnam) | `vi-VN`
| Zulu (South Africa) | `zu-ZA`

### Context

Some hints might be provided using the `context` array parameter to improve recognition quality. The values set might be the words or phrases expected from the user, for example, `['one', 'two', 'three']` or `['support', 'order starship']`.

### Barge In

If the user is calling not for the first time, they may already know the question to be asked, so the user may start speaking even before the audio message finishes. In order to support that, `bargeIn` parameter of the TTS (or `stream` - whatever action is used for the message) should be activated.

> It is recommended to have the initial TTS/audio message be a short initial greeting without activating the `bargeIn` option to improve the user experience. If `bargeIn` is turned on for the first initial greeting, then the user may inadvertently interrupt it without hearing the prompt at all, since background noise may be interpreted by the application as an active interaction in those first moments.

### Save Audio

The speech audio may be optionally stored. To get the recording, the `saveAudio` parameter should be set, then the `recording_url` will be included in the callback request payload. This might be useful to compare the transcription with the original audio. Use the [Download a recording](/voice/voice-api/code-snippets/download-a-recording) code snippet to download the audio.

### Event Payload Example

Once the NCCO `input` action is completed, the input callback will be sent:

```json
{
  "speech": {
    "recording_url": "https://api-us.nexmo.com/v1/files/ee94a327-ab6b-4bef-86bf-cadca33343e9",
    "timeout_reason": "end_on_silence_timeout",
    "results": [
      {
        "confidence": "0.9405097",
        "text": "Support"
      },
      {
        "confidence": "0.70543784",
        "text": "Supper"
      },
      {
        "confidence": "0.5949854",
        "text": "Puppy"
      }
    ]
  },
  "dtmf": {
    "digits": null,
    "timed_out": false
  },
  "uuid": "529aa8f0-0ad8-42b0-ad61-b76dca18bb52",
  "conversation_uuid": "CON-7347b08d-5f51-478f-bfb2-e8c3fd39509f",
  "timestamp": "2020-02-07T11:42:40.933Z"
}
```

In the response body, a new NCCO is expected, containing next call flow actions based on application logic and user input (`speech.results.text`). See also [Webhook Reference](/voice/voice-api/webhook-reference) for details. 
