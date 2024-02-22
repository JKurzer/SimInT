# SimInT
Simple Input Tester performs a series of basic calisthenics with the GameInput API to tell you about your devices. It's made to deduce the ADC bandwidth for joysticks, among other things.

Currently unfinished and unmaintained, but the core findings are:

8-10 bits per stick axis is enough, and stick axis is no more than 16 bits in hardware.  
Fixed point representation for stick axis is sufficient.  
A sample rate of 120hz is sufficient for most hardware.  
Some mice may functionally support higher.    
Triggers should be treated as digital buttons on all non-PS5 non-Sony controllers. Period.    

## Most control latency is due to implementation details in your engine.  
- Most engines use a fixed tick. On average, this adds half your fixed tick to latency.
- Most engines use significant abstraction layers on top of the platform's input frameworks. These tend to be really useful, but most use an event pump that runs on, you guessed it, fixed tick.
- Additionally, most of them actually use a fairly low polling rate under the hood, even if they appear to be evented.
- This is because high frequency devices have a tendency to badly overload the event pump. Event pumping is almost always single-threaded, and most engines have no threading for event handling.
- Fixing this is doable, but not trivial.
- https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getrawinputbuffer is gonna be your "friend."
