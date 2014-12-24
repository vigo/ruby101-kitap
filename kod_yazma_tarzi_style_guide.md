# Kod Yazma Tarzı (Style Guide)

Aşağıdaki kurallar, kodun doğru çalışmasından ziyade, kullanıcı tarafından doğru okunup algılanması için düşünülmüş, kabul edilmiş kurallardır.

Ruby'e ait resmi bir durum olmasa da, genelde tüm kullanıcılar bu kurallara uymaya çalışır.

Bu kuralları ben [GitHub](https://github.com/styleguide/ruby)'dan aldım.

* **soft-tabs** yani `TAB` karakteri yerine **2 adet space** karakteri ile girinti yapılmalı
* Mümkünse satır uzunluğu **80** karakteri geçmesin!
* Satır sonlarında boş karakter **white-space** bırakmayın!
* Hey `rb` dosyası ya da Ruby kodu içeren dosya boş bir satırla bitsin.
* Operatörler, virgül, iki nokta, noktalı virgül, `{` ve `}` lerin etrafında mutlaka boşluk `space` karakteri olsun!

**Yanlış**

```ruby
a=1
a,b=1,3
1>2?true:false;puts "Merhaba"
[1,2,3].each{|n| puts n}
```

**Doğru**

```ruby
a = 1
a ,b = 1, 3
1 > 2 ? true : false; puts "Merhaba"
[1, 2, 3].each{ |n| puts n }
```
* Parantez ve Köşeli parantez kullanırken, ne öncesine ne de sonrasına boş karakter `space` koyma!

**Doğru**

```ruby
my_method(arg1, arg2).other
[1, 2, 3].length
```

* Ünlemden sonra boş karakter `space` kullanma `!array.include?(element)`
* `when` ve `case` kullanırken girinti durumunu aşağıdaki gibi yap:

```ruby
case
when song.name == "Misty"
  puts "Not again!"
when song.duration > 120
  puts "Too long!"
when Time.now.hour > 21
  puts "It's too late"
else
  song.play
end

kind = case year
       when 1850..1889 then "Blues"
       when 1890..1909 then "Ragtime"
       when 1910..1929 then "New Orleans Jazz"
       when 1930..1939 then "Swing"
       when 1940..1950 then "Bebop"
       else "Jazz"
       end
```

* Method'lar arasında **1 satır boşluk** ver, gerekiyorsa mantıklı bir şekilde içeride ayrım yap!

```ruby
def some_method
  data = initialize(options)

  data.manipulate!

  data.result
end

def some_method
  result
end
```

## Syntax

* Eğer method'a parametre geçiyorsan parantez yaz!

```ruby
def some_method
  # argümansız
end

def some_method_with_arguments(arg1, arg2)
 # argümanlı
end
```

* `for` kullanımına dikkat edin, kafanıza göre heryerde kullanmayın:

```ruby
arr = [1, 2, 3]

# kötü örnek
for i in arr do
  puts i
end

# iyi örnek
arr.each { |i| puts i }
```

* İç içe **ternary** kullanmaktan kaçının! Okunabilirliği azaltıyor!

```ruby
# kötü
some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

# iyi
if some_condition
  nested_condition ? nested_something : nested_something_else
else
  something_else
end
```

* `and` ve `or` yerine `&&` ve `||` kullanın
* Mümkün oldukça `if` leri tek satır şeklinde kullanın:

```ruby
# kötü
if some_condition
  do_something
end

# iyi
do_something if some_condition
```

* `unless` içinde `else` kullanmaktan kaçının:

```ruby
# kötü
unless success?
  puts "hata"
else
  puts "ok"
end

# good
if success?
  puts "ok"
else
  puts "hata"
end
```

* Tek satırlık blok işlerinde `{` `}`, çok satırlık blok işlerinde `do` `end` kullanın.
* `return` kelimesini gerekmedikçe kullanmayın.
* Method'larda parametre olarak `default` değer atarken `=` etrafında **space** kullanın.

```ruby
# kötü
def some_method(arg1=:default, arg2=nil, arg3=[])
  # kod...
end

# iyi
def some_method(arg1 = :default, arg2 = nil, arg3 = [])
  # kod...
end
```

* Varlık operatörü kullanmaktan çekinmeyin!

```ruby
#  eğer isim nil ya da false ise isim değişkenine "vigo" ata
isim ||= "vigo"
```

* **Boolean** değerler için `||=` kullanmayın!

```ruby
# kötü
enabled ||= true

# iyi
enabled = true if enabled.nil?
```

* Parantezli kullanımda method'dan sonra **space** kullanmayın:

```ruby
# kötü
f (3 + 2) + 1

# iyi
f(3 + 2) + 1
```

* **Block** içinde kullanmayacağınız değişken için değer ataması yapmayın:

```ruby
# kötü, k boşa gitti
result = hash.map { |k, v| v + 1 }

# iyi
result = hash.map { |_, v| v + 1 }
```

* Veri tipi kontrolü için `===` kullanmayın! `is_a` ya da `kind_of?` kullanın.

## Naming (İsimlendirmeler)

* Method ve değişken isimleri için `snake_case` kullanın.
* Class ve Modül için `CamelCase` kullanın.
* Constant için `SCREAMING_SNAKE_CASE` kullanın.
* Boolean sonuç dönen method'lar `?` ile bitmeli: `User.is_valid?`
* Tehlike, nesneyi modifiye eden / değiştiren method'lar `!` ile bitmeli! `User.delete!`

## Class

* Singleton tanımlarken `self`kullanın:

```ruby
class TestClass
  # kötü
  def TestClass.some_method
    # kod
  end

  # iyi
  def self.some_other_method
    # kod
  end
end
```

* `private`, `public`, `protected` olan method'larda girintileme method adıyla aynı hizada olsun ve ilgili method'un bir üst satırı boş kalsın:

```ruby
class SomeClass
  def public_method
    # ...
  end

  private
  def private_method
    # ...
  end
end
```

## Exceptions

* Akış kontolü için kullanmayın!

```ruby
# kötü
begin
  n / d
rescue ZeroDivisionError
  puts "0'a bölünme hatası!"
end

# iyi
if d.zero?
  puts "0'a bölünme hatası!"
else
  n / d
end
```

## Diğer

* String'leri concat ederken interpolasyon kullanın:

```ruby
# kötü
email_with_name = user.name + " <" + user.email + ">"

# iyi
email_with_name = "#{user.name} <#{user.email}>"
```

* Gerekmedikçe değer atamalarında tek tırnak `'` kullanmayın, çift tırnağı tercih edin `"`
* String concat işlerinde **Array**'e ekleme tekniğini kullanabilirsiniz, hızlı da olur:

```ruby
  html = ""
  html << "<h1>Page title</h1>"

  paragraphs.each do |paragraph|
    html << "<p>#{paragraph}</p>"
  end
```
