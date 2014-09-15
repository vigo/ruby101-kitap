# Hash

Pek çok dilde **Dictionary** olarak geçen, Array'imsi, hatta **Associative Array** de denir, **Key**-**Value** çifti barındıran yine Array'e benzeyen başka bir taşıyıcıdır. **Key**-**Value** dediğimiz şey ise;

    { key1: "value1", key2: "value2", .... }

şeklindedir. Yukarıdaki örnek, yeni syntax'ı kullanır. Ruby programcılarının alışık olduğu örnek:

    { "key1" => "value1", "key2" => "value2", ... }

ya da

    { :key1 => "value1", :key2 => "value2", ... }

şeklindedir. Hepsi aynı kapıya çıkar... Array'deki sıra (_index_) mantığı burada **key**'ler ile oluyor gibi düşünebilirsiniz. Key'ler **unique**'dir yani bir Hash içinde **2 tane aynı key**'den olamaz.

Hash'de sonuçta bir class olduğu için `new` method'u ile Hash'i oluşturabiliriz;

```ruby
Hash.new # => {}
```

Aynı Array'deki gibi Hash'in de hızlı oluşurma yolu var : `h = {}`. Hemen Hash'in nereden geldiğine bakalım:

```ruby
Hash.class                                  # => Class
Hash.class.superclass                       # => Module
Hash.class.superclass.superclass            # => Object
Hash.class.superclass.superclass.superclass # => BasicObject
Hash.class.superclass.superclass.superclass.superclass # => nil
```

Dikkat ettiyseniz Hash'in bir üst sınıfı **Module**. Aynı Array'deki gibi. Peki bu modüller nelermiş?

```ruby
Hash.included_modules # => [Enumerable, Kernel]
```

Eğer Hash'i oluştururken **default** değer geçersek, tanımsız olak **key** için değer atamış oluruz:

```ruby
h = Hash.new("Tanımsız") # => {}
h[:isim] = "Uğur"        # => "Uğur"
h                        # => {:isim=>"Uğur"}
h[:soyad]                # => "Tanımsız"
h.default                # => "Tanımsız"
```

Olmayan bir key'e ulaşmak istediğimizde `"Tanımsız"` değeri geldi. Eğer bu default değeri atamasaydı ne olacaktı?

```ruby
h = Hash.new               # => {}
h[:isim] = "Uğur"          # => "Uğur"
h[:soyad]                  # => nil
```

`nil` gelecekti. Biraz sonra göreceğimiz `fetch` method'unu lütfen aklınızda tutun!

**Default** değer tanımlama mantığında;

```ruby
h = Hash.new { |hash, key| hash[key] = "User: #{key}" }
h["vigo"]             # => "User: vigo"
h["foobar"]           # => "User: foobar"
h["animal"] = "horse" # => "horse"
h                     # => {"vigo"=>"User: vigo", "foobar"=>"User: foobar", "animal"=>"horse"}
```
bu tarz ilginç bir yöntem de kullanılabilir. Normalde `vigo` key'ine karşılık **value** yok ama `Hash`in `new` method'unda yaptığımız bir blok işlemi ile olmayan `key` için değer ataması yaptığımız gibi key-value ataması da yapabiliyoruz.

## Hash Class Method'ları

Hash'den bir instance oluşturmadan kullandığımız methodlardır.

**Hash[ key, value, ... ] -> yeni_hash**
**Hash[ [ [key, value], ... ] ] -> yeni_hash**
**Hash[ object ] -> yeni_hash**

```ruby
Hash["user_count", 5]                            # => {"user_count"=>5}
Hash[ [["user_count", 5], ["active_users", 2]] ] # => {"user_count"=>5, "active_users"=>2}
Hash["user_count" => 5, "active_users" => 2]     # => {"user_count"=>5, "active_users"=>2}
```

**Hash.new**

Zaten ilgili örnekleri başta vermiştik, tekrar edelim:

```ruby
h = Hash.new
h # => {}
h["user_count"] = 5
h # => {"user_count"=>5}

h = Hash.new { |hash, key| hash[key] = "User ID: #{key}" }
h["1"] # => "User ID: 1"
h["2"] # => "User ID: 2"
h # => {"1"=>"User ID: 1", "2"=>"User ID: 2"}
```

**try_convert(obj) → hash ya da nil**

Hash'e dönüşebilme ihtimali olan nesneyi Hash haline çevirir.

```ruby
Hash.try_convert({"user_count"=>5})   # => {"user_count"=>5}
Hash.try_convert("user_count=>5")     # => nil
```

## Hash Instance Method'ları

Hash instance'ı oluşturduktan sonra kullanacağımız method'lardır.

Önce klasik değer okuma ve değer atama işlerine bakalım. Zaten bu noktaya kadar kabaca biliyoruz nasıl değer atarız geri okuruz. Ama biraz kafaları karıştırmak istiyorum:

```ruby
h = {username: "vigo", password: "1234"} # => {:username=>"vigo", :password=>"1234"}
```

Yukarıdaki gibi bir **Hash**'imiz var. Dikkat ettiyseniz, key,value olarak baktığımızda `:username` ve `:password` diye başlayan `key`ler var... Hatta:

```ruby
h.keys # => [:username, :password]
```

diye de sağlamasını yaparız. Peki, yeni bir `key` tanımlasak? `h["useremail"] = "vigo@example.com"`. Tekrar bakalım `key`lere:

```ruby
h.keys # => [:username, :password, "useremail"]
```

Bir sonraki bölümde karşımıza çıkacak olan **Symbol** tipi ile karşı karşıyayız. Sadece **symbol** mu? hayır, karışık keyler var elimizde. Hemen sağlamasını yapalım:

```ruby
h.keys.map(&:class) # => [Symbol, Symbol, String]
```

İlk iki key **Symbol** iken son key **String** oldu. Demekki Hash içine key cinsi olarak karşık atama yapabiliniyor. Biraz sıkıntılı bir durum ama genel kültür mahiyetinde aklınızda tutun bunu!

Şimdi genel olarak Hash hangi method'lara sahip hemen bakalım:

```ruby
h = Hash.new
h.methods # => [:rehash, :to_hash, :to_h, :to_a, :inspect, :to_s, :==, :[], :hash, :eql?, :fetch, :[]=, :store, :default, :default=, :default_proc, :default_proc=, :key, :index, :size, :length, :empty?, :each_value, :each_key, :each_pair, :each, :keys, :values, :values_at, :shift, :delete, :delete_if, :keep_if, :select, :select!, :reject, :reject!, :clear, :invert, :update, :replace, :merge!, :merge, :assoc, :rassoc, :flatten, :include?, :member?, :has_key?, :has_value?, :key?, :value?, :compare_by_identity, :compare_by_identity?, :entries, :sort, :sort_by, :grep, :count, :find, :detect, :find_index, :find_all, :collect, :map, :flat_map, :collect_concat, :inject, :reduce, :partition, :group_by, :first, :all?, :any?, :one?, :none?, :min, :max, :minmax, :min_by, :max_by, :minmax_by, :each_with_index, :reverse_each, :each_entry, :each_slice, :each_cons, :each_with_object, :zip, :take, :take_while, :drop, :drop_while, :cycle, :chunk, :slice_before, :lazy, :nil?, :===, :=~, :!~, :<=>, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]
```

Dikkat ettiyseniz method'ların bir kısmı **Array** ile aynı çünki ikisi de **Enumerable** modülünü kullanıyor.

Şimdi sıradan başlayalım! Önceki **Array** bölümünde anlattığım ortak method'ları pas geçeceğim!

**rehash**

Hash'e key olarak Array verebiliriz. Yani `h[key] = value` mantığında `key` olarak bildiğiniz Array geçebiliriz.

```ruby
a = [ "a", "b" ]
c = [ "c", "d" ]
h = { a => 100, c => 300 } # => {["a", "b"]=>100, ["c", "d"]=>300}
```
`h` Hash'inin keyleri nedir?

```ruby
h.keys                     # => [["a", "b"], ["c", "d"]]
```

2 key'i var biri `["a", "b"]` ve diğeri `["c", "d"]` nasıl yani?

```ruby
h[a]                       # => 100
h[["a", "b"]]              # => 100
h[c]                       # => 300
h[["c", "d"]]              # => 300
```

Şimdi işeri karıştıralım. `a` Array'inin ilk değerini değiştirelim. Bakalım `h` ne olacak?

```ruby
a[0] = "v"                 # => "v"
a                          # => ["v", "b"]
h[a]                       # => nil ????????
```

`h[a]` patladı? `nil` döndü. İşte şimdi imdadımıza ne yetişecek?

```ruby
h.rehash                   # => {["v", "b"]=>100, ["c", "d"]=>300}
h[a]                       # => 100
```

**to_hash**, **to_h**, **to_a**, **to_s**

Tip dönüştürmeleri için kullanılırlar. `to_h` ve `to_hash` eğer kendisi Hash ise sonuç yine kendisi olur. `to_a` ise Hash'den Array yapmak için kullanılır. Tahmin edeceğiniz gibi `to_s` de String'e çevirmek için kullanılır.

```ruby
h = {:foo => "bar"}
h                                 # => {:foo=>"bar"}
h.to_hash                         # => {:foo=>"bar"}
h.to_h                            # => {:foo=>"bar"}
["foo", "bar"].respond_to?(:to_h) # => true
[[:foo, "bar"]].to_h              # => {:foo=>"bar"}

[["a", 1], ["b", 2]].to_h   # => {"a"=>1, "b"=>2}

h.to_a                            # => [[:foo, "bar"]]
h.to_s                            # => "{:foo=>\"bar\"}"
```

**==** ve **eql?** Eşitlik

Hash içinde key'lerin sırası eşitlik kontrolünde önemli değildir. İçerik önemlidir. Eşitlik kontrolü için kullanılırlar.

```ruby
h1 = { "a" => 100, "c" => 200 }
h2 = { 70 => 350, "x" => 22, "y" => 11 }
h3 = { "y" => 11, "x" => 22, 70 => 350 }

h1 == h2 # => false
h2 == h3 # => true

h1.eql?(h2)  # => false
h2.eql?(h3)  # => true
```

`h2` ile `h3` key sıraları farklı olmasına rağmen içerik bazında eşittirler.

**fetch**

Hash içinden sorgu yaparken kullanılır. Eğer olmayan key çağırırsanız **exception** oluşur. Bu method güvenli bir yöntemdir. Aksi takdirde `nil` döner ve kompleks işlerde **Silent Fail** yani dipsiz kuyuya düşer bir türlü hatanın yerini bulamazsınız!

```ruby
h = {:user => "vigo", :password => "secret"}
puts h.fetch(:user) # "vigo"
puts h.fetch(:email)

KeyError: key not found: :email
```

Keza eğer key'e karşılık yoksa **default** değer ataması yapabilirsiniz:

```ruby
h = {:user => "vigo", :password => "secret"}
h.fetch(:user)               # => "vigo"
h.fetch(:email, "Not found") # => "Not found"
```

**Block** kabul ettiği için artistlik hareketler yapmak da mümkün :)

```ruby
h = {:user => "vigo", :password => "secret"}
h.fetch(:email) { |element| "key: #{element} is not defined!" } # => "key: email is not defined!"
```

**store**

Atama yapmanın farklı bir yöntemidir.

```ruby
h = {:user => "vigo", :password => "secret"}
h.store(:email, "vigo@example.com") # => "vigo@example.com"
h                                   # => {:user=>"vigo", :password=>"secret", :email=>"vigo@example.com"}

# ya da

h[:url] = "http://webbox.io"        # => "http://webbox.io"
h                                   # => {:user=>"vigo", :password=>"secret", :email=>"vigo@example.com", :url=>"http://webbox.io"}
```

**default**, **default=**

Karşılığı olmayan `key`ler için varsayılan değer ataması yapılmışsa bunu bulmak için ya da varsayılan değeri atamak için kullanılır. En başta benzer işler yaptık:

```ruby
h = Hash.new(10)
h[:user_age]            # => 10
h                       # => {}
h.default               # => 10
h.default(:user_weight) # => 10
```

ya da

```ruby
h = Hash.new
h                         # => {}
h.default = 100           # => 100
h[:user_weight]           # => 100
h[:foo]                   # => 100
```

**key**

Value'den key'i bulmak için kullanılır. Eğer key'i olmayan bir value kullanırsanız sonuç `nil` döner!

```ruby
h = {:user => "vigo", :password => "secret"}
h.key("vigo")   # => :user
h.key("foobar") # => nil
```

**size**, **length**, **count**

Aynı işi yaparlar, Arrar gibi Hash'in boyunu / uzunluğunu verir.

```ruby
h = {:user => "vigo", :password => "secret"}
h.length # => 2
h.size   # => 2
h.count  # => 2
```

## Key, Value Kontrolleri

**keys**, **values**, **values_at**

Tahmin edeceğiniz gibi `keys` ile Hash'e ait key'leri, `values` ile sadece key'lere karşılık gelen değerleri, `values_at` ile verdiğimiz key'lere ait değerleri alırız.

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.keys                        # => [:user, :password, :email]
h.values                      # => ["vigo", "secret", "vigo@foo.com"]
h.values_at(:user, :password) # => ["vigo", "secret"]
```

**key?**, **value?**, **has_key?**, **has_value?**

Soru işareti ile biten method'lar bize herzaman **Boolean** yani `true` ya da `false` döner demiştik. Acaba Hash'in içinde ilgili key var mı? ya da value var mı?

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.key?(:user)          # => true
h.has_key?(:user)      # => true

h.key?(:full_name)     # => false
h.has_key?(:full_name) # => false

h.value?("vigo")       # => true
h.has_value?("vigo")   # => true

h.value?("lego")       # => false
h.has_value?("lego")   # => false
```

**include?**, **member?**

`key?` ya da `has_key?` ile aynı işi yapar.

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.include?(:user)        # => true
h.member?(:user)         # => true
```

**empty?**

Hash'in içinde eleman var mı yok mu?

```ruby
{:user => "vigo", :password => "secret", :email => "vigo@foo.com"}.empty? # => false
{}.empty? # => true
```

**all?**, **any?**, **one?**, **none?**

**Array** bölümünde görmüştük, **Enumerable** modülünden gelen bu özellik aynen Hash'de de kullanılıyor. `all?` da tüm elemanlar, verilen koşuldan `nil` ya da `false` dışında birşey dönmek zorunda, aksi halde sonuç `false` oluyor:

```ruby
# value'su boş olan var mı?
{:user => "vigo", :password => "secret", :email => "vigo@foo.com"}.all?{ |k,v| v.empty? } # => false
{:user => "", :password => "", :email => ""}.all?{ |k,v| v.empty? }                       # => true
{:user => "vigo", :password => "", :email => ""}.all?{ |k,v| v.empty? }                   # => false
```

`any?` de içlerinden biri `false` ya da `nil` dönmezse sonuç `true`olur. `one?` da sadece bir tanesi `true` dönmelidir. `none` da ise block'daki işlem sonucu her eleman için `false` olmalıdır.

```ruby
{:is_admin => true, :notifications_enabled => true}.all?{ |option, value| value } # => true
{:is_admin => true, :notifications_enabled => false}.any?{ |option, value| value } # => true
{:is_admin => true, :notifications_enabled => false}.one?{ |option, value| value } # => true
{:is_admin => false, :notifications_enabled => false}.one?{ |option, value| value } # => false
{:is_admin => false, :notifications_enabled => false}.all?{ |option, value| value } # => false
{:is_admin => false, :notifications_enabled => false}.none?{ |option, value| value } # => true
{:is_admin => false, :notifications_enabled => false}.any?{ |option, value| value } # => false
```

**shift**

Hash'den key-value çiftini silmek için kullanılır. Her seferinde ilk key-value çiftini siler.

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.shift # => [:user, "vigo"]
h       # => {:password=>"secret", :email=>"vigo@foo.com"}
h.shift # => [:password, "secret"]
h       # => {:email=>"vigo@foo.com"}
h.shift # => [:email, "vigo@foo.com"]
h       # => {}
```

**delete**, **delete_if**, **keep_if**

Hash'den key kullanarak eleman silmek için `delete` method'u kullanılır.

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.delete(:user) # => "vigo"
h               # => {:password=>"secret", :email=>"vigo@foo.com"}
```

Block kullanıldığında, eğer olmayan bir key kullanılmışsa, bununla ilgili işlem yapmamızı sağlar:

```ruby
h.delete(:phone){ |key| "-#{key}- bulunamadı?" } # => "-phone- bulunamadı?"
```

`delete_if` de ise direk block kullanarak koşullu silme işlemi yapabiliyoruz.

```ruby
# 40'dan büyükleri silelim
h = { point_a: 10, point_b: 20, point_c: 50 } # => {:point_a=>10, :point_b=>20, :point_c=>50}
h.delete_if{ |k,v| v > 40 }                   # => {:point_a=>10, :point_b=>20}
h                                             # => {:point_a=>10, :point_b=>20}
```

`keep_if` ise `delete_if` in tam tersi gibidir. Eğer block'daki koşul `true` ise key-value çiftini tutar, aksi halde siler:

```ruby
# 20'dan küçükleri tutalım sadece
h = { point_a: 10, point_b: 20, point_c: 50 }
                         # => {:point_a=>10, :point_b=>20, :point_c=>50}
h.keep_if{ |k,v| v < 20} # => {:point_a=>10}
h                        # => {:point_a=>10}
```

**invert**

Hash'in key'leri ile value'lerini yer değiştirmek için kullanılır.

```ruby
h = { "a" => 100, "b" => 200 } # => {"a"=>100, "b"=>200}
h.keys                         # => ["a", "b"]
h.invert                       # => {100=>"a", 200=>"b"}
```

**merge**, **update**, **merge!**

İki Hash'i birbiryle birleştirmek için `merge` kullanılır.

```ruby
h1 = { "a" => 100, "b" => 200 }
h2 = { "x" => 1, "y" => 2, "z" => 3 }
h1.merge(h2) # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}
h1           # => {"a"=>100, "b"=>200}
```

Dikkat ettiyseniz `h1` ile `h2` yi birleştirdik ama `h1`in orijinal değerini bozmadık. Eğer bu birleşmenin kalıcı olmasını isteseydik ya `update` ya da `merge!` kullanmamız gerekecekti!

```ruby
h1 = { "a" => 100, "b" => 200 }
h2 = { "x" => 1, "y" => 2, "z" => 3 }
h1.update(h2) # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}
h1            # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}

h1 = { "a" => 100, "b" => 200 }
h2 = { "x" => 1, "y" => 2, "z" => 3 }
h1.merge!(h2) # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}
h1            # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}
```

**replace**

Hash'in içeriğini başka bir Hash ile değiştirmek için kullanılır. Aslında varolan Hash'i başka bir Hash'e çevirmek gibidir. Neden `replace` kullanılıyor? Tamamen hafızadaki adresleme ile ilgili. `replace` kullanıldığı zaman, aynı Hash kullanılıyor, yeni bir Hash instance'ı yaratılmıyor.

```ruby
h1 = { "a" => 100, "b" => 200, "c" => 0 }
h1.__id__ # => 70320602334320
```

Hafızadaki `h1` Hash'nin nesne referansı : 70320602334320. Şimdir `replace` ile değerlerini değiştirelim:

```ruby
h1.replace({ "foo" => 1, "bar" => 2 })
h1        # => {"foo"=>1, "bar"=>2}
h1.__id__ # => 70320602334320
```

Referansları aynı : **70320602334320**. Eğer direkt olarak atama yapsakdık `h1` gibi görünen ama bambaşka yepyeni bir Hash'imiz olacaktı.

```ruby
h1 = { "foo" => 1, "bar" => 2 }
h1.__id__ # => 70216424232360
```

## İterasyon ve Block Kullanımı

Aynı Array'lerdeki gibi Hash'lerde de iterasyon ve block kullanmak mümkün.

**each**, **each_pair**, **each_value**, **each_key**

`each` ve `each_pair` kardeş gibidirler:

```ruby
h = { "a" => 100, "b" => 200, "c" => 0 }
h.each { |key, value| puts "key: #{key}, value: #{value}" }
h.each_pair { |key, value| puts "key: #{key}, value: #{value}" }

# key: a, value: 100
# key: b, value: 200
# key: c, value: 0
```

`each_value` sadece **value**, `each_key` de sadece **key** döner.

```ruby
h = { "a" => 100, "b" => 200, "c" => 0 }

h.each_value { |value| puts "value: #{value}" }

# value: 100
# value: 200
# value: 0

h.each_key { |key| puts "key: #{key}" }

# key: a
# key: b
# key: c
```

**each_entry**, **each_slice**

Hash'deki key-value çifti Array şeklinde bir **entry** olur:

```ruby
h = { "a" => 100, "b" => 200, "c" => 0 }
h.each_entry{ |o| puts "o: #{o}" }

# o: ["a", 100]
# o: ["b", 200]
# o: ["c", 0]
```

`each_slice` ile **entry**'leri parçacıklara ayırırız:

```ruby
h = { "a" => 100, "b" => 200, "c" => 0 }

# 2'li dilimlere ayırdık
h.each_slice(2){ |s| puts "slice: #{s}" }

# slice: [["a", 100], ["b", 200]]
# slice: [["c", 0]]
```




