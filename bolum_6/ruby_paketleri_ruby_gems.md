# Ruby paketleri: RubyGems

Ruby, benzeri diğer dillerdeki gibi kendine ait bir paket yöneticisine sahiptir. Ruby paketlerine **Gem** denir. Gem'ler aslında tekrar tekrar kullanılabilecek Ruby kodlarının paketlenmiş halleridir ve herhangibir Ruby uygulamasına çok kolay entegre edilebilir.

Genelde tüm paketler http://rubygems.org sitesinde sunulur. İster local (*yerel*) ister özel repository isterseniz de RubyGems sitesinden bu paketleri kurabilirsiniz.

Ruby kurulumunda `gem` adında bir komut eklenir sisteme. Eğer `gem --help` derseniz, ilgili kullanımları ve komutları listeleyebilirsiniz.

RubyGems, efsane isim [Jim Weirich](http://en.wikipedia.org/wiki/Jim_Weirich) tarafından yazılmıştı. Kendisi 2014 Şubat'ta aramızdan ayrıldı.

Herhangi bir paketi kurmak için `gem install PAKET_ADI`şeklinde yazmak yeterli fakat genelde Ruby'nin kurulu olduğu yer sistem dosyalarının kurulu olduğu yerde olduğu için eğer Ruby versiyon yöneticisi (rvm ya da rbenv) kullanmıyorsanız `sudo` ile ile işlem yapmanı gerekir: `sudo gem install PAKET_ADI`.

wip...