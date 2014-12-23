# Meta Programming

Ruby'deki **Meta Programming**, yazılan kodun **run-time**'da pekçok şeyi değiştirmesi, Kernel'dan gelen sistem fonksiyonlarını manipule etmesi (*Class, Module, Instance ile ilgili şeyler*) şeklindedir.

Hatta bazen yazdığınız programı restart etmeden bile kodu değişikliği yapmak mümkün olur.

Bazı komutlar, kullanm şekilleri gerçekten de çok tehlikeli olabilir! Özellikle dış dünyadan gelecek input'ların run-time'da yorumlanması pek de önerilen bir yöntem değildir. Yani burada göreceğimiz bazı yöntemleri bilelim ama gerçek dünyada pek fazla **uygulamayalım**!

## Class'lar Değiştirilebilir!

İster Kernel ister dışarıdan eklenen, her tür Class modifiye edilebilir:

```ruby
class String
  def foo
    "foo: #{self}"
  end
end


a = "hello"
a.foo # => "foo: hello"
```

`String` Class'ına kafamıza göre `foo` method'u ekledik.

## Class'ların Birden Fazla `initialize` Yöntemi Olabilir!

Bu `Class Overloading` yani Class ya da method'u ezmek olarak düşünülebilir. Class'ın bir tane `initialize` method'u olduğu için, koşullu olarak Class'a başlangıç seviyesinde müdahale edebiliriz:

```ruby
# Dortgen.new([sol_ust_x, sol_ust_y], boy, en)
# Dortgen.new([sol_ust_x, sol_ust_y], [sag_alt_x, sag_alt_y])
class Dortgen
  def initialize(*args)
    if args.size < 2  || args.size > 3
      "Bu sınıf en az 2 en fazla 3 parametre alır"
    else
      "Doğru parametre kullanımı"
    end
  end
end
Dortgen.new([0, 0], 10, 10)     # => #<Dortgen:0x007fe3ea1330f0>
Dortgen.new([0, 0], [10, 10])   # => #<Dortgen:0x007fe3ea132d58>
```

İster 2, ister 3 parametre ile `initialize` ettiğimiz `Dortgen` sınıfı, parametre kullanımına göre farklı çıktılar üretebilir.

## Anonim Class

Anonim Class'lar, **Singleton**, **Ghost** ya da **Metaclass** diye de adlandırılır. Aslında her Ruby Class'ı kendine ait anonim bir sınıfa ve method'lara sahiptir. Tek farkı kendisine ait olmasıdır.

```ruby
class Developer
  class << self
    def personality
      "Awesome"
    end
  end
end

Developer.new         # => #<Developer:0x007fc9048a2738>
Developer.personality # => "Awesome"

a = Developer.new
a                     # => #<Developer:0x007fc9048a2120>
a.class               # => Developer
a.class.personality   # => "Awesome"

a.personality         # => undefined method `personality' for #<Developer:0x007fd3ca0a0e10> (NoMethodError)
```

`Developer` sınıfının kendi `class`'ına anonim bir method taktık. Class'dan instance üretmeden `Developer.personality` şeklinde erişebilirken, `a` instance'ından gitmek istediğimizde yani `a.personality` dediğimizde hata mesajı aldık. Oysa o methods sadece `a.class` a ait :)

Yaptığımız iş aslında bir **Singleton** oluşturma oldu.

**define_method**

Class içinde **run-time** yani dinamik olarak method oluşturabilirsiniz:

```ruby
class Developer
  define_method :personality do |arg|
    "You are #{arg} developer!"
  end
end

Developer.new.personality("an awesome") # => "You are an awesome developer!"
a = Developer.new
a.personality("an awesome") # => "You are an awesome developer!"
a.class.instance_methods(false) # => [:personality]
```

**send**

`send` method'u `Object` sınıfından gelen bir method'dur. Sınıfıa göndereceğimiz mesaj ilk parametre olup bu da aslında çağıracağımız method adıdır.

```ruby
class Developer
  def hello(*args)
    "Hello #{args.join(" ")}"
  end
end
d = Developer.new
d.send(:hello, "vigo", "how are you?") # => "Hello vigo how are you?"
```

Unutmayın, sadece `public` method'lara erişebilirsiniz!

**remove_method** ve **undef_method**

Adından da anlaşılacağı gibi method'u yoketmek için kullanılır ama eğer `remove_method` ile iptal edilmek istenilen method, türediği üst sınıfında var ise ne yazıkki yok edilemez. Bu durumda da `undef_method` devreye girer:

```ruby
class Developer
  def method_missing(m, *args, &block)
    "#{m} is not available!"
  end

  def hello
    "Hello from class Developer"
  end
end

class TurkishDeveloper < Developer
  def hello
    "Hello from class TurkishDeveloper"
  end
end

d = TurkishDeveloper.new
d.hello # => "Hello from class TurkishDeveloper"

class TurkishDeveloper
  remove_method :hello
end
d.hello # => "Hello from class Developer" # üst sınıfta varolduğu için çalıştı!
```

Eğer;

```ruby
class TurkishDeveloper
  undef_method :hello
end
d.hello # => "hello is not available!"
```

yaparsak, method komple uçar ve `method_missing` ile yakaladığımız kod bloğu çalışır.

**eval**

Pekçok programlama dilinde **evaluate** etmekten gelen, yani `String` formundaki metnin çalışabilir kod parçası haline gelmesi olayıdır `eval`:

```ruby
eval("5 + 5")            # => 10
eval('"Hello".downcase') # => "hello"
```

Aslında çok tehlikelidir. Yani programatik hiçbir kontrol olmadan dümdüz metnin **executable** hale getirilmesidir ve hiçbir zaman önerilmez. Güvenlik zafiyeti doğrurabilir.

**instance_eval**

Yazılan kod bloğunu sanki Class'ın bir method'uymuş gibi çalıştırır:

```ruby
class Developer
  def initialize
    @star = 10
  end
end

d = Developer.new
d.instance_eval do
  puts self
  puts @star
end

# #<Developer:0x007fe549a91e70>
# 10
```

ya da;

```ruby
class Developer
end
Developer.instance_eval do
  def who
    "vigo"
  end
end
Developer.who # => "vigo"
```

şeklinde kullanılır. Aynı şekilde sadece `public` olan method'lar için geçerlidir.

**module_eval** ve **class_eval**

İkisi de aynı işi yapar. Dışarıdan Class değişkenlerine erişmek için kullanılır:

```ruby
class Developer
  @@geek_rate = 10
end
Developer.class_eval("@@geek_rate") # => 10
```

Aynı şekilde method tanımlamak için;

```ruby
class Developer
end

Developer.class_eval do
  def who
    "vigo"
  end
end
Developer.new.who # => "vigo"
```

**class_variable_get** ve **class_variable_set**

Class konusunda Class ve Instance Variables arasındaki farkı görmüştük. Bu iki method yardımıyla sınıf değişkenine erişmek ve değerini değiştirmek mümkün:

```ruby
class Developer
  @@geek_rate = 10
end
Developer.class_variable_set(:@@geek_rate, "100") # => "100"
Developer.class_variable_get(:@@geek_rate)        # => "100"
```

**instance_variable_get** ve **instance_variable_set**

Aynı önceki gibi, bu method'lar da sadece Instance Variable için çalışır:

```ruby
class Developer
  def initialize(name, star)
    @name = name
    @star = star
  end
  
  def show
    "Name: #{@name}, Star: #{@star}"
  end
end

d = Developer.new("vigo", 10)
d.instance_variable_get(:@name) # => "vigo"
d.instance_variable_get(:@star) # => 10
d.show # => "Name: vigo, Star: 10"

d.instance_variable_set(:@name, "lego") # => "lego"
d.show # => "Name: lego, Star: 10"
```

**const_get** ve **const_set**

Constant yani sabitleri Class ve Module konusunda görmüştük. `const_set` ile Class'a sabit değer atıyoruz, `const_get` ile de ilgili değeri okuyoruz:

```ruby
class Box
end

Box.const_set("NAME", "web") # => "web"
Box.const_get("NAME")        # => "web"
Box::NAME                    # => "web"

a = Box.new
a.class.constants            # => [:NAME]
a.class::NAME                # => "web"
```


