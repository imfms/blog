---
layout: post
title:  "Android单元测试学习手记"
subtitle: "我只想关心当下我的业务逻辑与我的每一行代码实现，请让我忘记历史带给我的包袱与压力"
date:   2018-04-05 17:25:30 +0800
categories: program
tags: 
- java
- android
- test
- unit-test
---

## Android单元测试学习手记

### 一，前言

- 每次发版前，内心总会控制不住的开始焦虑，虽然测试代码的验证通过不能证明代码没有问题，但至少它能证明这庞大的历史代码还是相对安全的。
- 眼前代码的修改会不会导致其他问题的出现，我不知道，我可能需要细细查阅，细细摩挲，才能怯懦的告诉你：“应该不会吧”。
- 欲来欲能发觉人脑能同时顾及的东西太少太少，而真实情况下要顾及的东西又太多太多。
- 我只想关心当下我的业务逻辑与我的每一行代码实现，请让我忘记历史带给我的包袱与压力，让我享受当下。
- TDD，我来了。

### 二，学习历程

#### 1. JUnit4学习

在保证学习的易懂又不失系统情况下，选择了一些中文博文文章配合官方文档进行学习

- [在Eclipse中使用JUnit4进行单元测试（初级篇）](http://blog.csdn.net/andycpp/article/details/1327147)
- [在Eclipse中使用JUnit4进行单元测试（中级篇）](http://blog.csdn.net/andycpp/article/details/1327346)
- [在Eclipse中使用JUnit4进行单元测试（高级篇）](http://blog.csdn.net/andycpp/article/details/1329218)
- [JUnit4 Offical Document](http://junit.org/junit4/)
- [探索 JUnit 4.4 新特性](https://www.ibm.com/developerworks/cn/java/j-lo-junit44/)
  配合Junit4 Offical Document辅助学习

在测试框架出现之前，开发者们撰写测试用例会很痛苦，有很多问题需要面对，测试代码难以整理与维护，测试用例的执行时机，测试统一的规范性，测试覆盖量的衡量，测试用例复用...  想说爱你不容易

JUnit4是一个java单元测试框架，框架提供易懂易用且不失灵活性和扩展性的辅助测试功能，封装了大多在测试代码中所需要的功能：结果断言测试，异常测试，方法执行超时测试，测试前/后 预备/收尾工作，批量测试，分类测试，假设测试，理论测试... ... 并且有各大java ide及插件的良好支持(eclipse, IntelliJ).

#### 2. Junit练习, 给自己的小类库撰写测试

还是做做题吧，不然连自己什么都不会这件事都不知道

[java-library - multivalue, check library add unit test](https://github.com/imfms/java-library/commit/3563b799e9e3dc262f6fb90f34e3adda33e0acb3)

#### 3. Android测试学习, 相关文档阅读 

宏观了解在Android开发中通常使用的测试手段

- [Testing Apps on Android](https://developer.android.google.cn/training/testing/index.html)
- [Test Apps on Anroid Studio](https://developer.android.google.cn/studio/test/index.html)

#### 4. 调用任何方法都会throw exception的android.jar 

> By default, the [Android Plug-in for Gradle](https://developer.android.com/tools/building/plugin-for-gradle.html) executes your local unit tests against a modified version of the `android.jar` library, which does not contain any actual code. Instead, method calls to Android classes from your unit test throw an exception. This is to make sure you test only your code and do not depend on any particular behavior of the Android platform (that you have not explicitly mocked).
>
> 默认情况下，用于[Android Plug-in for Gradle](https://developer.android.com/tools/building/plugin-for-gradle.html) 执行本地单元测试，会提供一个修改版本的`android.jar`，它不包含任何实际的代码。相反，从单元测试中调用Android SDK的所有方法皆会抛出异常。这是为了确保您只测试您的逻辑代码，并且不依赖于Android平台的任何特定行为(在没有明确地进行Mock的情况下)。

- [Mock Android dependencies](https://developer.android.com/training/testing/unit-testing/local-unit-tests.html#mocking-dependencies)


如果希望android.jar中被调用的api不抛出异常，那么还有一个选择就是所有api的返回值为默认值，需要在module级别的build.gradle文件中配置如下代码

~~~groovy
android {
  ...
  testOptions {
    unitTests.returnDefaultValues = true
  }
}
~~~

当然，此种情况下开发者需要考虑撰写的单元测试代码会不会因为所有的api返回都为默认值而导致的有些可能被忽略的错误

#### 5. 幻想一切, 不受制于环境, Mockito

Mockito是一款Java平台模拟类行为的框架，以Java动态代理机制为基础，有着简单易用的api，多服务于Java单元测试作业。

项目开发过程中，被测试的代码中会存在一些对外部环境行为依赖的情况，同时由于种种原因不能将其真实引入测试环境

- 外部环境不稳定，属于不可控因素，不能保证外部环境时时准备待命
- 想要对 '代码对外部环境的某个行为做出的响应' 正确与否进行测试，可是外部环境想要达到该行为的成本很高
  - 时间成本, 外部环境达到该行为要耗费很多时间，降低测试效率
  - 精力成本, 外部环境达到该行为要执行各种五花八门的步骤
  - 金钱成本, 外部环境达到该行为每次就是要花一块钱，你就说测不测
- ... ...

**目的驱动**，当我想要测试这段代码是否对外部做出了正确响应的时候，我的目的是测试这段代码是否对外部做出了正确响应，所以所有与直接达到该目的无关内容，都不应该存在。代码不同于人生，不需要此生多娇通灵万物处处感悟。

学习Mockito的过程中像学习其他内容一样，碎片时间里搜索各种入门博文，也读了很多，觉得关于Mockito的学习已经可以了，大部分常用操作已经没问题了，大不了遇到解决不了的现查就好。忙忙碌碌闲闲适适工作后想起之前的一句不知是别人说的还是自己说的话：“你所了解的知识会影响你解决问题的思路与方法”。决定还是在这休息日略读一遍Mockito官方文档。

注：本来对Mockito的文档是拒绝的,因为是javadoc格式, 但后来认真看了下其实是把文档写到了javadoc中，官方也解释了原因：“让开发人员随时记录他们编写的代码而保持文档与代码的同步更新”。(后期写文档的时候各种苦逼可不是吗 :) 

- [Mockito framework](http://site.mockito.org/)
  Mockito 项目官网，在介绍中有指向最新版本javadoc的链接

在Android测试中Mockito配合调用任何方法都会throw exception的android.jar对Android这个“外部环境”进行模拟再合适不过咯 :)

#### 6. Robolectric 在JVM下执行依赖android环境代码的测试

[Robolectric](http://robolectric.org/)
代码在android环境下的执行速度会严重影响到开发者对测试的积极性，Robolectric是一套单元测试框架，允许开发者在JVM环境下执行依赖android环境的代码
android下撰写测试代码首选JVM, 其次考虑使用Robolectric, 之后是Instrumented, 最终是手动测试

在学习Rebolectric的过程中其实我个人感觉是挺迷惑的，并且是在迷惑挺久一段时间并读了N多文章之后才看清了他的真面目，然而这一切都只是因为自己给自己设置限制，把它想想的太复杂魔幻了，在此引用小创作在[Robolectric，在JVM上调用安卓的类](http://chriszou.com/2016/06/05/robolectric-android-on-jvm.html)结尾中的一句话
> Robolectric的角色，应该是一个让我们在做`测试`的过程中，能够调用安卓的类，测试安卓的类，把安卓的类当做普通的纯java类的一个framework，仅此而已

#### 7. 阅读及学习[小创作撰写的测试系列文章](http://chriszou.com/2016/06/07/android-unit-testing-everything-you-need-to-know.html)，补充知识链

[以及demo中readme中的相关文章链接](https://github.com/ChrisZou/android-unit-testing-tutorial)

#### 8. 项目中小试牛刀

1. 想给项目首页写个测试

2. 先从首页P(Presenter)开始

3. 这个P用M(Model)异步获取了个数据并调用V(View)填充了数据

4. 发现自己没办法mock M, 因为自己的M是在P中直接引用了一个全局static字段位置

5. 将P中所有对M的引用修改为从构造注入，谁调用谁负责

6. 啥都不管，先跑一下试试， Boom! "NullPointException"
   发现代码中引用了Application中一个全局字段，但是Application是安卓环境才会创建的，遂将所有对该字段对Application的依赖移除

7. 撰写P一个方法的测试，内容是请求数据后填充V
   Boom! "Method getMainLooper in android.os.Looper not mocked."

8. 获取数据后的主线程调度器也是依赖于安卓环境，所以P层不能直接做这个操作
   修改代码让V提供一个Ui可渲染线程调度器，mockV的时候提供的线程调度器不对线程作任何变更

9. 代码有时测过有时测不过 - -，发现是因为请求数据是异步操作
   本想像主线程调度器一样修改，将控制权交给调用者，但是想了想异步请求数据的行为好像跟安卓环境并没有什么关系，虽然为了测试可以修改不可测的代码，但总执拗的觉得还是不很优雅；遂暂时在测试中等待异步数据完成后再稍作等待后进行操作(依然不优雅:)

10. 阅读含有测试的Android Mvp架构开源项目[MovieGuide](https://github.com/esoxjem/MovieGuide)，发现RxJava/RxAndroid提供各常用调度器的替换方式
  [RxSchedulerRule](https://github.com/esoxjem/MovieGuide/blob/e3689c5c867abea8b01f1f862b1e8b4fd6a9a52d/app/src/test/java/com/esoxjem/movieguide/RxSchedulerRule.java)
  这下测试时而通过时而不通过的问题也优雅的解决了，也不用让View提供Ui可渲染线程调度器
  只是P层依然依赖了RxAndroid，看起来有点膈应，不过项目只在android平台上运行的话目测影响不大

11. 好，P层完了，再拿V层练练手吧

12. (- -|||), 这... Activity里面对M与P的持有我该怎么Mock ... ...
    P层对M的依赖优雅解决了，甩给了V, 到了V层还是得处理，V也不能甩出去了，V的调用者是系统了 :(

13. V层对P层的引用可mock方案制定
    第一想法是引入dagger也确实去着手学习并使用了，但后来理解了概念后想了想，这个问题并不是dagger可以直接解决的，不管用什么注入库最终都是要View内部从外部获取到Presenter依赖，故暂选择手写

    1. 新建一个创建所有Presenter实例的辅助类。称为PresenterProvider

    2. 创建一个提供PresenterProvider的工具，提供set方法与get方法。称为PresenterProviderHolder

    3. 所有View实例化Presenter时全部通过PresenterProviderHolder.get().getXxxPresenter(arg0, arg1, ...)方式获取

    4. 非单元测试环境

       在app入口application调用PresenterProviderHolder.set方法，设置非单元测试环境PresenterProvider

    5. 单元测试环境
       在测试View之前通过PresenterProviderHolder.set(mockedPresenterProvider)设置mock后的PresenterProvider

### 三，暂告段落

凭空横插一个代码级别管理方案，且该方案由于与正常业务开发的纬度不同(相当于对正常流程撰写的代码一一拆开横插一脚)，自然会对已有思考/编写方式和已有代码有一定的侵入性。改变是不容易的，但改变是值得的
