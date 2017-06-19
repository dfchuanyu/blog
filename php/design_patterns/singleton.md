# 单例模式

>
    class Singleton
    {
        private static $_instance;
        public $mix;
        private function __construct() {}
        private function __clone() {}
        public static function getInstance()
        {
            if (!(self::$_instance intanceof self)) {
                self::$_instance = new self();
            }
            return self::$_instance;
        }
    }
    $firstProduct = Singleton::getInstance();
    $secondProduct = Singleton::getInstance();
 
    $firstProduct->mix = 'test';
    $secondProduct->mix = 'example';
 
    print_r($firstProduct->mix);
    // example
    print_r($secondProduct->mix);
    // example
>
[PHP 单例模式优点意义及如何实现](https://yq.aliyun.com/ziliao/9254)
