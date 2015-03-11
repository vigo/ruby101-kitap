# Conditional Statements (Koşullar)

Verilen ifadenin testi yapılır, akış **true** ya da **false** durumuna göre seyreder.

## `if` durumu

`if a == b then` dediğimizde, **Eğer a, b'ye eşitse** demiş oluruz.

`if a != b then` dediğimizde, **Eğer a, b'ye eşit değilse** demiş oluruz.

Aynı mantıkta, `a > b` (a, b'den büyükse), `a < b` (a, b'den küçükse), `a >= b` (a, b'en büyük ya da eşitse), `a <= b` (a, b'den küçükse ya da eşitse) şeklinde ilerler.

## Negatiflik Operatörü

`!` işareti önermenin sol tarafında kullanılırsa, negatiflik ya da olumsuzluk kontrolü olduğu anlamındadır.

`if !a == b then puts "a, b'ye eşit değil" end` ya da `if !a > b then puts "a, b'den büyük değil" end` gibi kullanılır.

## Çoklu Kontrol

Eğer önermeler arasında `and` (_ve_), `or` (_veya_) ya da bunların kısa yollarını (_and_ için `&&`, _or_ için `||`) kullanırsak birden fazla şeyi kontrol etmiş oluruz.

```ruby
a = 5
b = 10
if a == 5 && b == 10
  puts "İşleme devam edebiliriz!"
end
```

`if a == 5 && b == 10` yerine `if a == 5 and b == 10` de yazabilirdik.

***

Ruby'nin konuşma diline yakın olması sebebiyle, çok daha anlaşılır kontrol satırları yazmak mümkün. Örneğin, **Eğer, a, 5'e eşitse Merhaba Yaz** demek için ilk akla gelen yöntem:

```ruby
if a == 5
  puts "Merhaba"
end
```

ama bunu çok daha basit hale getirebiliriz:

`puts "Merhaba" if a == 5` Tek satırda, `if`i koşul sonucunda olacak şeyin sağına yazmak yeterlidir.

## `elseif`

Programlama mantığında pek de sevmediğim (_daha düzgün yöntemleri var_) ama bazen mecbur kaldığımız bir durumdur.

```ruby
if a == b
  puts "a, b'ye eşit"
elsif a != b
  puts "a, b'ye eşit değil"
elsif a > b
  puts "a, b'ten büyük"
else
  puts "a, b'den küçük"
```

İlk olarak `a == b` kontrolü yapılır, eğer sonuç `false` ise, `a != b` kontrol edilir, o da `false` ise `a > b` eğer bu da olmazsa en sondaki `else` devreye giriyor.

## `unless`

Bu, aslında `if`in tersi gibi. Daha doğrusu `if not` anlamında. **Eğer a, b'ye eşit değilse** demek için;

```ruby
unless a == b
  puts "Eşit değil"
end
```

Aynı mantıkta `puts "Eşit değil" unless a == b` şeklinde de yazabiliriz. Semantik olarak olumsuzluk kontrolü yaparken `unless` kullanılması önerilir. Kodu okuma ve anlama açısından kolay olması için.

## `while`, `break`, `until`

Tanımlanan önerme `true` olduğu sürece **loop** yani döngü çalıştırma kontrolüdür.

```ruby
i = 0
while i < 5 do
  puts "i = #{i}"
  i += 1
end
```

Eğer `i += 1` yani `i`yi bir arttır, demezsek sonsuz döngüye gireriz. Eğer belli bir anda döngüyü kırmak istersek,

```ruby
i = 0
while i < 5 do
  puts "i = #{i}"
  break if i == 3
  i += 1
end
```

`i` **3** olduğunda loop devre dışı kalır...

Aynı `unless` mantığında, `until` kullanılır loop'larda.

```ruby
i = 0
until i == 10 do
  puts "i = #{i}"
  i += 1
end
```

Yani `i` **10**'a eşit olmadığı sürece bu loop çalışır.

## `case`, `when`
`elseif` yerine kullanılması muhtemel, daha anlaşılır kontrol mekanizmasıdır. Hemen örneğe bakalım:

```ruby
computer = "c64"
year = case computer
  when "c64" then "1982"
  when "c16" then "1984"
  when "amiga" then "1985"
  else
    "Tarih bilinmiyor"
end

puts "#{computer} çıkış yılı #{year}"

# c64 çıkış yılı 1982
```

Yukarıdaki kodu bir ton `if`, `elseif` ile yapmak yerine, `when` ve `then` ile daha anlaşılır hale getirdiğimizi düşünüyorum.

`when`kullanırken **range** (_aralık_) belirmesi de yapma şansı var.

```ruby
student_grade = 8
case student_grade
when 0
  puts "Çok kötü"
when 1..4
  puts "Başarısız"
when 5..7
  puts "İyi"
when 8..9
  puts "Çok İyi"
when 10
  puts "Süper"
end
```

Eğer not **1** ile **4** aralığındaysa (ve dahil ise) ya da **5** ile **7** aralığındaysa gibi bir kontrol ekledik.

## `for` Döngüsü

**1**'den **10**'a kadar (1 ve 10 dahil) bir döngü yapalım:

```ruby
for i in 1..10
  puts "i = #{i}"
end

# i = 1
# i = 2
# i = 3
# i = 4
# i = 5
# i = 6
# i = 7
# i = 8
# i = 9
# i = 10
```

Aynı işi çok daha kolay yapmanın yolunu **5.Bölüm**'de **Iteration** kısmında göreceğiz!

## Ternary Operatörü

Hemen hemen pek çok dilde kullanılan, **Eğer şu doğru ise bu değilse bu** ifadesi için kullanılır.

```ruby
amount = 2
pluralize = amount == 1 ? "apple" : "apples"
puts "#{amount} #{pluralize}."
```

Bu örnekte **Ternary** olarak `amount == 1 ? "apple" : "apples"` kullanılmış, eğer `amount` **1** ise "apple" dönecek, değil ise "apples" dönecek. Yani `pluralize` değişkenine kontrolden dönen atanıyor.

## BEGIN ve END

Ruby'de ilginç bir özellik de, kodun çalışmasından önceye ve sonraya bir ek takabiliyoruz.

```ruby
BEGIN { puts "Kodun başlama saati #{Time.now.to_s}" }

def say_hello(username)
  "Merhaba #{username}"
end

puts say_hello "Uğur"
sleep 5  # zaman farkı için 5 saniye bekle

END { puts "Kodun bitme saati #{Time.now.to_s}" }

# Kodun başlama saati 2014-08-04 09:30:24 +0300
# Merhaba Uğur
# Kodun bitme saati 2014-08-04 09:30:29 +0300
```

