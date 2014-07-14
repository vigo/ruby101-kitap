# Değişkenler

Değişken denen şey, yani Variable, nesneye atanmış bir işaretçidir aslında. Ne demiştik? Ruby'de herşey bir nesne yani Object. Bu nesnelere erişmek için aracıdır değişkenler.

![Resim 3.2.1 Obje Hiyeraşisi](../images/ruby-object-hiyerasi.png)

Farklı farklı türleri vardır. Birazdan bunlara değineceğiz. En basit anlamda değişken tanımlak;

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

String içine değişken kullanımı yaptığımız zaman yani;
```ruby
a = 41
puts "Siz tam #{a} yaşındasınız"
```
gibi bir durumda, gördüğünüz gibi `#{a}` şeklinde tekst içinde değişken kullandık. Format olarak Ruby'de, `#{BU KISIMDA KOD ÇALIŞIR}` şeklinde istediğimiz kodu çalıştırma yetkimiz var. Bu işlem sade **çift tırnak** kullanımında geçerlidir.

Aynı kodu tek tırnak kullanarak yapmış olsaydık;
```ruby
a = 41
puts 'Siz tam #{a} yaşındasınız'
```
çıktısı:

    Siz tam #{a} yaşındasınız

olacaktı. **Tek tırnak** içinde bu işlem çalışmaz!


## Local (Bölgesel)
Bölgesel ya da **Yerel** değişkenler, bir **scope** içindeki değişkenlerdir. Scope ne dir? kodun çalıştığı bölge olarak tanımlayabiliriz. Bu tür değişkenler mutlaka küçük harfle ya da `_` (_underscore_) işareti ile başlamalıdır. Kesinlike `@`, `@@` ya da `$` işareti gibi ön ekler alamazlar.

```ruby
out_text = "vigo"
def greet_user(user_name)
  out_text = "Merhaba #{user_name}"
  puts out_text
end

puts out_text       # vigo
greet_user("vigo")  # Merhaba vigo
```

`greet_user` method'undaki (_fonksiyonundaki_) `out_text` aslında `local variable` yani yerel değişken şeklinde çalışmaktadır.


## Global (Genel)
`$` işaretiyle başlayan tüm değişkenler **Global** değişkenlerdir. Kodun herhangi biryerinde kullanılabilir.
```ruby
$today = "Pazartesi"
def greet_user(user_name)
  out_text = "Merhaba #{user_name}, bugün #{$today}"
  puts out_text
end

puts "Bugün günlerden ne? #{$today}"
greet_user("vigo")  # Merhaba vigo
```
Bu örnekteki **Global** değişken `$today` değişkenidir.


## Constants (Sabitler)
Sabit ne dir? Değiştirelemeyendir. Yani bu tür değişkenler, ki bu değişken değildir :), **sabit** olarak adlandırılır. Kural olarak mutlaka **BÜYÜK HARF**'le başlar! Bazen de tamamen büyük harflerden oluşur.

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
**Class**'a ait değişkendir. Dikkat edin burada türeyen birşey yok. `@@` ile başlar.
