# Proc ve Lambda

**Block** kullanımını daha dinamik ve programatik hale getirmek için **Procedures** ya da genel adıyla **Procs**, object-oriented Ruby'nin vazgeçilmezlerindendir.

Bazen, her seferinde aynı bloğu sürekli geçmek gerektiğinde imdadımıza **Proc** yetişiyor. Nasıl mı?

Düşününki bir method'unuz (_fonksiyonunuz_) olsun, ve bu metodu dinamik olarak programlayabilseniz?

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

şeklinde olurdu. Neden **Proc** ile yaptık?
