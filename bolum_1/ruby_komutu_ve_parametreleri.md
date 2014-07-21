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
| -i | -- |
| -I | -- |
| -l | -- |
| -n | -- |
| -p | -- |
| -r | -- |
| -s | -- |
| -S | -- |
| -T | -- |
| -v, --verbose | -- |
| -w | -- |
| -W | -- |
| -x | -- |
| --copyright | -- |
| --enable= | -- |
| --version | -- |
| --help | -- |


