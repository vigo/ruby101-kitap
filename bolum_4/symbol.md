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