---
created: 2024-09-04T10:22:37 (UTC -03:00)
tags: [webdev,javascript,api,node,software,coding,development,engineering,inclusive,community]
source: https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest
author: 
---

# Introduction to WebRTC - DEV Community

> ## Excerpt
> Installation and Code Guide   WebRTC (Web Real-Time Communication) is an open-source...

---
[![Cover image for Introduction to WebRTC](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fynt5bho2y1xl32q2vkku.png)](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fynt5bho2y1xl32q2vkku.png)

### [](https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest#installation-and-code-guide)Installation and Code Guide

WebRTC (Web Real-Time Communication) is an open-source technology that enables real-time communication via simple APIs in web browsers and mobile apps. It allows audio, video, and data sharing directly between peers without needing an intermediary server, making it perfect for applications like video conferencing, live streaming, and file sharing.

In this blog, we'll dive into the following topics:

1.  What is WebRTC?
2.  Key Features of WebRTC
3.  Installing WebRTC
4.  Building a Basic WebRTC Application
5.  Understanding the Code
6.  Conclusion

___

### [](https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest#what-is-webrtc)What is WebRTC?

WebRTC is a set of standards and protocols that enables real-time audio, video, and data communication between web browsers. It includes several key components:

-   **getUserMedia**: Captures audio and video streams from the user's device.
-   **RTCPeerConnection**: Manages the peer-to-peer connection and handles audio and video streaming.
-   **RTCDataChannel**: Allows for real-time data transfer between peers.

### [](https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest#key-features-of-webrtc)Key Features of WebRTC

1.  **Real-Time Communication**: Low latency communication with minimal delay.
2.  **Browser Compatibility**: Supported by most modern web browsers (Chrome, Firefox, Safari, Edge).
3.  **No Plugins Required**: Works directly in the browser without additional plugins or software.
4.  **Secure**: Uses encryption for secure communication.

### [](https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest#installing-webrtc)Installing WebRTC

WebRTC is a client-side technology and does not require a specific server installation. However, you will need a web server to serve your HTML and JavaScript files. For local development, you can use a simple HTTP server.

#### [](https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest#prerequisites)Prerequisites

-   **Node.js**: To set up a local server.
-   **A Modern Web Browser**: Chrome, Firefox, Safari, or Edge.

#### [](https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest#setting-up-a-local-server)Setting Up a Local Server

1.  **Install Node.js**: Download and install Node.js from [nodejs.org](https://nodejs.org/).
    
2.  **Create a Project Directory**: Open a terminal and create a new directory for your project.  
    
    ```
    <span>mkdir </span>webrtc-project
    <span>cd </span>webrtc-project
    ```
    
3.  **Initialize a Node.js Project**:  
    
    ```
    npm init <span>-y</span>
    ```
    
4.  **Install HTTP Server**:  
    
    ```
    npm <span>install</span> <span>--save</span> http-server
    ```
    
5.  **Create Your Project Files**:
    
    -   `index.html`
    -   `main.js`

Create an `index.html` file with the following content:  

````
```html
&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;meta charset="UTF-8"&gt;
    &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
    &lt;title&gt;WebRTC Example&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;WebRTC Example&lt;/h1&gt;
    &lt;video id="localVideo" autoplay muted&gt;&lt;/video&gt;
    &lt;video id="remoteVideo" autoplay&gt;&lt;/video&gt;
    &lt;script src="main.js"&gt;&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
```
````

### [](https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest#building-a-basic-webrtc-application)Building a Basic WebRTC Application

We'll create a simple peer-to-peer video call application. This example will use two browser tabs to simulate the peer connection.

#### [](https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest#code-explanation)Code Explanation

1.  **Capture Local Video**: Use `getUserMedia` to capture video from the user's camera.
    
2.  **Create Peer Connection**: Establish a peer connection between the local and remote peers.
    
3.  **Exchange Offer and Answer**: Use SDP (Session Description Protocol) to exchange connection details.
    
4.  **Handle ICE Candidates**: Exchange ICE candidates to establish the connection.
    

Create a `main.js` file with the following content:  

```
<span>const</span> <span>localVideo</span> <span>=</span> <span>document</span><span>.</span><span>getElementById</span><span>(</span><span>'</span><span>localVideo</span><span>'</span><span>);</span>
<span>const</span> <span>remoteVideo</span> <span>=</span> <span>document</span><span>.</span><span>getElementById</span><span>(</span><span>'</span><span>remoteVideo</span><span>'</span><span>);</span>

<span>let</span> <span>localStream</span><span>;</span>
<span>let</span> <span>peerConnection</span><span>;</span>
<span>const</span> <span>serverConfig</span> <span>=</span> <span>{</span> <span>iceServers</span><span>:</span> <span>[{</span> <span>urls</span><span>:</span> <span>'</span><span>stun:stun.l.google.com:19302</span><span>'</span> <span>}]</span> <span>};</span>
<span>const</span> <span>constraints</span> <span>=</span> <span>{</span> <span>video</span><span>:</span> <span>true</span><span>,</span> <span>audio</span><span>:</span> <span>true</span> <span>};</span>

<span>// Get local video stream</span>
<span>navigator</span><span>.</span><span>mediaDevices</span><span>.</span><span>getUserMedia</span><span>(</span><span>constraints</span><span>)</span>
    <span>.</span><span>then</span><span>(</span><span>stream</span> <span>=&gt;</span> <span>{</span>
        <span>localStream</span> <span>=</span> <span>stream</span><span>;</span>
        <span>localVideo</span><span>.</span><span>srcObject</span> <span>=</span> <span>stream</span><span>;</span>
        <span>setupPeerConnection</span><span>();</span>
    <span>})</span>
    <span>.</span><span>catch</span><span>(</span><span>error</span> <span>=&gt;</span> <span>{</span>
        <span>console</span><span>.</span><span>error</span><span>(</span><span>'</span><span>Error accessing media devices.</span><span>'</span><span>,</span> <span>error</span><span>);</span>
    <span>});</span>

<span>function</span> <span>setupPeerConnection</span><span>()</span> <span>{</span>
    <span>peerConnection</span> <span>=</span> <span>new</span> <span>RTCPeerConnection</span><span>(</span><span>serverConfig</span><span>);</span>

    <span>// Add local stream to the peer connection</span>
    <span>localStream</span><span>.</span><span>getTracks</span><span>().</span><span>forEach</span><span>(</span><span>track</span> <span>=&gt;</span> <span>peerConnection</span><span>.</span><span>addTrack</span><span>(</span><span>track</span><span>,</span> <span>localStream</span><span>));</span>

    <span>// Handle remote stream</span>
    <span>peerConnection</span><span>.</span><span>ontrack</span> <span>=</span> <span>event</span> <span>=&gt;</span> <span>{</span>
        <span>remoteVideo</span><span>.</span><span>srcObject</span> <span>=</span> <span>event</span><span>.</span><span>streams</span><span>[</span><span>0</span><span>];</span>
    <span>};</span>

    <span>// Handle ICE candidates</span>
    <span>peerConnection</span><span>.</span><span>onicecandidate</span> <span>=</span> <span>event</span> <span>=&gt;</span> <span>{</span>
        <span>if </span><span>(</span><span>event</span><span>.</span><span>candidate</span><span>)</span> <span>{</span>
            <span>sendSignal</span><span>({</span> <span>'</span><span>ice</span><span>'</span><span>:</span> <span>event</span><span>.</span><span>candidate</span> <span>});</span>
        <span>}</span>
    <span>};</span>

    <span>// Create an offer and set local description</span>
    <span>peerConnection</span><span>.</span><span>createOffer</span><span>()</span>
        <span>.</span><span>then</span><span>(</span><span>offer</span> <span>=&gt;</span> <span>{</span>
            <span>return</span> <span>peerConnection</span><span>.</span><span>setLocalDescription</span><span>(</span><span>offer</span><span>);</span>
        <span>})</span>
        <span>.</span><span>then</span><span>(()</span> <span>=&gt;</span> <span>{</span>
            <span>sendSignal</span><span>({</span> <span>'</span><span>offer</span><span>'</span><span>:</span> <span>peerConnection</span><span>.</span><span>localDescription</span> <span>});</span>
        <span>})</span>
        <span>.</span><span>catch</span><span>(</span><span>error</span> <span>=&gt;</span> <span>{</span>
            <span>console</span><span>.</span><span>error</span><span>(</span><span>'</span><span>Error creating an offer.</span><span>'</span><span>,</span> <span>error</span><span>);</span>
        <span>});</span>
<span>}</span>

<span>// Handle signals (for demo purposes, this should be replaced with a signaling server)</span>
<span>function</span> <span>sendSignal</span><span>(</span><span>signal</span><span>)</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>'</span><span>Sending signal:</span><span>'</span><span>,</span> <span>signal</span><span>);</span>
    <span>// Here you would send the signal to the other peer (e.g., via WebSocket)</span>
<span>}</span>

<span>function</span> <span>receiveSignal</span><span>(</span><span>signal</span><span>)</span> <span>{</span>
    <span>if </span><span>(</span><span>signal</span><span>.</span><span>offer</span><span>)</span> <span>{</span>
        <span>peerConnection</span><span>.</span><span>setRemoteDescription</span><span>(</span><span>new</span> <span>RTCSessionDescription</span><span>(</span><span>signal</span><span>.</span><span>offer</span><span>))</span>
            <span>.</span><span>then</span><span>(()</span> <span>=&gt;</span> <span>peerConnection</span><span>.</span><span>createAnswer</span><span>())</span>
            <span>.</span><span>then</span><span>(</span><span>answer</span> <span>=&gt;</span> <span>peerConnection</span><span>.</span><span>setLocalDescription</span><span>(</span><span>answer</span><span>))</span>
            <span>.</span><span>then</span><span>(()</span> <span>=&gt;</span> <span>sendSignal</span><span>({</span> <span>'</span><span>answer</span><span>'</span><span>:</span> <span>peerConnection</span><span>.</span><span>localDescription</span> <span>}));</span>
    <span>}</span> <span>else</span> <span>if </span><span>(</span><span>signal</span><span>.</span><span>answer</span><span>)</span> <span>{</span>
        <span>peerConnection</span><span>.</span><span>setRemoteDescription</span><span>(</span><span>new</span> <span>RTCSessionDescription</span><span>(</span><span>signal</span><span>.</span><span>answer</span><span>));</span>
    <span>}</span> <span>else</span> <span>if </span><span>(</span><span>signal</span><span>.</span><span>ice</span><span>)</span> <span>{</span>
        <span>peerConnection</span><span>.</span><span>addIceCandidate</span><span>(</span><span>new</span> <span>RTCIceCandidate</span><span>(</span><span>signal</span><span>.</span><span>ice</span><span>));</span>
    <span>}</span>
<span>}</span>

<span>// Simulate receiving a signal from another peer</span>
<span>// This would typically be handled by a signaling server</span>
<span>setTimeout</span><span>(()</span> <span>=&gt;</span> <span>{</span>
    <span>receiveSignal</span><span>({</span>
        <span>offer</span><span>:</span> <span>{</span>
            <span>type</span><span>:</span> <span>'</span><span>offer</span><span>'</span><span>,</span>
            <span>sdp</span><span>:</span> <span>'</span><span>...</span><span>'</span> <span>// SDP offer from the other peer</span>
        <span>}</span>
    <span>});</span>
<span>},</span> <span>1000</span><span>);</span>
```

### [](https://dev.to/abhishekjaiswal_4896/introduction-to-webrtc-1ha8?context=digest#understanding-the-code)Understanding the Code

1.  **Media Capture**: `navigator.mediaDevices.getUserMedia` captures the local video stream.
2.  **Peer Connection Setup**: `RTCPeerConnection` manages the peer connection.
3.  **Offer and Answer**: SDP offers and answers are used to negotiate the connection.
4.  **ICE Candidates**: ICE candidates are used to establish connectivity between peers.
