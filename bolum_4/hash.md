# Hash

Pek çok dilde **Dictionary** olarak geçen, Array'imsi, hatta **Associative Array** de denir, **Key**-**Value** çifti barındıran yine Array'e benzeyen başka bir taşıyıcıdır. **Key**-**Value** dediğimiz şey ise;

    { key1: "value1", key2: "value2", .... }

şeklindedir. Yukarıdaki örnek, yeni syntax'ı kullanır. Ruby programcılarının alışık olduğu örnek:

    { "key1" => "value1", "key2" => "value2", ... }

ya da

    { :key1 => "value1", :key2 => "value2", ... }

şeklindedir. Hepsi aynı kapıya çıkar... Array'deki sıra (_index_) mantığı burada **key**'ler ile oluyor gibi düşünebilirsiniz. Key'ler **unique**'dir yani bir Hash içinde **2 tane aynı key**'den olamaz.

Hash'de sonuçta bir class olduğu için `new` method'u ile Hash'i oluşturabiliriz;

```ruby
Hash.new # => {}
```

Aynı Array'deki gibi Hash'in de hızlı oluşurma yolu var : `h = {}`. Hemen Hash'in nereden geldiğine bakalım:

```ruby
Hash.class                                  # => Class
Hash.class.superclass                       # => Module
Hash.class.superclass.superclass            # => Object
Hash.class.superclass.superclass.superclass # => BasicObject
Hash.class.superclass.superclass.superclass.superclass # => nil
```

Dikkat ettiyseniz Hash'in bir üst sınıfı **Module**. Aynı Array'deki gibi. Peki bu modüller nelermiş?

```ruby
Hash.included_modules # => [Enumerable, Kernel]
```

Eğer Hash'i oluştururken **default** değer geçersek, tanımsız olak **key** için değer atamış oluruz:

```ruby
h = Hash.new("Tanımsız") # => {}
h[:isim] = "Uğur"        # => "Uğur"
h                        # => {:isim=>"Uğur"}
h[:soyad]                # => "Tanımsız"
h.default                # => "Tanımsız"
```

Olmayan bir key'e ulaşmak istediğimizde `"Tanımsız"` değeri geldi. Eğer bu default değeri atamasaydı ne olacaktı?

```ruby
h = Hash.new               # => {}
h[:isim] = "Uğur"          # => "Uğur"
h[:soyad]                  # => nil
```

`nil` gelecekti. Biraz sonra göreceğimiz `fetch` method'unu lütfen aklınızda tutun!

**Default** değer tanımlama mantığında;

```ruby
h = Hash.new { |hash, key| hash[key] = "User: #{key}" }
h["vigo"]             # => "User: vigo"
h["foobar"]           # => "User: foobar"
h["animal"] = "horse" # => "horse"
h                     # => {"vigo"=>"User: vigo", "foobar"=>"User: foobar", "animal"=>"horse"}
```
bu tarz ilginç bir yöntem de kullanılabilir. Normalde `vigo` key'ine karşılık **value** yok ama `Hash`in `new` method'unda yaptığımız bir blok işlemi ile olmayan `key` için değer ataması yaptığımız gibi key-value ataması da yapabiliyoruz.

## Hash Class Method'ları

## Hash Instance Method'ları



