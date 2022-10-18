A thread about ongoing performance work in the Synth, updated periodically.

![1](https://chrismaughan.com/secondbrain/static/synth_perf_first.png)

Here is a Tracy profile of the synthesizer.  When the sound card asks for more audio samples, I have to 'run' the audio graph to generate the data.  That's the green blob on the left.  It is on the thread that the sound card/driver has called me on.  Here it is taking about 2ms, but this is with 8 note polyphony, 2 #oscillators per note and a low pass filter.  There is around 10ms for this work to complete before the sound card would get starved.  In future I'm going to modify the graph engine to run the oscillators in parallel, but I'm holding off on that because I believe that a typical scenario will involve several of these green blobs running parallel, representing different combinations of synths and effects.  The bit to the right of the green blob is the audio processing.  This is now scheduled onto unique threads in a pool.

![2](https://chrismaughan.com/secondbrain/static/synth_perf_view.png)

The processing is generating the frequency spectrum you can see here (I can optionally split the spectrum up into individual notes).
The data processing involves finding trigger points in the audio wave for stabilising the rendering (like the trigger function on an oscilloscope), and running an FFT analysis on the audio to generate frequency bins.
It's not a huge amount of work, but you can see in the graph that it is now parallelized and running around 700us in time
Typically I'll be running audio analysis x 2 for stereo; but it can't be done in parallel with the sounds generation, so I wanted it to be efficient.
I don't believe in premature optimisation, and push work like this towards the end of the project. Here, however, there are design considerations which mean I need to spend some time thinking about it up front so I don't box myself in.

![2](https://chrismaughan.com/secondbrain/static/synth_perf_thread_wakeup.png)

As one more interesting data point, you can see that my threadpool on this machine is taking 4.5us to 'wake up' after I schedule the processing onto it, which I think is pretty good - the processing threads have almost all woken up and begun before the end of the function that wakes them; and it doesn't do much else.
Initially I had a spinning thread waiting on a condition variable, but I realized that my threadpool code is doing the same thing, but without the extra management of maintaining the loop and communication.

![3](https://chrismaughan.com/secondbrain/static/synth_perf_workers.png)

... time passes....

In this update, a nice little dump of my synthesizer project - setup to generate many notes on many different synthesizer graphs.  Each row is a different worker thread, and the blue bar is the total duration of the audio callback thread.  I still have lots of optimizations to try, but this is just a first step to ensure I can do this without issues.
Some of this work is actually unnecessary too; the small blocks tacked on the end are the FFT for visualization.  They can be pushed out until just before the thread returns, so they don't take away audio processing cycles. 

http://www.rossbencina.com/code/real-time-audio-programming-101-time-waits-for-nothing

http://www.rossbencina.com/code/supercollider-internals-book-chapter

<div class="ui section divider"></div>
<section id="socialMediaLinks"></section>
<div class="ui section divider"></div>
<div id="disqus_thread"></div>

