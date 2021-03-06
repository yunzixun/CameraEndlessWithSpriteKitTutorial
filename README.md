
A Tutorial For How To Use SpriteKit Camera Making Endless Background 

<img src="https://upload-images.jianshu.io/upload_images/3896436-5ea7bb7aff8f1660.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/700" alt="效果" />
Player运用Camera节点向前移动的效果

<img class="alignnone size-full wp-image-469" src="http://www.ifiero.com/wp-content/uploads/2018/06/wobbing.png" alt="" width="1000" height="551" />
向前舞动

<img src="https://upload-images.jianshu.io/upload_images/3896436-7fa3ce6c8cc4f30d.png?imageMogr2/auto-orient/" alt="" />
命为SpriteNode为player

<img src="https://upload-images.jianshu.io/upload_images/3896436-05063a2325524555.png?imageMogr2/auto-orient/" alt="" />
player的Custom Class 为自定义Node

<img src="https://upload-images.jianshu.io/upload_images/3896436-5525c51c89ea758b.png?imageMogr2/auto-orient/" alt="" />
拖动Camera 进场景中，命名为mainCamera

<img src="https://upload-images.jianshu.io/upload_images/3896436-7596037a73110e5a.png?imageMogr2/auto-orient/" alt="" />
相机Camera的Position(0,0) ,Zposition为1

<img src="https://upload-images.jianshu.io/upload_images/3896436-852bd1f2232cd6c8.png?imageMogr2/auto-orient/" alt="" />
设置Scene的Camera为mainCamera

<img src="https://upload-images.jianshu.io/upload_images/3896436-229fb278525be6c3.png" alt="" />
camera的节点移动到2048(self.size.width)的时候，把红色框内的节点移动到最右边( node.position.x += self.size.width * SCENE_NUMBERS)


```
 /// 查找所有命名为ground的精灵节点
        enumerateChildNodes(withName: "//ground") { (node, _ ) in
            /// 如果当前的节点 + scene.size.with <  则移动节点
            if node.position.x + self.size.width < camera.position.x {
                node.position.x += self.size.width * SCENE_NUMBERS /// 更新节点的位置
            }
        }
```
        
完整的代码如下：

```
//  GameScene.swift
//  CameraEndless
//
//  Created by www.iFIERO.com on 2018/6/24.
//  Copyright © 2018 iFiero. All rights reserved.
//

import SpriteKit
import GameplayKit

public let CAMERA_MOVE_XPOS:CGFloat   = 12  /// 相机X-Axis移动的尺寸;
public let SCENE_NUMBERS:CGFloat = 2.0      /// 有几个场景scene

class GameScene: SKScene {
    
    private var player = PlayerClass()  /// 初始化自定义节点
    private var mainCamera:SKCameraNode!
    private var ground:SKSpriteNode! /// 地板
    private var bg:SKSpriteNode!    /// 背景
    private var cloud:SKSpriteNode! /// 云 cloud
    
    override func didMove(to view: SKView) {
        
        initUI()     /// 初始化Scene里的各个精灵节点
        initCamera() /// 初始化相机节点;
    }
    
    func initUI(){
        /// player名称为GameScene.sks 上自行命名的名称
        player = childNode(withName: "player") as! PlayerClass
        player.initPlayer() /// 开始Wobbing
        ground = childNode(withName: "ground") as! SKSpriteNode
        bg     = childNode(withName: "bg") as! SKSpriteNode
        cloud  = childNode(withName: "cloud") as! SKSpriteNode
    }
    
    func initCamera(){
        mainCamera = childNode(withName: "mainCamera") as! SKCameraNode
    }
    
    /// 移动节点
    func moveSprites(camera:SKCameraNode){
        /// 查找所有命名为ground的精灵节点
        enumerateChildNodes(withName: "//ground") { (node, _ ) in
            /// 如果当前的节点 + scene.size.with <  则移动节点
            if node.position.x + self.size.width < camera.position.x {
                node.position.x += self.size.width * SCENE_NUMBERS /// 更新节点的位置
            }
        }
        /// 查找bg
        enumerateChildNodes(withName: "//bg") { (node, _) in
            if node.position.x + self.size.width < camera.position.x {
                node.position.x += self.size.width * SCENE_NUMBERS
            }
        }
        ///查找所有云
        enumerateChildNodes(withName: "cloud") { (node, _) in
            if node.position.x + self.size.width < camera.position.x {
                node.position.x += self.size.width * SCENE_NUMBERS 
            }
        }
    }
    
    override func update(_ currentTime: TimeInterval) {
        mainCamera.position.x += CAMERA_MOVE_XPOS /// 向前移动;
        player.position.x += CAMERA_MOVE_XPOS     /// player向右移动的速度和camera的速度一致
        moveSprites(camera: mainCamera)           /// 传入相机节点
    }
}
```

更多教程：http://www.iFIERO.com
