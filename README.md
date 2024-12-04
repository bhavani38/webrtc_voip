# webrtc_voip

## Concept
Peer to Peer video communication between two devices/users is achieved using Webrtc by following the below steps:
1. Access to microhone/camera: Getting access to the media streams (camera/microphone) for both peers.
   1. Access needs to be provided to stream local video/audio using getUserMedia().
2. Signaling Server: Socket.IO is used to handle real-time communication between the two devices (peers) via the signaling server. This allows the exchange of messages like offer, answer, and ICE candidates.
   1.  Peer A (Creating the Offer)
   2.  Peer B (Handling the Offer and Sending an Answer)   3.   
3. PeerConnection: Created and managed the RTCPeerConnection to handle media streams and establish a direct peer-to-peer connection using RTCPeerConnection().
   1. Once the offer is created from Peer A, the offer is set as local description with peerConnection.setLocalDescription()
   2. Once Peer B receives answer,  it sets the offer received from Peer A as its remote description and creates answer.
4. ICE (Interactive Connectivity Establishment)Exchange: 
   1. Exchanging ICE candidates to ensure the peers can connect, even if they are behind NAT/firewalls.
   2. NAT traversal is handled with public STUN server stun:stun.l.google.com, stun:stun.services.mozilla.com since this is for demo prupose. Own STUN/TURN servers needs to be setup if large volume of traffic is expected while moving to production.
5. Receive remote stream
   1. Once the WebRTC connection is established between the peers, they can begin receiving each other's media streams.
   2. When the remote peer’s stream is received, it is attached to the remoteVideo element, which allows the user to see the other peer’s video.

## Folder structure:
webrtc-voip/
├── server.js
├── public/
│   ├── index.html
│   |── webrtc.js
│   └── index.css
└── package.json

### Server side code: 
`server.js` - a simple WebSocket server using Socket.io for signaling. 

### Client side code: 
`public/index.html` - This HTML page will capture video and audio from the user's webcam, set up a WebRTC connection, and communicate with the server using WebSocket.
`public/webrtc.js` - This js will set up the WebRTC logic and manage the signaling through the WebSocket connection.

## How to run 
Run the below commands in bash terminal:
- `npm install`
- `node server.js` - Server will be available at port 3000 - http://localhost:3000
- Server can intiate the video call from the above url where a unique code is generated. This code can be shared with any client who wants to join the call and Press `Start` to start the call.
- Meanwhile the clients who have the code (Shared as above by the Server) can join the call by pressing the `Join` button. 

## URL for client (Deploy the Server Publicly):  
1. Since this app is developed for demo purpose, the ngrok tunneling service is used to generate a temporary https url which can be share to client for testing prupose.
2. On deploying the server publicly, the url needs to be adjusted in webrtc.js while establishing the socket connection in the below line. 
`const socket = io("https://f9dc-83-233-18-141.ngrok-free.app");`
