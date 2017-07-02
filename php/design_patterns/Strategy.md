【策略模式中主要角色】
抽象策略(Strategy）角色：定义所有支持的算法的公共接口。通常是以一个接口或抽象来实现。Context使用这个接口来调用其ConcreteStrategy定义的算法
具体策略(ConcreteStrategy)角色：以Strategy接口实现某具体算法
环境(Context)角色：持有一个Strategy类的引用，用一个ConcreteStrategy对象来配置

【策略模式的优点和缺点】
策略模式的优点：
1、策略模式提供了管理相关的算法族的办法
2、策略模式提供了可以替换继承关系的办法 将算封闭在独立的Strategy类中使得你可以独立于其Context改变它
3、使用策略模式可以避免使用多重条件转移语句。

策略模式的缺点：
1、客户必须了解所有的策略 这是策略模式一个潜在的缺点
2、Strategy和Context之间的通信开销
3、策略模式会造成很多的策略类

【策略模式适用场景】
1、许多相关的类仅仅是行为有异。“策略”提供了一种用多个行为中的一个行为来配置一个类的方法
2、需要使用一个算法的不同变体。
3、算法使用客户不应该知道的数据。可使用策略模式以避免暴露复杂的，与算法相关的数据结构
4、一个类定义了多种行为，并且 这些行为在这个类的操作中以多个形式出现。将相关的条件分支移和它们各自的Strategy类中以代替这些条件语句
```
<?php
/**
 * 策略模式的PHP简单实现 2010-07-25 sz
 * @author 胖子 phppan.p#gmail.com  http://www.phppan.com                                                  
 * 哥学社成员（http://www.blog-brother.com/）
 * @package design pattern
 */
 
/**
 * 抽象策略角色，以接口实现
 */
interface Strategy {
 
    /**
     * 算法接口
     */
    public function algorithmInterface();
}
 
/**
 * 具体策略角色A
 */
class ConcreteStrategyA implements Strategy {
 
    public function algorithmInterface() {
        echo 'algorithmInterface A<br />';
    }
}
 
/**
 * 具体策略角色B
 */
class ConcreteStrategyB implements Strategy {
 
    public function algorithmInterface() {
        echo 'algorithmInterface B<br />';
    }
}
/**
 * 具体策略角色C
 */
class ConcreteStrategyC implements Strategy {
 
    public function algorithmInterface() {
        echo 'algorithmInterface C<br />';
    }
}
 
/**
 * 环境角色
 */
class Context {
    /* 引用的策略 */
    private $_strategy;
 
    public function __construct(Strategy $strategy) {
        $this->_strategy = $strategy;
    }
 
    public function contextInterface() {
        $this->_strategy->algorithmInterface();
    }
 
}
/**
 * 客户端
 */
class Client {
 
    /**
     * Main program.
     */
    public static function main() {
        $strategyA = new ConcreteStrategyA();
        $context = new Context($strategyA);
        $context->contextInterface();
 
        $strategyB = new ConcreteStrategyB();
        $context = new Context($strategyB);
        $context->contextInterface();
 
        $strategyC = new ConcreteStrategyC();
        $context = new Context($strategyC);
        $context->contextInterface();
    }
 
}
```
