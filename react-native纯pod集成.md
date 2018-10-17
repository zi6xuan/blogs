# React-native转换成纯pod依赖的方法及遇到的问题

## 将工程转换为纯pod有以下重要步骤： ##
1. 首先初始化pod工程，这些就不赘述了，自行google
2. 然后就是编辑podfile文件夹了，非常重要，文件如下：

```
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'xxxxxx' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!

  # Pods for xxxxxx
  rn_path = '../node_modules/react-native'
  pod 'yoga', path: "#{rn_path}/ReactCommon/yoga/"
  pod 'React', path: rn_path, subspecs: [
  'Core',
  'CxxBridge',
  'RCTBlob',
  'RCTActionSheet',
  'RCTAnimation',
  'RCTGeolocation',
  'RCTImage',
  'RCTLinkingIOS',
  'RCTNetwork',
  'RCTSettings',
  'RCTText',
  'RCTVibration',
  'RCTWebSocket'
  ]
  pod 'IQKeyboardManager' #iOS8 and later
  
  pod 'react-native-cookies', :path => '../node_modules/react-native-cookies/ios'

  pod 'react-native-fast-image', :path => '../node_modules/react-native-fast-image'
  
  pod 'RNVectorIcons', :path => '../node_modules/react-native-vector-icons'
  
  pod 'RNSVG', :path => '../node_modules/react-native-svg'

  pod 'react-native-video', :path => '../node_modules/react-native-video'

  pod 'rn-fetch-blob', :path => '../node_modules/rn-fetch-blob'

  pod 'RNImageCropPicker', :path => '../node_modules/react-native-image-crop-picker'

  target 'xxxxxxTests' do
    inherit! :search_paths
    # Pods for testing
  end

end

target 'xxxxxx-tvOS' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!

  # Pods for xxxxxx-tvOS

  target 'xxxxxx-tvOSTests' do
    inherit! :search_paths
    # Pods for testing
  end

end
```

3. 下面这部分依赖基本是就是React核心库部分的东西，如果和自己本身的工程有差异可以对比检查修改下，
这里面添加的工程路径就是物理路径，而模块名字则是对应目录下的xxx.podspec文件的名字，其他的就是自己用到的依赖库了。

```
 rn_path = '../node_modules/react-native'
 pod 'yoga', path: "#{rn_path}/ReactCommon/yoga/"
 pod 'React', path: rn_path, subspecs: [
 'Core',
 'CxxBridge',
 'RCTBlob',
 'RCTActionSheet',
 'RCTAnimation',
 'RCTGeolocation',
 'RCTImage',
 'RCTLinkingIOS',
 'RCTNetwork',
 'RCTSettings',
 'RCTText',
 'RCTVibration',
 'RCTWebSocket'
 ]
```
4. podfile改完记得pod install，还要记得删除原来libs引用方式的工程文件

### 问题汇总
莫名其妙找不到JS库,[相似问题]（https://github.com/facebook/react-native/issues/14749#issuecomment-318340658）
在React集成库里面添加~~~BatchedBridge~~~CxxBridge这个就可以了
