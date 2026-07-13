# meeting-copilot

Live meeting transcription with a real-time question engine. It streams audio through Whisper, keeps a rolling context of the conversation, and continuously drafts 2-3 sharp questions you could ask right now - based on what has been discussed, what has NOT been asked yet, and what your role in the meeting is. There is also a panic button: press it and get one good question immediately.

## why

Every notes app transcribes meetings after the fact. None of them help with the actual hard part: being in the meeting, knowing you should ask something, and blanking on what. As an intern this is a daily problem - the difference between looking checked-out and looking sharp is often one good question. The gap analysis ("nobody has asked about the rollback plan") is the genuinely interesting ML problem here.

## architecture

1. capture: tab/mic audio via a Chrome extension (Meet first)
2. transcribe: streaming Whisper, partial results over WebSocket, ~2s behind live
3. context: rolling window with topic segmentation, decayed relevance for old segments
4. question engine: every 30s (or on button press) generate candidates, rank by novelty vs what has already been asked, filter by role profile (engineer asks different questions than PM)
5. overlay: minimal on-screen panel, questions fade in without demanding attention

## hard parts, which are the point

- latency budget: audio -> usable question in under 4 seconds on the button press path
- "what has NOT been asked" requires tracking question-shaped utterances in the transcript and diffing against candidates - novelty detection, not just generation
- prompt discipline: questions must be specific to THIS conversation, generic filler gets filtered by a self-score threshold

Privacy note: everything runs on the local machine except the LLM calls; transcripts are never stored unless you export. Recording anyone still requires their consent - the tool nags you about this on start.

See docs/PLAN.md. Status: early.
