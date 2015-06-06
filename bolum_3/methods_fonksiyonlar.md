# Methods (Fonksiyonlar)

Programlama parçacıklarının ve/veya ifadelerin bir araya toplandığı şeydir method. Aslında lise matematiğinden hepimiz aşinayız.

Bildiğiniz matematik fonksiyonu. Bundan böyle fonksiyon yerine **method** kullanacağım. Çünkü Ruby demek neredeyse **Obje** (_Nesne_) ve **Method** (_Method_) demek.

Önceki konularda gördüğümüz operatörlerin neredeyse hepsi bir method! Hemen **Method Definition**'a yani nasıl method tanımlandığına bir göz atalım.

```ruby
def merhaba
  "Merhaba"
end

merhaba        # => "Merhaba"
```

`def` ve `end` anahtar kelimeleri arasına method’un adı geldi. Önce method’u tanımladık, sonra çağırdık.

Ruby'de herşey mutlaka **geriye birşey döner**. Ne demek bu? Prensip olarak method’lar zincir olarak çalıştığı için, method denen şey de aslında bir fonksiyon ve fonksiyon denen şey de bir dizi işlemin yapılıp geriye sonucun dönüldüğü bir taşıyıcı aslında.

Bir örnek vermek için hemen `irb` ye geçelim:

    irb(main):001:0> puts "Merhaba"
    Merhaba
    => nil
    irb(main):002:0>


`puts "Merhaba"` önce işini yaptı ve çıktı olarak **Merhaba** verdi. sonra `=> nil` dikkatinizi çekti mi?

Çünkü `puts` method’u işini yaptı bitirdi ve geriye `nil` döndü! Peki daha önceki programlama tecrübelerimize dayanak, **geriye döndü** işini hangi komut yapmış olabilir?

Pek çok dilde fonksiyondan birşey geri dönmek için **return** kelimesi kullanılır. Ruby'de de kullanılır ama zorunlu değildir. Yukarıdaki `def merhaba` örneğinde `return` kullanmamamıza rağmen geriye **Merhaba** dönebildi.

Ruby'de methodlar içerisindeki çalıştırılan en son satırın değerini döndürür. Ancak siz daha öncesinde özellikle `return` anahtar kelimesi ile bir sonuç dönmezseniz. Bir örnekle konuyu pekiştirelim.

```ruby
def merhabaBir
  m = "Merhaba"
  n = "Ornek"
end

def merhabaIki
  m = "Merhaba"
  n = "Ornek"
  return "Return Ornek"
end

puts merhabaBir   # Sonuç : Ornek
puts merhabaIki   # Sonuç : Return Ornek
```

İşte bu Ruby'nin özelliği. Kodu okurken bunu bilmezsek kafamız süper karışabilir.

`def` ile tanımlanan method’u, `undef` ile yok edebilirsiniz.

```ruby
def merhaba
  "Merhaba"
end

merhaba # => "Merhaba"

undef merhaba

merhaba # =>
# ~> -:9:in `<main>': undefined local variable or method `merhaba' for main:Object (NameError)
```

Gördüğünüz gibi `undefined local variable or method .. Object (NameError)` hatasını aldık.

Method’lar argüman alabilir. Yani fonksiyona, doğal olarak, parametre geçebilirsiniz.

```ruby
def merhaba(isim)
  "Merhaba #{isim}"
end

merhaba("vigo") # => "Merhaba vigo"
```

aynı örneği aşağıdaki gibi de yazabiliriz:

```ruby
def merhaba isim
  "Merhaba #{isim}"
end

merhaba "vigo" # => "Merhaba vigo"
```

Method’u tanımlarken ve çağırırken **parantez** kullanmadık! Bu alışmanız gereken önemli konulardan birisi. Şahsen ben, daha önce hiçbir programlama dilinde böyle birşey görmedim!

Bazı durumlara, argüman alan method çağırırken, argümanın tipine göre, eğer parantez kullanmadan çağırma yaparsanız **warning** alabilirsiniz!

## Method Yazma Kuralları (_Method Conventions_)

Ruby, pek çok konuda rahat gibi görünse bile bazı kuralları var tabi. Özellikle method’ların son karakteri ile ilgili. Eğer bir method’un son karakteri `?` ise bu o method’un `true` ya da `false` yani **Boolean** bir değer döneceğini ifade eder.

```ruby
a = "ali"
b = "ali"

a.eql? b  # => true
a.eql?(b) # => true
```

`.eql?` method’u eşitliği kontrol eder ve mutlaka sonuç **Boolean** döner.

Eğer method’un son karakteri `!` (_Ünlem_) ise; bu, o method’un tehlikeli bir iş yaptığını anlatır. Yani ilgili nesnenin kopyalanmadan direk üzerinde değişiklik yapacağı anlamına gelir.

```ruby
a = "deneme"

a.upcase   # => "DENEME"
a          # => "deneme"

a.upcase!  # => "DENEME"
a          # => "DENEME"
```

`a` değeri **deneme**. `.upcase` ile orijinal değeri değiştirmeden **uppercase** (_Büyük harf_) yaptık. Değeri kontrol ettiğimizde halen küçük harf olduğunu gördük. `.upcase!` kullandığımız anda değişkenin orijinal değerini de bozduk.

Eğer bir method `=` ile bitiyorsa, bu, o method’un bir **setter** method’u olduğu anlamına gelir ve **Class** ile ilgili bir konudur.

```ruby
class User
  def email=(email)
    @email = email
  end
end

u = User.new
u # => #<User:0x007ff7229ed880>

u.email = "vigo@xyz.com"
u # => #<User:0x007ff7229ed880 @email="vigo@xyz.com">
```

## Varsayılan Argümanlar (_Default Arguments_)

Method argümanlarına varsayılan değerler atayabilirsiniz. Bu, eğer methoda gönderilmesi beklenen argüman gelmemişse otomatik olarak değer atama yapmayı sağlar.

```ruby
def merhaba(isim="insalık!")
  "Merhaba #{isim}"
end

merhaba          # => "Merhaba insalık!"
merhaba("vigo")  # => "Merhaba vigo"
```

Parametre geçmeden çağırdığımızda, tanımladığımız varsayılan (_default_) değer atandı.

## Değişken Sayıda Argümanlar (_Variable-Length Arguments_)

Bazı durumlarda method’a değişken sayıda olarak parametre geçmek gerekebilir. Bu durumda argümanın başına `*` işareti gelir. Bu sayede o argüman artık bir dizi (_Array_) haline gelir.

```ruby
def merhaba(*isimler)
  "Merhaba #{isimler.join(" ve ")}"
end

merhaba("vigo")                        # => "Merhaba vigo"
merhaba("vigo", "yeşim", "ezel")       # => "Merhaba vigo ve yeşim ve ezel"

merhaba "dünya", "uzay", "evren", "ay" # => "Merhaba dünya ve uzay ve evren ve ay"
```

Keza, şu şekilde de kullanılabilir:

```ruby
def custom_numbers(first, second, *others)
  puts "ilk sayı: #{first}"
  puts "ikinci sayı : #{second}"
  puts "diğer sayılar : #{others.join(",")}"
end

custom_numbers 1,2,50,100 # => nil
# >> ilk sayı: 1
# >> ikinci sayı : 2
# >> diğer sayılar : 50,100
```

## Method’a Takma İsim Vermek (_Aliasing_)

Varolan bir method’u, başka bir isimle çağırmak. Bu aslında **Class** konusuyla çok ilintili ama kısaca değinmek istiyorum.

```ruby
def merhaba(isim)
  "Merhaba! #{isim}"
end

alias naber merhaba

merhaba "Uğur" # => "Merhaba! Uğur"
naber "Uğur"   # => "Merhaba! Uğur"
```

Formül şu: `alias` `TAKMA AD` `ORİJİNAL` yani `alias naber merhaba` derken, `merhaba` method’una takma ad olarak `naber`i tanımladık!


### Unutma!

* `return` kullanmadan method’dan geri dönüş yapılabilir
* Parantez kullanmadan method tanımlanabilir
* Parantez kullanmadan method çağırılıp parametre geçilebilir.
* `?` ile biten method mutlaka `true` ya da `false` döner.
* `!` ile biten orijinal değeri mutlaka değiştirir.
* `=` ile biten **setter**'dır.
