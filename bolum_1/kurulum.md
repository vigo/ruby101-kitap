# Kurulum

## OSX

Eğer Mac OSX kullanıyorsanız ilk etapta hiçbir şeye ihtiyacınız yok, çünkü Mac OSX'de Ruby hazır kurulu olarak geliyor.

OSX Mavericks (10.9.4) Ruby sürümü : `ruby 2.1.0p0 (2013-12-25 revision 44422) [x86_64-darwin13.0]`

## Linux

[Debian](http://debian.org) ve [Ubuntu](http://ubuntu.com) kullanan okuyucularımız

```bash
sudo apt-get install ruby # ya da
sudo aptitude install ruby
```
[CentOS](https://www.centos.org/), [Fedora](http://fedoraproject.org/), ya da [RedHat](http://www.redhat.com/) kullananlar:

```bash
sudo yum install ruby
```

[Gentoo](http://www.gentoo.org/) kullananlar;
```bash
sudo emerge dev-lang/ruby
```

## Kaynaktan Kurulum

Ruby'nin sitesinden [tar dosyasını](https://www.ruby-lang.org/en/downloads/) indirip;
```bash
./configure
make
sudo make install
```
şeklinde de kurulum yapabilirsiniz.

## Windows

[Bu siteden](http://rubyinstaller.org/) özel Windows için hazırlanmış Ruby kurulum paketini indirip klasik "next" > "next" diyerek kurulum yapabilirsiniz.


## Ruby Versiyon Yöneticileri

Bazen kullandığınız hazır kütüphaneler'in destekledikleri Ruby versiyonlarındaki kısıtlamalar, kişisel tercihiniz gibi farklı nedenlerle birden fazla Ruby sürümü ile çalışmak isteyebilirsiniz. Projelerinizden biri, örneğin `ruby 1.9.3` kullanırken, diğer bir projeniz `ruby 2.1.0` kullanıyor olabilir. Bu anlarda kullandığınız Ruby versiyonunu kolayca değiştirmek, aslında aktive etmek de diyebiliriz, için 2 adet popüler versiyon yöneticisi bulunmaktadır.

### Rbenv

[Rbenv](https://github.com/sstephenson/rbenv) meşhur [37 Signals](http://37signals.com/)'ın. Aslında orada çalışan [Sam Stephenson](https://github.com/sstephenson) tarafından geliştirilmiş bir araç.

Eğer OSX ve [Homebrew](http://brew.sh) kullanıyorsanız kurulum çok kolay:

```bash
brew install rbenv ruby-build
```

Eğer farklı bir işletim sistemi kullanıyorsanız (Linux/Unix tabanlı)

```bash
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv

# sonra PATH'e ekleyin
export PATH="$HOME/.rbenv/bin:$PATH"

# açılışa bunuda ekleyin
# hangisini kullanıyorsanız (.bashrc, .profile ya da .bash_profile)
eval "$(rbenv init -)"
```

Kurulumdan sonra istediğini Ruby versiyonu için;

```bash
# kurulabilecek versiyonları göster
rbenv install -l

# ruby 2.1.1'i kuralım
rbenv install 2.1.1
```

Kurulan Ruby'i

* Sistem genelinde `rbenv global`
* Sadece bulunduğumuz dizin içinde (Uygulamaya Özel) `rbenv local`
* Anlık, sadece Shell'de `rbenv shell`

aktive etme opsiyonlarımız var. Örneğin proje dizinin içine `.ruby-version` dosyası koyar ve içine de hangi versiyonu kullandığımızı yazarsak o dizine geçtiğimiz an Ruby versiyonu değişir.

Yani **A** projesinde versiyon `2.1.1`, **B** projesinde version `1.9.3` kullanmak için;

```bash
cd ~/projelerim/A/
echo "1.9.3" > .ruby-version

cd ~/projelerim/B/
echo "2.1.1" > .ruby-version

# bakalım hangi versiyonu aktive etmişiz?
rbenv version
```

### RVM

Adındanda anlaşılacağı gibi **R**uby **V**ersion **M**anager yani [RVM](https://rvm.io/) de aynı Rbenv gibi Ruby versiyonalarını kolay yönetmeyi sağlıyor. Ruby dünyasından Rbenv'ciler ve RVM'ciler olarak iki kanat olduğunu söyleyebilirim.

Kurulumu da zor değil:

```bash
gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable
```

Rbenv'den en büyük farklığı **Gem Set** yani proje bazlı Ruby paketi yönetimi özelliği.

Ben Rbenv'ci olduğum için RVM kullanmıyorum. Özellike yeni başlayanlar için RVM'i öneriyorum, Rbenv'e göre daha kolay kurulumu ve kullanımı var.
