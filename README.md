# sound_eeg A small testing utility to compare sounds with triggers from the parallel port

## Rationale
In EEG experiments you can measure with a relatively high temporal precision.
In order to achieve this A experimental program needs to synchronize the
stimuli with the EEG recording equipment, if there is no synchronization,
you will lose your signal in the noise.

## Goal: measure latency and jitter between trigger and output of the sound wave
This is a small utility to generate brief triggers with an Standard Parallel
Port (SPP) and generate tones with with a sound card.

## Requirements
- Zep2
- parallel port
- audio card with lineout cable
- oscilloscope

## Design

A large number of stimuli are presented. The stimuli are 20 ms of 1Khz. In
synchronous fashion, a large number of pulses/triggers are send.
The triggers are setting data line 0-7 of the SPP high for a 8ms as well. 8 ms
is chosen, because in the biosemidocumentation the usb -> parallel cable
sends pulses of 8 ms as well. The data lines are set high by writing 0xFF (255)
to the data register. Since we assume the triggers are presented at the same
time as the sound stimuli we can have a look at the signals on the
oscilloscope. Then we should have a reasonable idea of the offset and the
latency.

### Optimal results
So in the image below, we can see the desired results, where the trigger (yellow
line) starts simultaneous with the onset of the sound (purple line). This
should happen all the time ideally. Every square at the horizonal axis represents
5 ms. So you can see that the trigger duration is precisely 8 ms and the sound
duration is 4 squares, hence 20ms.

![alt text][simultaneous]

### Delayed results
In the scenario below one can see that the stimuli are always presented a little
to late. However, there is not any jitter, that is, the stimuli are alway quite
precisely 3.4ms to late. That is suboptimal, but something one can work with.
Using Zep on Linux, one can tell the SoundOutputDevice that there is a
hardware_delay of 3.4 ms and then zep will schedule the stimuli 3.4 ms earlier
to correct for the delay. With these settig the image above is made.

![alt text][delayed]

### Jittered results
Below is the situation of jittered pulse and signals, this is the worst case
scenario. Then there is a unpredictable delay for which cannot be controlled.
Hence than you should hope that the jitter is < 1ms, than it is reasonably alright.
but if it is much more, you will lose quite some temporal precision. The jitter
was generated using a jitter of 3ms, but this can be much more with some toolkits.
In the picture below you also see a "shadow" of all the traces of last second,
this enables to see the jitter in a picture.

![alt text][jitter]


## Notes
1. Parallel Port. Zep is not able to trigger properly when the previous trigger
hasn't been properly handled. With EEG experiments, this typically isn't a
problem. Since we trigger for roughly 5-10 ms, the BioSemi USB trigger triggers
with pulses with a duration of 8ms, So I'm thinking that should be used for
optimal results.
2. SoundPlayback. Make sure the audio is not at 100% either. Zep (its backends)
do not like it or the Xonar D2 devices do not like it. The waveforms are
distored.
3. Xonar has a hardware latency of roughly 3.4 ms. (on Linux).

[simultaneous]: ./images/oscilloscope.png
[delayed]: ./images/delayed.png
[jitter]: ./images/jitter.png

