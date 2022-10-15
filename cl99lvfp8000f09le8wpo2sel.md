# How to integrate a front-end framework into a Ruby on Rails project

I have many years of experience in front-end development and am currently on my way to full stack, choosing Ruby on rails as my full-stack framework. Although since the arrival of rails7 last year, hotwire can be said to shine on rails. I want to use it to write a little javascript to complete the front-end interactive rendering work. However, due to years of front-end development, I am a little reluctant to give up the so-called front-end frameworks (React, Vue, etc.), and I have to spend time and energy learning to hotwire, so I insist on moving forward on the front-end framework. How to integrate the current popular frameworks in the front-end well, fully justify your own advantages. In fact, many people explored it a few years ago, such as react_on_rails. With the maturity of rails7, there are already many solutions. There are several solutions:

1. React pure front-end (Create React App) + Rails JSON API, separation of front-end and back-end projects

2. NextJS + NodeJS + Rails JSON API front-end and back-end project separation

3. react_on_rails

4. Optionally use React apps in certain pages through Stimulus integration

5. lit 

6. Through document.addEventListener()+ReactDOM.createRoot()+fetch()+rails layer turbo_stream, Turbo.renderStreamMessage of front-end react

All of the above schemes have advantages and disadvantages, which need to be selected according to the actual situation. In my case, I am proficient in using front-end frameworks and do not like to pass APIs, and I am not familiar with Hotwire. Finally, through learning and searching, I saw the [inertia-rails](https: github.com/inertiajs/inertia-rails) and [inertiajs](https: inertiajs.com/client-side-setup) scheme, directly in the controllers of rails render data to the corresponding front-end framework components, where inertiajs is defined as Build single-page apps, without building an API. Inertia.js lets you quickly build modern single-page React, Vue and Svelte apps using classic server-side routing and controllers.

At present, I am changing to the official demo: [Ping CRM](https: demo.inertiajs.com/login). The choice is the rails integration solution, before there was [Ruby on Rails/Vue](https: github.com/ledermann/pingcrm /) by Georg Ledermann, I switched the front-end framework to react. This is my [github address](https: github.com/jisuanjixue/pingcrm_react), I switched the CSS technology to [chakra-ui](https: chakra -ui.com/getting-started), a react UI library. Welcome to start  !!