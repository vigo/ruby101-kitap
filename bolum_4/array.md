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

**include?**

Acaba verdiğim eleman Array'in içinde mi?

```ruby
[1, 2, 3, 4].include?(3)                   # => true
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


**array <=> başka_array**

**Spaceship** operatöründen bahsetmiştik. Array'ler arasında karşılaştırma yapmayı sağlar.

```ruby
[1, 2, 3, 4] <=> [1, 2, 3, 4] # => 0 # Eşit
[1, 2, 3, 4] <=> [1, 2, 3]    # => 1 # İlk değer büyük
[1, 2, 3] <=> [1, 2, 3, 4]    # => -1 # İlk değer küçük
```

**pop**, **shift**, **delete** ve **delete_at**

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

**assoc**

Elemanları Array olan bir Array içinde, ilk değere göre yakalama yapmaya yarar.

```ruby
a = ["renkler", "kırmızı", "sarı", "mavi"]
b = ["harfler", "a", "b", "c"]
c = "foo"

t  = [a, b, c]
t                 # => [["renkler", "kırmızı", "sarı", "mavi"], ["harfler", "a", "b", "c"], "foo"]
t.assoc("renkler") # => ["renkler", "kırmızı", "sarı", "mavi"]
t.assoc("foo")     # => nil
```

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

**clear**

Array'i temizlemek için kullanılır :)

```ruby
a = [1, 2, 3]
a.clear # => []
a       # => []
```


