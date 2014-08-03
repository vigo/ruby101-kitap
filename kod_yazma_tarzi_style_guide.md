# Kod Yazma Tarzı (Style Guide)

Aşağıdaki kurallar, kodun doğru çalışmasından ziyade, kullanıcı tarafından doğru okunup algılanması için düşünülmüş, kabul edilmiş kurallardır.

Ruby'e ait resmi bir durum olmasa da, genelde tüm kullanıcılar bu kurallara uymaya çalışır.

Bu kuralları ben [GitHub](https://github.com/styleguide/ruby)'dan aldım.

* **soft-tabs** yani `TAB` karakteri yerine **2 adet space** karakteri ile girinti yapılmalı
* Mümkünse satır uzunluğu **80** karakteri geçmesin!
* Satır sonlarında boş karakter **white-space** bırakmayın!
* Hey `rb` dosyası ya da Ruby kodu içeren dosya boş bir satırla bitsin.
* Operatörler, virgül, iki nokta, noktalı virgül, `{` ve `}` lerin etrafında mutlaka boşluk `space` karakteri olsun!

### Yanlış
```ruby
a=1
a,b=1,3
1>2?true:false;puts "Merhaba"
[1,2,3].each{|n| puts n}
```

### Doğru

```ruby
a = 1
a ,b = 1, 3
1 > 2 ? true : false; puts "Merhaba"
[1, 2, 3].each{ |n| puts n }
```
* Parantez ve Köşeli parantez kullanırken, ne öncesine ne de sonrasına boş karakter `space` koyma!

### Doğru
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
