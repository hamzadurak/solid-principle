# SOLID Prensipleri

Solid prensipleri, yazılım tasarımı ve geliştirme sürecinde sıkça kullanılan bir dizi prensiptir. İsimleri SOLID
prensiplerinin baş harflerini temsil eder. Bu prensipler şunlardır:

1.Single Responsibility Principle (Tek Sorumluluk Prensibi)

2.Open-Closed Principle (Açık Kapalı Prensibi)

3.Liskov Substitution Principle (Liskov Yerine Geçme Prensibi)

4.Interface Segregation Principle (Arayüz Ayrımı Prensibi)

5.Dependency Inversion Principle (Bağımlılık Tersine Çevirme Prensibi)

## 1.Single Responsibility Principle (Tek Sorumluluk Prensibi)

Bir sınıfın veya modülün yalnızca bir görevi olmalıdır. Bir sınıfın sorumluluğu fazla ise, kodun değiştirilmesi, bakımı
ve test edilmesi zorlaşabilir.

## Kod

```php
class Customer {
   public function getCustomer($id) {
      // Müşteri bilgilerini veritabanından getirir
   }

   public function saveCustomer($customerData) {
      // Müşteri bilgilerini veritabanına kaydeder
   }

   public function deleteCustomer($id) {
      // Müşteri bilgilerini veritabanından siler
   }
}
```

Yukarıdaki sınıf, müşteri verilerini veritabanından almak, kaydetmek ve silmek gibi üç ayrı görevi yerine getiriyor. Bu
sınıfın tek sorumluluğu olmadığı için, örneğin veritabanı bağlantısı değiştiğinde veya başka bir veri kaynağı
kullanılmaya karar verildiğinde, kodda değişiklik yapmak zorunda kalabilirsiniz. Tek sorumluluk prensibine uygun hale
getirmek için, her görev için ayrı sınıflar kullanabiliriz:

## Kod

```php
class CustomerGetter {
   public function getCustomer($id) {
      // Müşteri bilgilerini veritabanından getirir
   }
}

class CustomerSaver {
   public function saveCustomer($customerData) {
      // Müşteri bilgilerini veritabanına kaydeder
   }
}

class CustomerDeleter {
   public function deleteCustomer($id) {
      // Müşteri bilgilerini veritabanından siler
   }
}
```

## 2.Open-Closed Principle (Açık Kapalı Prensibi)

Bir sınıfın davranışını değiştirmek için kodun kendisini değiştirmeden, yeni kod eklemek mümkün olmalıdır.

## Kod

```php
interface Shape
{
    public function calculateArea();
}

class Rectangle implements Shape
{
    public $width;
    public $height;

    public function calculateArea()
    {
        return $this->width * $this->height;
    }
}

class Circle implements Shape
{
    public $radius;

    public function calculateArea()
    {
        return $this->radius * $this->radius * pi();
    }
}
```

Yukarıdaki kod, Shape arayüzünü uygulayan Rectangle ve Circle sınıflarını içerir. Bu sınıflar, calculateArea() yöntemini
uygular ve bu yöntem şeklin alanını hesaplar. Açık kapalı prensibine uygun hale getirmek için, yeni bir şekil eklemek
istediğimizde sınıfı değiştirmememiz gerekir. Bunu yapmak için, Shape arayüzünü uygulayan yeni bir sınıf ekleyebiliriz:

## Kod

```php
class Triangle implements Shape {
   public $base;
   public $height;

   public function calculateArea() {
      return 0.5 * $this->base * $this->height;
   }
}
```

## 3.Liskov Substitution Principle (Liskov Yerine Geçme Prensibi)

Alt sınıfların, üst sınıfların yerine geçebilmesi gerekir. Yani alt sınıfların, üst sınıfların davranışlarını
değiştirmeden kullanabilmesi gerekmektedir.

## Kod

```php
class Animal {
   public function makeSound() {
      return '...';
   }
}

class Cat extends Animal {
   public function makeSound() {
      return 'Meow';
   }
}

class Dog extends Animal {
   public function makeSound() {
      return 'Woof';
   }
}
```

Yukarıdaki kod, Animal sınıfının makeSound() yöntemini uygulayan Cat ve Dog sınıflarını içerir. Bu sınıflar, makeSound()
yöntemini ezerek kendi özel davranışlarını sağlarlar. Liskov yerine geçme prensibine uygun hale getirmek için, alt
sınıfların üst sınıfların davranışlarını değiştirmeden kullanabilmesi gerektiğinden emin olmalıyız. Yani alt sınıflar,
üst sınıfların yöntemlerini genişletebilir, ancak değiştiremez. Örneğin:

## Kod

```php
class Animal {
   public function makeSound() {
      return '...';
   }
}

class Cat extends Animal {
   public function makeSound() {
      return 'Meow';
   }

   public function sleep() {
      // Kediler uyurken yuvarlanır
   }
}

class Dog extends Animal {
   public function makeSound() {
      return 'Woof';
   }

   public function fetch() {
      // Köpekler top oynamayı sever
   }
}
```

## 4.Interface Segregation Principle (Arayüz Ayrımı Prensibi)

Bir sınıf, kullanmadığı özellikleri içermemelidir. Yani, sınıfın ihtiyaç duymadığı özelliklerin yer aldığı bir arayüzü
uygulamamalıdır.

## Kod

```php
interface Machine {
   public function turnOn();
   public function turnOff();
   public function print($document);
}

class Printer implements Machine {
   public function turnOn() {
      // Yazıcıyı açar
   }

   public function turnOff() {
      // Yazıcıyı kapatır
   }

   public function print($document) {
      // Belgeyi yazdırır
   }
}
```

Yukarıdaki kod, Machine arayüzünü uygulayan Printer sınıfını içerir. Bu sınıf, Machine arayüzündeki tüm yöntemleri
uygular. Ancak, bu sınıfın ihtiyaç duymadığı yöntemler de vardır (örneğin, turnOn() ve turnOff() yöntemleri, bir
yazıcının yazdırma işlemini gerçekleştirirken gerekli değildir. Bu durumda, Arayüz Ayrımı Prensibine uygun hale getirmek
için, Printer sınıfının ihtiyaç duyduğu yöntemleri içeren yeni bir arayüz tanımlayabiliriz:

## Kod

```php
interface Printer {
   public function print($document);
}

interface PowerSwitch {
   public function turnOn();
   public function turnOff();
}

class LaserPrinter implements Printer, PowerSwitch {
   public function turnOn() {
      // Lazer yazıcısını açar
   }

   public function turnOff() {
      // Lazer yazıcısını kapatır
   }

   public function print($document) {
      // Belgeyi yazdırır
   }
}
```

Yukarıdaki kod, Printer ve PowerSwitch arayüzlerini uygulayan LaserPrinter sınıfını içerir. Bu sınıf, ihtiyaç duyduğu
yöntemleri içeren yeni arayüzleri uygular ve bu şekilde Arayüz Ayrımı Prensibine uygun hale getirilir.

## 5.Dependency Inversion Principle (Bağımlılık Tersine Çevirme Prensibi)

Üst seviye modüller, alt seviye modüllere bağımlı olmamalıdır. Bunun yerine, her iki seviye de soyutlamalara (
interfacelere) bağımlı olmalıdır.

## Kod

```php
class Logger {
   public function log($message) {
      // Log mesajını kaydeder
   }
}

class User {
   protected $logger;

   public function __construct(Logger $logger) {
      $this->logger = $logger;
   }

   public function login() {
      // Kullanıcı giriş yapar
      $this->logger->log('User logged in.');
   }

   public function logout() {
      // Kullanıcı çıkış yapar
      $this->logger->log('User logged out.');
   }
}
```

Yukarıdaki kod, Logger sınıfını kullanan User sınıfını içerir. Bu durumda, Bağımlılık Tersine Çevirme Prensibine uygun
değildir, çünkü üst seviye User sınıfı, alt seviye Logger sınıfına doğrudan bağımlıdır. Bu durumu çözmek için, User
sınıfının Logger sınıfına değil, bir interface aracılığıyla bağımlı olması gerekir:

## Kod

```php
interface LoggerInterface
{
    public function log($message);
}

class Logger implements LoggerInterface
{
    public function log($message)
    {
        // Log mesajını kaydeder
    }
}

class User
{
    protected $logger;

    public function __construct(LoggerInterface $logger)
    {
        $this->logger = $logger;
    }

    public function login()
    {
        // Kullanıcı giriş yapar
        $this->logger->log('User logged in.');
    }

    public function logout()
    {
        // Kullanıcı çıkış yapar
        $this->logger->log('User logged out.');
    }
}
```

Yukarıdaki kod, LoggerInterface arayüzünü uygulayan Logger sınıfını kullanan User sınıfını içerir. Bu şekilde, User
sınıfı artık doğrudan bir sınıfa değil, bir interface'e bağımlı hale gelir ve Bağımlılık Tersine Çevirme Prensibine
uygun hale getirilir.