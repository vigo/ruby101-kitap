# Monkey Patching

Ruby'nin en şaibeli özelliklerinden biridir. Kimileri için müthiş bir şey kimileri için de çok tehlikeli bir özelliktir. 7.7'de bahsettiğim **Meta Programming** konusu ile de çok yakından alakalıdır.

Ruby'deki tüm sınıflar açıktır. Yani Kernel'dan gelen herhangi bir sınıfı modifiye etmek mümkündür. Bu durum yanlış ellerde çok tehlikeli olabilir.

Yani, `String` sınıfındaki herhangi bir method'u bozmak mümkündür. Örneğin, `String#length` methodunu değiştirelim:

```ruby
"Hello".length # => 5 # Bu normali

# Monkey Patching yapıyoruz ve length method'unu değiştiriyoruz.
class String
  def length
    "Uzunluk: #{self.size} karakterdir."
  end
end

"Hello".length # => "Uzunluk: 5 karakterdir."
```

Normal şartlar altında `length` methodu `Fixnum` dönmesi gerekirken, bozduğumuz method bize `String` döndü. Anlatabilmek için bu denli abartı bir örnek vermek istedim. Düşünsenize, kullandığınız herhangi bir kütüphane, kafasına göre, standart olan herhangi bir method'u bu şekilde bozsa?

Tüm kodunuz çorbaya döner ve içinden çıkamaz bir hale gelir. Peki asıl kullanım amacı bu mudur? Tabiiki değil. Bize kolaylık sağlayan işlerde kullanmamız gerekiyor.

Örneğin, basit bir matematik işlemi için, **5 kere 5** önermesini kullanmak istiyoruz:

```ruby
class Fixnum
  def kere(n)
    self * n
  end
end
```

`Fixnum` içine `kere` diye bir method taktık. Haydi kullanalım:

```ruby
5.kere(5)         # => 25
5.kere(5).kere(2) # => 50
```

İşte bu tür bir **Monkey Patching** işe yarar ve kullanılabilitesi yüksek olan bir yöntemdir. Keza Ruby on Rails webframework'ü neredeyse bu mantık üzerine kurulmuştur.

Örneğin **5 gün önce** şeklinde bir önerme yapmak istiyoruz.

```ruby
class Fixnum
  def gün
    self * 24 * 60 * 60
  end

  def önce
    Time.now - self
  end

  def sonra
    Time.now + self
  end
end
```

Şimdi şöyle birşey yapalım:

```ruby
Time.now    # => 2015-02-09 12:55:33 +0200
5.gün.önce  # => 2015-02-04 12:55:33 +0200
1.gün.sonra # => 2015-02-10 12:55:33 +0200
```

`Fixnum` yani basit sayılara `.gün.önce` ve `.gün.sonra` gibi iki tane method ekledik :)







