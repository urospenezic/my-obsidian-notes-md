
A reactive property, much like a dependency property in WPF. You generate a signal property like: 

protected propName = singal<type>(); 

now the UI will do fine grained change detection. So any time this signal value changes, the UI will rerender ONLY the parts of the app that actually use the signal value. Unlike before with zone where it would rerender the whole app.

We set the value of signal via signalProp.set() or update()

