# build plan

## phase 1 - audio + transcription
- [ ] Chrome extension: capture tab audio in Meet, chunk to 1s frames
- [ ] local relay server (FastAPI + WebSocket) feeding streaming Whisper
- [ ] partial transcript stream back to the extension, ~2s lag target
- [ ] consent nag on session start, always

## phase 2 - rolling context
- [ ] transcript segmentation into topics (embedding drift breakpoints)
- [ ] rolling window: recent segments full-res, older segments summarized
- [ ] question tracker: detect question-shaped utterances and log them (this feeds novelty)

## phase 3 - question engine
- [ ] candidate generation every 30s from current topic + role profile
- [ ] novelty rank: candidate vs already-asked questions (embedding distance)
- [ ] self-score filter: specificity check, generic filler dies here
- [ ] button path: press -> one question -> under 4s end to end

## phase 4 - overlay UI
- [ ] minimal panel injected into Meet, 2-3 questions, fade transitions
- [ ] button + keyboard shortcut
- [ ] nothing stored unless exported

## phase 5 - post-meeting
- [ ] optional export: transcript, questions asked vs suggested, action items

## latency budget (button path)
- audio already transcribed (streaming) : 0ms
- context assembly                      : <200ms
- LLM generation (small fast model)    : <2.5s
- rank + filter                        : <300ms
- render                               : <100ms

## decisions
- extension + local relay over a bot that joins the call - joining bots creep people out and get blocked
- streaming ASR locally (faster-whisper) to keep audio off third-party servers
- role profiles are a config file, not a login system. this is a tool, not a SaaS (yet)
