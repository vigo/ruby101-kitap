# Class (Sınıf)

Ruby, **Object-Oriented** (*OO*) bir dil olduğu için, methodları değişkenleri ve benzeri şeyleri içinde barından bir tür taşıyıcıya ihtiyaç duyar. İşte bu taşıyıcıya **Class** ya da sınıf diyoruz. Aslında ben **tür** demeyi tercih ediyorum sınıf yerine.

Zaten önceki konularda **Class Methods**, **Instance Methods** gibi kavramlara girmiştik.

Class'lar birbirinden türeyebilir (*Hani* `class.superclass` *şeklinde analizler yapmıştık*)

Teknik olarak bir dosyada birden fazla Class tanımlaması yapılabilir. Örneğin, `my_class.rb` adlı bir dosya içinde farklı farklı Class tanımlamaları olabilir;

```ruby
class MyClass
end

class OtherClass
end

a = MyClass.new    # => #<MyClass:0x007ffa2b09b758>
b = OtherClass.new # => #<OtherClass:0x007ffa2b09b3c0>
```

Class tek başına bir nesne (*Obje*) bu bakımdan **instantiate** etmeseniz bile (*Yani* `a = Array.new` *gibi*) kullanabileceğiniz bir şeydir.

Class'ların diğer bir özelliği de açık olmasıdır. Ruby'nin bence en harika özelliği, **built-in** yani dilin çekirdeğinden gelen Class'lara bile method / property eklemeniz mümkündür. Bu konuları **Monkey Patching** kısmında detaylı göreceğiz.

En basit tanımıyla Class aşağıdaki gibidir:

```ruby
class Merhaba
  def initialize(isim)
    @isim = isim
  end

  def selam_sana
    "Selam sana, #{@isim}"
  end
end

hey = Merhaba.new "Uğur"
hey.selam_sana # => "Selam sana, Uğur"
```

`initialize` methodu, Class'dan ilk örnek türedildiğinde (*yani bir instance oluşturulduğunda*) tetiklenir ve bir tür Class ile ilgili ön tanımlamaların yapıldığı alan konumundadır. Benzer dillerdeki **class constructor**'ı gibi düşünülebilir.

`@isim` ise **Instance Variable** yani Class'tan türeyen nesneye ait değişkendir.

`selam_sana` ise bu Class'ın bir method'udur. `hey` değişkeni, `Merhaba` Class'ından türemiş bir **Instance**'dır. `Merhaba` sınıfındaki tüm method'lar **inherit** yani miras olarak `hey` nesnesine geçmiştir.

Class'ı tanımlarken ister klasik ister block yöntemini kullanabilirsiniz. Klasik yöntem:

```ruby
class Person
end

jack = Person.new # => #<Person:0x007fb0521a4820>
```

Block ise;

```ruby
Person = Class.new do
end

jack = Person.new # => #<Person:0x007fc42c0c8648>
```

şeklindedir. Class isimleri **Büyük** harfle başlar.

## Public Instance Method'ları

Bir Class'tan türeyen şeye **Instance** diyoruz. Ruby'de Class'lar **first-class objects** olarak geçer yani birinci sınıf nesnelerdir. Bu da şu anlama gelir, aslında her Class, Kernel'dan gelen `Class` nesnesinden türemiş alt sınıftır :)

**allocate**, **new** ve **superclass**

`superclass` ilgili Class'ın kimden geldiğini / türediğini gösterir. Benzer örnekleri kitabın başında yapmıştık:

```ruby
String.superclass      # => Object
Object.superclass      # => BasicObject
BasicObject.superclass # => nil
```

`new` ise önce `allocate` method'unu çağırıp hafızada gereken yeri ayırır, yani **instantiate** edeceğimiz Class'ın sınıfını organize eder, sonra oluşacak Class'ın `initialize` method'unu çağırıp varsa ilgili argümanları pas eder. Her `.new` çağırıldığında bu iş olur.

## Private Instance Method'ları

**inherited(subclass)**

İlgili sınıfın alt sınıfı oluşturulduğunda tetiklenir.

```ruby
class Animal
  def self.inherited(subclass)
    puts "Yeni subclass: #{subclass}"
  end
end

class Cat < Animal
end

class Tiger < Cat
end

# Yeni subclass: Cat
# Yeni subclass: Tiger
```

Animal (*hayvan*) sınıfından, Cat (*kedi*) ürettik, Tiger (*kaplan*)'ı da yine Cat (*kedi*)'den ürettik...

## Accessors (getter + setter)

Özel method'lar kullanarak **Meta Programming** mantığıyla Ruby, Instance Variable'ları yönetmeyi kolaylaştırır. Yukarıdaki `Merhaba` Class'ındaki `@isim` için aslında `get`ve `set` yani oku ve yaz method'ları tanımlamamız lazım ki ilgili değişken üzerinde işlem yapabilelim. Önce uzun yolu, sonra doğru ve kısa yolu görelim:

```ruby
class Person
  def name
    @name
  end
  
  def name=(name)
    @name = name
  end
end

vigo = Person.new  # => #<Person:0x007f903b8d0590>
vigo.name          # => nil
vigo.name = "Uğur" # => "Uğur"
vigo.name          # => "Uğur"
```

`name` method'unu çağırınca bize instance variable olan `@name` dönüyor. İlk anda **set** etmediğimiz yani değer atamadığımız için `nil` geliyor. Daha sonra `vigo.name = "Uğur"` diyerek atama yapıyoruz ve artık değerini belirlemiş oluyoruz. Bu iş için 2 tane method yazdık. `name` ve `name=` method'ları.

İşte bu noktada **accessors** imdadımıza yetişiyor:

```ruby
class Person
  attr_accessor :name
end

vigo = Person.new  # => #<Person:0x007fb7c9a4c620>
vigo.name          # => nil
vigo.name = "Uğur" # => "Uğur"
vigo.name          # => "Uğur"
```

`attr_accessor :name` dediğimizde, Ruby, bizim için `name` ve `name=` method'ları oluşturuyor. Keza sadece bununla kalmayıp, pek çok farklı kullanım imkanları sunuyor.

`attr` modülüyle:

* attr
* attr_accessor
* attr_reader
* attr_writer

gibi özel getter/setter'lar geliyor. Yukarıdaki örneği `attr` ile yapalım;

```ruby
class Person
  attr :name, true
end

Person.instance_methods - Object.instance_methods # => [:name, :name=]
```

Otomatik olarak 2 method ekledi : `[:name, :name=]`. Aynı şeyi `attr_accessor :name` ile de yapabilirdik:

```ruby
class Person
  attr_accessor :name
end

Person.instance_methods - Object.instance_methods # => [:name, :name=]
```

Eğer sadece `attr_reader` kullansaydık, sadece ilgili instance variable'ını okuyabilir ama değerini set edemezdik!

```ruby
class Person
  attr_reader :name
end

Person.instance_methods - Object.instance_methods # => [:name]

vigo = Person.new
vigo.name # => nil
vigo.name = "Uğur" # => NoMethodError: undefined method `name=' for #<Person:0x007ffe4d0e8528>
```

Gördüğünüz gibi `NoMethodError` hatası aldık çünki setter yani `name=` method'u oluşmadı! Peki sadece `attr_writer` olsaydı?

```ruby
class Person
  attr_writer :name
end

Person.instance_methods - Object.instance_methods # => [:name=]

vigo = Person.new
vigo.name = "Uğur" # => "Uğur"
vigo.name # => NoMethodError: undefined method ‘name’ for #<Person:0x007fb92b9d45b8 @name="Uğur">
```

Set edebiliyoruz ama get edemiyoruz! Peki `attr_writer` nerede işimize yarar? Örneğin sadece Class'ı initialize ederken değer pas edip sınıf içinde bir değişkene atama yapmak gerektiğinde kullanabilirsiniz:

```ruby
class Person
  attr_writer :name
  
  def initialize(name)
    @name = name
  end
  
  def greet
    "Hello #{@name}"
  end
end

vigo = Person.new "Uğur"
vigo.greet # => "Hello Uğur"
```

`@name` değişkenini sadece ilk tetiklenmede set ediceksem ve dışarıdan okuma ihtiyacım yoksa bu şekilde kullanabilirim!

## Class Variables

`@@` ile başlayan değişkenler Class Variable (*Sınıf Değişkeni*) olarak tanımlanır. Yani Ana Class'a ait bir değişkendir. Her yeni instance oluştuğunda bu değer ait olduğu üst sınıftan erişilebilir:

```ruby
class Person
  attr_accessor :name
  
  @@amount = 0
  def initialize(name)
    @@amount += 1
    @name = name
  end
  
  def greet
    "Hello #{name}"
  end
  
  def how_many_people_created
    "Number of people: #{@@amount}"
  end
end

user1 = Person.new "Uğur"
user2 = Person.new "Yeşim"
user3 = Person.new "Ezel"

Person.class_variable_get(:@@amount) # => 3
user3.how_many_people_created        # => "Number of people: 3"
```

## Class Methods

İlgili Class'dan türetme yapmadan, direk Class'dan çağırılan özel method'dur. Bu method'u çağırmak için sınıftan herhangibir türetme yapmaya gerek olmaz, direkt olarak sınıf'tan çağırılır:

```ruby
class Person
  attr_accessor :name
  
  @@amount = 0
  def initialize(name)
    @@amount += 1
    @name = name
  end
  
  def greet
    "Hello #{name}"
  end
  
  def how_many_people_created
    "Number of people: #{@@amount}"
  end

  def self.how_many_people_created
    "We have #{@@amount} copie(s)"
  end
end

user1 = Person.new "Uğur"
user2 = Person.new "Yeşim"
user3 = Person.new "Ezel"

Person.how_many_people_created # => "We have 3 copie(s)"
```

`Person.how_many_people_created` direkt olarak çağırılır!

## Singletons

Sınıf içinde `class` komutunu kullanarak method oluşturmak içindir. Buna **Singleton** denir. Sadece bir kere **instantiate** (*tetiklenme diyelim*) olur. Örneğin alan hesabı yapacak bir sınıf düşünüyoruz ve bunun `calculate` method'u olsun. En x Boy bize metrekare'yi versin:

```ruby
class Area
 class << self
   def calculate(width, height)
     width * height
   end
 end
end

Area.calculate(5, 5) # => 25
```

Gördüğünüz gibi hiçbir şekilde `new` ya da benzer bir şey türetme kullanmadık direkt olarak `Area.calculate(5, 5)` şeklinde kullandık. Keza aynı işi;

```ruby
class Area
end

x = Area.new
def x.calculate(width, height)
  width * height
end
x.calculate 5,5 # => 25
```

şeklinde de yapabilirdik.

## Inheritance (Miras)

Aslında bu da bildiğimiz bir şey. Sınıftan türeme yaparkan, türettiğimiz sınıfın özellikleri türeyene miras geçer.

```ruby
class Animal
  attr_accessor :name, :kind
  
  def initialize(name)
    @name = name
  end
  
  def say_hi
    "Hello! I'm a #{@kind}, my name is #{@name}"
  end
end

class Cat < Animal
end

class Horse < Animal
end

bidik = Cat.new "Bıdık"
bidik.kind = "cat"

zuzu = Horse.new "Zuzu"
zuzu.kind = "horse"

bidik.say_hi # => "Hello! I'm a cat, my name is Bıdık"
zuzu.say_hi  # => "Hello! I'm a horse, my name is Zuzu"
```

`Cat` ve `Horse` **Animal** sınıfından `<` yöntemiyle türedi ve `Animal` deki tüm method'lar `Cat` ve `Horse`'a geçti.

## Access Level (Erişim): Public, Private, ve Protected Method'lar

Class içindeki method'lar duruma göre erişilebilirlik açısından kısıtlanabilir. `public` olanlar her yerden erişilebilirken (*bu default bir durumdur*), `private` olana sadece içeriden erişilebilir, `protected` olana ise ancak alt sınıftan türeyenden erişilebilir.

```ruby
class User
  def bu_sayede_private_cagirabilirim
    bu_sadece_iceriden
  end

  private
  def bu_sadece_iceriden
    puts "Bu private method. Bu method instance'dan çağırılamaz!"
  end

  protected
  def bu_sadece_subclass_veya_instance_dan
    puts "Bu proteced method."
  end
end

u = User.new
u.bu_sadece_iceriden # => NoMethodError: private method ‘bu_sadece_iceriden’ called for #<User:0x007feb9d0d2560>
```

Gördüğünüz gibi `bu_sadece_iceriden` method'unu `User` dan instantiate ettiğimiz `u` üzeriden çağıramıyoruz. `private` olduğu için ancak içeriden çağırılabilir:

```ruby
u.bu_sayede_private_cagirabilirim # => "Bu private method. Bu method instance'dan çağırılamaz!"
```

`public` olan `bu_sayede_private_cagirabilirim` method'u içeriden `private` method olan `bu_sadece_iceriden` 'e erişebildi. Peki ya `protected` ? Eğer direkt olarak çağırmaya kalksaydık:

```ruby
u.bu_sadece_subclass_veya_instance_dan # => NoMethodError: protected method ‘bu_sadece_subclass_veya_instance_dan’ called for #<User:0x007fff131c60a0>
```

Hemen gerekeni yapalım; `User` Class'ından başka bir Class üretelim:

```ruby
class SuperUser < User
  def initialize
    bu_sadece_subclass_veya_instance_dan
  end
end

y = SuperUser.new # => "Bu proteced method."
```

## Method Aliasing

Bazı durumlarda, üst sınıftaki method'u ezmek gerekir. Bu işlemi yaparken aslında üst sınıftaki orijinal method'a da erişmeniz gerekebilir.

```ruby
class User
  attr_accessor :name
  
  def initialize(name)
    @name = name
  end

  def give_random_age
    (20..45).to_a.sample
  end
end

class SuperUser < User
  alias :yedek :give_random_age # üst sınıftaki give_random_age’i sakladık, yedek adını verdik
  
  def give_random_age
    rnd = self.yedek
    "Kendi yaşım: 43, rnd= #{rnd}"
  end
end

u = User.new "vigo"
u.name            # => "vigo"
u.give_random_age # => 29

v = SuperUser.new "Uğur"
v.give_random_age # => "Kendi yaşım: 43, rnd= 44"
```

Örnekte, `SuperUser` Class'ında kafamıza göre `give_random_age` method'unu ezip kendi işlemimizi yaparken, üst sınıftan miras gelen orijinal method'u da yedekliyoruz, `yedek` adı altında.

## Sınıflar Açıktır, Modifiye Edilebilir!

İster Kernel'dan ister başka bir yerden gelsin, her şekilde Class'lar modifiye edilebilir. Detayları **Monkey Patching**'de göreceğiz. Kısa bir örnek yapalım. `String` Class'ına neşemize göre bir method ekleyelim:

```ruby
class String
  def hello
    "Hello: #{self}"
  end
end


"Deneme".hello # => "Hello: Deneme"
```

Tipi `String` olan herşeyin artık `hello` diye bir method'u oldu :)

## Nested Class

Aynı Module'lerde olduğu gibi, iç içe Class tanımlamak da mümkündür. Kimi zaman düzenli olmak için (*Namespace*) kimi zaman da belli bir kuralı uygulamak için kullanılır;

```ruby
class Animal
  attr_reader :name
  
  def initialize(name)
    @name = name
  end

  class Cat < Animal
  end
  
  class Horse < Animal
  end
  
  class Uber
  end
end

horse = Animal::Horse.new "Furry"
horse.name             # => "Furry"
horse.class            # => Animal::Horse
horse.class.superclass # => Animal

cat = Animal::Cat.new "Bıdık"
cat.name               # => "Bıdık"
cat.class              # => Animal::Cat
cat.class.superclass   # => Animal

alien = Animal::Uber.new
alien.respond_to?(:name) # => false
alien.class            # => Animal::Uber
alien.class.superclass # => Object
```

`Cat` ve `Horse`, `Animal` sınıfından türemiş, `Uber` ise sadece `Animal` namespace'i içinde olup kendi başına bir Class'ı temsil etmektedir.

---

# Module

Class'a benzeyen ama Class gibi **instantiate** edilemeyen şeydir modül. Modül denen şeye Class'e eklenebilir (*include edilir*) Modülden gelen methodlar artık ilgili Class'ın methodu haline gelir.

Yani düşünün ki bir Class var, bu Class'ın farklı 2-3 Class'tan özellik almasını istiyorsunuz. Bunu başarmak için ilgili Class'a o 2-3 Class'ı Modül olarak ekliyorsunuz!

Module'ler sayesinde **Namespace** ve **Mix-in** fonksiyonalitesi de gelmiş olur.

Tahmin edebileceğiniz gibi `module` kelimesiyle başlarlar ve aynı Class'larda olduğu gibi büyük harfle başlayan module adı ya da Namespace tanımlaması yapılır:

```ruby
module RandomNumbers
  def generate
    rand(10)
  end
end

class DiceGame
  include RandomNumbers
end

class RaceGame
  include RandomNumbers
end

g = DiceGame.new
g.generate # => 3

x = RaceGame.new
x.generate # => 7
```

`RandomNumbers` adında bir Module yaptık, iki farklı Class'ımız var, `DiceGame` ve `RaceGame` diye, `include` ile bu Module'ü 2 farklı Class'a ekledik. Şimdi her iki Class'ın da `generate` adında method'u oldu...

## Namespacing

Module içinde Module tanımlayabilirsiniz. Bu sayede belirlediğiniz Module tanımı altında başka alt Module'ler ve method'lar ekleyebilir, bu sayede tüm fonksiyonaliteyi ortak bir isim altından yürütebilirsiniz:

```ruby
module Framework
  module HttpFunctions
    def self.fetch_url
      "This is url fetcher"
    end
  end
end

Framework::HttpFunctions.fetch_url # => "This is url fetcher"
```

Alt Module'e ulaşmak için `::` kullandık. Aynı kodu şu şekilde de tanımlayabilirdik:

```ruby
module Framework
end

module Framework::HttpFunctions
  def self.fetch_url
    "This is url fetcher"
  end
end

Framework::HttpFunctions.fetch_url # => "This is url fetcher"
```

Bu sayede, başka bir kütüphaneden gelen Module'e ek Module'ler takma şansınız olur. Örneğin [Sinatra](http://sinatrarb.com) için ek bir özellik yapıyorsunuz. Bu durumda;

```ruby
module Sinatra::MyFeature
end
```

Şeklinde kullanabilirsiniz.

## Scope (Kapsama Alanı)

Dikkat ettiyseniz Module'ü kullanırken Class gibi instanciate etmedik. Keza örnekte `self.fetch_url` diye method tanımlaması yaptık. Aslında burada **Singleton** gibi kullandık. Örnekte `fetch_url` methodu için scope olarak `HttpFunctions` vermiş olduk. Yani `fetch_url` sadece `Framework::HttpFunctions.fetch_url` şeklinde erişilebilir oldu.

## Constants (Sabitler)

Module içinde sabit değer tanımlaması da yapmak mümkündür.

```ruby
module A
  SABIT = 5
end

A::SABIT # => 5
```

Eğer **nested** (*iç içe*) yani Module içinde Module yaparsak, sabitlere aşağıdaki gibi erişebiliriz:

```ruby
module A
  SABIT = 5
  module B
    def self.sabit_degeri_ver
      SABIT
    end
  end
end

A::SABIT              # => 5
A::B.sabit_degeri_ver # => 5
```

Peki, dışarıda tanımlanmış bir sabit varsa?

```ruby
SABIT = 5 # en dıştaki global
module A
  SABIT = 10 # içerideki
  module B
    def self.sabit_degeri_ver
      "#{::SABIT}, #{SABIT}"
    end
  end
end

A::B.sabit_degeri_ver # => "5, 10"
```

En dıştakini `::SABIT` ile aldık.

## Visibility, Access Level (Erişim):

Aynı Class'lardaki gibi `public`, `private` ve `protected` olayı Module'ler için de geçerlidir.

```ruby
module A
  def sadece_iceriden
    "Bu private method"
  end

  def bu_sayede_private_erisim_olur
    sadece_iceriden
  end

  private :sadece_iceriden
end

class Deneme
  include A
end

c = Deneme.new
c.sadece_iceriden # => NoMethodError: private method ‘sadece_iceriden’ called for #<Deneme:0x007f8f7c9188c8>
c.bu_sayede_private_erisim_olur # => "Bu private method"
```

# Extend ve Include Durumları

Ruby'de bir Class sadece tek bir Class'tan türeyebildiği için `module` ve `include` çözümlerinden bahsetmiştik:

```ruby
module Person
  attr_accessor :name
  def say_hi
    "Hello #{@name}"
  end
end

Person # => Person

class User
  include Person
  def initialize(name)
    @name = name
  end
end

User # => User

u = User.new("Uğur") # => #<User:0x007fcd42976de8 @name="Uğur">
u.say_hi # => "Hello Uğur"

u.name = "vigo"
u.say_hi # => "Hello vigo"
```

`Person` modülünden gelen `say_hi` method'una;

```ruby
User.new("Ezel").say_hi # => "Hello Ezel"
```

erişebiliyorsunuz ama ;

```ruby
User.say_hi # => undefined method `say_hi' for User:Class (NoMethodError)
```

yaptığımızda olmayan bir method çağrımı yapmış oluruz. Eğer `include` yerine `extend` kullansaydık;

```ruby
module Person
  attr_accessor :name
  
  def say_hi
    @name ||= "Undefined name"
    "Hello #{@name}"
  end
end

class User
  extend Person
  
  def initialize(name)
    @name = name
  end
end
```

```ruby
user = User.new("Yeşim") # => #<User:0x007f87c39702a0 @name="Yeşim">
user.name # => undefined method `name' for #<User:0x007f87c39702a0 @name="Yeşim"> (NoMethodError)
```

Çünki `Person`a ait özellikleri eklemek (*include*) yerine extend (*genişletme*) ettik ve;

```ruby
User.say_hi # => "Hello Undefined name"
User.instance_methods - Object.instance_methods # => [] # boş array
```

`say_hi` artık bir singleton haline geldi yani Instance method'u olmak yerine Class method'u oldu. Zaten ilgili sınıfın varolan method'larına baktığımızda boş Array döndüğünü görürüz.

Özetle, `include` ile sanki başka bir sınıftan türer gibi tüm özellikleri **instance method** olarak alırken, `extend` kullandığımızda direk Class kopyası gibi davranıyor.



