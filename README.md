# trunk

Video Conferencing Tool for GTK

Pure C. UNIX way. Ideal for in-home LAN. Traffic can be piped through Tor.

## How to run it?

1. Get linux machine
2. Look into obfuscated PagedOut article, manually extract files (make.sh, trunk.c, trunkd.c, avi_grabber.c, trunk_structs.h)
3. `sudo apt update && sudo apt install -y ffmpeg libsox-fmt-all sox libkeybinder-dev buffer libgtk2.0-dev pkg-config libzmq3-dev indent`
4. If your linux misses `ffmpeg`, try `libav-tools` (avconv) instead
5. `bash make.sh`
6. `./trunkd tcp://0.0.0.0:3345 001_1.pcm 001_2.pcm`
7. In another terminal window try this: `ffmpeg -hide_banner -loglevel 24 -f video4linux2 -framerate 3 -video_size 160x120 -i /dev/video0 -f alsa -i default -f avi -c:v rawvideo -pix_fmt rgb24 -c:a pcm_s16le -ac 1 -ar 24000 - | buffer | ./trunk 73 1 0 1 trunk_recv.pcm tcp://127.0.0.1:3345  | play -q -t raw -e signed-integer -b 16 -c 1 -r 24000 -`
8. Each component on the pipeline is useful as a separate tool, make yourself familiar with them. Try capturing video from your webcam before piping in into trunk
9. Run another `trunk` on another linux machine on the same network, let it connect to your `trunkd` on first machine, now you can communicate via TRUNK
9. Press `TALK` button to activate microphone in half-duplex mode.
9. You can realize full-duplex for two parties using `chan-B`
9. Use `8-bits` button to decrease network usage
9. If you are streaming video from file, enable `sync-start` from command line
9. TRUNK is not encrypted by default. You can enable built-in Zero-MQ public key encryption. Or expose `trunkd` as an .onion service.
9. Use `alsamixer` to setup your microphone pre-amp volume
9. .pcm files are raw audio 24000 16bit mono. Use `mhwaveedit` to open them

## Why TRUNK is different?

* Pure C
* No WebRTC
* No sound compression, no sound artifacts are introduced, rewind button (TODO)
* Works on slow hardware
* You can pull some code from it (video_out(), grab_avi()) for your project

## PagedOut what?

This source-code was intended to be posted into PagedOut magazine run by Gynvael Coldwind (https://pagedout.institute). This is why codebase is so small. I hope TRUNK will appear in PagedOut #3.

