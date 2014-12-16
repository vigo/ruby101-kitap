# String

Kabaca, insanların anlayacağı şekilde tekst / metin bilgisi içeren nesnelerdir. Örneğin;

```ruby
m = "Merhaba"
m.class # => String
```

şeklindedir. Tanımlama yaparken **tek** ya da **çift** tırnak kullanılabilir ama aralarında fark vardır. **Expression Substitution** ya da **String Interpolation** olarak geçen, **String** içinde değişken kullanımı esnasında **çift tırnak** kullanmanız gerekir.

```ruby
m = "Merhaba"
puts "#{m} Uğur" # Merhaba Uğur
puts '#{m} Uğur' # #{m} Uğur
```

Tek tırnak kullandığımız örnekte `#{m}` değişkeni işlenmeden olduğu gibi çıktı vermiştir. Aynı şekilde **escape codes** yani görünmeyen özel karakterleri de sadece çift tırnak içinde kullanmak mümkündür.

Çift tırnak içinde kullanılan `#{BURAYA RUBY KODU GELİR}` çok işe yarar. `{` ve `}` arasında kod çalıştırmamızı sağlar.

```ruby
puts "Saat: #{Time.now}" # Saat: 2014-08-12 10:37:22 +0300
```

dediğimizde; Ruby Kernel'dan `Time` nesnesinin `now` method'unu çalıştırmış oluruz.

```ruby
puts "Merhaba\nDünya"

# Merhaba
# Dünya
```

`\n` **New Line** ya da **Line Feed** ya da yeni satıra geçme karakteri çift tırnakta çalışır.

| Escape Kod | Anlamı |
| -- | -- |
| \n | Yeni satır (0x0a) |
| \s | Boşluk (0x20) |
| \r | Satır Başı (0x0d) |
| \t | Tab Karakteri (0x09) |
| \v | Dikey Tab (0x0b) |
| \f | Formfeed (0x0c) |
| \b | Backspace (Bir geri) (0x08) |
| \a | Bell/Alert (Uyarı) (0x07) |
| \e | Escape (0x1b) |
| \nnn | Octal, 8'lik değer |
| \xnn | Hexadecimal, 16'lık değer |
| \unnnn | Unicode: U+nnnn (Ruby 1.9+) |
| \cx | Control-x |
| \C-x | Control-x |
| \M-x | Meta-x |
| \M-\C-x | Meta-Control-x |
| \x | x'in kendisi (\" çift tırkan demektir.) |


String'ler **byte array**'lerden oluşur yani elemanları **byte** cinsinden dizidir aslında.

```ruby
m = ""
m << 65
puts m # A
```

`m` boş bir String, diziye eleman ekler gibi (`<<` _diziye eleman ekler, az sonra göreceğiz_) içine **65** ekledik. Bu `A` harfinin 10'luk sayı sistemindeki *ASCII* değeridir. Aslında `m = "A"` yaptık :)

Eğer **65** in karakter setindeki değeri neydi? dersek `put 65.chr` yaptığımızda bize `A` döner. **0** ile **255** arası değerlerdir bunlar.

Daha ilginç bir olay;

```ruby
puts "öz" "yıl" "maz" "el" # özyılmazel
```

yani `"tekst" "tekst" "tekst"` şekinde bir kullanım mevcuttur.

## String Literals (String Kalıpları)

Ruby'de, yine diğer dillerde olmayan ilginç bir özellik. `%` işareti ve sonrasında gelen bazı karakterler yardımıyla enteresan şeyler yapmak mümkün:

### `%`

Süslü parantezler arasında kalan herşey **concat** (_yani toplanarak_) edilir ve **String** olarak çıktı verir ve tırnakları **escape** eder.

```ruby
%{Merhaba Dünya Ben vigo nasılsınız?} # => "Merhaba Dünya Ben vigo nasılsınız?"

%{Bu işlemlerin %80'i "uydurma"} # => "Bu işlemlerin %80'i \"uydurma\""
```

aynı işi; `%|Merhaba Dünya Ben vigo nasılsınız?|` süslü parantez yerine **pipe** `|` kullanarak da yapabilirsiniz!

### `%w`

Hızlıca **Array** üretmeyi sağlar:

```ruby
%w{foo bar baz}       # => ["foo", "bar", "baz"]
%w{foo bar baz}.class # => Array
```

### `%i`

İçinde **Symbol** olan **Array** üretir:

```ruby
%i{foo bar baz} # => [:foo, :bar, :baz]
```

### `%q` ve `%Q`

`q` tek tırnak, `Q` çift tırnakla sarmalamış gibi yapar:

```ruby
person = "Uğur"
%q{Merhaba #{person}} # => "Merhaba \#{person}"
%Q{Merhaba #{person}} # => "Merhaba Uğur"
```

### `%s`

**Symbol**'e çevirir:

```ruby
%s{my_variable} # => :my_variable
%s{email} # => :email
```

### `%r`

**Regular Expression**'a çevirir:

```ruby
%r{(.*)hello}i # => /(.*)hello/i
```

### `%x`

Ruby'de **back tick** kullanarak **shell** komutu çalıştırabilirsiniz. Yani Linux ve Mac kullanan okuyucular **Terminal** ile haşır-neşir olmuşlardır. Örneğin, bulunduğunuz dizindeki dosya listesini almak için `ls` komutunu kullanırsınız. Örneğin kendi **home** dizinimde `ls` yaptığımda (_OSX kullanıyorum_)

    ls -1 # alt alta listemelek için

    Applications
    Desktop
    Development
    Documents
    Dotfiles
    Downloads
    Library
    VirtualBox VMs
    Works
    bin

gibi bir liste alıyorum. Yapacağım bir Ruby uygulamasında;

```ruby
%x{ls -1 $HOME} # => "Applications\nDesktop\nDevelopment\nDocuments\nDotfiles\nDownloads\nLibrary\nVirtualBox VMs\nWorks\nbin\n"
```

Tüm liste tek bir String olarak geldi ve `\n` karakteri ile birleşti çünki sonuç alt alta satır satır geldi. Eğer;

```ruby
%x{ls -1 $HOME}.split("\n")
```

deseydim sonuç bana dizi olarak gelecekti!

    # ["Applications", "Desktop", "Development", "Documents", "Dotfiles", "Downloads", "Library", "VirtualBox VMs", "Works", "bin"]

    %x{ruby --copyright} # => "ruby - Copyright (C) 1993-2014 Yukihiro Matsumoto\n"

## Here Document Kullanımı

Uzun metin kullanımlarında çok işe yarar:

```ruby
mesaj = <<END
Merhaba nasılsınız?
Biz de çok iyiyiz
Görüşürüz!
END
puts mesaj

# Merhaba nasılsınız?
# Biz de çok iyiyiz
# Görüşürüz!
```

Bu örnekte `mesaj = <<END` ile `END` kelimesini görene kadar içinde ne varsa kullan diyoruz! Daha da çılgın bir kullanım şekli;

```ruby
mesaj = [<<BİR, <<İKİ, <<ÜÇ]
  Bu Bir
BİR
  Bu iki....
İKİ
  Bu da üüüüüüüüç
ÜÇ

puts mesaj

# - Bu Bir
# - Bu iki....
# - Bu da üüüüüüüüç
```

## String Method'ları

### String % argüman -> yeni String

```ruby
"Merhaba %s" % "Uğur" # => "Merhaba Uğur"
"Sayı: %010d" % 2014  # => "Sayı: 0000002014"
"Kullanıcı Adı: %s, E-Posta: %s" % [ "vigo", "vigo@foo.com" ] # => "Kullanıcı Adı: vigo, E-Posta: vigo@foo.com"
"Merhaba %{username}!" % { :username => 'vigo' } # => "Merhaba vigo!"
```

Keza bu method, `printf`, `sprintf` gibi, **String Format** mantığında çalışır. `"Sayı: %010d" % 2014` örneğinde `%010d` aslında **10** basamaklı, **0** ile pad edilmiş şekilde göster anlamındadır. **2014** 4 basamaklıdır, solunda **6** tane sıfır gelmiştir.

### String * sayı -> yeni String

String ile sayıyı çarpmak mümkün!

```ruby
"Merhaba!" * 3 # => "Merhaba!Merhaba!Merhaba!"
"Merhaba!" * 0 # => ""
```

### String + String -> yeni String

```ruby
"Merhaba" + " " + "Dünya" # => "Merhaba Dünya"
```

### String << sayı -> String
### String << nesne -> String

```ruby
a = "Merhaba"
a << " dünya" # => "Merhaba dünya"
a # => "Merhaba dünya"
a.concat(33) # => "Merhaba dünya!"
a << 33 # => "Merhaba dünya!!"
```

### String <=> başka string → -1, 0, +1 ya da nil

`<=>` Ruby dünyasında **Spaceship** operatörü olarak geçer. Aynı cins nesneleri karşılaştırmak için kullanılır.

```ruby
"vigo" <=> "vigo"    # => 0   # eşit
"vigo" <=> "vig"     # => 1   # vigo büyük
"vigo" <=> "vigoo"   # => -1  # vigo küçük
"vigo" <=> 3         # => nil # alakasız iki şey
```

`casecmp` ile aynı işi yapar

### String =~ Nesne -> Fixnum ya da nil

```ruby
"Saat tam 4'de buluşalım" =~ /\d/ # => 9

# \d sayı yakaladı ve indeksi döndü

"Saat tam 4'de buluşalım"[9] # => "4"
```

### String içinde hareket

String aslında karakterlerden oluşan bir dizi olduğu için aşağıdaki gibi atraksiyonlar yapmak mümkün.

    String[indeks] -> yeni string ya da nil
    String[başlangıç, uzunluk] -> yeni string ya da nil
    String[range] -> yeni string ya da nil
    String[regexp] -> yeni string ya da nil
    String[regexp, yakalan] -> yeni string ya da nil
    String[metinni_bul] -> yeni string ya da nil

örnek olarak;

```ruby
m = "Merhaba Dünya"
m[0]         # => "M"  # 0.karakter
m[0, 2]      # => "Me" # 0'dan itibaren 2 karakter
m[0..4]      # => "Merha" # range, 0'dan 4 dahil
m[-1,]       # => "a" # son karakter
m[-13..-1]   # => "Merhaba Dünya" # sondan başa
m[15, 1]     # => nil # olmayan indeks
m[/(?<sesli>[aeiou])/, "sesli"] # => "e" # regexp
m["Merhaba"] # => "Merhaba" # metni bul
m["vigo"] # => nil # olmayan metin
```

aynı işi `slice` ile de yapabilirsiniz.

```ruby
m = "merhaba"
m.slice(2,5) # => "rhaba"
```

gibi...

### Yardımcı Methodlar

Sıkça kullanılanlar arasında; `capitalize`, `center`, `chars`, `chomp`, `chop`, `clear`, `count`, `size`, `length`, `delete`, `ljust`, `rjust`, `reverse`, `upcase`, `downcase`, `swapcase`, `reverse`, `index`, `hex`, `rindex`, `insert` gibi methodlar'dan örnekler ekledim. Herzaman olduğu gibi, hangi method'ların olduğunu görmek için;

```ruby
String.new.methods # => [:<=>, :==, :===, :eql?, :hash, :casecmp, :+, :*, :%, :[], :[]=, :insert, :length, :size, :bytesize, :empty?, :=~, :match, :succ, :succ!, :next, :next!, :upto, :index, :rindex, :replace, :clear, :chr, :getbyte, :setbyte, :byteslice, :scrub, :scrub!, :freeze, :to_i, :to_f, :to_s, :to_str, :inspect, :dump, :upcase, :downcase, :capitalize, :swapcase, :upcase!, :downcase!, :capitalize!, :swapcase!, :hex, :oct, :split, :lines, :bytes, :chars, :codepoints, :reverse, :reverse!, :concat, :<<, :prepend, :crypt, :intern, :to_sym, :ord, :include?, :start_with?, :end_with?, :scan, :ljust, :rjust, :center, :sub, :gsub, :chop, :chomp, :strip, :lstrip, :rstrip, :sub!, :gsub!, :chop!, :chomp!, :strip!, :lstrip!, :rstrip!, :tr, :tr_s, :delete, :squeeze, :count, :tr!, :tr_s!, :delete!, :squeeze!, :each_line, :each_byte, :each_char, :each_codepoint, :sum, :slice, :slice!, :partition, :rpartition, :encoding, :force_encoding, :b, :valid_encoding?, :ascii_only?, :unpack, :encode, :encode!, :to_r, :to_c, :>, :>=, :<, :<=, :between?, :nil?, :!~, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]
```

kullanabiliriz!

```ruby
m = "merhaba"

m.capitalize # => "Merhaba"
m # => "merhaba"

m.capitalize! # => "Merhaba" # m'in değeri artık değişti!
m # => "Merhaba"

"vigo".center(12) # => "    vigo    "
"vigo".center(12, "*") # => "****vigo****"

"merhaba".chars # => ["m", "e", "r", "h", "a", "b", "a"]

"merhaba\n".chomp # => "merhaba"
"merhaba dünya".chomp(" dünya") # => "merhaba"

"merhaba vigo".chop # => "merhaba vig"
"merhaba vigo\n".chomp # => "merhaba vigo"

"a".chr # => "a"

x = "Merhaba"
x.clear # => ""

"Merhaba Dünya".count("a") # => 3  # 3 adet a
"Merhaba Dünya".count("ab") # => 4 # a ve b toplam 4 tane
"Merhaba Dünya".count("e") # => 1  # e'den 1 tane

"Merhaba Dünya".delete("e") # => "Mrhaba Dünya"
"Merhaba Dünya".delete("a", "ba") # => "Merhb Düny"

"MERHABA".downcase # => "merhaba"
"merhaba".upcase # => "MERHABA"
"Merhaba".swapcase # => "mERHABA"

"merhaba".size # => 7
"merhaba".length # => 7

"merhaba".ljust(20)      # => "merhaba             "
"merhaba".ljust(20, "*") # => "merhaba*************"
"merhaba".rjust(20)      # => "             merhaba"
"merhaba".rjust(20, "*") # => "*************merhaba"

"   merhaba  ".strip  # => "merhaba"
"  merhaba".lstrip    # => "merhaba"
"merhaba  ".rstrip    # => "merhaba"

"merhaba".index("m") # => 0
"merhaba".index("ba") # => 5

"merhaba".rindex("m") # => 0
"merhaba".rindex("h") # => 3

"a".next     # => "b"
"abcd".next  # => "abce" # d'den sonra e geldi...
"b".succ     # => "c"

# baş, ayraç, son
"merhaba".partition("r") # => ["me", "r", "haba"]
"merhaba".partition("a") # => ["merh", "a", "ba"]
"merhaba".partition("x") # => ["", "", "merhaba"]

"merhaba dünya".reverse # => "aynüd abahrem"

"hey seeeeeeeeeeeeeeen! aloooooooo".squeeze # => "hey sen! alo"

"Merhaba".dump # => "\"Merhaba\""
"merhaba".getbyte(0) # => 109 # m'in ascii değeri

"0x0f".hex # => 15
"0x0fff".hex # => 4095

"merhaba".insert(0, "X") # => "Xmerhaba"
"merhaba".insert(3, "A") # => "merAhaba"
"merhaba".insert(-1, ".") # => "merhaba."

"123".oct # => 83 # Octal'e çevirdi (8'lik)

"A".ord # => 65  # Ascii değeri
"a".ord # => 97 # Ascii değeri

"dünya".prepend("Merhaba ") # => "Merhaba dünya" # öne ekledi

# transform
"merhaba hello".tr("el", "ip") # => "mirhaba hippo" # e=>i, l => p oldu
"ArkAdAşlar nasılsınız?".tr("A", "a") # => "arkadaşlar nasılsınız?"

# a'dan e'y kadar X ile transform yap
"merhaba dünya".tr("a-e", "X") # => "mXrhXXX XünyX"
```

### Convert Method'ları

Tip değiştirmek için kullanılır. `to_i`, `to_f`, `to_s`, `to_str`, `to_sym`, `to_r`, `to_c`, `to_enum` method'larına bakalım:

```ruby
"merhaba".to_i   # => 0 # integer'a çevirdi
"merhaba".to_f   # => 0.0 # float'a çevirdi
"5".to_i          # => 5
"1.5".to_f        # => 1.5
"merhaba".to_s    # => "merhaba" # string
"merhaba".to_str  # => "merhaba" # string
"merhaba".to_sym  # => :merhaba # symbol
"merhaba".to_r    # => (0/1) # Rasyonel sayı
"0.2".to_r        # => (1/5) # Rasyonel sayı
"merhaba".to_c    # => (0+0i) # Kompleks sayı
"1234".to_c       # => (1234+0i)
"merhaba".to_enum # => #<Enumerator: "merhaba":each> # Enumeratör'e çevirdi
```

### Kontrol Method'ları

Method adı `?` ilen bitiyor demek, bir kontrol olduğu ve sonucun **Boolean** yanı `true` ya da `false` döndüğü anlamında olduğunu söylemiştik.

```ruby
"merhaba".start_with?("m") # => true
"merhaba".start_with?("mer") # => true
"merhaba".start_with?("f") # => false

"merhaba".end_with?("a") # => true
"merhaba".end_with?("haba") # => true
"merhaba".end_with?("zoo") # => false

"merhaba".eql?("Merhaba") # => false
"merhaba".eql?("merhaba") # => true

"merhaba dünya".include?("dünya") # => true

"merhaba".empty? # => false
"".empty? # => true

"kedi".between?("at", "balık") # => false # başlangıç harfi a ve b arasındamı? gibi düşünün
"kedi".between?("fare", "sinek") # => true
```

### Array ve Block ile İlişkili Methodlar

**split**
Metni parçalara böler, varsayılan **delimiter** (_ayırıcı_) boşuk karakteridir.

```ruby
"Selam millet nasıl sınız?".split # => ["Selam", "millet", "nasıl", "sınız?"]
"Selam millet-nasıl sınız?".split("-") # => ["Selam millet", "nasıl sınız?"]
"A takımı: 65 B takımı: 120".split(/ +\d+ ?/) # => ["A takımı:", "B takımı:"]
"1,2,3,4,5".split(",") # => ["1", "2", "3", "4", "5"]
```

**each_byte**

```ruby
"merhaba".each_byte {|c| puts c }

# 109 (m)
# 101 (e)
# 114 (r)
# 104 (h)
# 97  (a)
# 98  (b)
# 97  (a)
```

**each_char**

```ruby
"merhaba".each_char {|c| puts c }

# m
# e
# r
# h
# a
# b
# a
```

**each_line**

```ruby
"Merhaba\nDünya\nNasıl sın?".each_line {|l| puts l }

# Merhaba
# Dünya
# Nasıl sın?

"Merhaba@@Dünya@@Nasıl sın?".each_line("@@") {|l| puts l }
# Merhaba@@
# Dünya@@
# Nasıl sın?
```

**upto**

```ruby
"a1".upto("b1"){ |t| puts t }

# a1
# a2
# a3
# a4
# a5
# a6
# a7
# a8
# a9
# b0
# b1
```


### Pattern Yakalama (Regexp)

Daha kapsamlı olarak **6.Bölüm**'de de değineceğimiz **Regular Expression** konusu, String'lerle çok ilişkili. Hemen ilgili method'lara bakalım

**gsub** ve **sub**

`sub` ile `gsub` arasındaki fark, `sub` ilk bulduğunu işler, `gsub` **Global** anlamındadır.

```ruby
"merhaba dünya, merhaba uzay".sub("merhaba", "olaa") # => "olaa dünya, merhaba uzay"
"merhaba dünya, merhaba uzay".gsub("merhaba", "olaa") # => "olaa dünya, olaa uzay"


"Merhaba Dünya".gsub(/[aeiou]/, "x") # => "Mxrhxbx Dünyx"
"Merhaba Dünya".gsub(/([aeiou])/, '(\1)') # => "M(e)rh(a)b(a) Düny(a)"
"Merhaba dünya, merhaba uzay, merhaba evren".gsub(/((m|M)erhaba)/){|c| c.upcase } # => "MERHABA dünya, MERHABA uzay, MERHABA evren"
"Merhaba Dünya".gsub(/(?<sesli_harf>[aeiou])/, '{\k<sesli_harf>}') # => "M{e}rh{a}b{a} Düny{a}"
"Merhaba Dünya".gsub(/[ea]/, 'e' => 1, 'a' => 'X') # => "M1rhXbX DünyX"
```

**match**

```ruby
"merhaba".match("a") # => #<MatchData "a">

"merhaba".match("(a)") # => #<MatchData "a" 1:"a"> # 1 tane yakaladı, (a) ve Array geldi
"merhaba".match("(a)")[0] # => "a" # yakalanan

"merhaba 2014".match(/\d/) # => #<MatchData "2">

"merhaba 2014".match(/(\d)/) # => #<MatchData "2" 1:"2">
"merhaba 2014".match(/(\d+)/) # => #<MatchData "2014" 1:"2014">
"merhaba 2014".match(/(\d+)/)[0] # => "2014"
"merhaba 2014".match(/(\d+)/)[0].to_i # => 2014
```

**scan**

**Match** gibi, metin üzerinde bir nevi arama yapıyoruz:

```ruby
"Merhaba millet!".scan(/\w+/) # => ["Merhaba", "millet"]
"Merhaba millet!".scan(/./) # => ["M", "e", "r", "h", "a", "b", "a", " ", "m", "i", "l", "l", "e", "t", "!"]
"Merhaba millet! Saat 10'da buluşalım".scan(/Saat \d+/) # => ["Saat 10"]
```
