 
Currently the most popular CSS template lib for Angular. Used with tailwind CSS for quick styling. Basically Daisy UI gives us fully fleshed out templates for things like cards, carousels etc, whilst tailwind is used to give us CSS classes like text-center, flex, rotate-90 etc. 


Good thing about these is that they have 0 dependencies on Angular, they are just plain CSS libraries, so frequent Angular updates will not fuck up the third party CSS library you use.

To install tailwind: https://tailwindcss.com/docs/installation/framework-guides/angular

To install Daisy UI: https://daisyui.com/docs/install/angular/?lang=en or https://daisyui.com/docs/install/?lang=en if using tailwind (which is nice). 

**Dislaimer: there's a warning at @plugin "daisyui". That's a VS Code bug. Ignore it via: Settings -> search for uknownAtRules -> ignore at css lint

There are extensions for styling libraries to offer intellisense. Check vs code tab of notes.

Daisy UI and Tailwind will also auto adjust color scheme based off of dark/light theme of browser without touching any colors.


**DAISY UI HAS A THEME GENERATOR???? daisyui.com/theme-generator

---------
Themes
--

To add a theme:
handleSelectTheme(theme: string) {
this.selectedTheme.set(theme);
localStorage.setItem('theme', theme);
document.documentElement.setAttribute('data-theme', theme);
//hide menu after selection
var elem = document.activeElement as HTMLDivElement;
if (elem) {
elem.blur();
}
}

it's that easy, just store some string array of available theme names somewhere. https://daisyui.com/docs/themes/?lang=en

!!Add:
@plugin "daisyui" {
themes: all;
}
in styles.css









