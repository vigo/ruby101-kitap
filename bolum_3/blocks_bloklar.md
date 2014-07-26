# Blocks (Bloklar)

Blok olayı, bence Ruby'nin en çılgın özelliklerinden bir. Aslında bu konu, neredeyse başlı başına kitap konusu olabilir. Genelde **Block, Proc, Lambda** üçlemesi olarak anlatılır.

Kitabımız 101 yani giriş seviyesinde olduğu için, kafaları minimum karıştırma adına basit şekilde değineceğim.

Blok'lar, genelde **closures** ya da **isimsiz/anonim** fonksiyonlar olarak tanımlanır. Sanki metod içinde başka bir metodu işaret eden ya da değişkenleri metodlar arasında paylaşan dış bir metod gibidirler.

Genelde ya `{` `}` ile ya da `do/end` ile sarmalanmışlardır.

```ruby
family_members = ["Yeşim", "Ezel", "Uğur", "Ömer"]

family_members.each do |member_name|
  puts member_name
end

# Yeşim
# Ezel
# Uğur
# Ömer
```

Aynı kod:

```ruby
family_members = ["Yeşim", "Ezel", "Uğur", "Ömer"]

family_members.each { |member_name| puts member_name }

# Yeşim
# Ezel
# Uğur
# Ömer
```

şeklinde de yazılabilirdi. `do/end` ya da `{}` arasında kalan kısım **Block** kısmıdır.

`family_members` bir **Array** yani dizidir. Eğer `puts family_members.class` dersek bize `Array` oldunu söyler. Array'in `each` metodu bize block işleme şansını sağlar.

Komut satırında `ri Array#each` dersek bize Array'in **each** metoduyla ilgili tüm bilgiler gelir.

`do` komutundan hemen sonra gelen `|member_name|` bizim kafamıza göre tanımladığımız bir değişkendir ve Array'in her elemanı bu değişkene atanır.

**Enumeration** bölümünde bunlarda sıkça bahsedeceğiz. Blockların esas gücü `yield` olayından gelir. Hemen bir örnek verelim:

```ruby
def test_function
  yield
end

test_function {
  puts "Merhaba"
}

# Merhaba

test_function do
  puts "Ben block içinden geliyorum"
end

Ben block içinden geliyorum

test_function do
  [1, 2, 3, 4].each do |n|
    puts "Sayı #{n}"
  end
end

# Sayı 1
# Sayı 2
# Sayı 3
# Sayı 4
```
`test_function` adında bir fonksiyonum var (_yani metodum var_) Hiç parametre almıyor! ama **Block** alıyor. İlkinde **curly brace** ile (_yani_ `{` _ve_ `}`), ikincisinde `do/end` ile, son örnekte `do/end` ile ve iç kısımda başka bir iterasyonla kullandım.

Kabaca, fonksiyona kod bloğu geçtim.

Peki, ya şu şekilde kullansaydım `test_function` ? yani hiçbirşey geçmeden? Alacağım hata mesajı:

    no block given (yield)

olacaktı. Demekki block geçilip geçilmediğimi anlamanın bir yolu var :)

```ruby
def test_function
  if block_given?
    yield
  else
    puts "Lütfen bana block veriniz!"
  end
end
```

`block_given?` ile bu kontrolü yapabiliyoruz. Şimdi biraz daha kafa karıştıralım :)

```ruby
def numerator
  yield 10
  yield 4
  yield 8
end

numerator do |number|
  puts "Geçilen sayı #{number}"
end
```

Örnekte, `yield` block içinden gelen kod bloğunu bir **fonksiyon** gibi çağırıyor, çağırırken de bloğa **parametre** geçiyor. Dikkat ettiyseniz kaç tane `yield` varsa o kadar kez block çağırıldı (_call edildi__)

```ruby
def print_users
  ["Uğur", "Yeşim", "Ezel"].each do |name|
    yield name
  end
end

print_users do |name|
  puts "Kullanıcı Adı: #{name}"
end

# Kullanıcı Adı: Uğur
# Kullanıcı Adı: Yeşim
# Kullanıcı Adı: Ezel
```

Fonksiyon içine fonksiyon geçtik gibi.

