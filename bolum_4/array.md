# Array

Kompakt, sıralı objeler içeren bir türü taşıcıyı nesnedir Array'ler. Neredeyse içinde her tür Ruby objesini taşıyabilir. (_String, Integer, Fixnum, Hash, Symbol hatta başka Array'ler vs..._)

Arka arkaya sıralanmış kutucuklar düşünün, her kutucuğun bir **index** numarası olduğunu hayal edin. Bu indeksleme işi otomatik olarak oluyor. Diziye her eleman eklediğinizde (_Eğer kendiniz indeks numarası atamadıysanız_) yeni indeks bir artarak devam ediyor. **Zero Indexed** yani ilk eleman **0**.eleman oluyor.

Aynı **String**'deki gibi negatif indeks değerleri ile tersten erişim sağlıyorsunuz. Yani ilk eleman `array[0]` ve son eleman `array[-1]` gibi...

Array oluşturmak için;

```ruby
a = []  # Ya bu şekilde
a.class # => Array

b = Array.new # => Ya da bu şekilde
b.class       # => Array
```

kullanabilirsiniz. Keza `a = [1, 2, 3]` dediğinizde de Array'i hem tanımlamış hem de elemanlarını belirlemiş olursunuz.

Array'i **initialize** ederken yani ilk kez oluştururken büyüklüğünü de verebilirsiniz.

```ruby
a = Array.new(5)  # içinde 5 eleman taşıyacak Array oluştur
a # => [nil, nil, nil, nil, nil]
```

Hatta, **default** değer bile atarsınız:

```ruby
aylar = Array.new(12, "ay") # 12 eleman olsun ve hepsi "ay" olsun
aylar # => ["ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay"]
```

Ruby'de her nesnenin bir **ID**'si ve **HASH** değeri vardır.

```ruby
[1, 2, 3].hash   # => 3384159031637530117
[1, 2, 3].__id__ # => 70147646473880
```

**Block** kabul ettiği için;

```ruby
dizi = Array.new(10) { |eleman| eleman = eleman * 4 }
dizi # => [0, 4, 8, 12, 16, 20, 24, 28, 32, 36]

# ya da aynı kodu

dizi = Array.new(10) do |eleman|
  eleman = eleman * 4
end
dizi # => [0, 4, 8, 12, 16, 20, 24, 28, 32, 36]
```

şeklinde de kullanabilirsiniz. Neticede Array'in **initialize** method'una parametre geçmiş oluyoruz:

```ruby
aylar = Array.[]("oca", "şub", "mar", "nis", "may", "haz", "tem", "ağu", "eyl", "eki", "kas", "ara")
aylar # => ["oca", "şub", "mar", "nis", "may", "haz", "tem", "ağu", "eyl", "eki", "kas", "ara"]
```

Yani: `aylar = Array.[](param, param, param)` gibi.

Başka nasıl üretiriz? Sayılardan oluşan bir dizi lazım olsa; Örneğin **1984** ile **2000** arasındaki yıllar lazım olsa;

```ruby
years = Array(1984..2000)
years # => [1984, 1985, 1986, 1987, 1988, 1989, 1990, 1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998, 1999, 2000]
```

Bir Array içinde farklı türden elemanlar olabilir;

```ruby
a = ["Hello", :word, 2014, 3.14] # içinde String, Symbol, Fixnum ve Float var!
a # => ["Hello", :word, 2014, 3.14]
```

Array içindeki elemanlar sıralı bir şekilde dururlar. Bu sıraka **Index** denir. **0**'dan başlar. Yani ilk eleman demek Array'in 0.elemanı demektir. İsteğimiz elemanı almak için ya `Array[index]` ya da `Array.fetch(index)` yöntemlerini kullanabiliriz.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a[0]                       # => "Uğur"
a.fetch(0)                 # => "Uğur"

a[4]                       # => nil
a.fetch(4, "Hatalı Index") # => "Hatalı Index"

a = [1, 2, 3, 4]           # İlk N elemanı al
a.take(2)                  # => [1, 2]

a.drop(2) # => [3, 4]      # take'in tersi... İlk 2 haricini al
```

Örnekte `a[4]` dediğimiz zaman, olmayan index'li elemanı almaya çalışıyor ve eleman olmadığı için `nil` geri alıyoruz. `fetch` kullanarak hata kontrolü de yapmış oluyoruz. `nil` yerine belirlediğimiz hata mesajını dönmüş oluyoruz.

`values_at` method'u ile ilgili index ya da index'lerdeki elemanları alabiliriz, keza `at` de benzer işe yarar.

```ruby
isimler = ["Uğur", "Ömer", "Yeşim", "Ezel", "Eren"]
isimler.values_at(0)         # => ["Uğur"]
isimler.values_at(1, 2)      # => ["Ömer", "Yeşim"]

["a", "b", "c", "d", "e"].at(1)  # => "b"
["a", "b", "c", "d", "e"].at(-1) # => "e"

```

`rindex` ile sağdan hizalı index'e göre elemana ulaşıyoruz:

```ruby
a = [ "a", "b", "b", "b", "c" ]

a.rindex("b") # => 3 # 3 tane b var, en sağdaki son b'yi verdi!
a.rindex("z") # => nil
```

Keza "acaba Array'in ne gibi method'ları var?" dersek; hemen `methods` özelliği ile bakabiliriz. Karıştırmamamız gereken önemli bir konu var. Detayını **Class** konusunda göreceğiz ama yeri gelmişken, Array'in **Class Method**'ları ve **Instance Method**'ları var.

`Array.methods` dediğimizde Kernel'dan gelen Array objesinin yani **Class**'ının method'larını görürüz. Eğer `Array.new.methods` dersek, Array'den türettiğimiz **instance**'a ait method'ları görürüz.

Yani `a = []` dediğimizde, aslında **Array**'den bir **instance** çıkartmış oluyoruz. Az önce `Array.[](...)` olarak yaptığımız şey de aslında Class method'u çağırmak.

## Class Method'ları

```ruby
Array.methods # => [:[], :try_convert, :allocate, :new, :superclass, :freeze, :===, :==, :<=>, :<, :<=, :>, :>=, :to_s, :inspect, :included_modules, :include?, :name, :ancestors, :instance_methods, :public_instance_methods, :protected_instance_methods, :private_instance_methods, :constants, :const_get, :const_set, :const_defined?, :const_missing, :class_variables, :remove_class_variable, :class_variable_get, :class_variable_set, :class_variable_defined?, :public_constant, :private_constant, :singleton_class?, :include, :prepend, :module_exec, :class_exec, :module_eval, :class_eval, :method_defined?, :public_method_defined?, :private_method_defined?, :protected_method_defined?, :public_class_method, :private_class_method, :autoload, :autoload?, :instance_method, :public_instance_method, :nil?, :=~, :!~, :eql?, :hash, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]
```

Bu kısımdan en fazla kullanacağımız `[]` ve `new` method'ları olacaktır.

## Instance Method'ları

En çok kullanacağımız method'larsa;

```ruby
Array.new.methods # => [:inspect, :to_s, :to_a, :to_h, :to_ary, :frozen?, :==, :eql?, :hash, :[], :[]=, :at, :fetch, :first, :last, :concat, :<<, :push, :pop, :shift, :unshift, :insert, :each, :each_index, :reverse_each, :length, :size, :empty?, :find_index, :index, :rindex, :join, :reverse, :reverse!, :rotate, :rotate!, :sort, :sort!, :sort_by!, :collect, :collect!, :map, :map!, :select, :select!, :keep_if, :values_at, :delete, :delete_at, :delete_if, :reject, :reject!, :zip, :transpose, :replace, :clear, :fill, :include?, :<=>, :slice, :slice!, :assoc, :rassoc, :+, :*, :-, :&, :|, :uniq, :uniq!, :compact, :compact!, :flatten, :flatten!, :count, :shuffle!, :shuffle, :sample, :cycle, :permutation, :combination, :repeated_permutation, :repeated_combination, :product, :take, :take_while, :drop, :drop_while, :bsearch, :pack, :entries, :sort_by, :grep, :find, :detect, :find_all, :flat_map, :collect_concat, :inject, :reduce, :partition, :group_by, :all?, :any?, :one?, :none?, :min, :max, :minmax, :min_by, :max_by, :minmax_by, :member?, :each_with_index, :each_entry, :each_slice, :each_cons, :each_with_object, :chunk, :slice_before, :lazy, :nil?, :===, :=~, :!~, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]
```
Aynı **String**'deki gibi, şu Array'in bir röntgenini çekelim:

```ruby
Array.class # => Class
Array.class.superclass # => Module
Array.class.superclass.superclass # => Object
Array.class.superclass.superclass.superclass # => BasicObject
Array.class.superclass.superclass.superclass.superclass # => nil
```

Array'in bir üst objesi ne? **Module** Yine **Class** konusunda göreceğiz diyeceğim ve siz bana kızacaksınız :) Ruby'de bir Class en fazla başka bir Class'dan türeyebilir. Örneğin Python'da bir Class N TANE Class'tan **inherit** olabilir (_Miras alabilir, türeyebilir_)

Ruby, bu sorunu **Module** yapısıyla çözüyor. Bu mantıkla aslında ortaklaşa kullanılan Kernel modülleri yardımıyla, ortak kullanılacak method'lar bu modüllerin **Include** edilmesiyle ilgili yerlere dağıtılıyor.

Bu bakımdan Array, Hash gibi nesnelerde benzer ortak method'lar görmek mümkün.

**length** ve ya **count**

Array'in boyu / içinde kaç eleman olduğu ile ilgili bilgiyi almak için kullanılır.

```ruby
[1, 2, 3, 4].length # => 4
[1, 2, 3, 4].count  # => 4
```

**empty?**

Array acaba boşmu? İçinde hiç eleman var mı?

```ruby
[1, 2, 3, 4].empty? # => false
[].empty?           # => true
```

**eql?**

Eşitlik kontrolü içindir. Eğer karşılığı aynı cinsse ve birebir aynı elemanlara sahipse `true` döner.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]

a.eql?(["Yeşim", "Ezel", "Ömer", "Uğur"]) # => false
a.eql?([])                                # => false
a.eql?(["Uğur", "Yeşim", "Ezel", "Ömer"]) # => true
```

**include?** ve **member?**

Acaba verdiğim eleman Array'in içinde mi? Verdiğim eleman bu dizinin üyesi mi?

```ruby
[1, 2, 3, 4].include?(3)                   # => true

[1, 2, 3, 4].member?(1) # => true
[1, 2, 3, 4].member?(5) # => false

["Uğur", "Ezel", "Yeşim"].include?("Uğur") # => true
["Uğur", "Ezel", "Yeşim"].include?("Ömer") # => false
```


**array & başka_bir_array**

İki dizide de kullanın ortak elemanları alır yeni Array döner:

```ruby
a = [1, 2, 3, 4]
b = [3, 1, 10, 22]
a & b # => [1, 3]
```

**array \* int [ya da] array \* str**

```ruby
a = ["a", "b", "c"]
a * 5 # => ["a", "b", "c", "a", "b", "c", "a", "b", "c", "a", "b", "c", "a", "b", "c"]

a * "-vigo-" # => "a-vigo-b-vigo-c"
```
**\*** çaparak **3** elemanlı `a` Array'inden sanki birleştirilmiş **15** elemanlı yeni bir Array oluşturduk. **String** ile çarpınca da aslında `join` methodu ile Array'den String yaptık ve birleştirici olarak **-vigo-** metni kullandık!

**array + başka_array**

İki Array'i toplar ve yeni Array döner:
```ruby
a = ["Uğur", "Yeşim", "Ezel"]
b = ["Ömer"]

a + b # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
```

**array - başka_array**

Array'ler arasındaki farkı Array olarak bulmak:
```ruby
a = ["Uğur", "Yeşim", "Ezel"]
b = ["Uğur", "Yeşim"]

a - b # => ["Ezel"] # a'da olab b elemanları kayboldu
```

**array | başka_array**

İki Array'i **unique** (_tekil_) elemanlar olarak birleştirdi. Aynı eleman varsa bunlardan birini aldı:

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
b = ["Uğur", "Ömer"]

a | b # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
```

**array << nesne** ya da **push**

Array'in sonuna eleman eklemek için kullanılır.

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
a << "Ömer"     # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.push("Eren")  # => ["Uğur", "Yeşim", "Ezel", "Ömer", "Eren"]
```

**concat**

Array sonuna Array eklemek için kullanılır.

```ruby
a = [1, 2, 3]
a.concat([4, 5, 6])
a # => [1, 2, 3, 4, 5, 6]
```

**join**

Array elemanlarını birleştirip **String**'e çevirmeye yarar. Eğer parametre verirses aradaki birleştiriciyi de belirlemiş oluruz.

```ruby
["Commodore 64", "Amiga", "Sinclair", "Amstrad"].join        # => "Commodore 64AmigaSinclairAmstrad"
["Commodore 64", "Amiga", "Sinclair", "Amstrad"].join(" , ") # => "Commodore 64 , Amiga , Sinclair , Amstrad"
```

**unshift**

Array'in başına eleman eklemek için kullanılır.

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
a.unshift("Ömer") # => ["Ömer", "Uğur", "Yeşim", "Ezel"]
```

**insert**

Array'de istediğiniz bir noktaya eleman eklemek için kullanılır. İlk parametre **index** diğer parametre/ler de eklenecek eleman/lar.

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
a.insert(1, "Ömer") # => ["Uğur", "Ömer", "Yeşim", "Ezel"]
a.insert(1, "Ahmet", "Ece", "Eren") # => ["Uğur", "Ahmet", "Ece", "Eren", "Ömer", "Yeşim", "Ezel"]
```

**replace**

Array'in içini, diğer Array'le değiştirir. Aslında Array'i başka bir Array'e eşitlemek gibidir. Eleman sayısının eşit olup olmaması hiç önemli değildir.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.replace(["Foo", "Bar"]) # => ["Foo", "Bar"]
a                         # => ["Foo", "Bar"]
```

**array <=> başka_array**

**Spaceship** operatöründen bahsetmiştik. Array'ler arasında karşılaştırma yapmayı sağlar.

```ruby
[1, 2, 3, 4] <=> [1, 2, 3, 4] # => 0 # Eşit
[1, 2, 3, 4] <=> [1, 2, 3]    # => 1 # İlk değer büyük
[1, 2, 3] <=> [1, 2, 3, 4]    # => -1 # İlk değer küçük
```

**pop**, **shift**, **delete**, **delete_at**, **delete_if**

Son elemanı çıkartmank için **pop** ilk elemanı çıkartmak için **shift** kullanılır. Herhangibir elemanı çıkartmak için **delete**, belirli bir index'deki elemanı çıkartmak için **delete_at** kullanılır.

```ruby
a = ["Uğur", "Ömer", "Yeşim", "Ezel", "Eren"]
a.pop              # => "Eren"
a                  # => ["Uğur", "Ömer", "Yeşim", "Ezel"]
a.shift            # => "Uğur"
a                  # => ["Ömer", "Yeşim", "Ezel"]
a.delete("Ömer")   # => "Ömer"
a                  # => ["Yeşim", "Ezel"]
a.delete_at(1)     # => "Ezel"
a                  # => ["Yeşim"]

# not 50'den küçükse sil :)
notlar = [40, 45, 53, 70, 99, 65]
notlar.delete_if { |notu| notu < 50 } # => [53, 70, 99, 65]
```

**pop**'a parametre geçersek **son n** taneyi uçurmuş oluruz:

```ruby
a = ["Uğur", "Ömer", "Yeşim", "Ezel", "Eren"]
a.pop(2) # => ["Ezel", "Eren"]
a        # => ["Uğur", "Ömer", "Yeşim"]
```


**compact** ve **uniq**

`nil` elemanları uçurmak için **compact**, duplike elemanları tekil hale getirmek için **uniq** kullanılır.

```ruby
["a", 1, nil, 2, nil, "b", 1, "a"].compact         # => ["a", 1, 2, "b", 1, "a"]
["a", 1, nil, 2, nil, "b", 1, "a"].uniq            # => ["a", 1, nil, 2, "b"]
["a", 1, nil, 2, nil, "b", 1, "a"].compact.uniq    # => ["a", 1, 2, "b"]
```

**array == başka_array**

İki Array nitelik ve nicelik olarak birbirine eşit mi?

```ruby
[1, 2, 3, 4] == [1, 2, 3, 4]   # => true
[1, 2, 3, 4] == ["1", 2, 3, 4] # => false
[1, 2, 3, 4] == [1, 2, 3]      # => false
```

**assoc** ve **rassoc**

Elemanları Array olan bir Array içinde, ilk değere göre yakalama yapmaya yarar.

```ruby
a = ["renkler", "kırmızı", "sarı", "mavi"]
b = ["harfler", "a", "b", "c"]
c = "foo"

t  = [a, b, c]
t                 # => [["renkler", "kırmızı", "sarı", "mavi"], ["harfler", "a", "b", "c"], "foo"]
t.assoc("renkler") # => ["renkler", "kırmızı", "sarı", "mavi"]
t.assoc("foo")     # => nil

t.rassoc("kırmızı")   # => ["renkler", "kırmızı", "sarı", "mavi"]
```

`rassoc` ise ikinci elemanına bakar, yani "renkler" yerine "kırımızı" kullanabiliriz:

**slice(başlangıç, boy) ya da slice(aralık)**

Array içinden kesip başka bir Array oluşturmak için kullanılır. **başlangiç** indeks'indeki eleman dahil olmak üzere, boy ya da aralık kadarını kes.

```ruby
[1, 2, 3, 4].slice(0, 2) # => [1, 2] # 0.dan itibaren 2 tane
[1, 2, 3, 4].slice(2..4) # => [3, 4] # 2.den itibaren 2 tane
```

**first** ve **last**

Adından da anlaşılacağı gibi, Array'in ilk ve son elemanları için kullanılır:

```ruby
a = [1, 2, 3, 4, 5]
a.first # => 1
a.last # => 5
```

Eğer parametre geçersek, **ilk n** ya da **son n** elemanları alabiliriz:

```ruby
a = [1, 2, 3, 4, 5]
a.first(2) # => [1, 2]
a.last(2)  # => [4, 5]
```

**find** (**detect**), **find_all**, **index**, **find_index**

`find` ile blok içinde koşula uyan ilk Array elemanını, `find_all` ile tümünü alırız:

```ruby
["Uğur", "Yeşim", "Ezel", "Ömer"].find { |n| n.length > 3 } # => "Uğur"
["Uğur", "Yeşim", "Ezel", "Ömer"].find_all { |n| n.length > 3 } # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
```
`detect` ile `find` aynı işi yapar.

`index`, `find_index` ile elemanın index'ini buluruz:

```ruby
["a", "b", "c", "d", "e"].index("e")                 # => 4
["Uğur", "Yeşim", "Ezel", "Ömer"].index("Ezel")      # => 2
["Uğur", "Yeşim", "Ezel", "Ömer"].find_index("Ezel") # => 2
```

**clear**

Array'i temizlemek için kullanılır :)

```ruby
a = [1, 2, 3]
a.clear # => []
a       # => []
```

**reverse**

Array'i terse çevir.

```ruby
a = [1, 2, 3, 4, 5]
a.reverse # => [5, 4, 3, 2, 1]
```

**sample**

Array'den **random** olarak eleman almaya yarar. Parameter geçilirse geçilen adet kadar random eleman döner.

```ruby
a = [1, 2, 3, 4, 5]
a.sample    # => 3
a.sample(3) # => [5, 1, 3]
```

**shuffle**

Array'in içindeki elemanların index'lerini karıştırı :)

```ruby
a = [1, 2, 3, 4, 5]
a.shuffle # => [5, 4, 1, 3, 2]
a.shuffle # => [1, 2, 3, 5, 4]
```

**sort**

Array içindeki elemanları `<=>` mantığıyla sıralar.

```ruby
a = [1, 4, 2, 3, 11, 5]
a.sort # => [1, 2, 3, 4, 5, 11]

b = ["a", "c", "b", "z", "d"]
b.sort # => ["a", "b", "c", "d", "z"]
```

**fill**

Array'in içini ilgili değerle doldurmak için kullanılır. İşlem sonucunda orijinal Array'in değeri değişir. Yani ne ile **fill** ettiyseniz Array artık o değerlerdedir.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.fill("x")       # => ["x", "x", "x", "x"] # tüm elemanları x yaptı
a                 # => ["x", "x", "x", "x"] # artık a'nın yeni değeri bu

a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.fill("y", 2)    # => ["Uğur", "Yeşim", "y", "y"] # 2.den itibaren y ile doldur

a = ["Uğur", "Yeşim", "Ezel", "Ömer"] # 2.den itibaren 1 tane doldur
a.fill("z", 2, 1) # => ["Uğur", "Yeşim", "z", "Ömer"]
```

Keza;

```ruby
a = [1, 2, 3, 4, 5]
a.fill { |i| i * 5 }     # => [0, 5, 10, 15, 20]
a                        # => [0, 5, 10, 15, 20]
```

şeklinde de kullanılır.

**flatten**

Array içinde Array elemanları varsa, tek harekette bunları düz tek bir Array haline getirebiliriz.

```ruby
[1, 2, ["a", "b", :c], [66, [5.5, 3.1]]].flatten # => [1, 2, "a", "b", :c, 66, 5.5, 3.1]
```

**rotate**

Array elemanları kendi içinde kaydırır.

```ruby
a = [1, 2, 3, 4, 5]

a.rotate    # => [2, 3, 4, 5, 1] # 1 kaydırdı
a.rotate(2) # => [3, 4, 5, 1, 2] # 2 kaydırdı, ilk 2 elemanı sona koydu!
```

Varsayılan değer **1**'dir.

**zip**

```ruby
a = [ 4, 5, 6 ]
b = [ 7, 8, 9 ]

[1, 2, 3].zip(a, b)   # => [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
[1, 2].zip(a, b)      # => [[1, 4, 7], [2, 5, 8]]
a.zip([1, 2], [8])    # => [[4, 1, 8], [5, 2, nil], [6, nil, nil]]
```

`[1, 2, 3].zip(a, b)` işlemini yaparken, önce 0.elemanı yani **1**'i aldı, sonra **a**'nun 0.elemanını aldı, sonra da **b**'nin 0.elemanını aldı ve paketledi : `[1, 4, 7]` aynı işi 1. ve 2. elemanlar için yaptı.

`[1, 2].zip(a, b)` yaparken, Array boyları eşit olmadığı için `[1, 2]` sadece 2 elemanlı olduğu için bu işlemi 0. ve 1. elemanlar için yaptı.

Son örnekte index'e karşılık gelmediği için elemanlar `nil` geldi!

**transpose**

Array içindeki Array'leri satır gibi düşünüp bunları sütuna çeviriyor gibi algılayabilirsiniz.

```ruby
a = [[1, 2], [3, 4], [5, 6]]
a.transpose   # => [[1, 3, 5], [2, 4, 6]]

# [
#   [1, 2],
#   [3, 4],
#   [5, 6]
# ]
# -> [1, 3, 5], [2, 4, 5]
#
```

## Tip Çeviricileri

`to_a` ve `to_ary` kendisini döner, asıl görevi eğer alt sınıftan çağrılmışsa, yani Array'den türeyen başka bir Class'da kullanıldığında direk Array'e dönüştürür.

```ruby
["a", 1, "b", 2].to_a       # => ["a", 1, "b", 2]
["a", 1, "b", 2].to_ary     # => ["a", 1, "b", 2]
[["a", 1], ["b", 2]].to_h   # => {"a"=>1, "b"=>2}
["a", 1, "b", 2].to_s       # => "[\"a\", 1, \"b\", 2]"
["a", 1, "b", 2].inspect    # => "[\"a\", 1, \"b\", 2]"
```

## Iterasyon ve Block Kullanımı

**collect / map { |eleman| blok } → yeni_array**

Blok içinde gelen kodu her elemana uygular, yeni Array döner:

```ruby
a = [1, 2, 3, 4, 5]
a.collect { |i| i * 2 }       # => [2, 4, 6, 8, 10]
a.collect { |i| "sayı #{i}" } # => ["sayı 1", "sayı 2", "sayı 3", "sayı 4", "sayı 5"]
```

`map` de aynı işi yapar:

```ruby
["Uğur", "Yeşim", "Ezel", "Ömer"].map { |isim| "İsim: #{isim}" } # => ["İsim: Uğur", "İsim: Yeşim", "İsim: Ezel", "İsim: Ömer"]
["Uğur", "Yeşim", "Ezel", "Ömer"].collect { |isim| "İsim: #{isim}" } # => ["İsim: Uğur", "İsim: Yeşim", "İsim: Ezel", "İsim: Ömer"]
```

**select**

Blok içinden gelen ifadenin **true** / **false** olmasına göre filtre yapar ve yeni Array döner:

```ruby
[1, 2, 3, 10, 15, 20].select { |n| n % 2 == 0 } # => [2, 10, 20] # 2'ye tam bölünenler
[1, 2, "3", "ali", 15, 20].select { |n| n.is_a?(Fixnum) } # => [1, 2, 15, 20] # sadece sayılar
```

**reject**

`select`in tersidir.

```ruby
[1, 2, 3, 10, 15, 20].reject { |n| n % 2 == 0 } # => [1, 3, 15] # 2'ye tam bölülenleri at
[1, 2, "3", "ali", 15, 20].reject { |n| n.is_a?(Fixnum) } # => ["3", "ali"] # Sayı olanları at
```

**keep_if**

Blok içindeki ifade'den sadece `false` dönenleri atar ve Array'in orjinal değerini bozar, değiştirir.

```ruby
a = [1, 2, 3, 10, 15, 20]
a.keep_if { |n| n % 2 == 0 } # => [2, 10, 20] # 2'ye bölünemeyenler false geldiği için düştüler.
a # => [2, 10, 20] # a artık bu!
```

**combination(n) { |c| blok } → array**

Matematikteki kombinasyon işlemidir. 1, 2 ve 3 sayılarının 2'li kombinasyonu:

```ruby
a = [1, 2, 3]
a.combination(1).to_a # => [[1], [2], [3]]
a.combination(2).to_a # => [[1, 2], [1, 3], [2, 3]]

a.combination(2) { |c| puts "Olasıklar: #{c.join(" ve ")}" }

# Olasıklar: 1 ve 2
# Olasıklar: 1 ve 3
# Olasıklar: 2 ve 3
```

**count**

Az önce method olarak işlediğimiz `count` ile başka ilginç işler de yapabiliyoruz:

```ruby
a = [1, 2, 3, 4, 2]

a.count                    # => 5 # eleman sayısı
a.count(2)                 # => 2 # kaç tane 2 var?
a.count { |n| n % 2 == 0 } # => 3 # kaç tane 2'ye tam bölünen var?
```

**cycle(n=nil) { |obje| blok } → nil**

Pas edilen blok'u **n** defa tekrar eder.

```ruby
a = [1, 2, 3]

a.cycle(2).to_a # => [1, 2, 3, 1, 2, 3] # 2 defa
a.cycle(4).to_a # => [1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3] # 3defa
a.cycle(2) { |o| puts "Sayı #{o}" }

# Sayı 1
# Sayı 2
# Sayı 3
# Sayı 1
# Sayı 2
# Sayı 3
```

Eğer `[1, 2, 3].cycle { |i| puts i }` gibi bir işlem yaparsanız, default olarak `nil` geçmiş olursun ve bu sonsuz döngüle girer, sonsuza kadar 1, 2, 3, 1, 2, 3 .... şeklinde devam eder!

**drop_while { |array| blok } → yeni array**

`delete_if` ile aynı işi yapar.

```ruby
notlar = [40, 45, 53, 70, 99, 65]
notlar.drop_while {|notu| notu < 50 }   # => [53, 70, 99, 65]
```
Koşula göre Array'den atar gibi düşünebilirsiniz. Not 50'den küçükse bırak.

**take_while**

Aynı `drop_while` gibi çalışır ama tersini yapar:

```ruby
notlar = [40, 45, 53, 70, 99, 65]
notlar.take_while { |notu| notu < 50 } # => [40, 45]
```

Koşula göre Array'e ekler gibi düşünebilirsiniz. Not 50'den küçükse sepete ekle! :)

**each**, **each_index**, **each_with_index**, **each_slice**, **each_with_object**, **reverse_each**

Array ve hatta Enumator'lerin can damarıdır. Ruby yazarken siz de göreceksiniz `each` en sık kullandığınız iterasyon (_yineleme / tekrarlama_) yöntemi olacak.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]

a.each # => #<Enumerator: ["Uğur", "Yeşim", "Ezel", "Ömer"]:each>
a.each { |isim| puts "İsim: #{isim}" }

# İsim: Uğur
# İsim: Yeşim
# İsim: Ezel
# İsim: Ömer
```

Array ve içinde dolaşılabilir her nesnede işe yarar. Birde bunun **index**'li hali var;

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]

a.each_index # => #<Enumerator: ["Uğur", "Yeşim", "Ezel", "Ömer"]:each_index>
a.each_index.to_a # => [0, 1, 2, 3]
a.each_index { |i| puts "Index: #{i}, Değeri: #{a[i]}" }

# Index: 0, Değeri: Uğur
# Index: 1, Değeri: Yeşim
# Index: 2, Değeri: Ezel
# Index: 3, Değeri: Ömer
```

ya da bu işi;

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.each_with_index { |eleman, index| puts "index: #{index}, eleman: #{eleman}" }

# index: 0, eleman: Uğur
# index: 1, eleman: Yeşim
# index: 2, eleman: Ezel
# index: 3, eleman: Ömer
```

`each_slice` da Array'i gruplamak, parçalara ayırmak içindir. Geçilen parametre bu işe yarar:

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.each_slice(2) # => #<Enumerator: ["Uğur", "Yeşim", "Ezel", "Ömer"]:each_slice(2)>
a.each_slice(2).to_a # => [["Uğur", "Yeşim"], ["Ezel", "Ömer"]]
a.each_slice(2) { |ikili_grup| puts "#{ikili_grup}" }

# ["Uğur", "Yeşim"]
# ["Ezel", "Ömer"]
```

`each_with_object` de ise, iterasyona girerken bir nesne pas edip, o nesneyi doldurabilirsiniz.

```ruby
[1, 2, 3, 4].each_with_object([]) { |number, given_object|
  given_object << number * 2
} # => [2, 4, 6, 8]
```

`number` Array'den gelen eleman (_1, 2, 3, 4 gibi_), `given_object` ise `each_with_object([])` method'da geçtiğimiz boş Array `[]`.

`reverse_each` aslında Array'i otomatik olarak ters çevirir yani **reverse** eder ve içinde dolaşmanızı sağlar:

```ruby
computers = ["Commodore 64", "Amiga", "Sinclair", "Amstrad"]
computers.reverse_each # => #<Enumerator: ["Commodore 64", "Amiga", "Sinclair", "Amstrad"]:reverse_each>
computers.reverse_each.to_a # => ["Amstrad", "Sinclair", "Amiga", "Commodore 64"]

computers.reverse_each { |c| puts "Bilgisayar: #{c}" }

# Bilgisayar: Amstrad
# Bilgisayar: Sinclair
# Bilgisayar: Amiga
# Bilgisayar: Commodore 64
```

**find_index**

İndeks'i ararken blok işleyebiliriz:

```ruby
computers = ["Commodore 64", "Amiga", "Sinclair", "Amstrad"]
computers.find_index { |c| c == "Amstrad" } # => 3
```

**freeze** ve **frozen?**

Array'i kitlemek için kullanılır. Yani **freeze** (_dondurulmuş_) bir Array'e yeni eleman eklenemez. Keza `Array#sort` esnasında da otomatik olarak **freeze** olur sort bitince buz çözülür!

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.freeze
a << "Fazilet"  # Yeni isim eklemek mümkün değildir!
```

    RuntimeError: can't modify frozen Array

Array'de buzlanma var mı yok mu anlamak için **frozen?** kullanırız:

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.freeze  # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.frozen? # => true
```

**min**, **max**, **minmax**, **min_by**, **max_by** ve **minmax_by**

`min` ve `max` ile Array elemanlarından en küçük/büyük değeri alırız:

```ruby
a = [6, 1, 8, 4, 11]
a.min # => 1
a.max # => 11
```

Peki sayı yerine metinler olsa ne olacaktı?

```ruby
m = ["a", "ab", "abc", "abcd"]
m.min # => "a"
m.max # => "abcd"
```

peki, `m` Array'i şöyle olsaydı : `m = ["a", "ab", "abc", "abcd", "111111111"]` sonuç ne olurdu?

```ruby
m = ["a", "ab", "abc", "abcd", "111111111"]

m.min # => "111111111" # ?
m.max # => "abcd"
```

Önce **Comparable** mı diyer bakılır, sayılar için çalışan bu yöntem, **String** de `a <=> b` karşılaştırmasına girer ve **Lexicological** karşılaştırma yapar. `"111111111"` karakter sayısı olarak diğerlerine göre çok olmasına rağmen, `min` değer olarak gelir. Eğer karakter sayına göre karşılaştırma yapmak gerekiyorsa;

```ruby
m = ["a", "ab", "abc", "abcd", "111111111"]
m.min { |a, b| a.length <=> b.length }  # => "a"
m.max                                   # => "abcd"
```

Şeklinde yapmak gerekir. Blok kullanabildiğimiz için aynı iş `max` için de geçerlidir. Ya da bu işleri yapabilmek için `min_by` ve `max_by` kullanabiliriz:

```ruby
m = ["a", "ab", "abc", "abcd", "111111111"]
m.min_by { |x| x.length }   # => "a"
m.max_by { |x| x.length }   # => "111111111"
```

`minmax` da Array'in minimum ve maximum'unu döner:

```ruby
m = ["a", "ab", "abc", "abcd"]
m.min    # => "a"
m.max    # => "abcd"
m.minmax # => ["a", "abcd"]
```

Aynı mantıkta `minmax_by` da gerekli şarta göre min, max döner:

```ruby
m = ["a", "ab", "abc", "abcd"]
m.minmax_by { |x| x.length } # => ["a", "abcd"]
```



## Tehlikeli İşlemler

Başlarda da bahsettiğimiz gibi method ismi `!` ile bitiyorsa bu ilgili nesnede değişiklik yapıyor olduğumuz anlamına gelir. Array'lerde de bu tür method'lar var:

    [:reverse!, :rotate!, :sort!, :sort_by!, :collect!, :map!, :select!, :reject!, :slice!, :uniq!, :compact!, :flatten!, :shuffle!, :!]



