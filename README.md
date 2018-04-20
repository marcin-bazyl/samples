[![Build Status](https://travis-ci.org/webrtc/samples.svg?branch=gh-pages)](https://travis-ci.org/webrtc/samples/)

# Fork of WebRTC code samples #

## This fork has been created to show a desktop share video quality problem in Chrome ##

The modifications are done to the "Basic peer connection demo" sample (samples/src/content/peerconnection/pc1/):

* desktop share is done instead of camera capture
* use of h.264 is forced instead of VP8 by modifying the SDP

These modifications are meant to prove that there is a problem with desktop sharing video quality in Chrome on Windows when hardware acceleration is enabled and H264 encoder is used. The modifications to the code are specific to Chrome (there are hardcoded assumptions about the RTP payload types that match values used by Chrome), so it only makes sense to run this code in Chrome.

To do the desktop capture I've used Screen-Capturing.js from https://github.com/muaz-khan/WebRTC-Experiment/blob/master/Chrome-Extensions/Screen-Capturing.js/Screen-Capturing.js and the Chrome extension from https://github.com/muaz-khan/WebRTC-Experiment/tree/master/Chrome-Extensions/desktopCapture

Desktop sharing requires a Chrome extension and an https connection to work. Therefore, to use this sample, the following setup is needed:

1. Create an https server using Node.js running on your localhost.
 * I've used the script from here: https://gist.github.com/bencentra/909830fb705d5892b9324cffbca3926f (follow the instructions from the script to create the key and certificate: key.pem and cert.pem files)
 * Place the git clone of this fork of WebRTC code samples into the "dist" subfolder
 * install dependencies:
 ````
 npm install express
 ````
 * and run the script:
 ````
 node server.js
 ````
2. Install the Chrome extension
 * I've used the one from https://github.com/muaz-khan/WebRTC-Experiment/tree/master/Chrome-Extensions/desktopCapture
 * Remember to change line 17 of manifest file to say:
 ````
 "matches": ["https://localhost/*"]
 ````
 * Use Chrome's "load unpacked" option to load the extension

Once the above setup is done, the issue can be reproduced by folowing the steps:

1. Make sure you're using Windows 10 on a modern laptop that has graphics acceleration and the "Use hardware acceleration when available" setting is enabled in Chrome.
2. Open Chrome version 65.0.3325.181 and point it to https://localhost:8443/samples/src/content/peerconnection/pc1/
3. Scroll down and click the "Start" button - a dialog box will apear asking which screen you want to share (it's best to share a second screen, not the one you're viewing the page on), the desktop share should appear in the top video element on the page
4. Click the "Call" button - the same shared desktop content should appear in the bottom video element of the page
5. Move some windows around the shared screen, minimise them, maximise them. Observe that as the shared screen content changes with time the quality in the bottom video element keeps getting worse and worse.

