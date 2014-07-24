# Operatörler

Operatörler çeşitli kontrolleri yapmak için kullanılır. Hatta bazıları aynı zamanda **method** olarak çalışır. Başı operatörlerin birden farklı işlemi vardır. Örneğin `+` hem matematik işlemi olan **toplama** için hem de **pozitif** değer kontrolü için kullanılabilir.

| Operatör | Açıklama | Methodmu? |
| -- | -- | -- |
| :: | İki tane iki nokta. **Scope resolution** anlamındadır. Class ve Modül konusunda detayları göreceğiz. | |
| [] | Referans, set | Evet |
| []= | Referans, set | Evet |
| ** | Üssü, kuveti | Evet |
| + | Pozitif | Evet |
| - | Negatif | Evet |
| ! | Mantıksal uzlaşma |  |
| ~ | Tamamlayıcı | Evet |
| * | Çarpma | Evet |
| / | Bölme | Evet |
| % | Modülo (_Kalan_) | Evet |
| + | Ekleme | Evet |
| - | Çıkartma | Evet |
| << | Sola kaydır | Evet |
| >> | Sağa kaydır | Evet |
| & | **Bit** seviyesinde **and** (_Ve_) işlemi | Evet |
| &#124; | **Bit** seviyesinde **or** (_Veya_) işlemi | Evet |
| ^ | **Bit** seviyesinde **exclusive or** (_Veya'nın tersi gibi_) işlemi | Evet |
| > | Büyüktür | Evet |
| >= | Büyük ve eşit | Evet |
| < | Küçüktür | Evet |
| <= | Küçük ve eşit | Evet |
| <=> | Eşitlik karşılaştırma operatörü (_Spaceship_ yani uzay gemisi ) | Evet |
| == | Eşitlik | Evet |
| === | Denklik | Evet |
| != | Eşit değil |  |
| !~ | Yakalanmayan (_not match_) |   |
| =~ | Yakalanan (_match_) | Evet |
| && | Mantıksal ve (_and_) |  |
| &#124;&#124; | Mantıksal veya (_or_) |  |
| .. | Range'i kapsayan | Evet |
| ... | Range'i kapsamayan |  |
| ? : | **Ternary** |  |
| = | Atama |  |
| += | Arttırma ve atama |  |
| -= | Eksiltme ve tama |  |
| *= | Çarpma ve atama |  |
| /= | Bölme ve atama |  |
| %= | Modülo ve atama |  |
| **= | Üssü ve atama |  |
| <<= | Bit seviyesinde sola kaydırma ve atama |  |
| >>= | Bit seviyesinde sağa kaydırma ve atama |  |
| &= | Bit seviyesinde **and** ve atma |  |
| &#124;= | Bit seviyesinde **or** ve atama |  |
| ^= | Bit seviyesinde **eor** ve atama |  |
| &&= | Mantıksal **and** ve atama |  |
| &#124;&#124;= | Varlık ataması (_Existential Operator_) | &#20; |

İlk bakışta insanın aklını durduran bir sürü garip işaretler bunlar değil mi? Hemen örneklerle pekiştirelim.

```ruby
a = []
a.class  # => Array
a.length # => 0
```
## `[]=` Kullanım Örneği

```ruby
a = []             # Bu bir dizi / Array
a.[]=5,"Merhaba"   # 0 indekli, 5.eleman Merhaba olsun
p a                # [nil, nil, nil, nil, nil, "Merhaba"]
```

## Unary Operatörleri

**Unary** demek, `+=`, `-=`, `*=` gibi işlemleri yaptığımız operatörler. Yani `x+= 5` dediğimizde (_x'in değerine 5 ekle ve sonucu tekrar x'e ata_) aslında **Unary** operatörü kullanmış oluruz.

