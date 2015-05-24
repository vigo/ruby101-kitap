# Ruby Hakkında

1990'lı yılların ortalarında (1995) **Yukuhiro "Matz" Matsumoto** tarafından geliştirilen Ruby, günümüzde en çok kullanılan [açık-kaynak](https://github.com/ruby/ruby) yazılımların başında geliyor.

> Üretkenlik (az kod, çok iş) ve basitliğe odaklı, dinamik, açık-kaynak programlama dili. Okuması ve yazması kolay, anlaşılabilir nitelikte!

Dilin en büyük esin kaynakları tabii ki yine varolan diller. Bunlar; Perl, Smalltalk, Eiffel, Ada ve Lisp dilleri.

İlk kararlı (stable) sürümü 1995'de yayınlanan Ruby'nin geliştiricilerin tam anlamıyla dikkatini çekmesi **2006** yılına kadar sürdü. Keza ilk versiyonları gerçekten çok yavaş ve sıkıntılıydı.

Ruby en büyük patlamasını [Ruby on Rails](http://rubyonrails.org/) framework'ü ile yaptı. Danimarkalı yazılımcı [@dhh](http://twitter.com/dhh)'in (_David Heinemeier Hansson_) yayınladığı bu framework ne yazık ki Ruby dilinin önüne bile geçti.

Kitabı yazdığım an itibariyle (_13 Temmuz 2014, Pazar_) Ruby'nin en son sürümü `2.1.2` (Stabil sürüm)

* Güncelleme: Ruby versiyon `2.1.3` oldu. (*26 Ekim 2014, Pazar*)
* Güncelleme: Ruby versiyon `2.1.5` oldu. (*6 Aralık 2014, Pazar*)
* Güncelleme: Ruby versiyon `2.2.0` oldu. (*25 Aralık 2014*)
* Güncelleme: Ruby versiyon `2.2.1` oldu. (*3 Mart 2015*)
* Güncelleme: Ruby versiyon `2.2.2` oldu. (*1 Mayıs 2015*)


Ruby'nin en önemli özelliği her şeyin bir nesne yani **Object** olmasıdır. Nesneyi bir tür paket / kutu gibi düşünebilirsiniz. Doğal olarak, **Object** yani nesne olan bir şeyin, action'ları / method'ları da olur.

Ruby'nin yaratıcısı **Matz** şöyle demiş:

> **Perl** dilinden daha güçlü, **Python** dilinden daha object-oriented bir script dili olmasını istedim.

Pek çok programlama dilinde sayılar primitive (ilkel/basit) tiplerdir, nesne değildirler. Halbuki Ruby'de sayılar dahil herşey nesnedir. Yani sayının method'ları vardır :)

[Örnek](https://www.ruby-lang.org/en/about/)

```ruby
class Numeric
  def topla(x)
    self.+(x)
  end
end

5.topla(6)  # => 11
5.topla(16) # => 21
```

Sayılara (yani Numeric tipine) `topla` diye bir method ekledik...

Block ve Mixin ise Ruby'nin yine öne çıkan özelliklerindendir. Block denen şey aslında [closure](http://en.wikipedia.org/wiki/Closure_%28computer_programming%29)'dur. Herhangi bir method'a block takılabilir:

```ruby
search_engines = %w[Google Yahoo MSN].map do |engine|
  "http://www." + engine.downcase + ".com"
end

search_engines # => ["http://www.google.com", "http://www.yahoo.com", "http://www.msn.com"]
```

Bu örnekte `%w` string'i boşluklarından ayırarak bir array (dizi) formatına çeviri. Yani sonuçta `%w[Google Yahoo MSN]` dediğimizde elimize `["Google", "Yahoo", "MSN"]` dizisi gelir. `map` metodu bize bu diziden bir `Enumarator` dönecektir ve `do/end` kısmı ise bizim `block` kısmımızdır. Bu konuda daha açıklayıcı bilgiyi 3. bölümdeki Bloklar başlığı altında bulacaksınız.

Ruby'de bir **Class** (sınıf) sadece tek bir sınıftan türeyebilir. Yani A class'ı B'den türer ama aynı anda hem B'den hem C'den türeyemez. Bu Python'da mümkün olan bir şeydir. Ruby'de ise bunun üstesinden gelmek için class'lar **Module**'leri kullanır. Bir Class N tane Module içerebilir, işte bu tür nesnelere **Mixin** denir:

```ruby
class MyArray
  include Enumerable
end
```

Ortak kullanılacak metodları ya da değişkenleri ayrı bir Module olarak tasarlayıp, gerektiği yerde `include` ederek Class + Module karışımından oluşan Mixin'ler ortaya çıkar.

Diğer dillerdeki gibi Exception Handling, Garbage Collector özelliklerinin yanı sıra, C-Extension'ı yazmak diğer dillere göre daha kolaydır. İşletim sisteminden bağımsız **threading** imkanı sunmaktadır. Pek çok işletim sisteminde Ruby kullanmak mümkündür: Linux / Unix / Mac OS X / Windows / DOS / BeOS / OS/2 gibi...

Ruby'den türemiş farklı Ruby uygulamaları da var:

[JRuby](http://jruby.org/), [Rubinius](http://rubini.us/), [MacRuby](http://www.macruby.org/), [mruby](http://www.mruby.org/), [IronRuby](http://www.ironruby.net/), [MagLev](http://ruby.gemstone.com/), [Cardinal](https://github.com/parrot/cardinal)

Son olarak, **Test Driven Development** yani test'le yürüyen geliştirme mentalitesinin en iyi oturduğunu düşündüğüm dillerden biri Ruby'dir. Çok güzel test kütüphaneleri var ve nasıl kullanabileceğimize dair tonlarca blog/video sitesi de mevcut!

> Ruby ile programlamak eğlencelidir!

diyor Matz, haydi o halde biz de bu eğlenceye katılalım :)
