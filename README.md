# HotBox
![HOTBOX LOGO](http://i.imgur.com/495tedr.png)

React Native wrapper around [TokBox OpenTOK SDK](https://tokbox.com/).

### iOS

1. `yarn add https://github.com/mobileDevNativeCross/rnopentok.git` or inferiorly `npm install --save https://github.com/mobileDevNativeCross/rnopentok.git`
2. You're going to want a bridging header:

```
#import <React/RCTBridgeModule.h>
#import <React/RCTEventEmitter.h>
#import <React/RCTViewManager.h>
```
3. You will also want a Podfile:

```
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'Example' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks

  use_frameworks!

  pod 'RxSwift'
  pod 'OpenTok'

end
```

## Usage

Something like: 

```
import {Session, PublisherView, SubscriberView} from 'rnopentok'
	
var session = new Session()

session.on('sessionDidConnect', () => console.log('connected'))
session.on("sessionDidDisconnect", () => console.log('disconnected'))
session.on('publisherStreamCreated', () => console.log("PUBLISHER CREATED"))
session.on('sessionStreamCreated', () => console.log('sessionStreamCreated'))
session.on('subscriberDidConnect', (streamId) => console.log("New subscriber", streamId))
session.on("subscriberDidDisconnect", (streamId) => console.log("Subscriber disconnected", streamId))
session.on('sessionStreamDestroyed', (streamId) => console.log("Stream destroyed", streamId))

let apiKey = ''
let sessionId = ''
let token = ''

session.createSession(apiKey, sessionId, token)
```

### Events

```
* sessionDidConnect 				(sessionId)
* sessionDidDisconnect 				(sessionId)
* sessionConnectionCreated 			(connectionId)
* sessionConnectionDestroyed			(connectionId)
* sessionStreamCreated 				(streamId)
* sessionStreamDidFailWithError 		(err)
* sessionStreamDestroyed 			(streamId)
* sessionReceivedSignal 			({type, connectionId, message})
* publisherStreamCreated 			(streamId)
* publisherStreamDidFailWithError 		(err)
* publisherStreamDestroyed 			(streamId)
* subscriberDidConnect 				(streamId)
* subscriberDidFailWithError 			(streamId)
* subscriberDidDisconnect 			(streamId)
```

### Methods

```
session.createSession(apiKey, sessionId, token)
session.createPublisher() // You can manually create the publisher but by default it's created automatically
session.disconnectAllSessions()
	
// Turn on/off video stream for given streamId (null for publisher)
session.requestVideoStream(streamId, on)
	
// Turn on/off audio stream for given streamId (null for publisher)
session.requestAudioStream(streamId, on)
	
// Flip publisher camera
session.requestCameraSwap(toBack) // toBack==true ==> back camera
	
//Send messages
session.sendMessage(type, message, connectionId)
session.broadcastMessage(type, message)
	
//Subscribe to Stream
session.subscribe(streamId)
	
//Subscribe to signals
session.on(event, handler)
	
```

### Views

#### Publisher

`<PublisherView style={styles.viewStyle} />`

### Subscriber

`<SubscriberView style={styles.viewStyle} streamId={streamId} />`
