# Ruby Komutu ve Parametreleri

Kurulum işlemleri bittikten sonra ya da kullandığınız **OS** (_İşletim Sistemi_) ön tanımlı Ruby ile geliyorsa hemen aşağıdaki testi yapabilirsiniz:

    $ ruby --help

    Usage: ruby [switches] [--] [programfile] [arguments]

`ruby`'yi çağırırken, her shell aracı gibi (_binary mi diyim, executable mı diyim karar veremedim!_) Ruby de çeşitli parametreler alabiliyor. Bu kısıma dikkat edelim çünkü burada bahsi geçecek **Switch**'ler 2.Bölümde göreceğimiz **Ön Tanımlı Değişkenler** ile çok alakalı.

| Switch | Açıklaması |
| -- | -- |
| -0[octal] | `$/` değeridir. (_Ön tanımlı değişkenlerde göreceğiz_) **octal** yani 8'lik sistemde değer atanır. Örneğin `ruby -0777` şeklinde çalıştırılsa, Ruby, dosya okuma işlemleri sırasında tek seferde dosyayı okur ve tek **String** haline getirir. |
| -a | `-n` ve `-p` ile birlikte kullanılınca **autosplit mode** olarak çalışır. Yani `$F` => `$_.split` şeklinde işler. |
| -c | **Syntax Check** yani dosya içindeki kodu çalıştırmadan, sadece söz dizimi kontrolü yapar ve çıkar. |
| -C**directory** | Ruby önce belirtilen **directory**'ye `cd` (_Shell'de bir dizine geçiş yapmak_) yapar ve daha sonra kodu çalıştırır. `ruby -C/tmp/foo` gibi.. |
| -d, --debug | Debug modda çalıştırır. Bu esnada `$DEBUG` değişkeni de `true` değerini alır. Yani eğer kodunuzun içinde `if $DEBUG` gibi bir ifade kullanabilir ve sonucunu görebilirsiniz. |
| -e **'command'** | Komut satırından tek satırda Ruby kodu çalıştırmak için. `ruby -e 'puts "hello"'` gibi. |
| -Eex[:in], --encoding=ex[:in] | Varsayılan karakter encoding'i (_Internal ve External için..._) |
| --external-encoding=encoding | `-E` gibi |
| --internal-encoding=encoding | `-E` gibi |
| -F**pattern** | **auto split** için ve `split()` için varsayılan regex pattern'i. `$;`değeri. |
| -i | **in-place-edit** mod. Lütfen örneğe bakın! |
| -I | `$LOAD_PATH`'e ilave path ekleme. |
| -K | Japonca (_KANJI_) encoding belirtilir. `UTF-8` için `-K u` kullanılabilir. |
| -l | Otomatik satır sonu (_Line Ending_) işlemi. `-n`ve `-p` ile çalışır. Önce `$\` değişkenine `$/` değeri atanır, `chop!` method’u her satıra uygulanır. |
| -n | Komut satırındaki `sed -n` ya da `awk` gibi çalışır. Sanki kodun etrafında **loop** varmış gibi davranarak süzgeçten geçirir. |
| -p | `-n` gibi çalışır, farkı `$_` den gelen değeri döngünün sonunda **print** eder. |
| -r | `require` komutunun yaptığı gibi verilen değeri **require** eder. (**require** komutunu ileride göreceğiz) |
| -s | Komut satırı argümanlarını **parse** etme (_işleme_) özelliğini aktive eder. |
| -S | `$PATH` çevre değişkenini bulmayı forse eder. Örneğe bakınız! |
| -T | Güvenlik seviyesi, **tainted** kontrolünü devreye sokmak. `$SAFE` değişkenine `-T` ile geçilen değer atanır. |
| -v, --verbose | Önce versiyon numarasını yazar sonra da **verbose** (_Ayrıntılı çalıştırma_) modu aktive eder. Yani `$VERBOSE` **true** olur. |
| -w | `-v` ile aynı işi yapar sadece versiyon numarasını yazmaz. |
| -W | Uyarı seviyesini belirler (_Warning Level_). **0** Sessiz, **1** Orta şekerli, **2** Verbose! |
| -x | **Shebang**'den önceki teksti siler atar ve alternatif olara ilgili dizine `cd` yapar.  |
| --copyright | Ruby'e ait telif bilgisini yazar. `ruby - Copyright (C) 1993-2013 Yukihiro Matsumoto` |
| --enable=feature[,...], --disable=feature[,...] | Örneğin kodun **RubyGem**'lerini kullanmasını istemiyorsanız `--disable-gems` şeklinde, ya da `$RUBYOPT` çevre değişkenini devre dışı bırakmak için `--disable-rubyopt` gibi. `--disable-all` her ek özelliği devre dışı bırakır. `--enable-all` ya da devreye sokar. |
| --version | Versiyon numarasını yazar. |
| --help | Yardım sayfasını gösterir. |

`ruby --help` dışında daha detaylı bilgi `man ruby` yani **man pages**'da bulmak mümkündür, ben de pek çok şeye oradan baktım.

## `-i` örneği:

Önce içinde düz metin olan bir dosya oluşturalım:

    echo vigo > /tmp/test.txt
    cat /tmp/test.txt
    # vigo

sonra;

    ruby -p -i.backup -e '$_.upcase!' /tmp/test.txt
    cat /tmp/test.txt.backup
    # VIGO

Ne oldu? Amacımız, `/tmp/test.txt` dosyasında, satır satır okuyup her satırda yazan metni **uppercase** yani büyük harfe çevirmek. Normalde bu işlemi;

    ruby -p -e '$_.upcase!' /tmp/test.txt

şeklinde komut satırından yapabiliyoruz. Ama **in-place-edit** mod ve **extension** özelliği ile, çalıştırılmış kod çıktısını başka bir dosyada görüntüleyebiliriz. Bu noktada `-i` devreye giriyor. `-i.backup` sonucun görüntülendiği dosya oluyor.

## `-n` örneği:
Loop'tan kastım, sanki;

```ruby
while gets
    # kod...
end
```
çevreler.

## `-s` örneği:
`example_s.rb` adında bir dosyamız olsun ve içinde;
```ruby
print "xyz argümanı kullanıldı\n" if $xyz
```
yazsın. Bu dosyayı çalıştırın;

    ruby example_s.rb

Hiçbir çıktı görmezsiniz. Eğer şu şekilde çalıştırırsanız;

    ruby -s example_s.rb -xyz

şu çıktıyı alırsınız:

    xyz argümanı kullanıldı

## `-S` örneği:
Bazı işletim sistemlerin **Shebang** sorunu olabilir. `#!/usr/bin/env ruby` Bu gibi durumlarda;

```bash
#!/bin/sh
exec ruby -S -x $0 "$@"
```
şekinde, `bash` üzeriden Ruby scripti'ni çalıştırabiliriz.
