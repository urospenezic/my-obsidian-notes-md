
**Parent to child
--

In child component add an Input that's required (the old way, before input signals):
@Input({required: true}) propertyName: type = defaultValue;
with input signals:
propertyName = input.required<type>();
Bind to that prop from parents markup:
<child-component [propertyName]="parentsSignal()"/>
   
And just keep inheriting like this as deep as needed

**IMPORTANT -- CHECK THE ANGULAR PROJECTS FOR VIEWCHILD AND OTHER WAYS TO ACCESS PARENT/CHILD DATA IN CODE BEHIND


**Child to parent
--

same shit, just use output<T>. output has the emit() method that notifies the parent binding to it. binding for output in parent doesn't use squared brackets, but parethesis: (childOutputName)="parentMethod($event)". Make sure that if binding to a method, that method accepts a parameter of the same type as output, because it will be auto passed in.
   
   