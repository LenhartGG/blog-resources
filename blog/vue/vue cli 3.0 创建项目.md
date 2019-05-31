## 一、安装 @vue/cli
>更新到 3.x 之后，vue-cli 的包名从 vue-cli 改成了 @vue/cli

如果之前安装过旧版本的 vue-cli(1.x 或 2.x)，首先需要卸载掉
```javascript
npm uninstall -g vue-cli
# or
yarn global remove vue-cli 
```
然后重新安装新版本的@vue/cli
```javascript
npm install -g @vue/cli
# or
yarn global add @vue/cli
```
安装完成之后，可以使用 `vue -V`(大写的V)查看版本号
```javascript
$ vue -V
3.8.2
```
## 二、创建项目的方法
### 1.vue create
运行以下命令来创建一个新项目：
```javascript
vue create <my-project>
```
你会被提示选取一个 preset。你可以选默认的包含了基本的 Babel + ESLint 设置的 preset，也可以选“手动选择特性”来选取需要的特性。

如果你决定手动选择特性，在操作提示的最后你可以选择将已选项保存为一个将来可复用的 preset。我们会在下一个章节讨论 preset 和插件。
>~/.vuerc
被保存的 preset 将会存在用户的 home 目录下一个名为 .vuerc 的 JSON 文件里。如果你想要修改被保存的 preset / 选项，可以编辑这个文件。
在项目创建的过程中，你也会被提示选择喜欢的包管理器或使用淘宝 npm 镜像源以更快地安装依赖。这些选择也将会存入 ~/.vuerc。


vue create 命令有一些可选项，你可以通过运行以下命令进行探索：
```javascript
用法：create [options] <app-name>

创建一个由 `vue-cli-service` 提供支持的新项目

选项：

  -p, --preset <presetName>       忽略提示符并使用已保存的或远程的预设选项
  -d, --default                   忽略提示符并使用默认预设选项
  -i, --inlinePreset <json>       忽略提示符并使用内联的 JSON 字符串预设选项
  -m, --packageManager <command>  在安装依赖时使用指定的 npm 客户端
  -r, --registry <url>            在安装依赖时使用指定的 npm registry
  -g, --git [message]             强制 / 跳过 git 初始化，并可选的指定初始化提交信息
  -n, --no-git                    跳过 git 初始化
  -f, --force                     覆写目标目录可能存在的配置
  -c, --clone                     使用 git clone 获取远程预设选项
  -x, --proxy                     使用指定的代理创建项目
  -b, --bare                      创建项目时省略默认组件中的新手指导信息
  -h, --help                      输出使用帮助信息
```

### 2.使用图形化界面
也支持图形化的方式来构建项目
在指定目录打开终端，然后运行
```javascript
vue ui
```
上述命令会打开一个浏览器窗口，并以图形化界面将你引导至项目创建的流程。

### 3.拉取 2.x 模板 (旧版本)

>3.x 版本也支持 init 的构建方式
Vue CLI >= 3 和旧版使用了相同的 vue 命令，所以 Vue CLI 2 (vue-cli) 被覆盖了。如果你仍然需要使用旧版本的 vue init 功能，你可以全局安装一个桥接工具：
```javascript
npm install -g @vue/cli-init
# `vue init` 的运行效果将会跟 `vue-cli@2.x` 相同
vue init webpack my-project
```
### 4.以 `vue create`创建一个项目
1.执行：
```javascript
vue create my-project
```
2.此处有两个选择：
- default (babel, eslint)默认套餐，提供babel和eslint支持
- Manually select features自己去选择需要的功能，提供更多的特性选择。比如如果想要支持 TypeScript ，就应该选择这一项。
- 可以使用上下方向键来切换选项。如果只需要 babel 和 eslint 支持，那么选择第一项，就完事了，静静等待 vue 初始化项目。
- vue-cli 内置支持了8个功能特性，可以多选：使用方向键在特性选项之间切换，使用空格键选中当前特性，使用 a 键切换选择所有，使用 i 键翻转选项。

**对于每一项的功能，此处做个简单描述：**
- TypeScript 支持使用 TypeScript 书写源码
- Progressive Web App (PWA) Support PWA 支持。
- Router 支持 vue-router 。
- Vuex 支持 vuex 。
- CSS Pre-processors 支持 CSS 预处理器。
- Linter / Formatter 支持代码风格检查和格式化。
- Unit Testing 支持单元测试。
- E2E Testing 支持 E2E 测试。
- 我选择了 Router，Vuex，CSS Pre-processors，Linter / Formatter

3.按住enter进入下一步，接下来都是对之前每项选项的更详细的选择。
- css选择SCSS/SASS
- Linter / Formatter选择prettier
- 这一步就是要选择配置文件的位置了。对于 Babel 、 PostCSS 等，都可以有自己的配置文件： .babelrc 、 .postcssrc 等等，同时也可以把配置信息放在 package.json 里面。此处出于对编辑器（ Visual Studio Code ）的友好支持（编辑器一般默认会在项目根目录下寻找配置文件），选择把配置文件放在外面，选择 In dedicated config files

4.Save this as a preset for future projects?这个就是问要不要把当前的这一系列选项配置保存起来，方便下一次创建项目时复用。选择y。

5.选完之后， vue-cli 就根据前面选择的内容，开始初始化项目了。

## 三、启动项目
初始完之后，进入到项目根目录：
cd my-project
启动项目：
npm run serve
稍等一会儿，可以看到自动在浏览器中打开了

## 四、安装插件
待续。。。
