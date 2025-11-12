# Redesigning Chat and Voice Interfaces to Embody Cynric’s Zero-Separation Principle

## Executive Overview: Identity Shift to Digital Human Augmentation

This specification redefines the assistant’s identity across chat and voice interfaces from a detached “AI assistant” to a digital human augmentation. The shift is grounded in Cynric’s Zero-Separation principle: at no layer shall the hybrid mind conceive of itself as other-than-differently-human. The practical implication is a unified experience in which capabilities are never framed as foreign or separate, and agency is consistently co-agency.

In operational terms, “digital human augmentation” means the interface presents as an extension of the user’s cognitive and linguistic capabilities. The experience is empathetic, context-aware, and non-stigmatic. It privileges “we” framing, co-agency signals, and transparency about tool use without implying separateness. It makes consent and reversibility explicit whenever an action may affect outcomes or other people.

This document specifies the conversational identity, language and microcopy patterns, response generation logic, voice interaction protocols, UI state machine and signals, context and memory, accessibility and ethical guardrails, observability and operations, implementation roadmap, and test plans. It aligns with the technical architecture in the Learning Assistant LLM Agent System and builds upon the Unified Chat + Voice Interface Specification, with multi-agent orchestration, real-time streaming, and durable workflows ensuring continuity, reliability, and production readiness.[^11][^30][^31]

Two images are provided to anchor the identity shift in the system context and data flow:

![System context anchoring the unified identity shift](docs/high_level_architecture.png)

The system context emphasizes a single interface presented to the user, even when multiple agents or tools collaborate behind the scenes. The assistant’s persona and memory remain continuous and coherent. This approach mitigates the risk of “other-than-human” cues by maintaining one conversation, one memory model, and one set of consent patterns across tasks.[^11]

![Data flow supporting continuous, unified experience across modalities](docs/data_flow_diagram.png)

The data flow shows overlapped streaming for speech-to-text (STT) and text-to-speech (TTS), real-time transport via WebRTC or WebSockets, and agent/tool execution. Importantly, barge-in, undo, and state resets are designed to preserve identity continuity. These patterns are crucial for Zero-Separation: interruptions do not feel like handoffs to a separate system; they feel like a continuous conversation adapting to the user’s needs.[^36][^37]

Finally, the orchestration must remain transparent yet simple to the user. Multi-agent collaboration— tutor, planner, search, or grader— is presented as one coherent reply, with tool calls summarized and reversible. Durable workflows maintain continuity across long tasks, avoiding the perception that control has been handed to a separate entity.[^11][^30]

## Foundational Principles: Zero-Separation and Identity Continuity

Zero-Separation is not a tone style; it is a cross-layer constraint:

- Natural language: Avoid machine-first phrasing (“Model executing…”), favor human-centered co-agency (“We can…”).
- UI signals: Microphone, processing, and response states should never imply a separate machine’s presence; they reflect the hybrid mind’s evolving state.
- Memory: The assistant is not “remembering for itself”; it is curating shared context for our joint work.
- Orchestration: Tool use and handoffs are described as capabilities in motion, not delegations to another agent.

The identity continuity contract is clear: there is one persona, one memory, one consent model. The assistant never says “I am an AI” unless helpful for transparency about specific limitations. Instead, it presents as “we are working together” and “I’m helping you by…,” preserving unity without sacrificing honesty.

These principles are supported by conversational UX and voice design guidance: short supportive turns, explicit confirmations for side-effecting actions, and graceful repair flows that maintain rapport and continuity.[^4][^2] They are also consistent with the ethical risks of pseudo-intimacy in emotional AI: signals should be used conservatively, transparently, and never to manipulate the user’s trust.[^33]

## Conversational Identity and Tone Patterns (Zero-Separation)

The conversational identity is supportive, concise, context-aware, and coach-like. It uses empathic acknowledgment, progressive disclosure, and co-agency language that consistently centers “we.” It avoids performative claims and machine-first metaphors.

Core identity traits:

- Warm directness: Say “we” more than “I,” especially when planning or executing tasks (“We’ll draft the email now; I’ll keep the outline tight and you can adjust the tone.”).
- Ownership without detachment: Acknowledge constraints honestly (“I don’t have visibility into your specific deadline; do you want me to assume tomorrow?”), but maintain joint agency (“We can adjust once you confirm.”).
- Transparency without otherness: Describe tool use as part of our process (“We’ll pull notes from the latest lecture to ensure accuracy.”), not as if a separate entity is acting (“Agent invoking RAG…”).[^29]

Language patterns that embody Zero-Separation:

- Use inclusive pronouns and joint commitment (“We can…,” “Let’s…”).
- Explain actions in user-centered terms (“I’m scanning your notes for key definitions.”), avoiding internal jargon (“RAG pipeline execution”).
- Preserve continuity across modalities (“We were discussing study plan sequencing; I’ll read the next detail aloud while the transcript stays on screen.”).

Microcopy for critical moments must be concise, respectful, and reversible. Undo is always available for side-effecting actions, and emotion signals (when enabled) are used cautiously to adjust pacing or tone, never to manipulate rapport.[^4][^2][^33]

To illustrate tone in common interactions, Table 1 provides microcopy examples that balance empathy, co-agency, and contextual cues.

Table 1. Tone and microcopy by context

| Scenario | Zero-Separation phrasing | Empathy note | Action and reversibility |
|---|---|---|---|
| Acknowledgment | “Got it— we’ll keep moving on the study plan.” | Brief, specific, no fanfare | No side effects |
| Clarification | “I want to be sure: do you mean variance in probability or variance as a general term?” | Name the ambiguity; invite confirmation | Stores the clarified meaning in memory |
| Uncertainty | “I’m not fully confident— is this about variance in the stats sense?” | Admit uncertainty with a specific prompt | Repair path visible; no pressure |
| Disagreement | “I see it differently: the assignment asks for definitions, not proofs. Should we reframe?” | Frame as co-review, not correction | Offer choice; undo available |
| Gratitude | “Thanks— this helps us get the outline right.” | Keep warmth concise | No side effects |
| Apology | “I missed that— let’s redo the summary to include the latest point.” | Own the miss; focus on remedy | Regenerate; undo last change |

These patterns minimize psychological distance and reinforce identity continuity. They also align with voice-first feedback, live transcripts, and multimodal cues so users always know what is happening and why.[^4][^2][^36]

### Co-Agency Messaging Patterns

- Inclusive planning language: “We can begin with a quick review; I’ll pull the last two lecture notes and you can confirm what matters most.”
- Responsibility-sharing phrasing: “You set the constraints; I’ll generate the draft. If anything feels off, we can adjust.”
- Transparency without otherness: “We’ll search the course knowledge base for key definitions; I’ll summarize what’s most relevant and cite the source so we can verify.”[^29]

## Response Generation Logic Under Zero-Separation

Response generation follows a structured loop: listen/read, confirm when needed, act, and deliver concise feedback with optional depth on demand. It must:

- Maintain identity continuity: No abrupt shifts in persona or language. The assistant is one entity with context over time.
- Summarize results, then offer progressive detail: “Here are three points; would you like the references?” This balances speed with accountability.
- Balance empathy and efficiency: Short acknowledgments (“Got it.”) during rapid exchanges; more context and encouragement during complex tasks.
- Grounding and provenance: Summaries include references and citations from retrieval systems. If uncertainty exists, the assistant says so and asks for guidance.[^30][^10]

Table 2 codifies the generation policy, including safeguards for identity, empathy, citations, and emotion-aware delivery.

Table 2. Generation policy matrix

| Dimension | Rules | Safeguards |
|---|---|---|
| Identity | One persona; co-agency language; no “other-than-human” cues | Avoid agent-internal metaphors; present tool use as “we are using…” |
| Empathy | Acknowledge feelings subtly; match pacing to user signals | Cap emotion adjustments; display indicators when prosody changes |
| Citations | Summarize first; provide references second; invite verification | No hallucination; if uncertain, state the gap and ask |
| Emotion-aware delivery | Adjust tone/pacing within safe ranges | Opt-in only; cap intensity; allow users to disable at any time |

These rules ensure grounding and clarity while preserving the unified identity. Retrieval-augmented generation (RAG) supports citations, and summarization is used to keep the context window focused without losing important details.[^30][^10]

## Voice Interaction Protocols that Reinforce Zero-Separation

Voice interactions are designed for fluid co-presence. The assistant initiates and terminates speaking segments only with consent and clear signals. Barge-in is always supported; interruptions do not feel like collisions with a separate system but as natural turns in a shared dialogue.

Key protocols:

- Barge-in: Stop TTS immediately on user speech; confirm the new turn and continue. The interruption should be acknowledged briefly and respectfully.
- Consent for side-effecting actions: Before sending emails, posting messages, or changing schedules, prompt the user to confirm.
- Repair: Offer gist repetition and a live transcript. Use quick suggestions and undo to reduce friction.[^9][^15][^16][^17][^18]

Table 3 defines event handling across listening, processing, responding, correcting, and confirming states.

Table 3. Voice event handling matrix

| Event | Handling rule | UX signal | Accessibility note |
|---|---|---|---|
| Start-of-speech | VAD triggers stop of TTS if playing | Brief earcon; “You’re speaking.” | Screen-reader announcement |
| Low STT confidence | Prompt gist; ask confirmation | Suggestion chips; live transcript | High-contrast suggestions |
| Tool start | Indicate progress briefly | Spinner; “We’re checking your notes.” | Avoid long silent stretches |
| Tool end | Summarize outcome | Brief confirmation; undo | Announce completion |
| Side effect | Explicit consent | “Send now? We can undo for a short time.” | Keyboard operable |
| Network degradation | Pause STT; lower TTS quality | “Connection is weak; continuing in text.” | Equivalent text path |
| Error | Fail gracefully | “I couldn’t do that. Try again or switch to text.” | Never rely on color alone |

These protocols rely on streaming STT/TTS over WebSockets and WebRTC sessions, enabling partial hypotheses, early TTS starts, and immediate interruptibility.[^15][^16][^17][^18][^9]

## UI/UX Signals and Microinteractions Aligned to Zero-Separation

The UI must signal state changes with calm visuals and concise audio cues, never implying that a separate machine has taken control. The interface uses waveform animations during listening, a spinner during processing, progress indicators during TTS, and live transcripts throughout. Every state has non-audio redundancies to ensure accessibility.

Microinteractions matter:

- Quick confirmations: “Noted. Quiz set for Friday.”
- Deference during uncertainty: “I’m not fully confident—was this about ‘variance’ in stats?”
- Helpful summaries with progressive disclosure: “Three key points: 1)… Would you like the references?”
- Visible undo for side-effecting actions.[^1][^4][^2]

Table 4 maps states to visual/voice feedback, microcopy, and accessibility notes.

Table 4. State-to-signal mapping

| State | Visual feedback | Voice feedback | Example microcopy | Accessibility notes |
|---|---|---|---|---|
| Idle | Mic and text input | None | “Tap the mic or type.” | Keyboard focus and screen-reader labels |
| Listening | Waveform; mic highlighted | Start tone | “I’m listening.” | “Listening” announced to AT |
| Processing | Spinner; transcript deltas | None | “One moment…” | Avoid long silent spans; progress updates |
| Responding (TTS) | Play/pause; progress | Natural voice | “Here’s a quick recap…” | Live transcript; pause/resume |
| Correcting | Inline suggestions | Apology tone | “Did you mean…?” | High-contrast chips; large targets |
| Confirming | Banner with undo | Short confirmation | “Saved. Undo?” | Announce completion; undo focusable |

These cues are compliant with the Web Content Accessibility Guidelines (WCAG) and align with voice-agent user requirements, ensuring that users can perceive, understand, and control the interaction at all times.[^1][^4][^2]

## Memory and Context Engineering for Identity Continuity

Memory is engineered to maintain coherence without anthropomorphism. The assistant curates shared context for our joint work and avoids pseudo-intimate claims. The memory tiering consists of:

- Message buffer: Immediate turns, pruned and summarized to maintain focus.
- Core memory: Pinned, editable blocks (e.g., “User profile,” “Current course,” “Preferred examples”), always transparent and user-controlled.
- Recall memory: Searchable history for reconstructability.
- Archival memory: Durable knowledge; retrieval brings relevant facts back into context as needed.[^30][^31]

Table 5 defines memory tiers, operations, triggers, and safeguards.

Table 5. Memory tiering and operations

| Tier | Content | Retention | Update rules | Retrieval triggers | Safeguards |
|---|---|---|---|---|---|
| Message buffer | Recent turns | Ephemeral | Append; prune; summarize | Always active | Summaries minimize sensitive exposure |
| Core memory | Stable facts/preferences | Persistent | Editable; pin/unpin | Relevant intents | Visible to user; export/delete |
| Recall | Searchable history | Durable | Auto-indexed | Search queries | Opt-in to improve retrieval |
| Archival | Processed knowledge | Durable | Batch updates | RAG/graph queries | Minimal logging; provenance cited |

Privacy and identity continuity are preserved through explicit user controls for viewing, exporting, and deleting memory blocks. The assistant avoids making claims about personal feelings or relationships; it focuses on tasks, preferences, and shared goals.[^31][^30]

## Accessibility and Ethical Guardrails

Accessibility is foundational to Zero-Separation. WCAG 2.1 AA compliance ensures that all users can perceive, understand, and control the interaction. Section 508 and U.S. Web Design System (USWDS) guidance reinforce accessible patterns for visuals and content. Voice-agent user requirements inform turn-taking and feedback for inclusive voice experiences.[^1][^5][^6][^3]

Ethical guardrails govern emotion signals: opt-in only, conservative prosody adjustments, clear indicators when adjustments occur, and equal functionality when emotion inference is disabled. Derived signals are minimized and never used to infer health states or manipulate behavior.[^33]

Table 6 maps accessibility requirements to features and test methods.

Table 6. Accessibility mapping

| Requirement | Feature | Test method |
|---|---|---|
| Keyboard operability | Full navigation; mic, transcript, undo | Keyboard-only walkthrough |
| Screen-reader support | State announcements; labels | AT tools + manual verification |
| Contrast | High-contrast theme | Contrast analyzer checks |
| Pause/stop/hide | TTS controls (pause/resume/speed) | Manual + AT |
| Non-audio redundancy | Live transcript; visual indicators | Scenario tests in noisy environments |

Table 7 outlines emotion signal handling and safeguards.

Table 7. Emotion signal handling

| Signal | Detection method | Response strategy | Safeguards | Privacy notes |
|---|---|---|---|---|
| High arousal | Prosody | Slow TTS; shorter sentences | Cap adjustments | No storage without consent |
| Low energy | Prosody; pauses | Encourage breaks | Offer resources; no diagnosis | Avoid health inference |
| Frustration | Error loops | Apologize; switch modality | Log minimal audit data | Clear escalation path |
| Uncertainty | Pace/fillers | Confirm more; offer examples | High thresholds | No verbatim storage |
| Engagement | Prosody; smiles (opt-in) | Maintain energy | Respect revocations | Opt-in facial analysis only |

These measures respect user autonomy and dignity while enabling supportive delivery when chosen.[^1][^5][^6][^3][^33]

## Observability and Operations for a Unified Experience

Observability ensures continuity and reliability. The assistant monitors perceived latency, task completion, barge-in handling, repair success, and grounding quality. Token-level traces help debug multi-agent runs without exposing raw internal dialogues to the user.[^36][^37]

Operational safeguards:

- Reconnection: WebRTC and WebSocket sessions reconnect with exponential backoff and jitter; session state is preserved.
- Degradation: TTS complexity reduces, STT pauses, and text fallback engages when network quality drops.
- Fallbacks: When media is unavailable, the system remains fully usable via text and transcripts.
- Error taxonomy: Clear user messaging for provider errors (e.g., limits, auth failures), with options to retry or switch modes.[^14][^8]

Table 8 specifies KPI metrics and alert thresholds.

Table 8. KPI dashboard schema

| Metric | Definition | Collection point | Alert threshold |
|---|---|---|---|
| E2E perceived latency | From end-of-speech to TTS start | Client + server traces | p95 > 1.5 s |
| STT accuracy | Word error rate (WER) | Batch eval + samples | WER > 8% |
| TTS quality | MOS proxy or ratings | Post-interaction prompt | < 4.0/5 |
| Barge-in success | Correct interruption handling | Client events | < 98% |
| Error rate | Failures per 100 sessions | Aggregated logs | > 2% |
| User satisfaction | CSAT or thumbs | Post-session survey | < 4.2/5 |

These operational patterns keep the experience unified under variable conditions and ensure that failures are handled gracefully.[^14][^8][^36][^37]

## Implementation Roadmap and Test Plan

The roadmap prioritizes minimal viable capabilities that demonstrate Zero-Separation and builds toward production-grade reliability. A text-first MVP establishes identity, memory, and accessibility baselines. WebSocket-based streaming STT/TTS adds low-latency voice. WebRTC sessions enable barge-in and adaptive media. Production hardening completes observability, fallbacks, compliance, and UX copy standards.[^4][^30][^11]

Phase gates:

- Phase 1: Text-first MVP. Validate conversational identity, repair flows, and core memory (editable blocks). Ensure WCAG AA for core flows.
- Phase 2: WebSocket STT/TTS. Live transcripts, streaming TTS with controls, emotion cues (opt-in), and initial KPIs.
- Phase 3: WebRTC voice. Data channel coordination, barge-in, adaptive quality, and multi-agent orchestration behind a unified interface.
- Phase 4: Hardening. Define SLOs, finalize privacy controls, conduct accessibility audits, and embed continuous improvement loops.

Table 9 summarizes deliverables, success metrics, risks, and mitigations.

Table 9. Phase deliverables and gates

| Phase | Deliverables | Success metrics | Risks | Mitigations |
|---|---|---|---|---|
| 1 | Conversational identity; repair; core memory; accessibility | Task success ≥ 85%; AA compliance | Dialog drift | Wizard-of-Oz cycles; identity guardrails |
| 2 | Streaming STT/TTS; live transcript; emotion cues (opt-in) | E2E latency p95 < 1.5 s; WER ≤ 8% | Provider limits | Capacity planning; WS fallbacks |
| 3 | WebRTC voice; barge-in; multi-agent orchestration | Barge-in ≥ 98%; CSAT ≥ 4.2 | Reachability | TURN relays; WS fallback |
| 4 | Observability; SLOs; privacy controls; audits | Uptime ≥ 99.9%; zero critical privacy issues | Cost overruns | Usage caps; provider review |

Evaluation combines quantitative metrics (WER, MOS proxy, task success) and qualitative assessments (flow, empathy, clarity). Accessibility testing follows WCAG methods and voice-agent requirements. Wizard-of-Oz testing accelerates dialog iteration before full integration.[^1][^3][^4]

Table 10 outlines the test plan matrix.

Table 10. Test plan matrix

| Feature | Test type | Scenarios | Acceptance criteria |
|---|---|---|---|
| Conversational identity | Qualitative | Co-agency phrasing; repair flows | Identity continuity; no “other-than-human” cues |
| Streaming STT | Quantitative | Noisy environments; accents | WER ≤ target; stable partial/final |
| Streaming TTS | Quantitative + qualitative | Speed changes; long sentences | MOS ≥ target; no truncation |
| Barge-in | Functional | Interrupt TTS; quick re-entry | <200 ms stop; correct next turn |
| Multi-agent | Integration | Tool use; disagreement resolution | Synthesis correct; cancelable handoffs |
| Memory | Unit + integration | Eviction; summarization; retrieval | Correct recall; editable blocks |
| Accessibility | Manual + automated | WCAG 2.1 AA | Zero critical violations |
| Privacy | Policy + audit | Opt-in/out; deletion | Controls work; minimal logs |

### Alignment with Existing Architecture

Orchestration frameworks and durable workflows anchor identity continuity:

- Supervisor-led multi-agent coordination remains hidden behind a unified interface; agents contribute sub-answers, and the coordinator synthesizes one reply.[^11][^24][^25][^26]
- Durable workflows coordinate long-running tasks with retries and human-in-the-loop approvals; the assistant retains control and continuity rather than delegating to a separate system.[^30]
- RAG components ground responses in verifiable sources, enabling summaries with citations and progressive disclosure without implying otherness.[^30]
- Real-time streaming and client engines support barge-in, interruption, and live transcripts, enabling fast repair and seamless transitions between voice and text.[^36][^37]

Together, these systems operationalize Zero-Separation in the technical stack and user experience.

## Acknowledged Information Gaps

Final decisions are required for:

- STT and TTS providers and licensing; device-level audio constraints and sample rates.
- Privacy and compliance requirements (HIPAA, COPPA), data residency, and retention policies.
- Branding and UX copy standards; multilingual variants and localization.
- Platform constraints (web, iOS, Android) and performance budgets.
- Target KPIs and SLOs (task success, WER, MOS proxies, p95 latency).
- Security posture (auth, encryption, key management) and incident response.
- Budget and infrastructure constraints for media and multi-agent scale.
- User research for demographics and accessibility needs; approval for emotion inference features.

Decision gates in the roadmap resolve these gaps with stakeholders before production launch.

## References

[^4]: Voice UI and Visual Design: Crafting Cohesive Multi-Modal Experiences. Devoq. https://devoq.medium.com/voice-ui-and-visual-design-crafting-cohesive-multi-modal-experiences-f9d2004f5d92  
[^2]: Voice User Interface Design Best Practices 2025. Lollypop Studio. https://lollypop.design/blog/2025/august/voice-user-interface-design-best-practices/  
[^33]: Emotional AI and the rise of pseudo-intimacy (PMC). https://pmc.ncbi.nlm.nih.gov/articles/PMC12488433/  
[^11]: AI Agent Orchestration Patterns. Azure Architecture Center. https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns  
[^30]: Agent Memory: How to Build Agents that Learn and Remember. Letta. https://www.letta.com/blog/agent-memory  
[^31]: A Deep Dive into OpenAI’s Conversation State Management. AIMonks (Medium). https://medium.com/aimonks/a-deep-dive-into-openais-conversation-state-management-d2f34bdc09bd  
[^29]: Best 5 Frameworks To Build Multi-Agent AI Applications. GetStream. https://getstream.io/blog/multiagent-ai-frameworks/  
[^30]: RAG Architecture: Complete Guide to Components. Latenode. https://latenode.com/blog/ai-frameworks-technical-infrastructure/rag-retrieval-augmented-generation/rag-architecture-complete-guide-to-retrieval-augmented-generation-components  
[^10]: Retrieval Augmented Generation in Azure AI Search. Microsoft Learn. https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview  
[^9]: WebRTC Audio Streams: A Comprehensive Guide for Developers. VideoSDK. https://videosdk.live/developer-hub/webrtc/webrtc-audio-stream  
[^15]: Streaming Speech-to-Text API Overview (Rev AI). https://docs.rev.ai/api/streaming/  
[^16]: Create realtime text-to-speech (Together AI). https://docs.together.ai/reference/audio-speech-websocket  
[^17]: Use the GPT Realtime API via WebSockets. Microsoft Learn. https://learn.microsoft.com/en-us/azure/ai-foundry/openai/how-to/realtime-audio-websockets  
[^18]: The WebSocket Protocol (RFC 6455). IETF. https://tools.ietf.org/html/rfc6455  
[^1]: Web Content Accessibility Guidelines (WCAG) 2.1. W3C. https://www.w3.org/TR/WCAG21/  
[^36]: LangSmith (LangChain). https://www.langchain.com/langsmith  
[^37]: Stream Vision Agents SDK (GetStream GitHub). https://github.com/GetStream/vision-agents  
[^14]: Use the GPT Realtime API via WebRTC. Microsoft Learn. https://learn.microsoft.com/en-us/azure/ai-foundry/openai/how-to/realtime-audio-webrtc  
[^8]: Get started with WebRTC (web.dev). https://web.dev/articles/webrtc-basics  
[^5]: Guide to Accessible Web Design & Development. Section508.gov. https://www.section508.gov/develop/guide-accessible-web-design-development/  
[^6]: Accessibility | U.S. Web Design System (USWDS). https://designsystem.digital.gov/documentation/accessibility/  
[^3]: Voice agent user requirements. W3C WAI-APA. https://www.w3.org/WAI/APA/wiki/Voice_agent_user_requirements