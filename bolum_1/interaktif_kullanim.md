# İnteraktif Kullanım

Ruby'nin (*ve benzer dillerin*) en çok hoşuma giden özelliği İnteraktif Shell özelliği olmasıdır. Aynı eskiden Commodore 64 günlerindeki gibi, shell'i açıp Ruby yazmaya başlayabiliriz.

Genel olarak bu interaktif kullanım **REPL** olarak geçer. REPL aslında **R**ead **E**xecute **P**rint **L**oop'un baş harfleridir. Yani, kullanıcıdan bir input (_Read_) gelir, bu girdi çalıştırılır (_Evaluate_), sonuç ekrana yazdırılır (_Print_) ve son olarak başa döner ve yine input bekler (_Loop_).

REPL olayı, Python ve PHP'de de var.

## IRB

Ruby kurulumu ile beraber gelir. Yapmanız gereken Terminal'i açıp `irb` yazıp `enter`a basmak.

```bash
irb
irb(main):001:0> print "Merhaba Dünya"
Merhaba Dünya=> nil
irb(main):002:0>
```

Örnekleri yaparken çok sık kullanacağız bu komutu. Keza, daha da geliştirilmiş bir versiyon olan `pry` gem'ini de göreceğiz ilerleyen bölümlerde.


## Shell

**Shebang** dediğimiz yöntemli Linux/Unix tabanlı işletim sistemlerinde Ruby dosyalarını aynı bir uygulama çalıştırır gibi kullanabilirsiniz.

`test.rb` dosyası olduğunu düşünün; bu dosya

```bash
#!/usr/bin/env ruby
puts "Merhaba dünya"
```

şeklinde olsun. Bu dosyayı çalıştırmak için ya

```bash
ruby test.rb
```

ya da, dosyanın Execute flag'ini aktif hale getirerek

```bash
chmod +x test.rb
./test.rb
```

çalıştırabilirsiniz.
