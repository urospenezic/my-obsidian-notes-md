
A reactive property, much like a dependency property in WPF. You generate a signal property like: 

protected propName = singal<type>(); 

now the UI will do fine grained change detection. So any time this signal value changes, the UI will rerender ONLY the parts of the app that actually use the signal value. Unlike before with zone where it would rerender the whole app.

We set the value of signal via signalProp.set() or update()
Signals are a replacement for observables tracking non async proccesses

computed() is a signal that's based off of another signal

input() is a signal that stores data passed from parent compoennts

output() -> signal that's supposed to be emitted up the visual tree

model() -> signal that's an input, but we can write to it (so like 2way)
