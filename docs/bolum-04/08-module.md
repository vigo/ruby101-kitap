# Module

Class’a benzeyen ama Class gibi **instantiate** edilemeyen şeydir modül. Modül
denen şeye Class eklenebilir (*include edilir*) Modülden gelen methodlar artık
ilgili Class’ın methodu haline gelir.

Yani düşünün ki bir Class var, bu Class’ın farklı 2-3 Class’tan özellik
almasını istiyorsunuz. Bunu başarmak için ilgili Class’a o 2-3 Class’ı Modül
olarak ekliyorsunuz!

Module’ler sayesinde **Namespace** ve **Mix-in** fonksiyonalitesi de gelmiş
olur.

Tahmin edebileceğiniz gibi `module` kelimesiyle başlarlar ve aynı Class’larda
olduğu gibi büyük harfle başlayan module adı ya da Namespace tanımlaması
yapılır:

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

`RandomNumbers` adında bir Module yaptık, iki farklı Class’ımız var,
`DiceGame` ve `RaceGame` diye, `include` ile bu Module’ü 2 farklı Class’a
ekledik. Şimdi her iki Class’ın da `generate` adında method’u oldu...

## Namespacing

Module içinde Module tanımlayabilirsiniz. Bu sayede belirlediğiniz Module
tanımı altında başka alt Module’ler ve method’lar ekleyebilir, bu sayede tüm
fonksiyonaliteyi ortak bir isim altından yürütebilirsiniz:

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

Alt Module’e ulaşmak için `::` kullandık. Aynı kodu şu şekilde de
tanımlayabilirdik:

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

Bu sayede, başka bir kütüphaneden gelen Module’e ek Module’ler takma şansınız
olur. Örneğin [Sinatra](http://sinatrarb.com) için ek bir özellik
yapıyorsunuz. Bu durumda;

```ruby
module Sinatra::MyFeature
end
```

Şeklinde kullanabilirsiniz.

## Scope (Kapsama Alanı)

Dikkat ettiyseniz Module’ü kullanırken Class gibi instanciate etmedik. Keza
örnekte `self.fetch_url` diye method tanımlaması yaptık. Aslında burada
**Singleton** gibi kullandık. Örnekte `fetch_url` methodu için scope olarak
`HttpFunctions` vermiş olduk. Yani `fetch_url` sadece
`Framework::HttpFunctions.fetch_url` şeklinde erişilebilir oldu.

## Constants (Sabitler)

Module içinde sabit değer tanımlaması da yapmak mümkündür.

```ruby
module A
  SABIT = 5
end

A::SABIT # => 5
```

Eğer **nested** (*iç içe*) yani Module içinde Module yaparsak, sabitlere
aşağıdaki gibi erişebiliriz:

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

Aynı Class’lardaki gibi `public`, `private` ve `protected` olayı Module’ler
için de geçerlidir.

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

Ruby’de bir Class sadece tek bir Class’tan türeyebildiği için `module` ve
`include` çözümlerinden bahsetmiştik:

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

`Person` modülünden gelen `say_hi` method’una;

```ruby
User.new("Ezel").say_hi # => "Hello Ezel"
```

erişebiliyorsunuz ama ;

```ruby
User.say_hi # => undefined method `say_hi’ for User:Class (NoMethodError)
```

yaptığımızda olmayan bir method çağrımı yapmış oluruz. Eğer `include` yerine
`extend` kullansaydık;

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
user.name # => undefined method `name’ for #<User:0x007f87c39702a0 @name="Yeşim"> (NoMethodError)
```

Çünki `Person`a ait özellikleri eklemek (*include*) yerine **extend**
(*genişletme*) ettik ve;

```ruby
User.say_hi # => "Hello Undefined name"
User.instance_methods - Object.instance_methods # => [] # boş array
```

`say_hi` artık bir **singleton** haline geldi yani **Instance Method**’u olmak
yerine **Class Method**’u oldu. Zaten ilgili sınıfın varolan method’larına
baktığımızda boş `Array` döndüğünü görürüz.

Özetle, `include` ile sanki başka bir sınıftan türer gibi tüm özellikleri
**instance method** olarak alırken, `extend` kullandığımızda direk **Class**
kopyası gibi davranıyor.

Modülden gelen method’ları **Class** içinde **instance method** gibi kullanmak
yerine **singleton method** olarak kullanacağınız zaman extend
kullanabilirsiniz!
