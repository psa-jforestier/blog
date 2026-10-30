# Audio / Video Synchronization Test

## Purpose

This video is designed to help estimate the audio/video synchronization delay between a TV and an audio system such as an AV receiver, soundbar, bluetooth audio device, or home cinema system. 

[files/av_sync_single_scale_30fps_60s.mkv](files/av_sync_single_scale_30fps_60s.mkv) | [files/av_sync_single_scale_60fps_60s.mkv](files/av_sync_single_scale_60fps_60s.mkv)

The test provides:

- A synchronization beep every 1 second
- A white video frame at the exact reference point
- A large millisecond counter
- A moving visual indicator
- Separate AAC and PCM audio tracks
- Versions at 30 fps and 60 fps

The goal is to determine whether the sound is heard before or after the corresponding picture.

## What is displayed

During normal playback, the screen is black and displays a large timing value:
```
-100
-067
-033
 000
+033
+067
+100
```

At the synchronization point:
```
 000
```

the video frame becomes white and the beep is generated.

At 30 fps:

- One frame = 33.33 ms
- -033 = 33 ms before the reference frame
- 000 = reference frame + beep
- +033 = 33 ms after the reference frame
- +067 = approximately 67 ms after the reference frame

At 60 fps:

- One frame = 16.67 ms
- -017 = approximately 17 ms before
- 000 = reference frame + beep
- +017 = approximately 17 ms after
- +033 = approximately 33 ms after


## Visual scale

A smaller scale is displayed above the main counter.

For example:
```
654321:123456
      _
```

The : represents the zero/reference point.

The red _ moves from left to right as the video approaches and passes through the synchronization point.

At zero it should be positioned directly under :
```
654321:123456
      _
```

Each position represents exactly one video frame.

Therefore:

- 30 fps
- 1 step = 33.33 ms

- 60 fps
- 1 step = 16.67 ms

## How to perform the test
1. Play the video normally

Play the file directly on the TV using the playback method you want to test.

For example:

USB drive → TV → HDMI/eARC → AV receiver


or:

Media player → HDMI → TV → eARC → AV receiver


It is important to use the same playback and audio path that you normally use, because the objective is to measure that complete chain.

2. Listen to the repeating beep

The synchronization event occurs once per second.

Listen to the sequence for several seconds:

BEEP

BEEP

BEEP

BEEP


Do not initially try to read the display.

**Concentrate on the rhythm.**

3. Close your eyes

Keep your eyes closed while listening to the repeating beep.

After several repetitions, the rhythm becomes predictable.

4. Blink in synchronization with the sound

Open your eyes very briefly when you hear or anticipate each beep, then immediately close them.

In effect, your eyes become a crude visual shutter.

Instead of seeing the counter continuously, you should briefly glimpse values such as:

+067


or:

-033


Repeat this over several beeps.

Interpreting the result

Do not rely on a single observation.

Look for a value or small range that appears repeatedly.

For example, you might observe:

+067
+100
+067
+067
+100
+067


A reasonable visual estimate would then be approximately:

+70 to +100 ms


Your blink timing and perception introduce uncertainty, so this test is better suited to identifying a consistent range than claiming an exact measurement to the millisecond.

## 30 fps vs 60 fps

The 60 fps version provides twice the temporal resolution.

30 fps
1 frame = 33.33 ms


Typical values are:

-100
-067
-033
 000
+033
+067
+100

60 fps
1 frame = 16.67 ms


Typical values are:

-100
-083
-067
-050
-033
-017
 000
+017
+033
+050
+067
+083
+100


I recommend testing both versions.

If you repeatedly obtain approximately the same offset with both, that gives you a useful sanity check on the result.

## AAC vs PCM

The MKV video contains two audio tracks:

AAC 192k
PCM 16-bit 48kHz


Use your TV or media player's audio-track selector to switch between them.

Run the same test with:

AAC
PCM

Keep all TV and home-cinema settings unchanged between the two tests.

This can help determine whether the playback chain behaves differently depending on the audio format.

## Recommended test procedure

For a reasonably repeatable test:

1. Select 30 fps + PCM
2. Observe approximately 10-20 beeps
3. Note the values you repeatedly see

4. Select 30 fps + AAC
5. Repeat

6. Select 60 fps + PCM
7. Repeat

8. Select 60 fps + AAC
9. Repeat


Record the results.

The most useful result is the repeatable cluster of observations, not an isolated frame you happen to see.

## Important limitation

This is a human-perception test, not laboratory measurement equipment.

Your eyes, auditory perception, anticipation of the beep, display refresh behaviour, video processing, and audio processing can all influence what you perceive.

For precise measurement, the better method is to record the TV flash and speaker output with external measurement equipment or a suitable high-frame-rate recording setup.

For quickly identifying something like a 50 ms, 100 ms, or 150 ms A/V synchronization problem, however, this video provides a simple practical test.

## How to generate such video 
Use the `ffmpeg` program with this one-liner command. Note it is for Windows Git Bash, change the path to the monospace font if needed.
```shell
VIDEO_FPS=60
VIDEO_DURATION=60
HALF_FPS=$((VIDEO_FPS / 2))

ffmpeg \
-f lavfi -i "color=c=black:s=1080x720:r=${VIDEO_FPS}:d=${VIDEO_DURATION}" \
-f lavfi -i "sine=frequency=1000:sample_rate=48000:duration=${VIDEO_DURATION}" \
-filter_complex "
[0:v]
geq=r='if(eq(mod(N,${VIDEO_FPS}),0),255,0)':
     g='if(eq(mod(N,${VIDEO_FPS}),0),255,0)':
     b='if(eq(mod(N,${VIDEO_FPS}),0),255,0)',

drawtext=fontfile='C\:/Windows/Fonts/consolab.ttf':
text='654321\:123456':
fontsize=40:fontcolor=gray:
x=396:y=160,

drawtext=fontfile='C\:/Windows/Fonts/consolab.ttf':
text='_':
fontsize=40:fontcolor=red:
x='528+22*(mod(n+${HALF_FPS}\,${VIDEO_FPS})-${HALF_FPS})':
y=190:
enable='between(mod(n+${HALF_FPS}\,${VIDEO_FPS})-${HALF_FPS},-6,6)',

drawtext=fontfile='C\:/Windows/Fonts/consolab.ttf':
text='-%{eif\:abs((mod(n+${HALF_FPS}\,${VIDEO_FPS})-${HALF_FPS})*1000/${VIDEO_FPS})\:d\:3}':
fontsize=180:fontcolor=white:
x=300:y=270:
box=1:boxcolor=black:boxborderw=30:
enable='lt(mod(n+${HALF_FPS}\,${VIDEO_FPS})-${HALF_FPS},0)',

drawtext=fontfile='C\:/Windows/Fonts/consolab.ttf':
text=' 000':
fontsize=180:fontcolor=black:
x=300:y=270:
box=0:
enable='eq(mod(n+${HALF_FPS}\,${VIDEO_FPS})-${HALF_FPS},0)',

drawtext=fontfile='C\:/Windows/Fonts/consolab.ttf':
text='+%{eif\:(mod(n+${HALF_FPS}\,${VIDEO_FPS})-${HALF_FPS})*1000/${VIDEO_FPS}\:d\:3}':
fontsize=180:fontcolor=white:
x=300:y=270:
box=1:boxcolor=black:boxborderw=30:
enable='gt(mod(n+${HALF_FPS}\,${VIDEO_FPS})-${HALF_FPS},0)',

drawtext=fontfile='C\:/Windows/Fonts/consolab.ttf':
text='Each step = %{expr\:1000/${VIDEO_FPS}} ms':
fontsize=30:fontcolor=gray:
x=350:y=550
[v];

[1:a]
volume='if(lt(mod(t,1),1/${VIDEO_FPS}),1,0)':eval=frame,
asplit=2[aac][pcm]
" \
-map "[v]" \
-map "[aac]" \
-map "[pcm]" \
-c:v libx264 -threads 8 -preset medium \
-pix_fmt yuv420p -r ${VIDEO_FPS} \
-c:a:0 aac -b:a:0 192k -ar:a:0 48000 \
-c:a:1 pcm_s16le -ar:a:1 48000 \
-metadata:s:a:0 title="AAC 192k" \
-metadata:s:a:1 title="PCM 16-bit 48kHz" \
-disposition:a:0 default \
-disposition:a:1 0 \
-t ${VIDEO_DURATION} \
"av_sync_${VIDEO_FPS}fps_${VIDEO_DURATION}s.mkv"
```
