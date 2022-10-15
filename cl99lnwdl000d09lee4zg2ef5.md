# 怎样很好的把前端框架集成到Ruby on Rails项目中

我有多年的前端开发经验，目前在朝全栈的路上前进，选择了Ruby on rails作为我的全栈框架。虽然自从去年rails7的到来，hotwire在ralis上可谓是大放异彩。我有想把它收下发挥的写很少的javascript 来完成前端交互渲染的工作。但是由于多年的前端开发， 有点依依不舍所谓的前端框架（React、Vue等），另外是自己还要花时间精力去学习hotwire, 所以我坚持还是在前端框架上继续前进。那怎样很好的的把前端现在流行的框架集成进去，充分理由自己的优势。其实几年前很多人就探索，比如react_on_rails.随着rails7的成熟，已经有很多方案.有几种方案：

1. React 纯前端 (Create React App) + Rails JSON API,　前后端项目分离

2. NextJS + NodeJS + Rails JSON API 前后端项目分离

3. react_on_rails

4. 通过 Stimulus 集成，选择性的在某一些页面里采用 React app

5. lit 

6. 通过前端react的document.addEventListener()+ReactDOM.createRoot()+fetch()+ rails层的turbo_stream, Turbo.renderStreamMessage

7. 

以上方案都有优缺点，需要结合实际情况选择。我的情况就是熟练用前端框架又不喜欢通过api, 不熟悉Hotwire。最后通过学习查找， 看到了[inertia-rails](https://github.com/inertiajs/inertia-rails )和 [inertiajs](https://inertiajs.com/client-side-setup)方案，直接在 rails 的 controllers 里 render data 到相应的前端框架 components 上，这里的inertiajs 定义为 Build single-page apps, without building an API. Inertia.js lets you quickly build modern single-page React, Vue and Svelte apps using classic server-side routing and controllers.

目前我在改在官方的demo： [Ping CRM](https://demo.inertiajs.com/login). 选择就是rails集成方案，之前有[Ruby on Rails/Vue](https://github.com/ledermann/pingcrm/) by Georg Ledermann， 我把前端框架改用react.
这是我的[github地址](https://github.com/jisuanjixue/pingcrm_react)，我把css的技术转用[chakra-ui](https://chakra-ui.com/getting-started)， 一个react UI 库。欢迎start！！