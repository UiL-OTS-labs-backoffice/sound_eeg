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

A large number of stimuli are presented. The stimuli are 10 ms of 1Khz. In
synchronous fashion, a large number of pulses/triggers are send.
The triggers are setting data line 1 of the SPP high for a 10ms as well.
Since we assume the triggers are presented at the same time as the sound
stimuli we can have a look at the signals on the oscilloscope. Then we should
have a reasonable idea of the offset and the latency.

So in the image below, we can see the desired results, where the trigger starts
simultaneous  with the onset of the sound. This should happen all the time ideally.

![alt text][simultaneous]

in the current



## Notes
1. Parallel Port. Zep is not able to trigger properly when the previous trigger
hasn't been properly handled. With EEG experiments, this typically isn't a
problem. Since we trigger for roughly 5-10 ms, the BioSemi USB trigger triggers
with pulses with a duration of 8ms, So I'm thinking that should be used for
optimal results.
2. SoundPlayback. Make sure the audio is not at 100% either. Zep (its backends)
do not like it or the Xonar D2 devices do not like it. The waveforms are
distored.
3. Xonar has a hardware latency of about 3.4 ms. (on Linux).

[simultaneous]: ./images/oscilloscope.png
[delayed]: ./images/delayed.png
