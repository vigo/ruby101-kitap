# Proc ve Lambda

**Block** kullanımını daha dinamik ve programatik hale getirmek için **Procedures** ya da genel adıyla **Procs**, object-oriented Ruby'nin vazgeçilmezlerindendir.

Bazen, her seferinde aynı bloğu sürekli geçmek gerektiğinde imdadımıza **Proc** yetişiyor. Nasıl mı?

Düşünün ki bir method'unuz (_fonksiyonunuz_) olsun, ve bu method’u dinamik olarak programlayabilseniz?

```ruby
def multiplier(with)
  return Proc.new {|number| number * with }
end
```

Çarpma yaptıran dinamik bir fonksiyon. Sayıyı kaç ile çarpacaksak `with` e o sayıyı geçiyoruz.

```ruby
multiply_with_5 = multiplier(5)
```

Elimizde geçtiğimiz sayıyı **5** ile çarpacak, fonksiyondan türemiş bir fonksiyon oluştu. Eğer `multiply_with_5` acaba neymiş dersek?

```ruby
puts multiply_with_5
# #<Proc:0x007fcc9c94a938@/Users/vigo/Desktop/ruby101_book_tests.rb:2>

puts multiply_with_5.class
# Proc
```

Gördüğünüz gibi elimizde bir adet `Proc` objesi var!. Haydi kullanalım!

```ruby
puts multiply_with_5.call(5)  # 25
puts multiply_with_5.call(10) # 50
```

Bu kadar kasmadan, basit bir method ile yapsaydık:

```ruby
def multiplier(number, with)
  return number * with
end

puts multiplier 5, 5 # 25
```

şeklinde olurdu. Benzer bir örnek daha yapalım:

```ruby
multiplier = Proc.new { |*number|
  number.collect { |n| n ** 2 }
}

multiplier.call(1)       # => [1]
multiplier.call(2,4,6)   # => [4, 16, 36]
multiplier[2,4,6].class  # => Array
```

Az ileride `Array` konusunda göreceğimiz `collect` method'unu kullandık. Bu method ile Array'in her elemanını okuyor ve karesini `n ** 2` alıyoruz. `*` işareti yine Array'de göreceğimiz **splat** özelliği. Yani geçilen parametre grubunu **Array** olarak değerlendir diyoruz.

# Lambda

Python'la uğraşan okurlarımız **Lambda**'ya aşina olabilirler. **Proc** ile ilk göze çarpan fark, argüman olarak geçilen parametrelerin sayısı önemlidir Lambda'da. Aynı **Proc** gibi çalışır.

```ruby
custom_print = lambda { |txt| puts txt }
custom_print.call("Hello") # Hello
```

Eğer **2** parametre geçseydik:

```ruby
custom_print = lambda { |txt| puts txt }
custom_print.call("Hello", "World")

# ArgumentError: wrong number of arguments (2 for 1)
```

Ya da;

```ruby
l = lambda { "Merhaba" }
puts l.call # Merhaba
```

Başka bir kullanım;

```ruby
l = lambda do |user_name|
  if user_name == "vigo"
    "Selam vigo! nasılsın?"
  else
    "Selam sana #{user_name}"
  end
end

puts l.call("vigo")   # Selam vigo! nasılsın?
puts l.call("uğur")   # Selam sana uğur
```

## Proc ve Lambda Farkı

```ruby
def arguments(input)
  one, two = 1, 2
  input.call(one, two)
end

puts arguments(Proc.new{ |a, b, c| puts "#{a} ve #{b} ve #{c.class}" })

# 1 ve 2 ve NilClass

puts arguments(lambda{ |a, b, c| puts "#{a} ve #{b} ve #{c.class}" })

# ArgumentError: wrong number of arguments (2 for 3)
```

Aynı kodu kullandık, **Lambda** yeterli parametre almadığı için hata verdi.

## Proc'u Block'a çevirmek

Elimizdeki **Array** elemanlarının her birini bir fonksiyona tabii tutsak? Dizinin her elemanını ekrana yazdıran bir şey?

```ruby
items = ["Bu bir", "bu iki", "bu üç"]
print_each_element = lambda { |item| puts item }
items.each(&print_each_element)
```

Bu örnekte `items.each(&print_each_element)` satırı Proc'u Block'a çevirdiğimiz yer. `&` işin sırrı. `each`'den gelen eleman, sanki method çağırır gibi `print_each_element` e pas ediliyor, karşılayan da `{` `}` içinde tanımlı kod bloğunu çalıştırıyor.

Keza aynı işi;

```ruby
items = ["Bu bir", "bu iki", "bu üç"]
def print_each_element(item)
  puts item
end
items.each(&method(:print_each_element))
```

şeklinde de yapabilirdik!


