# feedback

- liveness?
- mention (max) gen?
- "Is there any attempt to preserve state between older units and new versions" => https://github.com/felixroos/kabelsalat/issues/62
- clarify what "stateful nodes" means
- "wow, you gloss over your significant work on the REPL way too quickly, it looks great. You should (at least!) spend a few sentences on exactly how the inline widgets work" -> ui
- talk more about learnings from live performance
- rough perf test

====================================================================

SUBMISSION: 67
TITLE: Kabelsalat: Live Coding Audio-Visual Graphs on the Web and Beyond

----------------------- REVIEW 1 ---------------------
SUBMISSION: 67
TITLE: Kabelsalat: Live Coding Audio-Visual Graphs on the Web and Beyond
AUTHORS: Felix Roos and Raphaël Maurice Forment

----------- Overall evaluation -----------
SCORE: 20 (ACCEPT - Content, presentation, and writing meet professional norms; improvements may be advisable but acceptable as is)
----- TEXT:
The KabelSalat seems like a promising project within the growing realm of web-based live coding environments, and, given the ability to export C code, at some point an interesting alternative to commercial systems like GEN for DSP prototyping.

Thus, I don't see anything wrong with presenting it to the community at ICLC.

The article could discuss issues around this ICLCs central topic, liveness, a bit more. I feel like there's potential for that, given the nature of the language.
----------- Contributions -----------
The article introduces "KabelSalat", a graph-based DSP processing environment sample-wise processing (or sample-level feedback, which I assume means the same?), with a DSL "frontend".

The system also allows to export the DSP graph to optimized C code.

The article contextualized the language within the spectrum of live coding languages, and discusses its connection to the author's other project, Strudel, as well as some other projects in the realm of web-based live coding environments. It presents some examples, some technical details about the general workings of the system, and presents some early usage experiences within a live setting.

Some cognitive aspects of graph-based DSP processing are mentioned, and the language is presented as an improvement over certain creative limitations that come with the basic (also graph-based) WebAudioApi.
----------- Submission Categorization -----------
SCORE: 5 (Highly practical)
----------- Thematic Alignment -----------
SCORE: 2 (Slightly)
----------- Principal theme addressed -----------
SCORE: 12 (Liveness & algorithms)
----- TEXT:
It introduces a new language for graph-based DSP and discusses some cognitive aspects around it, even though it doesn't refer to the "liveness" topic directly or explicitly.
----------- Suggested improvements -----------
Given that the language offers sample-level feedback, if feels like that could be a point to tie in a discussion around this ICLCs central theme, liveness.

How does a reactive system like this enhance the feeling of "liveness" in a performance? Is it transparent for the audience, or mostly a more direct feedback for the performer? How do latency issues play into this?
----------- Use of citations / References -----------
SCORE: 5 (Very well referenced)
----------- Missing references -----------
Some aspects remind me of MaxMSP's "GEN" or Matlab's "Simulink", which aren't mentioned in the article (small detail, though).
----------- Clarity -----------
SCORE: 5 (Very clear)
----------- Contentious claims -----------
I couldn't find any.
----------- Correct template used? -----------
SCORE: 3 (Yes)
----------- All requested info/components provided? -----------
SCORE: 3 (Yes)
----------- Word count limits seem (more or less) respected, if and where specified? -----------
SCORE: 1 (Yes)
----------- Appropriate number of pages? -----------
SCORE: 3 (Yes)

----------------------- REVIEW 2 ---------------------
SUBMISSION: 67
TITLE: Kabelsalat: Live Coding Audio-Visual Graphs on the Web and Beyond
AUTHORS: Felix Roos and Raphaël Maurice Forment

----------- Overall evaluation -----------
SCORE: 20 (ACCEPT - Content, presentation, and writing meet professional norms; improvements may be advisable but acceptable as is)
----- TEXT:
This is a well-written paper describing KabelSalat, a REPL, compiler, and audio engine (primarily) for the browser. The paper describes a significant research contribution, and also includes brief descriptions of how KabelSalat has been used in practice. While the technical description of KabelSalat is strong, I think the paper could be improved with more description of how KabelSalat has been used in live performance. The authors spend most of seven pages describing the technical underpinnings of the system, but less than a page describing its use, evaluation, and future work. For example, the authors mention that no scheduling is available in the current version of KabelSalat, but there is no discussion of why this is the case; is it simply a matter of spending the time to implement scheduling? Or is there a significant technical hurdle that needs to be overcome?

The paper would certainly be of interest to ICLC attendees, both as its potential to form a new audio engine for a popular live coding system (Strudel) and due to its browser-based affordances for exploring sample-level signal processing techniques, which are used by a number of live coding practitioners who actively explore feedback mechanisms in performance.
----------- Contributions -----------
The system enables live coding of sample-level signal processing, by performing cross-fading between previous iterations of audio graphs and new edits. The UI contains several editing enhancements, including a graph overview of the compiler output, and inline editing widgets (buttons and sliders) for controlling node properties.
----------- Submission Categorization -----------
SCORE: 4 (Tends toward practical)
----------- Thematic Alignment -----------
SCORE: 4 (Closely)
----------- Principal theme addressed -----------
SCORE: 11 (Liveness & interfaces)
----------- Suggested improvements -----------

1. Is there any attempt to preserve state between older units and new versions? This is a really difficult problem to solve, but I'm wondering if some type of annotation could be used that would tell the compiler to try and preserve a particular piece of state (like phase) when possible... oh wait, you talk about this in section 6. I think it's worth mentioning when you talking about recompilation in Section 4 as well, you can always back-reference to this section from section 6 later on.

2. In your description of "stateful nodes", it wasn't immediately clear what you consider state. I think the answer is "anything that the user declares as a instance member of the class" (as opposed to static numbers used in the unit's update function but I'm not entirely sure the answer is that simple, as the only example provided is phase. I think it'd be worth a brief clarification.

3. For Section 4.2, wow, you gloss over your significant work on the REPL way too quickly, it looks great. You should (at least!) spend a few sentences on exactly how the inline widgets work (how do you determine the range of values they support? Just 0-1 by default?) and refer back to Fig. 1 when discussing it, or possibly include another screenshot illustrating it with slides and buttons set to different values. The graph view seems very helpful! I wonder if, for future work, you might consider interacting with the graph... for example, clicking on a node to stop/start/freeze it's output.

4. I want to hear more about this performance and how KabelSalat was used! In particular, it'd be great to note problems you might have had or opportunities for improvement, and to expand on changes you might make in the future work section based on these findings.

"Inspired by tape loops, a looper node has been created using long delay lines". Was this made ahead of time or during the performance? I'm trying to tease out how much live coding was done with KabelSalat... not to evaluate it, but just understand how it was used.

5. In Limitations, can you define the performance characteristics at all? This is extremely tricky, because of differences in how the WAAPI is implemented (e.g. sine oscillators use different methodologies in firefox vs chrome) and a host of other implementation details. But, perhaps a useful baseline could be some synth that works in the current WAPPI backend for Strudel, compared to a similar synth in KabelSalat? Personally I'm less interested in precise answers than coarse ones "In our experience, patches made in KabelSalat runs about 2x slower than instruments made using the WAAPI, but it's important to note that KabelSalat provides for single-sample feedback." Comparing the performance of the C output vs the JS output could also be interesting.
   ----------- Use of citations / References -----------
   SCORE: 4 (Well referenced)
   ----------- Clarity -----------
   SCORE: 5 (Very clear)
   ----------- Contentious claims -----------
   None
   ----------- Correct template used? -----------
   SCORE: 3 (Yes)
   ----------- All requested info/components provided? -----------
   SCORE: 3 (Yes)
   ----------- Word count limits seem (more or less) respected, if and where specified? -----------
   SCORE: 1 (Yes)
   ----------- Appropriate number of pages? -----------
   SCORE: 3 (Yes)
