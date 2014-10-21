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

