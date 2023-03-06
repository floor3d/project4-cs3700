### High-level approach
  Receiver:
    1. receive message
    2. check if it was already received; if so, drop it
    3. send an ack no matter what
    4. if it has not yet been received, see if it is properly sequenced (i.e. packet 3 after we 
       just acked packet 2): if so, print. if not, add to window
    5. periodically check window to see if we can print out some packets (ex. we received 4 before 3
       so we held it in the window but once we print 3 we can immediately print 4)
  Sender:
    1. alternate betweenr receiving and sending
    2. only let so many messages be sent at once. if we receive acks we should increase that window
       size, if packets are dropped divide it.
    3. keep track of acked messages and sent messages so we know which ones have been dropped
    4. check for dropped messages and see if the sent time of the message has taken more than 2 * rtt
       to get an ack; if so, resend
    5. if acks are being sent more quickly, send packets out more quickly because the network can handle it
  
### Challenges we faced
  It was difficult to get started with this project. Getting our bearings and figuring out the 
  starter code (as short as it was) was an important step.
  Another challenge was dealing with dropped packets. We originally tried to do a time.sleep but it
  took too long and made the communication slow. We also originally did not try to ack packets if we were waiting on another one
  but that slowed communication because the communication would stall just because of a single drop.
  We also tried to overcomplicate the RTT calculation but ended up making it very simple by just
  calculating the time between the sending and the ack for the most recent message. Overall we didn't
  have a very tough time, only issue was that we kept overcomplicating or overlooking things.

### Features / Design Choices we thought were good
  We tried to keep the code simple and easy to read, and not have too much code per method, so that
  it is clear to any onlookers what each function does. 
  We kept a running window of received messages that couldn't be printed yet and sorted and looped
  through it to ensure quick printing of waiting messages after we received and printed dropped packets.
  We also acked every time we received a message so long as it was valid so that the sender knew it
  wasn't being dropped.
  Finally, we ensured that the mangled packets would be dropped by hashing the payload as well as
  try - catching when we parsed the json object. This was able to eliminate all mangled packets with ease.

### How we tested the code
  For testing code, we employed three main strategies.
  1. Running it through the simulator. There is no better way to see if the code works than to run
  it in a simulation that checks for correctness.
  2. print checking. When we realized there was an error in our logic or the simulator was returning
  errors, we always made sure to print the events that were going on, for example what was in the sent messages or in the receiver's window,
  to make sure that our logic was sound. This helped tremendously because it identified exactly where we went wrong so we could
  check over the code that was causing errors.
  3. Logic checking in environment. For more technical functionalities like doing datetime calculus
  to check for a timed out ack, we ran the code by itself in a python3 environment with dummy data
  to make sure that the function did its job correctly and did not run into any runtime errors. 
  This helped to get our functionality working even before we simulated it.
