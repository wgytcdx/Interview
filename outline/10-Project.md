<!--
 * @Author: your name
 * @Date: 2021-01-15 16:26:33
 * @LastEditTime: 2021-02-23 15:07:58
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Interview Files/大纲/10-项目经验.md
-->
#### 1. 本地测试客户端

实现类postman的桌面应用, 

技术栈:Vue, Electron, Nodejs

负责实现功能:

本地 读取/存储 用户测试配置信息文件,可视化展示用户配置数据,实时获取应用日志. 撰写开发文档

~~由于项目前期并没有考虑做桌面应用, 所以立项时只考虑将配置信息存储到本地Storage中. 后来项目需求改变需要将配置存储到本地文件中, 所以在Vue中监听Storage的变化进行数据渲染, 在Electron的渲染进程中监听Vue中的按钮事件, 使用Node的FileSystem进行文件的读写与创建~~

~~如何监听vue的按钮触发: 在进行vue的按钮事件时,将按钮的名称存储到window.name上,然后去监听window.name的值进行操作~~

#### 2. 海洋检测客户端/服务端/安卓端

海洋数据 实时/回放 数据可视化展示

技术栈:Nodejs, Express, Sequelize, Vue, Axios, Echarts, Electron, MySQL, AndroidStudio, webView, github

负责实现功能:
使用Express搭建服务器,使用Sequelize的MySQL查询API进行数据查询, 将数据返回给前端/webview后进行数据渲染. 前端进行数据实时调取、渲染Echarts图表. 数据库日志存储本地文件

~~ 调取Echarts维度配置接口,将Echarts的X轴 多个Y轴及legend初始化, 并获取到第一条数据后初始化X轴, 使用按钮控制X轴显示数量, 之后进行数据渲染. 检测每次渲染Echarts的时候是否为undefined, 减少内存泄漏导致的页面卡顿, 定时使用Clear()方法清除Echarts的缓存. 使用组件传值方式控制组件大小/关闭. 回放客户端的数据展示:N个Echarts 关闭打开功能, 连续显示三条数据,自动回放数据间隔. 安卓端则是创建一个安卓的app 内嵌Webview,因为前端代码一起打包在AndroidStudio中,会导致ajax请求失败,所以webview请求的是服务器端的前端代码.  ~~

#### 3. inst数据分析平台

技术栈:Vue, Echarts, Axios, Electron

负责实现功能:

展示数据, 分析数据涨跌幅, 可视化合约配置等, 使用Echarts展示合约关系和数据分析. 使用Electron打包成桌面应用, 可配置的IP地址配置不同服务器使用

#### 4. 工行校招面试系统

技术栈:Vue, Axios, RSA

校招信息数据管理系统. 展示个人招聘信息, 提供上传下载修改等功能. 分类型记录面试场所的面试情况, 及考试信息等内容. 

#### 5. 任你学后台管理系统

技术栈:Vue, Axios, Echarts, FullCalender, gitee

后台数据管理系统, 集成了公司销售、讲师、财务、师资等部门的业务流程, 主要负责迭代业务逻辑, 重铸项目结构及开发公共组件, 撰写开发文档

#### 6. 任你学线上教室

技术栈:React, JavaScript, webpack, gitee

任你学线上课堂系统, 基于React开发, 集成线上视频实时对话, 画板, 聊天框等功能. 负责结合公司主要经营业务, 更新教室的一些线上活动, 针对版权进行一些防护措施(录屏水印, 防录屏等功能), 及维护

#### 7. 任你学H5

技术栈:JavaScript, WebView, Promise, gitee

任你学App的活动页面. 针对公司发布的一些活动、招生、评价等业务进行宣传. 对适配不同分辨率、安卓机型进行优化

#### 8. 任你学小程序

技术栈:微信小程序, iView, github

任你学App的小程序版本, 对客户的一些主要功能进行开发, 如付款, 课程, 个人信息, 客服服务等. 

#### 9. 泥王商城客户端

技术栈:MUI, JavaScript, Template.js, Promise, gulp, svn

基于微信浏览器开发的电商网站, 负责公共组件开发, 框架环境搭建, 开发规范及编写前端开发文档 

#### 10. 长春市双阳区公安局民警管理系统

负责业务功能实现,复用组件开发,项目重铸,优化性能,撰写开发文档

技术栈:JavaScript, Angular1, JQuery, Amap, Svn, Seajs

#### 11. 长春市双阳区公安局行业场所管理系统

主要负责针对兼容IE7以上浏览器的优化工作, 复用组件开发, 测试

技术栈:JavaScript, JQuery, Svn, ActiveX




























