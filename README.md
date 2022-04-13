前言：通常在vue的开发中，我们会使用到各式各样的插件库，比如说最常见的elementUI、vant等。通常我们只会使用这些插件，却不知道其中的原理。现在我来记录一下，从0到1开发一个简单的插件，然后再发布到npm官网开源的整个过程。以后有时间的时候，可以通过这种方式开发出更多的插件，从而提高工作和开发的效率。

源码：[DGT的自定义插件](https://github.com/DGT18229282792/npm-auto-loader)

首先，通过脚手架vue create 创建一个基本的vue结构的框架项目，用来专门存放你所开发的插件库（如果你在现有的项目中去进行插件的发布可能会导致影响到原来的项目的功能）。

在src目录下创建plugins文件夹用来存放你所开发的插件

![图片](data:image/png;base64,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)


创建基本的组件，这个你可以自定义，随意写一个组件，这里以一个vue项目中最常见的提示框组件为例。

![图片](data:image/png;base64,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)


dgtmessage.vue:

```typescript
<template>
  <div ref="messageVueRef" class="messageVue" :class="{isShowActive:showStatus}">
      这是通知消息
  </div>
</template>

<script>
export default {
    name:'dgt-message',
    data(){
        return{
            showStatus:false,
        }
    },
    methods:{
        MessageFun(isShow=false){
            this.showStatus = isShow
            setTimeout(()=>{
                this.showStatus = false
            },2000)
        }
    }
}
</script>

<style>
.messageVue{
    display: none;
    width: 150px;
    height: 30px;
    background: rgba(0,0,0,0.8);
    color: #fff;
    line-height: 30px;
    text-align: center;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    opacity: 0;
}
.isShowActive{
    display: block;
     background: rgba(0,0,0,0.8);
    width: 150px;
    height: 30px;
    color: #fff;
    line-height: 30px;
    text-align: center;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    opacity: 1;
}
</style>
```

这里实现的功能很简单，抛出了一个方法，接收显示和隐藏来控制组件的显示和隐藏。

目录下新建index.js，用来编写你的install入口方法，这个文件会自动读取当前plugins文件下所有的vue后缀结尾的组件，进行自动注册。所以不需要像传统的组件一个一个进行注册，做到了自动识别。

index.js:

```typescript
const requireComponent = require.context('./', true, /\.vue$/);
// require提供的一个读取文件的方法，第一个参数是路径，第二个参数是是否读取所有子目录，第三个是正则以vue结尾的文件
// 必须要提供一个install方法，这个方法用作在Vue.use方法里面自动识别install方法并执行。传入的参数是Vue
const install = (Vue) => {
    if (install.installed) return;
// 如果有注册过则返回
    install.installed;
// 没有注册过则进行注册
    console.log(requireComponent.keys())
// 获取拿到的所有组件
    requireComponent.keys().forEach(fileName => {
        // 拿到当前的组件
        const config = requireComponent(fileName)
        // 拿到当前组件的组件名
        const componentName = config.default || config
        Vue.component(componentName.name, config.default || config)
    });
}
// 环境检测
if (typeof window !== 'undefined' && window.Vue) {
    install(window.Vue)
}
export default {
    install
}
```

修改package.json

```typescript
 {"name": "dgt-message",
  "version": "0.1.0",
  "private": false,
  "description": "一个简易的弹窗插件，用来讲述npm插件的开发与上传",
  "license": "MIT",
  "main": "lib/dgt-message.umd.min.js", // 先不写
  .
  .
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    "lib": "vue-cli-service build --target lib --name dgt-message --dest lib src/plugins/index.js"
  },// 补充lib打包指令，指定名称dgt-message,目标路径lib 为插件的入口js
```
这里有几个需要注意的点，name属性必须跟组件提供的name值一致，version指定版本号，默认的private都是true，需要修改成false。license这里选择的是‘’MIT‘’main最后再进行指定。
然后在终端运行

```typescript
npm run lib //打包
```
然后会在根目录下生成一个lib文件
![图片](data:image/png;base64,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)


这里需要注意两个文件，第一个是dgt-message.css，这个文件需要在main.js中手动引入。

dgt-message.umd.min.js这个文件需要补充上面那个 "main": "lib/dgt-message.umd.min.js"

最后一步

```typescript

npm login // 登录到npm官网
// 输入账号，密码，邮箱
npm publish
```

最后登录到你的npm主页，你就会发现多了一个刚刚上传的插件。




## 使用

使用的时候，跟其他的插件使用方式一致，当你开发的是一个UI组件，你需要引入插件，并引入插件的样式文件

当你开发的是一个指令，或者没有样式文件则仅引入插件即可。

```typescript
import dgtMessagr from 'dgt-message'
import 'dgt-message/lib/dgt-message.css';
Vue.use(dgtMessagr)

```
项目中
```typescript
<template>
  <div class="hello">
    <button @click="testPop">测试</button>
    <dgt-message ref="messageRef"></dgt-message>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  methods:{
    testPop(){
      console.log(this.$refs.messageRef)
      this.$refs.messageRef.MessageFun('shimie')
    }
  }
}
</script>
```

到此，一个完整的插件开发流程已经全部完成。通过这种方式，你还可以在index.js里面不仅仅是进行组件的注册，还可以开发一些自定义的指令。后面会继续补充这一块。

2022/4/13 DGT

