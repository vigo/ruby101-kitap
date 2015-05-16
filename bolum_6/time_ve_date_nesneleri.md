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

Tahmin edebileceğiniz gibi, İngilizce olarak, günleri kontrol edebiliyoruz. Yani `sunday?`, `monday?` ... gibi

Eğer UNIX'in epoch zaman formatında istersek, yapmamız gereken `to_i` method'u ile integer'a çevirmek:

```ruby
Time.new(1972, 8).to_i # => 81464400
```

Eğer elimizde epoch cinsinden bir zaman varsa:

```ruby
Time.at(81464400)      # => 1972-08-01 00:00:00 +0300
Time.at(81464400).year # => 1972
```

şeklinde de kullanabiliriz. Bunlara ek olarak;

```ruby
Time.now.zone # => "EEST"
Time.now.day  # => 29
Time.now.wday # => 3      # çarşamba
Time.now.utc? # => false
Time.now.gmt? # => false
```

Zamanlar arasındaki farkı bulmak için de aynı matematik işlemi gibi yaparmış gibi davranabilirsiniz.

```ruby
birth_day = Time.new(1972, 8)         # Ağustos 1972
Time.now.to_i  # => 1430284388
birth_day.to_i # => 81464400

Time.now.to_i - birth_day.to_i        # => 1348819988 saniye
1348819870 / (60 * 60 * 24)           # => 15611 gün
1348819870 / (60 * 60 * 24 * 30)      # => 520 ay
1348819870 / (60 * 60 * 24 * 30 * 12) # => 43 yıl
```

Ayni şekilde karşılaştırma işlemleri de matematik işlemleri gibi. Doğduğum yıl ile bugünü karşılaştıralım:

```ruby
birth_day = Time.new(1972, 8)
now = Time.now
tomorrow = now + (60 * 60 * 24)

now.to_i                       # => 1430284645
tomorrow.to_i                  # => 1430371045
birth_day.to_i                 # => 81464400

now.to_i > birth_day.to_i      # => true      bugün > doğum tarihi
now.to_i > tomorrow.to_i       # => false     bugün < yarın
tomorrow.to_i > now.to_i       # => true      yarın > bugün
```

## Zamanı Formatlı Şekilde Göstermek

Tarih bilgisini istediğimiz şekilde format ederek çıktı alabiliriz.

```ruby
t = Time.now
t.strftime("Bugün %d %B %Y, %A, saat: %H:%M") # => "Bugün 01 May 2015, Friday, saat: 12:35"
```

Dikkat ettiyseniz İngilizce olarak çıktıyı aldık. Ne yazık ki Ruby'de **locale** kavramı yok. Bu yüzden Türkçe çıktı almak için `I18n` gem'ini kullanmamız gerekiyor.

```bash
gem install i18n
```

Daha sonra herhangi bir dizin altında;

```bash
cd ~
mkdir i18n-works
cd i18n-works/
mkdir locales
cd locales/
curl -O https://raw.githubusercontent.com/svenfuchs/rails-i18n/master/rails/locale/tr.yml
cd ../..
touch run.rb
```

şimdi `run.rb` dosyası içine;

```ruby
require 'i18n'

I18n.load_path = Dir['./locales/*.yml']
I18n.locale = :tr
puts I18n.locale
t = Time.now
puts I18n.localize t, :format => "Bugün %d %B %Y, %A, saat: %H:%M"
```

yaptığımızda çıktı:

```
tr
Bugün 01 Mayıs 2015, Cuma, saat: 12:55
```

şeklinde olacaktır.

Kullanımda: `%<flags><width><modifier><conversion>` şeklinde bir yöntem bulunmakta.

**Flag'ler**

```
-  don't pad a numerical output
_  use spaces for padding
0  use zeros for padding
^  upcase the result string
#  change case
:  use colons for %z
```

Hemen örneklerle görelim, ilk olarak `-`, `_` ve `0` kullanımına bakalım:

```ruby
t = Time.now       # => 2015-05-02 11:35:26 +0300
t.strftime("%d")   # => "02"

# şimdi - ile yapıyoruz
t.strftime("%-d")  # => "2"   # 0'la doldurmadı...

# şimdi _ ile yapıyoruz
t.strftime("%_d")  # => " 2"  # SPACE karakteri ile doldurdu...

# şimdi 0 ile yapıyoruz
t.strftime("%0d")  # => " 02" # 0 ile doldurdu...
```

Şimdi `^`, `#` ve `:` flag'lerine bakalım:

```ruby
t.strftime("%A")        # => "Saturday"
t.strftime("%^A")       # => "SATURDAY" # upcase yaptı
t.strftime("%#A")       # => "SATURDAY" # changecase demek, upcase ise down, downcase ise up yapmak demek.
                        # saturday downcase geldi, upcase oldu
t.strftime("%z")        # => "+0300"
 t.strftime("%:z")      # => "+03:00" # : ile ayırdı
```

**width**

```ruby
t.strftime("%d")   # => "02"
t.strftime("%10d")   # => "0000000002" # 10 basamak yaptı.
```

**Formatlama**

| İşaret | Açıklama |
| -- | -- |
| %Y | 4 dijitli yıl |
| %C | yıl/100, yüzyıl için |
| %y | yıl mod 100, 2015 için **15** gelir. |
| %m | Yılın ayı. (01..12) |
| %B | Ayın tam adı (January) |
| %b ya da %h | Ayın kısa adı (Jan) |
| %d | Ayın günü, 0 eklemeli (01..31) |
| %e | Ayın günü, SPACE karakteri eklemeli ( 1..31) |
| %j | Yılın günü (001..366) |
| %H | Saat, 24-saat formatında 0 eklemeli (00..23) |
| %k | Saat, 24-saat formatında SPACE karakteri eklemeli ( 0..23) |
| %I | Saat, 12-saat formatında 0 eklemeli (01..12) |
| %l | Saat, 12-saat formatında SPACE karakteri eklemeli ( 1..12) |
| %P | Meridyen göstergeci, küçük harf (`am` ya da `pm`) |
| %p | Meridyen göstergeci, büyük harf (`AM` ya da `PM`) |
| %M | Dakika (00..59) |
| %S | Saniye (00..60) |
| %L | Milisaniye (000..999) |
| %N | Kesirli saniye, varsayılan 9 dijitli |
| %z | saat ve dakika ofsetli UTC zaman kuşağı (time zone) |
| %Z | Zaman kuşağının harfsel karşılığı |

**Örnek**

```ruby
t = Time.now        # => 2015-05-16 14:29:43 +0300
t.strftime("%N")    # => "691659000"
t.strftime("%3N")   # => "691" # milisaniye, 3 dijit
t.strftime("%6N")   # => "691659" # mikrosaniye, 6 dijit

t.strftime("%z")    # => "+0300"
t.strftime("%Z")    # => "EEST"
                    #    Eastern European Summer Time yani
                    #    yaz saati :)
```



@wip




