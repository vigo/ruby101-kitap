# Object (Nesne)

Ruby, **Object Oriented Programming**'in dibidir :) Herşey nesnelerden oluşur. Nasıl mı? hemen basit bir değişken oluşturup içine bir tekst yazalım.

    mesaj = "Merhaba"

`mesaj` değişkeninin türü ne?

    mesaj.class # => String

Bu değişken **String** nesnesinden türemiş. Peki, **String** nereden geliyor?

    mesaj.class.superclass # => Object

Dikkat ettiyseniz burada `superclass` kullandık. Yani bu hiyeraşideki bir üst sınıfı arıyoruz. Karşımıza ne çıktı? **Object**. Peki acaba **Object** nereden türemiş?

    mesaj.class.superclass.superclass # => BasicObject

Hmmm.. Peki **BasicObject** nereden geliyor?

    mesaj.class.superclass.superclass.superclass # => nil

İşte şu anda dibi bulduk :) Demekki hiyeraşi;

    BasicObject > Object > String

şeklinde bir hiyeraşi söz konusu.

Peki, sayılarda durum ne?

    numara = 1
    numara.class # => Fixnum
    numara.class.superclass # => Integer
    numara.class.superclass.superclass # => Numeric
    numara.class.superclass.superclass.superclass # => Object
    numara.class.superclass.superclass.superclass.superclass # => BasicObject
    numara.class.superclass.superclass.superclass.superclass.superclass # => nil

Ufff... bir an için bitmeyecek sandım :) Çok basit bir sayı tanımlası yaptığımızda bile, arka plandaki işleyiz yukarıdaki gibi oluyor. Yani

    BasicObject > Object > Numeric > Integer > Fixnum

şeklinde yine ana nesne **BasicObject** olmak koşuluyla uzun bir hiyeraşi söz konusu.

Herşey **BasicObject** den türüyor, bu yüzden de aslında herşey bir **Class** dolayısıyla bu durum dile çok ciddi esneklik kazandırıyor.

## Nesne Metodları (Object Instance Methods)

Şimdi boş bir nesne oluşturalım. **Class** bölümünde daha detaylı göreceğimiz **instantiate** işlemiyle `new` methodunu kullanarak;

```ruby
o = Object.new # => #<Object:0x007fe552099a68>
o.__id__       # => 70311450299700
```

yaptığımızda, oluşan nesnenin hafızada **unique** (_yani bundan sadece bir tane_) bir **identifier**'ı (_kabaca buna kimlik diyelim__) yani **ID**'si olduğunu görürüz. `__id__` yerine `object_id` yani `o.object_id` şeklinde de kullanabiliriz.

Eğer `hash` method’unu çağırırsak, Ruby bize ilgili objenin `Fixnum` türünde sayısal değerini üretir ve verir.

```ruby
o = Object.new # => #<Object:0x007f8c3b0a3420>
o.__id__       # => 70120131336720
o.object_id    # => 70120131336720
o.hash         # => -229260864779029724
```

Neticede **String** de bir nesne ve;

```ruby
t = String.new("Hello")  # => "Hello"
t.__id__                 # => 70170408456140
t.methods                # => [:<=>, :==, :===, :eql?, :hash, :casecmp, :+, :*, :%, :[], :[]=, :insert, :length, :size, :bytesize, :empty?, :=~, :match, :succ, :succ!, :next, :next!, :upto, :index, :rindex, :replace, :clear, :chr, :getbyte, :setbyte, :byteslice, :scrub, :scrub!, :freeze, :to_i, :to_f, :to_s, :to_str, :inspect, :dump, :upcase, :downcase, :capitalize, :swapcase, :upcase!, :downcase!, :capitalize!, :swapcase!, :hex, :oct, :split, :lines, :bytes, :chars, :codepoints, :reverse, :reverse!, :concat, :<<, :prepend, :crypt, :intern, :to_sym, :ord, :include?, :start_with?, :end_with?, :scan, :ljust, :rjust, :center, :sub, :gsub, :chop, :chomp, :strip, :lstrip, :rstrip, :sub!, :gsub!, :chop!, :chomp!, :strip!, :lstrip!, :rstrip!, :tr, :tr_s, :delete, :squeeze, :count, :tr!, :tr_s!, :delete!, :squeeze!, :each_line, :each_byte, :each_char, :each_codepoint, :sum, :slice, :slice!, :partition, :rpartition, :encoding, :force_encoding, :b, :valid_encoding?, :ascii_only?, :unpack, :encode, :encode!, :to_r, :to_c, :>, :>=, :<, :<=, :between?, :nil?, :!~, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]

t.method(:upcase).call   # => "HELLO"
```
`t.methods` ise **String**'den türeyen `t` ye ait tüm method’ları listeledik. Sonuç **Array** (_Dizi_) olarak geldi ve bu dizinin tüm elemanları `:` işaretiyle başlıyor. Çünki bu elemanlar birer **Symbol**.

`t.method(:upcase).call` da ise, `t`'nin `:upcase` method’unu `call` ile çağırdır. Aslında yaptığımız iş: `"hello".upcase` ile birebir aynı.

Acaba bu nesne ne?

`t.is_a?(String) # => true` `is_a?` method’u ile nesnenin türünü kontrol edebiliriz.

Diğer dillerin pek çoğunda (_özellikle Python_) bir işi yapmanın bir ya da en fazla iki yolu varken, **Ruby** bu konuda çok rahattır. Bir işi yapmanın herzaman birden fazla yolu olur ve bunların neredeyse hepsi doğrudur. (_Kullanıldığı yere ve amaca bağlı olarak_)

`is_a?` yerine `kind_of?` da kullanabiliriz!

Bir nesneye ait hangi method'ların olduğunu;

```ruby
o = Object.new
o.methods # => [:nil?, :===, :=~, :!~, :eql?, :hash, :<=>, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :frozen?, :to_s, :inspect, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :==, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]

o.public_methods # => [:nil?, :===, :=~, :!~, :eql?, :hash, :<=>, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :frozen?, :to_s, :inspect, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :==, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]

o.private_methods # => [:initialize_copy, :initialize_dup, :initialize_clone, :sprintf, :format, :Integer, :Float, :String, :Array, :Hash, :warn, :raise, :fail, :global_variables, :__method__, :__callee__, :__dir__, :eval, :local_variables, :iterator?, :block_given?, :catch, :throw, :loop, :respond_to_missing?, :trace_var, :untrace_var, :at_exit, :syscall, :open, :printf, :print, :putc, :puts, :gets, :readline, :select, :readlines, :`, :p, :test, :srand, :rand, :trap, :exec, :fork, :exit!, :system, :spawn, :sleep, :exit, :abort, :load, :require, :require_relative, :autoload, :autoload?, :proc, :lambda, :binding, :caller, :caller_locations, :Rational, :Complex, :set_trace_func, :gem, :gem_original_require, :initialize, :singleton_method_added, :singleton_method_removed, :singleton_method_undefined, :method_missing]

o.protected_methods # => []

o.public_methods(false) # => []
```

`public_methods` default olarak `public_methods(all=true)` şeklinde çalışır. Eğer parametre olarak `false` geçersek ve bu bizim oluşturduğumuz bir nesne ise, sadece ilgili nesnenin **public_method**'ları geri döner.

Başta belirttiğimiz gibi, basit bir nesne bile **Object**'den türediği için ve default olarak bu türeme esnasında tüm özellikler diğerine geçtiği için, sadece sizin method'larınızı görüntülemek açısından `false` olayı çok işe yarar.

## Method Missing

Bence Ruby'nin en süper özelliklerinden biridir. Olmayan bir method'u çağırdığınız zaman tetiklenen method `method_missing` method'udur. Ruby on Rails framework'ü neredeyse bu mekanizma üzerine kurulmuştur. 3 parametre alır; çağırılan method, eğer parametre geçilmişse parametreler, eğer block geçilmişse block.

```ruby
class User
  def method_missing(method_name, *args, &block)
    if method_name == :show_user_info
      "This user has no information"
    else
      "You've called #{method_name}, You've passed: #{args}"
    end
  end
end

u = User.new
u.show_user_info # => "This user has no information"
u.show_user_age  # => "You've called show_user_age, You've passed: []"
```

`User` adında bir Class'ımız var. İçinde hiçbir method tanımlı değil. `u.show_user_info` satırında, olmayan bir method'u çağırıyoruz. Tanımladığımız `method_missing` method'u ile olmayan method çağırılmasını yakalıyoruz. Eğer `show_user_info` diye bir method çağrılırsa yakalıyoruz, bunun dışında birşey olursa da method adını ve geçilen parametreleri gösteriyoruz.

Bu sayede `NoMethodError` hatası almadan işimize devam edebiliyoruz.

Anlamak açısından, Roman rakamları için bir sınıf yaptığınızı düşünün. Sadece örnek olması için gösteriyorum, C,X ve M için;

```ruby
class Roman
  def roman_to_str(str)
    case str
    when "x", "X"
      10
    when "c", "C"
      100
    when "m", "M"
      1000
    end
  end
  def method_missing(method)
    roman_to_str method.id2name
  end
end

r = Roman.new
r.x # => 10
r.X # => 10
r.C # => 100
r.M # => 1000
```

Bunu geliştirim "MMCX" ya da "III" gibi gerçek dönüştürme işini yapabilirsiniz.

**respond_to_missing?**

Yukarıdaki örnekte, olmayan method'ları ürettik. Peki, acaba bu olmayan method'ları nasıl çağırabilir ya da kontrol edebiliriz? Normalde, bir Class'ın hangi method'u olduğunu `respond_to?` ile öğreniyorduk. Örneğe uygulayalım;

`r.C` derken aslında `:C` method'unu çağırıyoruz. Peki böyle bir method var mı?

```ruby
r.method(:C)      # => `method': undefined method `C' for class `Roman' (NameError)
```

Nasıl yani? peki kontrol edelim?

```ruby
r.respond_to?(:C) # => false
```

Çünkü biz `:C` yi dinamik olarak ürettik ama öylece ortada bıraktık. Yapmamız gereken `respond_to_missing?` ile gereken cevabı vermekti:

```ruby
class Roman
  def roman_to_str(str)
    case str
    when "x", "X"
      10
    when "c", "C"
      100
    when "m", "M"
      1000
    end
  end
  def method_missing(method)
    roman_to_str method.id2name
  end
  def respond_to_missing?(method_name, include_private = false)
    [:x, :X, :c, :C, :m, :M].include?(method_name) || super 
  end
end

r.method(:C)      # => #<Method: Roman#C>
r.respond_to?(:C) # => true
r.respond_to?(:Q) # => false # olmayan method
```
