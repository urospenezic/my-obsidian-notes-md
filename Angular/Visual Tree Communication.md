
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

same shit, just use output<T> 
   
   