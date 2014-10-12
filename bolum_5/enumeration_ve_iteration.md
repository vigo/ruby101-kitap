# Enumeration ve Iteration

Sayılabilen nesneler `Enumerator` sınıfından türemişlerdir ve içinde döngü / tekrar yapılabilen nesneler haline geldikleri için de **Iterable** hale gelmişlerdir. Yani ne demek istiyorum?

```ruby
["a", "b", "c"].class      # => Array
["a", "b", "c"].each.class # => Enumerator
```

`["a", "b", "c"]` aslında bir `Array` iken, `#each` method'unu çağırdığımız anda elimideki `Array` birden `Enumerator` haline geldi ve içinde **iterasyon** yapılabilecek yani teker teker dizi içindeki elemanlara erişip istediğimizi yapabilecek bir hale geldi.