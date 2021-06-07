# RFC XX: Make use of aiortc to enable low-level WebRTC testing


## Summary

This is a proposal to make use of aiortc, a WebRTC stack implemented in python, to enable writing low-level WebRTC tests.


## Background

Current WebRTC testsuite is testing API extensively but it is difficult to validate that these APIs actually do what they are intended to do.
As an example, WebRTC tests can validate that WebRTC stats are gathered but it is difficult to validate that WebRTC stats values actually make sense.
To improve WebRTC test coverage, the plan would be to build an aiortc peer that could be used by the test page to validate what goes on the wire.


## Details

As illustrated in https://www.w3.org/2011/04/webrtc/wiki/images/0/0c/WebRTCWG-2021-02-22.pdf, slide 12, aiortc can be used to implement custom WebRTC endpoints.
As an example, a WebRTC endpoint could forward any network data it receives from DTLS to the test page through WebSocket.
It will then be up to the web page to parse and validate the data coming back through WebSocket.


## Architecture

Test writers would typically author a WebSocket server python scripts that would be used to start, set up and close a non-browser WebRTC peer.
These server python scripts would use the aiortc library to run the non-browser WebRTC peer.


## Dependencies

aiortc is using the following dependencies:
* aioice
* av
* cffi
* crc32c. Due to crc32c license, a plan might be to use google-crc32c as a replacement.
* cryptography. This is already a dependency.
* dataclasses. This is already a dependency.
* pyee
* pylibsrtp


### Outcomes and risks

The expected achievement is to increase WebRTC test coverage by writing WPT tests that validate low-level functionalities, in particular networking.
The risks are limited to maintaining that aiortc can continue being used when upgrading other parts of the infrastructure.
There might also be a need to upgrade aiortc as it is developed, which should be done on per-request basis.
