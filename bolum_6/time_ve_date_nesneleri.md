# Time ve Date Nesneleri

Zaman, tarih ve saat gibi işlemleri yapmak için Ruby ile birlikte gelen `Time` ve `Date` sınıflarından bahsedeceğim. En basit tanımıyla saatin kaç olduğunu ya da `now` yani **şimdi** / **şu anda** yı bulmak için:

## Time

```ruby
Time.now # => 2015-04-28 08:49:20 +0300
```

yapmamız yeterlidir. Saniye bazında ekleme ya da çıkartma yaparak başka zamanları da bulabiliriz. **1 saat önceyi** bulmak için **60 saniye * 60 dakika** yapmamız yeterli.

```ruby
Time.now - (60 * 60) # => 2015-04-28 07:51:01 +0300
```

Hemen Ruby'sel bir hareketle ufak birşey yapalım:

```ruby
class Fixnum
  def seconds
    self
  end
  def minutes
    self * 60
  end
  def hours
    self * 60 * 60
  end
  def days
    self * 60 * 60 * 24
  end
end

# 10 gün sonrayı bulalım
Time.now + 10.days # => 2015-05-08 08:54:02 +0300
```

Saat farkları yüzünden çeşitli **Time Zone**'lar mevcut. An itibariyle (_28 Nisan 2015_) yerel zamana baktığımda:

```ruby
Time.local(2015, 4, 28, 8, 54) # => 2015-04-28 08:54:00 +0300
```

Peki [GMT](http://wwp.greenwichmeantime.com/)'ye göre durum ne?

```ruby
Time.gm(2015, 4, 28, 8, 54)    # => 2015-04-28 08:54:00 UTC
```

Şimdi kendi istediğimiz bir zamanı oluşturalım. Doğduğum yıl ve ayı kullanarak bir zaman oluşturup kabaca hangi güne denk geldiğini bulalım. Sadece **YIL** ve **AY** kullanıyoruz!

```ruby
t = Time.new(1972, 8) # => 1972-08-01 00:00:00 +0300
t.monday?  # => false
t.tuesday? # => true
```

Ruby bize otomatik olarak ayın birini işaret etti ve 1 Ağustos 1972'nin salı gününe denk geldiğini `tuesday?` method'u ile anladık.

@wip
