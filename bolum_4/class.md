# Class

Ruby, **Object-Oriented** (*OO*) bir dil olduğu için, methodları değişkenleri ve benzeri şeyleri içinde barından bir tür taşıyıcıya ihtiyaç duyar. İşte bu taşıyıcıya **Class** ya da sınıf diyoruz. Aslında ben **tür** demeyi tercih ediyorum sınıf yerine.

Zaten önceki konularda **Class Methods**, **Instance Methods** gibi kavramlara girmiştik.

Class'lar birbirinden türeyebilir (*Hani* `class.superclass` *şeklinde anlalizler yapmıştık*)

Teknik olarak bir dosyada birden fazla Class tanımlaması yapılabilir. Örneğin, `my_class.rb` adlı bir dosya içinde farklı farklı Class tanımlamaları olabilir;

```ruby
class MyClass
  end

class OtherClass
end

a = MyClass.new    # => #<MyClass:0x007ffa2b09b758>
b = OtherClass.new # => #<OtherClass:0x007ffa2b09b3c0>
```

Class tek başına bir nesne (*Obje*) bu bakımdan **instantiate** etmeseniz bile (*Yani* `a = Array.new` *gibi*) kullanabileceğiniz birşeydir.

Class'ların diğer bir özelliği de açık olmasıdır. Ruby'nin bence en harika özelliği, **built-in** yani dilin çekirdeğinden gelen Class'lara bile method / property eklemeniz mümkündür. Bu konuları **Monkey Patching** kısmında detaylı göreceğiz.

En basit tanımıyla Class aşağıdaki gibidir:

```ruby
class Merhaba
  def initialize(isim)
    @isim = isim
  end

  def selam_sana
    "Selam sana, #{@isim}"
  end
end

hey = Merhaba.new "Uğur"
hey.selam_sana # => "Selam sana, Uğur"
```

`initialize` methodu, Class'dan ilk örnek türedildiğinde (*yani bir instance oluşturulduğunda*) tetiklenir ve bir tür Class ile ilgili ön tanımlamaların yapıldığı alan konumundadır. Benzer dillerdeki **class constructor**'ı gibi düşünülebilir.

`@isim` ise **Instance Variable** yani Class'tan türeyen nesneye ait değişkendir.

`selam_sana` ise bu Class'ın bir method'udur. `hey` değişkeni, `Merhaba` Class'ından türemiş bir **Instance**'dır. `Merhaba` sınıfındaki tüm method'lar **inherit** hani miras olarak `hey` nesnesine geçmiştir.

---

# Module

Class'a benzeyen ama Class gibi **instantiate** edilemeyen şeydir modül. Modül denen şeye Class'e eklenebilir (*include edilir*) Modülden gelen methodlar artık ilgili Class'ın methodu haline gelir.

Yani düşününki bir Class var, bu Class'ın farklı 2-3 Class'tan özellik almasını istiyorsunuz. Bunu başarmak için ilgili Class'a o 2-3 Class'ı Modül olarak ekliyorsunuz!