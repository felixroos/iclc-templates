# Practical Considerations?

The preceding chapter was concerned with the theoretical aspect of compiling a domain specific language describing graphs into a sequence of low level instructions.
Up until now, we have not yet explored how the constraints of live coding practices might affect its practical implementation.
Graph compilation is generic enough to be used in different kinds of real-time audio/video applications central to live coding. So far, in order to test our method, we have developed a WebAudio version (kabel.salat.dev), a GLSL shader version mimicking Hydra (_hydro_) and a version that compiles to static C code.

## Code Updates

In a live coding context, each code update written by the user triggers the compilation of a new graph -- a new set of instructions.
Swapping between iterations of a graph at runtime requires special attention, as without proper handling, transitioning may lead to a range of issues: amplitude jumps, audible cracks, phase discontinuities, etc.
In the web version of KabelSalat, updates are communicated to the AudioWorklet via a MessagePort [@Roberts18].
Inside the AudioWorklet, each compiled graph is contained by an instance of the `Unit` class, which represents a snapshot of the state at the time of evaluation. It contains the compiled instruction sequence, as well as the therein referenced `nodes`, as described in section 3.2.1.
Similar to the strategy used by the JITLIB library [@jitlibscbook], KabelSalat creates a parametrable audio fade between the old and the new `Unit`, smoothening the transitioning and effectively eliminating potential audio artifacts. By contrast, fades are not necessary -- and thus not implemented -- for certain applications such as compiling GLSL shaders^[The worst case in the visual domain is a flash from a light to a dark color.].
For video applications, a new shader program is created and swapped with the old one when the code is updated.

## State preservation

A key complexity arising from manual code updates is the preservation of state -- vital for maintaining a sense of temporal continuity for the musician.
This encompasses playing sequences of values over time, maintaining phase between evaluations and dealing with other time-dependant processes.
This challenge becomes particularly acute when managing low-frequency periodic signals and/or slow signal variations, where minor state disruptions can have a significant impact on perceptible results.
Without further annotation by the compiler, a new set of `nodes` has to be created for each new `Unit`.
This will cause any state within `Node`'s to reset, even if the structure of the graph itself has not changed between two evaluations.
The current version of KabelSalat is not offering a solution to this particular problem.
At the moment of writing these lines, any state will reset on update.
To preserve state, a mechanism is needed to let `nodes` of the next graph find their counterpart in the old graph, continuing where they left off.
One potential solution would be to annotate `nodes` with a unique identifier, which can be used to match old and new Nodes, passing state along.

---

## Real Time Input

The Web Audio version of KabelSalat supports both Audio and MIDI Input (through the Web MIDI API).
These inputs allow direct integration with the code through a microphone, through MIDI Controllers and/or in-source UI elements.
As a result, KabelSalat can be used as a synthesis-oriented companion tool for various live coding setups, allowing the live coding of synthesizers and audio treatments on-the-fly.

<!-- Elaborate, this is super interesting for the non technical people! -->

## REPL

KabelSalat's website^[Website link: [https://kabel.salat.dev/](https://kabel.salat.dev/) (accessed on September 27, 2024). ] hosts the latest version of KabelSalat's web runtime.
It can be used as a way to experiment, share patches and live code without any audio interruption.
It consists of a code editor (1), a graph visualizer (2), example patches (3) and an interactive documentation (4).
Similar to the Strudel REPL [@strudel], the code editor supports in-source UI elements, such as buttons and sliders.
The URL always reflects the latest code change, allowing patches to be shared as a hyperlink.

## Live Performance

KabelSalat has already been used a few times in a live context.
Felix made a performance using KabelSalat and Strudel side-by-side. KabelSalat was mainly used for live looping a trumpet and the input from a MIDI keyboard. Inspired by tape loops, a looper node has been created using long delay lines.
The in-source UI controls were a handy tool to control the looper with one hand while playing.
Strudel was also used as a sequencer to trigger synthesizers hosted by KabelSalat via MIDI.
The combination of algorithmic patterns with a flexible way to design sounds on-the-fly proved to be fruitful^[Felix's performance on Youtube: [https://www.youtube.com/watch?v=MXz8131Ut0A](https://www.youtube.com/watch?v=MXz8131Ut0A) (_idem_)].

Besides some great code contributions to the project, programmer and artist pulu has used KabelSalat to write a handful of exciting patches, including a goa trance track that has been performed with a MIDI controller^[pulu's performance on Youtube: [https://www.youtube.com/watch?v=uGn2mVF_jkI](https://www.youtube.com/watch?v=uGn2mVF_jkI) (_idem_)]. The track contains various sections along with controls to manipulate individual effect chains. It is a great demonstration of how a MIDI controller can be used to play a patch like an instrument.

Additionally, jan Ten has used KabelSalat as a target for ORCA sequences, using both tools side-by-side. While ORCA sequences were live coded, KabelSalat was mainly used to set up instruments and change parameters via sliders. https://www.youtube.com/watch?v=wiHH35GR908

![Live performance using KabelSalat and Strudel side-by-side (Rudolf5 Algorave in Karlsruhe Germany, July 26th 2024). The performance included live looped trumpet sounds and MIDI input. Photography: Jia Liu](./images/live.jpg)

# Conclusions

## Limitations and future outlooks

Though KabelSalat's runtime and its surrounding experimental ecosystem show promising potential, they currently exist in an early developmental stage.
This early phase comes with a set of limitations that will be adressed in future iterations of the project.
Each limitation or obstacle opens a field of investigation for future research.

While it is possible to update node values from the outside without the need to recompile, updates cannot be scheduled in the future. Such a mechanism is required to queue events similar to SuperDough in Strudel [@strudel]. Furthermore, the current version does not reuse nodes from previous evaluations. This means that the node state will reset on each update. This leads to sequences and phases being reset as well, which is often undesirable. Finding nodes that can be kept across evaluations would be possible by employing a diffing algorithm between the old and the new graph. Potential performance gains could be achieved in the web version by compiling to WebAssembly instead of JavaScript [@Roberts22].

In the future, further steps will be taken in the direction of becoming an event based audio engine, as required by SuperDough.
The handling of Unit's could be extended to allow evaluating graphs in a block based fashion, where multiple Unit's can coexist in parallel.
Tidal patterns [@tidal;@tidal2] might also be combined with an audio graph in a different way, by using Patterns as inputs for individual nodes, rather than composing expressions to a single pattern.
Being able to collaboratively build patches would be a great addition as well, either through the KabelSalat REPL or as an integration into a tool like Flok.cc^[Flok is a peer-to-peer collaborative live coding environment created by Damián Silvani. The website is available under [https://flok.cc/](https://flok.cc/) (accessed on September 27, 2024).].

## License and Acknowledgements

All code is open source under the AGPL-3.0 License. KabelSalat development is taking place on GitHub^[Repository link: [https://github.com/felixroos/KabelSalat](https://github.com/felixroos/KabelSalat) (_idem_).]. Contributions are welcome.

Thanks to the Strudel and wider Tidal, live coding, WebAudio and free/open source software communities for inspiration and support.
I'd like to express my gratitude to Maxime Chevalier-Boisvert for creating NoiseCraft, which was the starting point of the project.
Similarly, I would like to thank pulu for being an early adopter, creating mesmerizing patches and providing valuable feedback and contributions.
Finally, I wish to thank Raphaël Maurice Forment for being a good conversation partner during the journey of the implementation of KabelSalat, eventually joining this paper as a second author, helping with proof-reading, phrasing and further research.

\newpage

# References
