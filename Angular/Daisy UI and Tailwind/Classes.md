**all of these are used like css normal classes, so class="w-full p-3 fixed" is valid

- top-0 -> top margin 0
- z (z-0...z-50) -> z index depth
- bg- (lots of stuff, like bg-gradient-to-r from-primary to-colorName) will add a gradient to the right, pretty self explanatory just lots of typing
- bg-colorName -> sets background color to green for example
- bg-base-100 - white text with 100 
- fixed -> fixes the element at position -> useful for footer/headers/nav bars
- w-1/2 -> half of screen (centered on screen) w-full to fill available width!!!!
- mx-auto -> auto margins left and right
- p- -> spacing of 2.5rem (padding). py- (spacing tob and bottom) p-10 (10 spacing everywhere) etc
- mt-10 -> margin
- shadow-xl -> add a shadow
- list -> daisy ui list, used on UL for example
- list-row -> typically on LI of UL, orderes items in a row
- items-center -> centers any direct children
- size-12 -> size
- rounded-box -> give a div, image, anything really a bit of rounding on the edges
- flex - flex system **check out flexboxfroggy.com
- my ->margin top and bottom
- gap -padding
- text- -> text manipulation
- uppercase
- ml - margin left. **MARGIN HAS AUTO, so ml-auto is auto left margin
- ** HERO ** -> homepage class for main div by daisy
- min-h -> min height (min-h-screen == full sreen)
- btn (button stuff). btn-primary, btn-ghost, btn-lg(large) etc
- justify -> children spacing and allignment
- text-accent
- h -> height, can be h-full for all the space, h-20 for specific, h-[85vh] for 85% of viewport size etc
- object-cover - expands the image/item to available space of the container\
- divider - added to no content div to give us a separator
- menu
- rounded box

`color-base-100` refers to a semantic CSS variable, common in UI frameworks . The exact color is theme-dependent, allowing for light and dark modes, but it serves as the base for the page background, with subsequent colors designed to provide contrast for foreground elements.

card bg-base-100 w-1/2 mx-auto flex flex-col p-6 rounded-lg shadow-lg -> card with theme dependent bg color, width half of screen, auto margins, children vertically aligned (flex-col), padding of 6, rounded on the edges, large shadow. JEEEZ

example for nav bar: 
<header class="p-3 w-full fixed top-0 z-50 bg-gradient-to-r from-primary to-black">

  <div class="flex align-middle items-center px-10 mx-auto gap-6">

    <a class="max-h-16 text-white border-r-white border-r-2 pr-6">

      <div class="flex flex-row align-middle items-center gap-2">

        <svg

          xmlns="http://www.w3.org/2000/svg"

          fill="none"

          viewBox="0 0 24 24"

          stroke-width="1.5"

          stroke="currentColor"

          class="size-6"

        >

          <path

            stroke-linecap="round"

            stroke-linejoin="round"

            d="M15 19.128a9.38 9.38 0 0 0 2.625.372 9.337 9.337 0 0 0 4.121-.952 4.125 4.125 0 0 0-7.533-2.493M15 19.128v-.003c0-1.113-.285-2.16-.786-3.07M15 19.128v.106A12.318 12.318 0 0 1 8.624 21c-2.331 0-4.512-.645-6.374-1.766l-.001-.109a6.375 6.375 0 0 1 11.964-3.07M12 6.375a3.375 3.375 0 1 1-6.75 0 3.375 3.375 0 0 1 6.75 0Zm8.25 2.25a2.625 2.625 0 1 1-5.25 0 2.625 2.625 0 0 1 5.25 0Z"

          />

        </svg>

        Dating App

      </div>

    </a>

    <nav class="flex gap-3 my-2 uppercase text-lg text-white">

      <a>Matches</a>

      <a>Lists</a>

      <a>Chat</a>

    </nav>

    <div class="flex align-middle ml-auto gap-3">

      <form class="flex items-center gap-3">

        <input type="text" class="input" placeholder="Email" />

        <input type="password" class="input" placeholder="Password" />

        <button class="btn btn-primary" type="submit">Login</button>

      </form>

    </div>

  </div>

</header>

Looks absolutely stunning in angular :)

Animations:
transition-all duration-300 ease-in-out transform hover:-translate-y-2- lots of stuff, check documentation i guess. this one moves the div up on mouseover