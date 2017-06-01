<span id="Introduction" class="on_page_navigation"></span>
# Introduction
Video chat or video calling is essentially streaming both audio and video inputs asynchronously between two or more end users. Video calling is a great way to have productive and visual communication between your users hence high popularity of this feature in QuickBlox developers community.

Typically both audio and video calling is used along with 1:1 / IM textual chat communication but there are use cases (such is in gaming or when walking / driving for example) where they are used on their own.

<span id="How_it_works" class="on_page_navigation"></span>
# How it works
QuickBlox SDK client library works with input sources (camera, microphone), codecs, compression and then the data is streamed peer-to-peer between end users. This way video calling doesn't impact the server much so the system is highly scalable. Server however enables the handshake between end users before streaming starts to take place and also it resolves NAT traversal in case configuration of networks and firewalls between end users makes call impossible otherwise. This is done with the help of QuickBlox STUN/TURN server.

<span id="Technologies_used" class="on_page_navigation"></span>
# Technologies used
Our latest video chat solution uses the open-source technology [WebRTC](https://webrtc.org/). It is intended for the organisation of streaming media data between browsers or other supporting it applications for peer-to-peer technology without any additional plugins.

Get an overview of WebRTC from the [Google I/O presentation](https://www.youtube.com/watch?v=p2HzZkd2A40) or read [WebRTC FAQ](https://webrtc.org/faq).

To achieve real-time media communication, several transfer servers for data exchanges and a specific signaling mechanism are needed. 

Their roles are:

* Get network information such as IP addresses and ports, and exchange this with other WebRTC clients (known as **peers**) to enable connection, even through NATs and firewalls
* Coordinate signaling communication to report errors and initiate or close sessions
* Exchange information about media and client capability, such as resolution and codecs
* Communicate streaming audio, video or data

The signaling in QuickBox WebRTC module is implemented over **XMPP protocol** using QuickBlox Real-time API. The Video chat module is a high-level solution around WebRTC technology. Read more about signaling in the next paragraph.

<span id="Connecting_to_server" class="on_page_navigation"></span>
# Connecting to server
Signaling is the process of coordinating communication. In order for a WebRTC application to set up a 'call', its clients need to exchange information. Video Chat API uses the same [Real-time API](/realtime_api/Introduction.html#Authenticating_requests) channel for signaling.

<span id="ICE_servers" class="on_page_navigation"></span>
# ICE servers
WebRTC apps can use the [ICE](https://en.wikipedia.org/wiki/Interactive_Connectivity_Establishment) framework to overcome the complexities of real-world networking. To enable this to happen, your application must pass ICE server URLs to RTCPeerConnection, as described below.

ICE tries to find the best path to connect peers. It tries all possibilities in parallel and chooses the most efficient option that works. ICE first tries to make a connection using the host address obtained from a device's operating system and network card; if that fails (which it will for devices behind NATs) ICE obtains an external address using a STUN server, and if that fails, traffic is routed via a TURN relay server.

We use 3 TURN servers: in North Virginia, Asia and Europe.

TURN server endpoint: **turn.quickblox.com**.

**How does WebRTC select which TURN server to use if multiple options are given?** During the connectivity checking phase, WebRTC will choose the TURN relay with the lowest round-trip time. Thus, setting multiple TURN servers allows your application to scale-up in terms of bandwidth and number of users.

Here is a list with default settings that we use, you can customise all of them or only some particular:

```javascript
var iceServers = [
  {
    'url': 'stun:stun.l.google.com:19302'
  },
  {
    'url': 'stun:turn.quickblox.com',
    'username': 'quickblox',
    'credential': 'baccb97ba2d92d71e26eb9886da5f1e0'
  },
  {
    'url': 'turn:turn.quickblox.com:3478?transport=udp',
    'username': 'quickblox',
    'credential': 'baccb97ba2d92d71e26eb9886da5f1e0'
  },
  {
    'url': 'turn:turn.quickblox.com:3478?transport=tcp',
    'username': 'quickblox',
    'credential': 'baccb97ba2d92d71e26eb9886da5f1e0'
  }
]
```

<span id="Signaling" class="on_page_navigation"></span>
# Signaling v1.0
The following signaling protocol is used in QuickBlox WebRTC Video chat iOS/Android/Web code samples.

Developers also can use this protocol to build WebRTC library and video chat applications for other platforms (e.g. desktop apps).

<div class="attention">
All video chat signalling messages have <b>type=headline</b> and an extra parameter <b>moduleIdentifier</b>.
Check these 2 values to detect the video chat signalling message.
</div>

## Call
A signal to initiate a call.

### Format
```xml
<message to="..."  type="headline" id="...">
    <extraParams xmlns="jabber:client">
        <moduleIdentifier>WebRTCVideoChat</moduleIdentifier>
        <signalType>call</signalType>
        <sessionID>...</sessionID>
        <callType>...</callType>
        <sdp>...</sdp>
        <platform>...</platform>
        <callerID>...</callerID>
        <opponentsIDs>
           <opponentID>...</opponentID>
           <opponentID>...</opponentID>
           ...
        </opponentsIDs>
        <userInfo>... </userInfo>
    </extraParams>
</message>
```

### Parameters
TBA

## Accept
A signal to accept an incoming call.

### Format
```xml
<message to="..."  type="headline" id="...">
    <extraParams xmlns="jabber:client">
        <moduleIdentifier>WebRTCVideoChat</moduleIdentifier>
        <signalType>accept</signalType>
        <sessionID>...</sessionID>
        <sdp>...</sdp>
        <platform>...</platform>
        <userInfo>...</userInfo>
    </extraParams>
</message>
```

### Parameters
TBA

## Reject

A signal to reject an incoming call.

### Format
```xml
<message to="..."  type="headline" id="...">
    <extraParams xmlns="jabber:client">
        <moduleIdentifier>WebRTCVideoChat</moduleIdentifier>
        <signalType>reject</signalType>
        <sessionID>...</sessionID>
        <userInfo>...<userInfo>
    </extraParams>
</message>
```

### Parameters
TBA

## Hang Up
A signal to finish the call.

### Format
```xml
<message to="..."  type="headline" id="...">
    <extraParams xmlns="jabber:client">
        <moduleIdentifier>WebRTCVideoChat</moduleIdentifier>
        <signalType>hangUp</signalType>
        <sessionID>...</sessionID>
        <userInfo>...</userInfo>
    </extraParams>
</message>
```

### Parameters
TBA

## ICE candidates
A signal to send WebRTC ICE candidates.

### Format
```xml
<message to="..."  type="headline" id="...">
    <extraParams xmlns="jabber:client">
        <moduleIdentifier>WebRTCVideoChat</moduleIdentifier>
        <signalType>iceCandidates</signalType>
        <sessionID>...</sessionID>
        <iceCandidates>
            <iceCandidate>
                <sdpMLineIndex>...</sdpMLineIndex>
            	<sdpMid>...</sdpMid>
            	<candidate>...</candidate>
            </iceCandidate>
            <iceCandidate>
            	<sdpMLineIndex>...</sdpMLineIndex>
            	<sdpMid>...</sdpMid>
            	<candidate>...</candidate>
             </iceCandidate>
             ...
         </iceCandidates>
    </extraParams>
</message>
```

### Parameters
TBA

## Update parameters
A signal to notify an opponent that some call's parameters is updated.

### Format
```xml
<message to="..."  type="headline" id="...">
    <extraParams xmlns="jabber:client">
        <moduleIdentifier>WebRTCVideoChat</moduleIdentifier>
        <signalType>update</signalType>
        <sessionID>...</sessionID>
        <param1>...</param1>
        <param2>...</param2>
        ...
    </extraParams>
</message>
```

### Parameters

| Parameter | Description |
|:----:|:-------:|
| **moduleIdentifier** | An identifier of the module, hold the **WebRTCVideoChat** value |
| **signalType** | A type of signal, hold the **update** value |
| **sessionID** | An unique id of current video chat session. Users have to use the same **sessionID** within particular call. Timestamp can be used as a **sessionID** value |