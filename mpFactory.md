## Problems

1. Gulp 打包成为多个应用复杂, 目录,文件所需要的东西模糊不清
2. UI 无法复用, 需要重复开发, 无法交互 UI 等一致性
3. 未使用环境变量配置, 打包编译手动去改动请求域名
4. 重复目录比如 mixPage page 文件夹, 
5. 对于模板应用及非模板应用的区分不清晰
6. 对于数据处理的不清晰, 是否有可能以中间件形式介入, 比如模板需要 cookiea 非模板需要 cookieb 等
7. changelog, 维护每次需求变更, UI, 改动涉及人员, 影响范围.
8. 组件函数化
9. 静态数据提取, 全局静态数据规范化, 命名模块化
10. 以楼层为维度拆组件
11. 文件路径不统一导致复杂性增加
12. 目标 npm 包,插件必须要实现, taro 如何搞出插件
13. taro 能否打出插件





1. Taro 原生支持打 plugin, 需要新增script: taro build --plugin weapp --watch(开发模式)
2. Taro 普通项目模板支持转为 plugin 模式, 所有依赖相同, 需要更改 plugin 配置及 project.config.json 

具体参考代码库:

1. 普通Taro 项目 git@coding.jd.com:MP.FE/normal-taro.git
2. 普通 Taro Plugin 项目:  git@coding.jd.com:MP.FE/taro-plugin.git
3. 普通 Taro 项目转 Plugin 项目:  git@coding.jd.com:MP.FE/taroNoraml2Plugin.git



1. bdapp下线watchTower监控 已上线
2. 芥么签到接口加固问题修复 已上线
2. Taro 项目转换 Plugin 是否 ok 调研
2. 上报问题查看



## 上报问题

1. 参数不清,到底传了什么参数?get 请求 post 请求都没有参数展示, 错误码是什么,后端返回是什么, 是什么 ua, 时间戳是什么. 环境信息是什么, 是哪个平台, 来源是哪里, 是哪个项目, 网络情况是什么. 客户端是什么, appid 是什么. 