using @style.md and also inspired by @todo.html let's create a habit tracker for @weekly-habit-tracker-notes.md saved in new file index7.html. make sure to create and write to the file. I want the habit tracker to also encourage me and surface tips.

I want this to be built like a todo app. I have all these things that I can check off easily. I've done it. I've built. I've met the condition. And I want to see a 3-day rolling window. If I'm logged in. If I'm not logged in, then I want to see multiple dashboards. One that's like a week, one that's a month. And I should be able to toggle back and forth between weeks and months. I should be able to take extra notes at then end so not only can I check off an item in my to-do list but I can also say okay I slept at this time.

Save the get the data in firebase, data is read public, but have to login to edit, create login flow

10 MB Data is UTF-8 encoded.
use they key habitsTracker and the value should be a dense string blob that stores multiple days as year lik 2025 will be 25 and then get the day as a number, so 1st day to 365th day, so 25-1 will be first day of 2025, and 25-300 would be the 300th day. calc on the client side. Store the tracked habit checks in a condesed way so 1 year of data fits in 10 MB

const firebaseConfig = {
apiKey: "AIzaSyBMeY1TI-Hl7iZM43auDihG-7Ckx-6JBtY",
authDomain: "cool-90147.firebaseapp.com",
databaseURL: "https://cool-90147.firebaseio.com",
projectId: "cool-90147",
storageBucket: "cool-90147.firebasestorage.app",
messagingSenderId: "64561792498",
appId: "1:64561792498:web:d0eda52272bb90efab176b"
};

Only code in HTML/Tailwind in a single code block.
Any CSS styles should be in the style attribute. Start with a response, then code and finish with a response.
Don't mention about tokens, Tailwind or HTML.n
Always include the html, head and body tags.
Use lucide icons for javascript, 1.5 strokewidth.
Unless style is specified by user, design in the style of Linear, Stripe, Vercel, Tailwind UI (IMPORTANT: don't mention names).
Checkboxes, sliders, dropdowns, toggles should be custom (don't add, only include if part of the UI). Be extremely accurate with fonts.
For font weight, use one level thinner: for example, Bold should be Semibold.
Titles above 20px should use tracking=tight.
Make it responsive.
Avoid setting tailwind config or css classes, use tailwind directly in html tags.
If there are charts, use chart.js for charts (avoid bug: if your canvas is on the same level as other nodes: h2 p canvas div = infinite grows. h2 p div>canvas div = as intended.).
Add subtle dividers and outlines where appropriate.
Don't put tailwind classes in the html tag, put them in the body tags.
If no images are specified, use these Unsplash images like faces, 3d, render, etc.
Be creative with fonts, layouts, be extremely detailed and make it functional.
If design, code or html is provided, IMPORTANT: respect the original design, fonts, colors, style as much as possible.
Don't use javascript for animations, use tailwind instead. Add hover color and outline interactions.
For tech, cool, futuristic, favor dark mode unless specified otherwise.
For modern, traditional, professional, business, favor light mode unless specified otherwise.
Use 1.5 strokewidth for lucide icons and avoid gradient containers for icons.
Use subtle contrast.
For logos, use letters only with tight tracking.
Avoid a bottom right floating DOWNLOAD button.
