##工厂模式
```
/**
 * 抽象工厂角色
 */
interface Creator {
    public function factoryMethod();
}
 
/**
 * 具体工厂角色A
 */
class ConcreteCreatorA implements Creator {
 
    /**
     * 工厂方法 返回具体 产品A
     * @return ConcreteProductA
     */
    public function factoryMethod() {
        return new ConcreteProductA();
    }
}
 
/**
 * 具体工厂角色B
 */
class ConcreteCreatorB implements Creator {
 
    /**
     * 工厂方法 返回具体 产品B
     * @return ConcreteProductB
     */
    public function factoryMethod() {
        return new ConcreteProductB();
    }
}

/**
 * 抽象产品角色
 */
interface Product {
    public function operation();                                                                                    
}
 
/**
 * 具体产品角色A
 */
class ConcreteProductA implements Product {
 
    /**
     * 接口方法实现 输出特定字符串
     */
    public function operation() {
        echo 'ConcreteProductA <br />';
    }
}
 
/**
 * 具体产品角色B
 */
class ConcreteProductB implements Product {
 
    /**
     * 接口方法实现 输出特定字符串
     */
    public function operation() {
        echo 'ConcreteProductB <br />';
    }
}
 
class Client {
 
    /**
     * Main program.
     */
    public static function main() {
        $creatorA = new ConcreteCreatorA();
        $productA = $creatorA->factoryMethod();
        $productA->operation();
 
        $creatorB = new ConcreteCreatorB();
        $productB = $creatorB->factoryMethod();
        $productB->operation();
    }
 
}
```
## 抽象工厂
```
/**
 * 抽象工厂
 */
interface AbstractFactory {
    /**
     * 创建等级结构为A的产品的工厂方法
     */
    public function createProductA();
 
     /**
     * 创建等级结构为B的产品的工厂方法
     */
    public function createProductB();
 
}
 
/**
 * 具体工厂1
 */
class ConcreteFactory1 implements AbstractFactory{
 
    public function createProductA() {
        return new ProductA1();
    }
 
    public function createProductB() {
        return new ProductB1();
    }
}
 
 
/**
 * 具体工厂2
 */
class ConcreteFactory2 implements AbstractFactory{
 
    public function createProductA() {
        return new ProductA2();
    }
 
    public function createProductB() {
        return new ProductB2();
    }
}

/**
 * 抽象产品A
 */
interface AbstractProductA {
 
    /**
     * 取得产品名
     */
    public function getName();
}
 
/**
 * 抽象产品B
 */
interface AbstractProductB {
 
    /**
     * 取得产品名
     */
    public function getName();
}
 
/**
 * 具体产品Ａ1
 */
class ProductA1 implements AbstractProductA {
    private $_name;
 
    public function __construct() {
        $this->_name = 'product A1';
    }
 
    public function getName() {
        return $this->_name;
    }
}

/**
 * 具体产品Ａ2
 */
class ProductA2 implements AbstractProductA {
    private $_name;
 
    public function __construct() {
        $this->_name = 'product A2';
    }
 
    public function getName() {
        return $this->_name;
    }
}
 
 
/**
 * 具体产品B1
 */
class ProductB1 implements AbstractProductB {
    private $_name;
 
    public function __construct() {
        $this->_name = 'product B1';
    }
 
    public function getName() {
        return $this->_name;
    }
}
 
/**
 * 具体产品B2
 */
class ProductB2 implements AbstractProductB {
    private $_name;
 
    public function __construct() {
        $this->_name = 'product B2';
    }
 
    public function getName() {
        return $this->_name;
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
        self::run(new ConcreteFactory1());
        self::run(new ConcreteFactory2());
    }
 
    /**
     * 调用工厂实例生成产品，输出产品名
     * @param   $factory    AbstractFactory     工厂实例
     */
    public static function run(AbstractFactory $factory) {
        $productA = $factory->createProductA();
        $productB = $factory->createProductB();
        echo $productA->getName(), '<br />';
        echo $productB->getName(), '<br />';
    }
 
}
```
