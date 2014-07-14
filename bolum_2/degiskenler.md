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


## Paralel Atama


## Instance Variable


## Class Variable
