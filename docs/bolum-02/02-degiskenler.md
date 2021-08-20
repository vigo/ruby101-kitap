# Değişkenler

Değişken denen şey, yani **Variable**, nesneye atanmış bir işaretçidir aslında. Ne demiştik? Ruby'de her şey bir nesne yani **Object**. Bu nesnelere erişmek için aracıdır değişkenler.

![Resim 3.2.1 Obje Hiyeraşisi](../images/ruby-object-hiyerasi.png)

Farklı farklı türleri vardır. Birazdan bunlara değineceğiz. En basit anlamda değişken tanımlamak;

```ruby
a = 5
user_email = "example@foo.com"
```

şeklinde olur. Yukarıdaki örnekte `a` ve `user_email` değişkenin adıdır. Değeri ise `eşittir` işaretinden sonra gelendir.

Yani yukarıda; `a` ya sayı olarak `5` ve `user_email` e metin olarak `example@foo.com` değerleri atanmıştır.

Ruby **Duck Typing** şeklinde çalışır. Yani atama yapmadan önce ne tür bir değer ataması yapacağımızı belirtmemize gerek yok. Ruby zaten `a = 5` dediğimizde, "Hmmm, bu değer Fixnum türünde" diye değerlendirir.

[Duck Typing](http://en.wikipedia.org/wiki/Duck_typing) demek şudur; Eğer ördek gibi yürüyorsa, ördek gibi ses çıkartıyorsa e o zaman bu bir Ördektir! İngilizcesi;

> When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck

Yani, bir kuş, eğer ördek gibi yürüyorsa, ördek gibi yüzüyorsa ve ördek gibi ses çıkarıyorsa ben buna Ördek derim!

## Metinsel Atamalar ve Tırnak Kullanımı
Yeri gelmişken hızlıca bir konuya değinmek istiyorum. Metinsel değişkenler tanımlarken (**String**) eşitlik esnasında tek ya da çift tırnak işareti kullanabiliriz. Fakat aradaki farkı bilerek kullanmamız gerekir.

String içinde değişken kullanımı yaptığımız zaman yani;

```ruby
a = 41
puts "Siz tam #{a} yaşındasınız"
```

gibi bir durumda, gördüğünüz gibi `#{a}` şeklinde yazı içinde değişken kullandık. Bu kodun çıktısı aşağıdaki gibi olacak. 

    Siz tam 41 yaşındasınız

Format olarak Ruby'de, `#{BU KISIMDA KOD ÇALIŞIR}` şeklinde istediğimiz kodu çalıştırma yetkimiz var. Bu işlem sadece **çift tırnak** kullanımında geçerlidir. Başka bir örnek vermek gerekirse; 

```ruby
a = 41
puts "Siz tam #{a+2} yaşındasınız"
```

Yukarıdaki örnekte Ruby `a+2` komutunu çalıştıracaktır ve sonuç olarak `43` değerini bulacaktır ve bunu ekrana yazdıracak. Yani sonuç:

    Siz tam 43 yaşındasınız

şeklinde olacaktır. Ancak aynı kodu tek tırnak kullanarak yapmış olsaydık;

```ruby
a = 41
puts 'Siz tam #{a} yaşındasınız'
```

çıktısı:

    Siz tam #{a} yaşındasınız

olacaktı. **Tek tırnak** içinde bu işlem çalışmaz!


## Local (Bölgesel)
Bölgesel ya da **Yerel** değişkenler, bir **scope** içindeki değişkenlerdir. Scope nedir? Kodun çalıştığı bölge olarak tanımlayabiliriz. Bu tür değişkenler mutlaka küçük harfle ya da `_` (_underscore_) işareti ile başlamalıdır. Kesinlike `@`, `@@` ya da `$` işareti gibi ön ekler alamazlar.

```ruby
out_text = "vigo"
def greet_user(user_name)
  out_text = "Merhaba #{user_name}"
end

puts greet_user("vigo")  # Merhaba vigo
puts out_text            # vigo
```

Program çalıştığında `out_text` değişkeninin değeri `vigo` olarak atanmaktadır. Daha sonra 6. satırda `greet_user` method'u (_fonksiyonu_) çalıştığında, o methodun içerisinde `out_text` değişkeninin değeri değiştiriliyor gibi görünüyor. Daha sonra 7. satırda `out_text` değişkeninin değeri `puts` methodu ile ekrana yazdırılmaktadır. Ancak burada çıktılara baktığınızda methodun içerisindeki `out_text` değişkenindeki değişim, programın en başında tanımladığımız `out_text` değişkenin değerini etkilememiştir. Method içerisinde kalmıştır. Burada method içerisindeki `out_text` değişkeni aslında `local variable` yani yerel değişken şeklinde çalışmaktadır.


## Global (Genel)
`$` işaretiyle başlayan tüm değişkenler **Global** değişkenlerdir. Kodun herhangi bir yerinde kullanılabilir ve erişilebilir.

```ruby
$today = "Pazartesi"
def greet_user(user_name)
  out_text = "Merhaba #{user_name}, bugün #{$today}"
  puts out_text
end

puts "Bugün günlerden ne? #{$today}"
greet_user("vigo")  # Merhaba vigo, bugün Pazartesi
```

Bu örnekteki **Global** değişken `$today` değişkenidir.


## Constants (Sabitler)
Sabit nedir? Değiştirelemeyendir. Yani bu tür değişkenler, ki bu değişken değildir :), **sabit** olarak adlandırılır. Kural olarak mutlaka **BÜYÜK HARF**'le başlar! Bazen de tamamen büyük harflerden oluşur.

```ruby
My_Age = 18
your_age = 22

puts defined?(My_Age)    # constant
puts defined?(your_age)  # local-variable
```

`My_Age` sabit, `your_age` de yerel değişken...

Ruby'de ilginç bir durum daha var. Constant'lar **mutable** yani değiştirilebilir. Nasıl yani?

```ruby
My_Age = 18

puts defined?(My_Age)    # constant
puts "My_Age: #{My_Age}" # My_Age: 18

My_Age = 22

puts defined?(My_Age)    # constant
puts "My_Age: #{My_Age}" # My_Age: 22
```

ama `warning` yani **uyarı mesajı** aldık!

    untitled:6: warning: already initialized constant My_Age
    untitled:1: warning: previous definition of My_Age was here

`My_Age` sabiti 6.satırda zaten tanımlıydı. Önceki değeri de 1.satırda diye bize ikaz etti Ruby yorumlayıcısı.


## Paralel Atama
Hemen ne demek istediğimi bir örnekle açayım:

```ruby
x, y, z = 5, 11, 88
puts x # 5
puts y # 11
puts z # 88

a, b, c = "Uğur", 5.81, 1972
puts a # Uğur
puts b # 5.81
puts c # 1972
```

`x, y, z = 5, 11, 88` derken tek harekette `x = 5`, `y = 11` ve `z = 88` yaptık. İşte bu paralel atama.


## Instance Variable
**Instance** dediğimiz şey **Class**'dan türemiş olan nesnedir. Bu konuyu detaylı olarak ileride inceleyeceğiz. Sadece ön bilgi olması adına değiniyorum. `@` işareti ile başlarlar.

```ruby
class User
  attr_accessor :name
  def initialize(name)
    @name = name
  end

  def greet
    "Merhaba #{@name}"
  end
end

u = User.new("Uğur")
puts u.greet         # Merhaba Uğur
puts u.name          # Uğur
```

`u.name` diye çağırdığımız şey `User` class'ından türemiş olan `u` nesnesinin **Instance Variable**'ı yani türemiş nesneye ait değişkenidir. Fazla takılmayın, `Class` konusunda bol bol değineceğiz...


## Class Variable
**Class**'a ait değişkendir. Dikkat edin burada türeyen bir şey yok. `@@` ile başlar. Kullanmadan önce bu değişkeni mutlaka `init` etmelisiniz (_Yani ön tanımlama yapmalısınız_)

```ruby
class User
  attr_accessor :name
  @@instance_count = 0   # Kullanmadan önce init ettim
  def initialize(name)
    @name = name
    @@instance_count += 1 # Class'dan her instance oluşmasında sayacı 1 arttırıyorum
  end

  def greet
    "Merhaba #{@name}"
  end

  def self.instance_count # burası öneli
    @@instance_count
  end
end

user1 = User.new("Uğur")
user2 = User.new("Ezel")
user3 = User.new("Yeşim")

puts "Kaç defa User instance'ı oldu? #{User.instance_count}" # Kaç defa User instance'ı oldu? 3
```

Eğer kafanız karıştıysa sorun değil, **Class** konusunda hepsinin üzerinden tekrar geçeceğiz.
