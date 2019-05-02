## Producer
{: #Producer}

<section markdown="1">

A producer represents an audio or video source that will be transmitted to the mediasoup router through a WebRTC transport.

</section>


### Dictionaries
{: #Producer-dictionaries}

<section markdown="1">

#### ProducerCodecOptions
{: #ProducerCodecOptions .code}

<div markdown="1" class="table-wrapper L3">

Field           | Type    | Description   | Required | Default
--------------- | ------- | ------------- | -------- | ---------
`opusStereo`    | bool | Enable OPUS stereo (if the audio source is stereo). | No | Browser specific.
`opusFec`       | bool | Enable OPUS in band FEC. | No | Browser specific.
`opusDtx`       | bool | Enable OPUS discontinuous transmission. | No | Browser specific.
`opusMaxPlaybackRate` | Number | Set OPUS maximum playbak rate. | No | Browser specific.
`videoGoogleStartBitrate` | Number | Just for libwebrtc based browsers. Set video initial bitrate. | No |
`videoGoogleMaxBitrate` | Number | Just for libwebrtc based browsers. Set video maximum bitrate. | No |
`videoGoogleMinBitrate` | Number | Just for libwebrtc based browsers. Set video minimum bitrate. | No |

</div>

</section>


### Methods
{: #Producer-methods}

<section markdown="1">

#### producer.GetId()
{: #producer-id .code}

Producer identifier.

> `@type` const std::string&

#### producer.IsClosed()
{: #producer-closed .code}

Whether the producer is closed.

> `@type` bool

#### producer.GetKind()
{: #producer-kind .code}

The media kind ("audio" or "video").

> `@type` std::string

#### producer.GetTrack()
{: #producer-track .code}

The audio or video track being transmitted.

> `@type` webrtc::MediaStreamTrackInterface\*

#### producer.GetRtpParameters()
{: #producer-rtpParameters .code}

Producer RTP parameters. These parameters are internally built by the library and conform to the syntax and requirements of mediasoup, thus they can be transmitted to the server to invoke [transport.produce()](/documentation/v3/mediasoup/api/#transport-produce) with them.

> `@type` const nlohmann::json& [RtpSendParameters](/documentation/v3/mediasoup/rtp-parameters-and-capabilities/#RtpSendParameters)

<div markdown="1" class="note">
Check the [Communication Between Client and Server](/documentation/v3/communication-between-client-and-server/) section for more details.
</div>

#### producer.IsPaused()
{: #producer-paused .code}

Whether the producer is paused.

> `@type` bool

#### producer.GetMaxSpatialLayer()
{: #producer-maxSpatialLayer .code}

In case of simulcast, this value determines the highest stream (from 0 to N-1) being transmitted. See the [setMaxSpatialLayer()](#producer-setMaxSpatialLayer) method for more about this.

> `@type` const uint8_t

#### producer.GetAppData()
{: #producer-appData .code}

Custom data Object provided by the application in the producer factory method. The app can modify its content at any time.

> `@type` const nlohmann::json&

#### producer.Close()
{: #producer-close .code}

Closes the producer. No more media is transmitted.

<div markdown="1" class="note">
This method should be called when the server side producer has been closed (and vice-versa).
</div>

#### producer.GetStats()
{: #producer-getStats .code}

Gets the local RTP sender statistics by calling `getStats()` in the underlying `RTCRtpSender` instance.

> `@returns` nlohmann::json [RTCStatsReport](https://w3c.github.io/webrtc-pc/#dom-rtcstatsreport)

#### producer.Pause()
{: #producer-pause .code}

Pauses the producer (no RTP is sent to the server).

<div markdown="1" class="note">
This method should be called when the server side producer has been paused (and vice-versa).
</div>

#### producer.Resume()
{: #producer-resume .code}

Resumes the producer (RTP is sent again to the server).

<div markdown="1" class="note">
This method should be called when the server side producer has been resumed (and vice-versa).
</div>

#### producer.ReplaceTrack(track)
{: #producer-replaceTrack .code}

Replaces the audio or video track being transmitted. No negotiation with the server is needed.

<div markdown="1" class="table-wrapper L3">

Argument        | Type    | Description | Required | Default 
--------------- | ------- | ----------- | -------- | ----------
`track`         | webrtc::MediaStreamTrackInterface\* | An audio or video track. | Yes |

</div>

```c++
producer.ReplaceTrack(newVideoTrack);
```

#### producer.SetMaxSpatialLayer(spatialLayer)
{: #producer-setMaxSpatialLayer .code}

In case of simulcast, this method limits the highest RTP stream being transmitted to the server.

<div markdown="1" class="table-wrapper L3">

Argument        | Type    | Description | Required | Default 
--------------- | ------- | ----------- | -------- | ----------
`spatialLayer`  | uint8_t  | The index of the entry in `encodings` representing the highest RTP stream that will be transmitted. | Yes |

</div>

```c++
// Assuming `encodings` array has 3 entries, let's enable just the first and
// second streams (indexes 0 and 1).
producer.setMaxSpatialLayer(1);
```

</section>


## Producer::Listener
{: #ProducerListener}

<section markdown="1">

#### listener.OnTransportClose(producer)
{: #producer-on-transportclose .code}

Executed when the transport this producer belongs to is closed for whatever reason. The producer itself is also closed.

<div markdown="1" class="table-wrapper L3">

Argument        | Type    | Description | Required | Default 
--------------- | ------- | ----------- | -------- | ----------
`consumer`  | [Producer\*](#Producer)  | The consumer instance executing this method. | Yes |

</div>

```c++
void Sender::OnTransportClose(mediasoupclient::Producer* /*producer*/)
{
	std::cout << "OnTransportClose" << std::endl;
}
```

</section>