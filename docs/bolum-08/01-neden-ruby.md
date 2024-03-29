# Neden Ruby?

Yazılım hayatıma [Commodore 16](http://en.wikipedia.org/wiki/Commodore_16)
BASIC dili ile başladım. Daha sonra 
[Commodore 64](http://en.wikipedia.org/wiki/Commodore_64)’e geçtim, Assembly (Makine
Dili), daha sonra da [Amiga](http://en.wikipedia.org/wiki/Amiga)’da da
Assembly’ye devam ettim. Bir gün geldi ve makine dili çöp oldu :) Teknoloji
değişti :)

90’lı yıllarda [Windows](http://en.wikipedia.org/wiki/Microsoft_Windows) ve
[ASP](http://en.wikipedia.org/wiki/Active_Server_Pages) diliyle tanıştım.
ASP’nin ilk günleri, [VBScript](http://en.wikipedia.org/wiki/VBScript) :)
Uzunca bir süre ASP ile yola devam ettikten sonra
[PHP](http://en.wikipedia.org/wiki/PHP) ile tanıştım ve "oooh be DÜNYA varmış"
dedim.

PHP ile hatırı sayılır pek çok proje yaptım. Daha sonra, sevgili kardeşim
[Fırat Can Başarır](https://twitter.com/firat)’ın yönlendirmesiyle
[Python](http://en.wikipedia.org/wiki/Python_\(programming_language\)) ve
[Django](http://en.wikipedia.org/wiki/Django_\(web_framework\)) ile tanıştım.
Özellikle web development işleriyle uğraşan biri, PHP’den sonra Python ve
Django’yu görünce hakikatten aklı duruyor!

Özellikle Django ile birlikte gelen **Admin Panel** olayı insanın tabiri
caizse dibini düşürüyor! Daha sonra anlaşılıyor ki, bu panel, sadece
**developer** için. Yani sadece geliştirme amaçlı. İlk anda "ooof, bununla
hemen hızlıca işleri çıkartırım" diyorsunuz haklı olarak. Teoride mümkün de.

Ancak büyük bir işe kalkıştığınızda bu Admin Panel kabusunuz oluyor ve işi
gücü bırakıp sadece bu paneli **customize** (*yani özelleştirme*) etmekle
uğraşıyorsunuz.

Keza Python 2 mi? 3 mü? gibi durumlar da söz konusu. Python 3 neredeyse
tamamen farklı. An itibariyle (*30 Kasım 2014*) halen
[Django](https://docs.djangoproject.com/en/1.7/topics/python3/) ve
[Python3](https://docs.python.org/3/) desteği resmi olarak gelmiş değil.

---

**20 Ağustos 2021**

Django version 3+ oldu, Python 3 default oldu. Python 3.9 var, Python 4 yolda :)

---

Bunun dışında Python topluluğu çok ağır hareket ediyor. Yeni çıkan bir
servisin modül olarak hazırlanması ya da en son 3 sene önce güncellenmiş,
yüzlerce pull-request’in beklediği GitHub projeleri mi istersiniz?

Bu konular beni geliştirici olarak zorladı. Amacım hızla işimi düzgün bir
şekilde yapıp yoluma devam etmekten başka bir şey değil.

## Programming is Fun!

Yani **Programlama eğlencelidir!** ne demek bu? İlk bakışta insan düşünüyor,
programlama dilinin neresi eğlenceli olabilir ki? Neticede bilimsel bir işlem
diye düşünüyor insan. Taa ki Ruby ile uğraşmaya başlayana kadar...

İlk dikkat çeken şey, Ruby dilinin insan diline çok benzemesi. Yani İngilizce
bilen biri için okunması çok kolay. Hiç Ruby bilmeyen biri bile rahatlıkla ne
tür bir işlem olduğunu anlayabilir.

Daha önce hiçbir programlama dilinde rastlamadığım bir `if` kullanımı:

```ruby
puts "vigo" if a > 2
```

Bu şu demek, eğer `a`’nın değeri `2`’den büyükse ekrana `vigo` yaz. Asp,
Python, Php, Perl, C gibi dillerde genelde `if` bloğu içinde ya da **oneline**
yani tek satırda ifade şeklinde olurken ilk kez Ruby’de `if` koşulunun bu
şekilde kullanıldığını gördüm.

`unless` kelime anlamı olarak **if not** anlamındadır. Yani eğer `x`’in değeri
`false` ise şunu yap derken:

```ruby
puts "vigo" unless x
```

gibi bir kullanım söz konusu. yani aşağıdaki gibi de kullanılabilir ama
mantıksal olarak tercih edilmez:

```ruby
puts "vigo" if !x
```

Keza method isimleri, standart kütüphane ile gelen özellikler gayet akılda
kalıcı ve mantıklı. Ruby sizin adınıza pek çok şeyi düşünüp hazır kullanıma
sunuyor.

> Acaba bugün günlerden cuma mı?

Hemen bakalım:

```ruby
Time.now.friday? # => true # evet
```

Aslında çok basit ama çok işe yarayan bir özellik. Bu tarz konulara
**Syntactic Sugar** deniliyor.

## Çok Güçlü ve Çalışkan Ruby Topluluğu

Ne yazık ki ülkemizde çok da bilinen ya da tercih edilen bir dil olmamasına
rağmen, dünyada durum çok farklı. Pek çok tanıdık proje Ruby, Ruby on Rails,
Sinatra gibi Ruby dünyasının araçlarını kullanmakta.

Python’dan gelen bir geliştirici olarak, yaşadığım en büyük sıkıntılardan biri
de yavaş ilerleme durumuydu. Python’u ben Alman Mühendisliğine benzetiyorum.
Her şey inanılmaz kurallı, süper sistemli olmak zorunda. Tamam bu çok güzel
bir yaklaşım, kabul ediyorum ama bazen ağır kalıyor.

Bazen öyle bir Python modülüne ihtiyacınız oluyor ve bir bakıyorsunuz son 3
yıldır güncelleme yapılmamış, GitHub’da bekleyen 50 tane Pull Request, kodu
kimin maintain ettiği belli değil, uzay boşluğunda kendi kendine giden bir
durumda kaderini bekliyor.

Ruby topluluğu inanılmaz derecede üretken. Yeni bir API’mı çıktı? Hemen
gem’ini bulmanız mümkün! Ruby mi öğrenmek istiyorsunuz? Tonlarca
ücretsiz/ücretli online videolar, eğitimler var! Kitap, kod, konferans
aklınıza ne gelirse...

Ne yazık ki bu denli aktif bir dünyayı ben diğer uğraştığım dillerde göremedim.

## Bir işi yapmanın pek çok farklı yolu olabilir!

Son olarak, Ruby’deki en çok hoşuma giden mentaliteden bahsetmek istiyorum.
Bir işi doğru yapmanın birden fazla yolu olabilir.

Örneğin Python’da sadece "tek bir yol" varken Ruby’de farklı farklı
yöntemlerle, komutlarla aynı işi değişik şekillerde yapabilirsiniz ve hepsi de
doğrudur!

Neredeyse hiçbir programlama dili **DSL** (yani Domain Specific Language)
yapmak için bu denli müsait değil. Yani Ruby kullanarak kafanıza göre Ruby
üzerinde çalışan başka bir dünya yapabilirsiniz!

Neticede hayatta her şey tercihler ve zevklerle ilgilidir. Ben kendi
nedenlerimi belirtiyorum, bunlar size uymayabilir, hatta nefret bile
edebilirsiniz. Eğer böyle bir durum varsa, sizden ricam sakin bir şekilde Ruby
dünyasını incelemeniz.

An itibariyle ne ile uğraşıyorsanız aynı şeyi Ruby ile yapmaya çalışmanız.
Mutlaka deneyin. Denedikten, tadına baktıktan sonra karar verin.

Sevgili annem küçükken hep derdi "önce bi tadına bak ondan sonra istemiyorsan
yemeği yeme" diye :)


## Güncel: 20 Ağustos 2021

An itibariyle gece gündüz [Golang](https://golang.org/) yazıyorum, yanında da
[Django](https://www.djangoproject.com/). Hangi dilde kod yazarsam yazayım
mutlaka içeride bir `Rakefile` oluyor ve ben Ruby kaslarımı hep çalıştırmaya
devam ediyorum...
