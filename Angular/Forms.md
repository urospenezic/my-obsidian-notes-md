

**angular.dev/guide/forms

Template driven and reactive. Check the maximillian project on forms to see the difference. TLDR: template driven are easier to manage and offer two-way data binding. quick to set up. reactive are a lot more robust and offer tons of features, but require 5k lines of code for a simple filtering form


**Template forms
--

They use # to assign names to elements which are then usable in code behind. <form #loginForm="ngForm"> will be able to be referenced in code behind

[(ngModel)]="creds.email" -> two way binding syntax

 #emailCtl="ngModel" is syntax supported by ngModel directive that tells angular that we wanna store this control created by angular into a variable called emailCtl-->





