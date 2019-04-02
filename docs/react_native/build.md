# react native构建

## 使用构建工具构建(Expo)

1. 安装expo-cli
>npm i expo-cli -g
2. 运行expo 初始化一个项目
   
>expo init mwproject

>cd myproject

>npm start

3. 之后就可以通过手机的expo客户端扫描二维码实时预览页面效果

## react native cli构建结合原生(混合开发)

1. 安装react native 脚手架
>npm install react-native-cli  -g
2. 初始化一个项目
>react-native init myproject
3. 然后根据提示运行在各个平台模拟器上运行
>react-native run-ios

>react-native run-android

## 在模拟器上开启developer menu
- android模拟器 command+M
- iOS模拟器: command+d
- 在真机上可以通过摇动手机开启