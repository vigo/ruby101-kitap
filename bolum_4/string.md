# String

Kabaca, insanların anlayacağı şekilde tekst / metin bilgisi içeren nesnelerdir. Örneğin;

    m = "Merhaba"
    m.class # => String

şeklindedir. Tanımlama yaparken **tek** ya da **çift** tırnak kullanılabilir ama aralarında fark vardır. **Expression Substitution** ya da **String Interpolation** olarak geçen, **String** içinde değişken kullanımı esnasında **çift tırnak** kullanmanız gerekir.

```ruby
m = "Merhaba"
puts "#{m} Uğur" # Merhaba Uğur
puts '#{m} Uğur' # #{m} Uğur
```

Tek tırnak kullandığımız örnekte `#{m}` değişkeni işlenmeden olduğu gibi çıktı vermiştir. Aynı şekilde **escape codes** yani görünmeyen özel karakterleri de sadece çift tırnak içinde kullanmak mümkündür.

Çift tırnak içinde kullanılan `#{BURAYA RUBY KODU GELİR}` çok işe yarar. `{` ve `}` arasında kod çalıştırmamızı sağlar.

    puts "Saat: #{Time.now}" # Saat: 2014-08-12 10:37:22 +0300

dediğimizde; Ruby Kernel'dan `Time` nesnesinin `now` method'unu çalıştırmış oluruz.

    puts "Merhaba\nDünya"

    # Merhaba
    # Dünya

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

    puts "öz" "yıl" "maz" "el" # özyılmazel

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

    %w{foo bar baz}       # => ["foo", "bar", "baz"]
    %w{foo bar baz}.class # => Array

### `%i`

İçinde **Symbol** olan **Array** üretir:

    %i{foo bar baz} # => [:foo, :bar, :baz]

### `%q` ve `%Q`

`q` tek tırnak, `Q` çift tırnakla sarmalamış gibi yapar:

```ruby
person = "Uğur"
%q{Merhaba #{person}} # => "Merhaba \#{person}"
%Q{Merhaba #{person}} # => "Merhaba Uğur"
```

### `%s`

**Symbol**'e çevirir:

    %s{my_variable} # => :my_variable
    %s{email} # => :email

### `%r`

**Regular Expression**'a çevirir:

    %r{(.*)hello}i # => /(.*)hello/i

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

    %x{ls -1 $HOME} # => "Applications\nDesktop\nDevelopment\nDocuments\nDotfiles\nDownloads\nLibrary\nVirtualBox VMs\nWorks\nbin\n"

Tüm liste tek bir String olarak geldi ve `\n` karakteri ile birleşti çünki sonuç alt alta satır satır geldi. Eğer;

    %x{ls -1 $HOME}.split("\n")

deseydim, yani **Array** konusunda göreceğimiz `split` methodu'nu da kullansaydım sonuç bana dizi olarak gelecekti!

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
