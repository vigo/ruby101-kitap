# Symbol

Symbol (*sembol*) Ruby'e ait özel bir nesnedir. Bir tür **placeholder** (*yer tutucu*) görevindedir. `:` işareti ile başlayan herşey semboldür.

Sembolün, değişkenden en önemli farkı tekil olmasıdır. Yani sembole atanan değişkenden hafızada 1 adet bulunur.

```ruby
user = "vigo"
user.object_id # => 70122132113780

# şimdi başka değer atayalım
user = "bronx"
user.object_id # => 70122132113340
# object_id değişti!
```

Eğer Symbol kullansaydık:

```ruby
user = :vigo
user.object_id # => 420488

user = :bronx
user.object_id # => 420648
```

`user` değişkeninin değeri **Symbol** cinsinden olduğu için, artık hafızada sabit bir yer ayrılmış oldu bu iş için. Değer değişse bile hafızadaki adreslendiği alan değişmemiş oluyor :)

String olarak atanmış değişkeni de Symbol'e çevirmek mümkün:

```ruby
full_name = "Uğur"         # => "Uğur"
full_name.to_sym           # => :Uğur
full_name == :Uğur.id2name # => true
user_full_name = :Uğur     # => :Uğur
user_full_name.object_id   # => 420428

:is_user_admin.id2name     # => "is_user_admin"
:is_user_admin.to_s        # => "is_user_admin"
```

Symbol'ler, değişkenler gibi direkt atama yöntemiyle yani `:a = 1` gibi bir şekilde çalışmazlar. Eğer bir String'den Symbol üretmek isterseniz `to_sym` methodunu kullanmanız gerekiyor.

Hafızayı idareli kullanmak, boşu boşuna değişken kirliği yaratmamak gibi konularda tercih edilir. Keza Hash'lerde de **KEY** ataması Symbol olarak yapılıyor bu tür hız / tasarruf işleri için.

```ruby
{:user=>"vigo"}
```
