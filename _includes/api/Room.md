## Room
{: #Room}

A `room` holds a multiparty RTC (Real-Time Communication) conference.


### Dictionaries
{: #Room-dictionaries}

<section markdown='1'>

#### RoomOptions
{: #Room-RoomOptions .code}


<div markdown='1' class='table-wrapper'>

Option                    | Type    | Description   | Default
------------------------- | ------- | ------------- | -------------
`forceUniquePayloadTypes` | Boolean | TBD. | `false`

</div>

</section>


### Properties
{: #Room-properties}

<section markdown='1'>

#### room.closed
{: #room-closed .code}

A boolean indicating whether the `room` has been closed.

#### room.peers
{: #room-peers .code}

An Array with the list of [Peer](#Peer) instances in the `room`.

</section>


### Methods
{: #Room-methods}

<section markdown='1'>

#### room.close()
{: #room-close .code}

Closes the `room`, including all its `peers`, and triggers a [close](#room-on-close) event.

#### room.dump()
{: #room-dump .code}

Returns a Promise that resolves to an Object containing the current status and details of the `room`.

*TBD:* Document it.

#### room.Peer(name)
{: #room-Peer .code}

Creates a [Peer](#Peer) instance within this `room`.

<div markdown='1' class='table-wrapper'>

Parameter  | Type    | Required  | Description  
-----------| ------- | --------- | -------------
`name`     | String  | Yes       | Peer name. Must be unique within the `room`.

</div>

Usage example:

```javascript
var peer = room.Peer('alice');
```

#### room.getPeer(name)
{: #room-getPeer .code}

Returns a [Peer](#Peer) with the given `name`, or `undefined` if such a peer does not exist in the `room`.

<div markdown='1' class='table-wrapper'>

Parameter  | Type    | Required  | Description  
-----------| ------- | --------- | -------------
`name`     | String  | Yes       | Peer name.

</div>


</section>


### Events
{: #Room-events}

The `Room` class inherits from [EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter).

<section markdown='1'>

#### room.on('close', fn(error))
{: #room-on-close .code}

Emitted when the `room` is closed. In case of error, the callback is called with the corresponding `Error` object.

```javascript
room.on('close', (error) => {
  if (error)
    console.error('room closed with error: %o', error);
});
```

</section>