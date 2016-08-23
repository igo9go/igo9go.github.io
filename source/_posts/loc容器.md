---
title: loc容器
date: 2016-08-22 12:51:35
tags: loc容器
categories: PHP 
---
# loc容器
1. superman类依赖 power类。在superman构造函数new power应用
    - 单一的依赖
2. superman类又依赖了Flight类，Force类，Shot类
    - 产生了跟多的依赖
3. 工厂模式，依赖转移！ 
    - 是由原来对多个外部的依赖变成了对一个 “工厂” 的依赖  
    > 我们不应该手动在 “超人” 类中固化了他的 “超能力” 初始化的行为，而转由外部负责，由外部创造超能力模组、装置或者芯片等（我们后面统一称为 “模组”），植入超人体内的某一个接口，这个接口是一个既定的，只要这个 “模组” 满足这个接口的装置都可以被超人所利用，可以提升、增加超人的某一种能力。这种由外部负责其依赖需求的行为，我们可以称其为 “控制反转（IoC）”。
`class SuperModuleFactory
{
    public function makeModule($moduleName, $options)
    {
        switch ($moduleName) {
            case 'Fight':     return new Fight($options[0], $options[1]);
            case 'Force':     return new Force($options[0]);
            case 'Shot':     return new Shot($options[0], $options[1], $options[2]);
            // case 'more': .......
            // case 'and more': .......
            // case 'and more': .......
            // case 'oh no! its too many!': .......
        }
    }
}`
    - 工厂模式的缺点就是：接口未知（即没有一个很好的契约模型，关于这个我马上会有解释）、产生对象类型单一。
4. 为了规范每一个超能力，需要定义一个接口加以规范， 每一个类继承接口
    - 因为一个 对象（object） 本身是由他的模板或者原型 —— 类 （class） ，经过实例化后产生的一个具体事物，而有时候，实现统一种方法且不同功能（或特性）的时候，会存在很多的类（class），这时候就需要有一个契约，让大家编写出可以被随时替换却不会产生影响的接口。
    - 只要不是由内部生产（比如初始化、构造函数 __construct 中通过工厂方法、自行手动 new 的），而是由外部以参数或其他形式注入的，都属于 依赖注入（DI） 
    `// 超能力模组
       $superModule = new XPower;
       // 初始化一个超人，并注入一个超能力模组依赖
       $superMan = new Superman($superModule);`
       
    - 手动的创建了一个超能力模组、手动的创建超人并注入了刚刚创建超能力模组


一个类需要绑定、注册至容器中，才能被“制造”。

个类要被容器所能够提取，必须要先注册至这个容器。既然 laravel 称这个容器叫做服务容器，那么我们需要某个服务，就得先注册、绑定这个服务到容器，那么提供服务并绑定服务至容器的东西，就是 服务提供者

服务提供者 将需要的类 注册绑定到服务容器
服务提供者主要分为两个部分，register（注册） 和 boot（引导、初始化），具体参考文档。register 负责进行向容器注册“脚本”，但要注意注册部分不要有对未知事物的依赖，如果有，就要移步至 boot 部分。

 
    


