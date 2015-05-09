# Enumeration ve Iteration

Sayılabilen nesneler `Enumerator` sınıfından türemişlerdir ve içinde döngü / tekrar yapılabilen nesneler haline geldikleri için de **Iterable** hale gelmişlerdir. Yani ne demek istiyorum?

```ruby
["a", "b", "c"].class      # => Array
["a", "b", "c"].each.class # => Enumerator
```

`["a", "b", "c"]` aslında bir `Array` iken, `#each` method'unu çağırdığımız anda elimizdeki `Array` birden `Enumerator` haline geldi ve içinde **iterasyon** yapılabilecek yani teker teker dizi içindeki elemanlara erişip istediğimizi gibi kullanabileceğimiz bir hale geldi.

**each_with_object**, **with_object**

İki method'da aynı işi yapar. Elimizde **Enumerator** varsa, yani bu içinde dolaşılabilen bir nesne ise, bu iterasyona ara elementler takabiliriz.

Bu iki method, Enumerator'deki her elemana verilen şeyi takar. Aşağıdaki örnekte `each_with_object("foo")`, `["a", "b", "c"]` dizisindeki her eleman içindir. Dolayısıyla, bu işlem sonrasında ne olduğunu anlamak için `Enumerator`ü `to_a` method'u ile `Array`e çevirdik.

```ruby
enumerator = ["a", "b", "c"].each
enumerator_with_foo = enumerator.each_with_object("foo")
enumerator_with_foo.to_a # => [["a", "foo"], ["b", "foo"], ["c", "foo"]]
```

Keza, bu durumda `enumerator_with_foo` da `each` method'unu kullanarak, `each_with_object` ile pas edilen nesneye de iterasyon esnasına ulaşabiliyoruz;

```ruby
enumerator = ["a", "b", "c"].each
enumerator_with_foo = enumerator.each_with_object("foo")
enumerator_with_foo.each do |element, obj|
  puts "eleman: #{element}, obj: #{obj}"
end

# eleman: a, obj: foo
# eleman: b, obj: foo
# eleman: c, obj: foo
```

Elimizde `Enumerator` varsa, bir şekilde sıra / index bilgisi de var demektir;

```ruby
sayilar = [1, 2, 3, 4].each
sayilar.next # => 1
sayilar.next # => 2
sayilar.next # => 3
sayilar.next # => 4
sayilar.next # => StopIteration hatası!
```

`next` method'unu kullanarak sonraki elemana ulaşabiliyoruz. Hatırlarsanız, **Array**'ler 0 index'lidir. Elimizdeki Array içinde dolaşırken index kullanmak istersek `each_with_index` method'unu kullanırız;

```ruby
["Uğur", "Yeşim", "Ezel", "Ömer"].each_with_index do |name, index|
  puts "İsim: #{name}, index: #{index}"
end
# İsim: Uğur, index: 0
# İsim: Yeşim, index: 1
# İsim: Ezel, index: 2
# İsim: Ömer, index: 3
```

Keza `for` kullanarak da aşağıdaki gibi bir işlem yapabiliriz;

```ruby
isimler = ["Uğur", "Yeşim", "Ezel", "Ömer"]
for isim in isimler
  puts "isim: #{isim}"
end
# isim: Uğur
# isim: Yeşim
# isim: Ezel
# isim: Ömer
```

**next_values**, **peek**, **peek_values**, **rewind**

Aynı `next` gibi çalışır fakat geriye `Array` döner ve ilgili index pozisyonunu ileri taşır. Sona geldiğinde de `StopIteration` hatası verir (*Exception*)

```ruby
a = [1, 2, 3, 4].each
a.next        # => 1
a.next_values # => [2]
a.next_values # => [3]
```

`peek` ile `next` den sonraki değeri görürüz. Eğer sona gelinmişse yine StopIteration raise edilir.

```ruby
a = [1, 2, 3, 4].each
a.next        # => 1
a.peek        # => 2

a.next        # => 2
a.peek        # => 3

a.next        # => 3
a.peek        # => 4

a.next        # => 4
a.peek        # => StopIteration: iteration reached an end
```

aynı `next_values` gibi `peek_values` de bize Array olarak bilgi verir `next` sonrasında kalan elemanları.

`rewind` ile pozisyonu başa alırız, yani kaydı geri sararız :)

```ruby
a = [1, 2, 3, 4].each
a.next        # => 1
a.next        # => 2
a.next        # => 3
a.rewind      # => #<Enumerator: [1, 2, 3, 4]:each>
a.peek_values # => [1]
a.next        # => 1
```

## Diğer Nesnelerdeki Enumeration Durumları

### Fixnum

**upto**, **downto**, **times**

Bir sayıdan yukarı doğru sayarken `upto`, aşağı doğru sayarken `downto` ve kaç defa aynı işlemi yaparken de `times` kullanırız.

```ruby
# 10'dan 5'e sayıyoruz, 10'da 5'de dahil..
10.downto(5){ |i| puts "Sayı: #{i}" }
# >> Sayı: 10
# >> Sayı: 9
# >> Sayı: 8
# >> Sayı: 7
# >> Sayı: 6
# >> Sayı: 5

# 5'den 10'a sayıyoruz, 10'da 5'de dahil..
5.upto(10){ |i| puts "Sayı: #{i}" }
# >> Sayı: 5
# >> Sayı: 6
# >> Sayı: 7
# >> Sayı: 8
# >> Sayı: 9
# >> Sayı: 10

# 3 defa block içindeki kod çalışsın
# 0'dan 3'e kadar 3 hariç :)
3.times{ |i| puts "#{i}" }
# >> 0
# >> 1
# >> 2
```

### String

Aynı mantıkta `upto` ve `downto` ilginç bir şekilde `String` için de kullanılır. Örneğin A'dan itibaren M'ye kadar demek için:

```ruby
"A".upto("M"){ |s| puts s }
# >> A
# >> B
# >> C
# >> D
# >> E
# >> F
# >> G
# >> H
# >> I
# >> J
# >> K
# >> L
# >> M
```

ya da, "AB", "AC", "AD" gibi sekans olarak gitmek gerektiğinde de;

```ruby
"AB".upto("AE"){ |s| puts s }
# >> AB
# >> AC
# >> AD
# >> AE
```

tam tersi için `downto`kullanılır.

