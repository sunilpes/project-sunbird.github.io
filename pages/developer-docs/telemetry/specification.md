---
type: landing
directory: developer-docs/telemetry
title: Telemetry Specifications
page_title: Telemetry Specifications
description: telemetry specification of Sunbird
published: true
allowSearch: true
--- 

## Telemetry V3 Event Structure

All events follow a common data structure, though the event data structure (“edata”) differs for each event. The  complete data structure is as follows: 

<pre>
{
 // About the event
 "eid": , // Required. TODO: Shall we rename it to "verb" ?? 
 "ets": , // Required. Epoch timestamp of event (time in milli-seconds. For ex: 1442816723)
 "ver": , // Required. Version of the event data structure, currently "3.0"
 "mid": , // Required. Unique message ID. Used for deduplication, replay and update indexes
 
 // Who did the event
 "actor": { // Required. Actor of the event.
   "id": , // Required. Id of the actor. For ex: uid incase of an user
   "type":  // Required. User, System etc.
 },
 
 // Context of the event
 "context": { // Required. Context in which the event has occured.
   "channel": , // Required. Channel which has produced the event
   "pdata": { // Optional. Producer of the event
     "id": , // Required. unique id assigned to that component
     "pid": , // Optional. In case the component is distributed, then which instance of that component
     "ver":  // Optional. version number of the build
   },
   "env": , // Required. Unique environment where the event has occured.
   "sid": , // Optional. session id of the requestor stamped by portal
   "did": , // Optional. uuid of the device, created during app installation
   "cdata": [{ // Optional. correlation data
     "type":"", // Required. Used to indicate action that is being correlated
     "id": "" // Required. The correlation ID value
   }],
   "rollup": { // Optional. Context rollups
     "l1": "",
     "l2": "",
     "l3": "",
     "l4": ""
   }
 },
 // What is the target of the event
 "object": { // Optional. Object which is the subject of the event.
   "id": , // Required. Id of the object. For ex: content id incase of content
   "type": , // Required. Type of the object. For ex: "Content", "Community", "User" etc.
   "ver": , // Optional. version of the object
   "rollup": { // Optional. Rollups to be computed of the object. Only 4 levels are allowed.
   	"l1": "",
     "l2": "",
     "l3": "",
     "l4": ""
   }
 },
 
 // What is the event data
 "edata": {} // Required.
 
 // Tags
 "tags": [] // Optional. Encrypted dimension tags passed by respective channels
}
</pre>

**Note:**

* All events have the same structure with only difference in edata structures.

* All events have unique event codes i.e., (IDs).

* All events are as per platform schema
