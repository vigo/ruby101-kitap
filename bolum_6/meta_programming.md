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

wip

## Anonim Class

wip