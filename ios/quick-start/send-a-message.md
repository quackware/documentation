# Send a Message
The following demonstrates the logic needed to create a conversation and send a message between 3 users named "Device", "Simulator", and "Dashboard".

```objectivec
//Please note, You must set `LYRConversation *conversation` and `LYRClient *layerClient` as properties of the ViewController.
@property (nonatomic) LYRConversation *conversation;
// You should also pass the layerClient from the App Delegate to the ViewController.
@property (nonatomic) LYRClient *layerClient;

- (void)sendMessage:(NSString *)messageText{
    // If no conversations exist, create a new conversation object with two participants
     // For the purposes of this Quick Start project, the 3 participants in this conversation are 'Device'  (the authenticated user id), 'Simulator', and 'Dashboard'.
    if (!self.conversation) {
        NSError *error = nil;
        self.conversation = [self.layerClient newConversationWithParticipants:[NSSet setWithArray:@[ @"Simulator", @ "Dashboard" ]] options:nil error:&error];
        if (!self.conversation) {
            NSLog(@"New Conversation creation failed: %@", error);
        }
    }

    // Creates a message part with text/plain MIME Type
    LYRMessagePart *messagePart = [LYRMessagePart messagePartWithText:messageText];

    // Creates and returns a new message object with the given conversation and array of message parts
    LYRMessage *message = [self.layerClient newMessageWithParts:@[messagePart] options:@{LYRMessageOptionsPushNotificationAlertKey: messageText} error:nil];

    // Sends the specified message
    NSError *error;
    BOOL success = [self.conversation sendMessage:message error:&error];
    if (success) {
        NSLog(@"Message queued to be sent: %@", messageText);
    } else {
        NSLog(@"Message send failed: %@", error);
    }
}
```

```emphasis
 Check the [Quick Start iOS Xcode project](https://github.com/layerhq/quick-start-ios) for more details.
```

You can verify that your message has been sent by looking at the logs inside [Developer Dashboard](https://developer.layer.com). Once you've sent a message, learn how to [display messages](/docs/quick-start/ios#display-messages).
