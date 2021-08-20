# Syntax (Söz Dizimi) ve Rezerve Kelimeler

## Syntax (Söz Dizimi)
Genel olarak çok kolay ve anlaşılır bir **syntax**'a sahiptir. Sanki İngilizce okur/konuşur gibi söz dizimi bulunur. Örneğin,

> Eğer a'nın değeri 5'ten büyükse ekrana "Merhaba" yaz

demek istiyorsak;

```ruby
puts "Merhaba" if a > 5
```
şeklinde yazabiliriz.

Diğer dillerden farklı olarak, Ruby'de fonksiyon (method) çağırırken **parantez** kullanmak zorunluğu yoktur. Bu ilk etapta kafa karıştırıcı gibi dursa da, alışınca ne kadar kolay okunabilir olduğunu görüyorsunuz. Mecburi değil, yani parantez kullanmanızda sorun yok. Parantezli kullanım;

```ruby
def greet_user(user_name)
  puts "Merhaba #{user_name}"
end

greet_user("Uğur") # Merhaba Uğur
```

Eğer parantez kullanmazsak;

```ruby
def greet_user user_name
  puts "Merhaba #{user_name}"
end

greet_user "Uğur" # Merhaba Uğur
```

şeklinde olur. Keza, pek çok dilde, fonksiyon eğer bir şey dönerse geriye, mutlaka `return` komutu kullanılır. Ruby'de buna da gerek yok. Çünkü her method (_yani Fonksiyon_) mutlaka default olarak bir şey döner. Hiçbir şey dönmese bile `nil` döner. Bu bakımdan da;

```ruby
def greet_user user_name
  "Merhaba #{user_name}"
end

puts greet_user "Uğur" # Merhaba Uğur
```
şeklinde kullanabiliriz. Korkmayın, kafalar karışmasın. Detaylara ileride gireceğiz.


## Comments (Yorum Satırları)
Her dilde olduğu gibi **Comment out** yani "işaretli kısmı çalıştırma" demek için kullandığımız şey Ruby'de de var.

**Comment** için `#` işareti kullanılıyor. Genelde `line-comment` yani satır bazlı, ve `block-comment` yani kod bloğu bazlı yorum yapma şekilleri var.

```ruby
# Bu satır line-comment, yani tek satırlı yorum

# Bu kısım block-comment
#
# def test_user
#   true
# end

# ya da

=begin
Bu yorum...
Bu da yorum...
Hatta bu da...
=end
```

Gördüğünüz gibi `block-comment` için ilave olarak `=begin` ve `=end` kelimeleri kullanılabiliyor.


## Rezerve Edilmiş Kelimeler
Her dilde olduğu gibi Ruby'de de daha önceden rezerve edilmiş bazı kelimeler bulunmaktadır. Bu kelimeleri değişken ya da method adı olarak kullanamıyoruz. Bu kelimeler Ruby komutları ve özel durumlar için rezerve edilmiş.

| Kelime | Kelime |
| -- | -- |
| BEGIN | next |
| END | nil |
| alias  | not |
| and  | or |
| begin  | redo |
| break  | rescue |
| case  | retry |
| class  | return |
| def  | self |
| defined?  | super |
| do  | then |
| else  | true |
| elsif  | undef |
| end  | unless |
| ensure  | until |
| false  | when |
| for  | while |
| if  | yield |
| in  | \_\_FILE\_\_ |
| module  | \_\_LINE\_\_ |

