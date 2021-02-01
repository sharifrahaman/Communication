# Release Notes for February 1, 2021

This release contains following changes for ACS Calling Android (Java) SDK.

## Version 1.0.0-beta.6

## New Features:
1. Teams meetings interop Added. New types `GroupCallLocator`, `TeamsMeetingCoordinatesLocator`, `TeamsMeetingLinkLocator` introduced to support the interop scenarios.
### Usage example with `GroupCallLocator`
```java
    CallAgentOptions callAgentOptions = new CallAgentOptions();
    callAgentOptions.setDisplayName("My Custom Display name");
    CommunicationTokenCredential tokenCredential = new CommunicationTokenCredential("<USER ACCESS TOKEN>");
    CallAgent callAgent = callClient.createCallAgent(tokenCredential, callAgentOptions);

    android.content.context appContext = this.getApplicationContext(); // From within an activity or fragment
    java.util.UUID groupCallId = java.util.UUID.fromString(<GROUP CALL ID>);
    GroupCallLocator groupCallLocaltor = new GroupCallLocator(groupCallId);
    Call call = callClient.join(appContext, groupCallLocaltor, joinCallOptions);
```

### Usage example with `TeamsMeetingLinkLocator`
```java
    CallAgentOptions callAgentOptions = new CallAgentOptions();
    callAgentOptions.setDisplayName("My Custom Display name");
    CommunicationTokenCredential tokenCredential = new CommunicationTokenCredential("<USER ACCESS TOKEN>");
    CallAgent callAgent = callClient.createCallAgent(tokenCredential, callAgentOptions);

    android.content.context appContext = this.getApplicationContext(); // From within an activity or fragment
    string meetingLink = <TEAMS MEETINK LINK>;
    TeamsMeetingLinkLocator teamsMeetingLinkLocator = new TeamsMeetingLinkLocator(meetingLink);
    Call call = callClient.join(appContext, teamsMeetingLinkLocator, joinCallOptions);
```

### Usage example with `TeamsMeetingCoordinatesLocator`
```java
    CallAgentOptions callAgentOptions = new CallAgentOptions();
    callAgentOptions.setDisplayName("My Custom Display name");
    CommunicationTokenCredential tokenCredential = new CommunicationTokenCredential("<USER ACCESS TOKEN>");
    CallAgent callAgent = callClient.createCallAgent(tokenCredential, callAgentOptions);

    android.content.context appContext = this.getApplicationContext(); // From within an activity or fragment
    string threadId = <TEAMS THREAD ID>;
    java.util.UUID organizerId = java.util.UUID.fromString(<TEAMS ORGANIZER ID>);
    java.util.UUID tenantId = java.util.UUID.fromString(<TEAMS TENANT ID>);
    string messageId = <TEAMS MESSAGFE ID>;
    public TeamsMeetingCoordinatesLocator(String threadId, java.util.UUID organizerId, java.util.UUID tenantId, String messageId)

    TeamsMeetingCoordinatesLocator teamsMeetingCoordinatesLinkLocator = new TeamsMeetingCoordinatesLocator(meetingLink);
    Call call = callClient.join(appContext, teamsMeetingCoordinatesLinkLocator, joinCallOptions);
```

2. Cleanup logs from console, logs are stored in a the file instead

## Bug fixes
1. Device orientation video bug
2. Allow LocalVideoStream to be reused in multiple calls
3. Fix pointer corruption issue happening when we create VideoOptions will null LocalVideoStream
4. [Android] Unecessary to hold strong references on Objects with events (Call, CallAgent, DeviceManager, RemoteParticipant, RemoteVideoStream)

## Breaking API changes
Renamed `CommunicationUserCredential` to `CommunicationTokenCredential`
`RemoteParticipant` identifiers renamed:
- `PhoneNumber` to `PhoneNumberIdentifier`
- `CommunicationUser` to `CommunicationUserIdentifier`
- `CallingApplication` to `CallingApplicationIdentifier`

New `MicrosoftTeamsUserIdentifier` Added to support Teams interop scenarios

`CallAgent.join` API has been changed to support Teams interoperability
`join(android.content.Context context, GroupCallContext groupCallContext, JoinCallOptions joinCallOptions)`
is now:
`join(android.content.Context context, AbstractJoinMeetingLocator meetingLocator, JoinCallOptions joinCallOptions)`

With new types `GroupCallLocator`, `TeamsMeetingCoordinatesLocator`, `TeamsMeetingLinkLocator` being subclasses of `AbstractJoinMeetingLocator` that can be used for the various scenarios.

### Usage example with `GroupCallLocator`
```java
    CallAgentOptions callAgentOptions = new CallAgentOptions();
    callAgentOptions.setDisplayName("My Custom Display name");
    CommunicationTokenCredential tokenCredential = new CommunicationTokenCredential("<USER ACCESS TOKEN>");
    CallAgent callAgent = callClient.createCallAgent(tokenCredential, callAgentOptions);

    android.content.context appContext = this.getApplicationContext(); // From within an activity or fragment
    java.util.UUID groupCallId = java.util.UUID.fromString(<GROUP CALL ID>);
    GroupCallLocator groupCallLocaltor = new GroupCallLocator(groupCallId);
    Call call = callClient.join(appContext, groupCallLocaltor, joinCallOptions);
```
