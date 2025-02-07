---
editable: false
---
# Quotas and limits

In the [!KEYREF speechkit-full-name] service, the following limitations apply:

[!INCLUDE [quotes-limits-def.md](../../_includes/quotes-limits-def.md)]

## Speech recognition {#stt}

### Recognition of short audio files {#stt-short}

Limits applying to [recognition of short audio files](../stt/request.md).

#### Limits {#stt-short-limits}

| Type of limit | Value |
| ----- | ----- |
| Maximum file size | [!KEYREF stt-short—fileSize] |
| Maximum duration of audio | [!KEYREF stt-short-audioLength] |
| Maximum number of audio channels | [!KEYREF stt-short-channelsCount] |

### Recognition of long audio files {#stt-long}

Limits applying to [recognition of long audio files](../stt/streaming.md).

#### Quotas {#stt-long-quotas}

| Type of limit | Value |
| ----- | ----- |
| Recognition requests per day | 50 |
| Operation status check requests per hour | 200 |

#### Limits {#stt-long-limits}

| Type of limit | Value |
| ----- | ----- |
| Maximum file size | [!KEYREF stt-long-fileSize] |
| Period for storing recognition results on the server | [!KEYREF stt-long-resultsStorageTime] |

### Data streaming recognition {#stt-streaming}

Limits that apply to [data streaming recognition](../stt/streaming.md).

#### Limits {#stt-streaming-limits}

| Type of limit | Value |
| ----- | ----- |
| Maximum duration of transmitted audio for the entire session | [!KEYREF stt-streaming-fileSize] |
| Maximum size of transmitted audio data | [!KEYREF stt-streaming-audioLength] |

## Speech synthesis {#tts}

Currently, there are no quotas or limits for speech synthesis.

