# Object (Nesne)

Ruby, **Object Oriented Programming**'in dibidir :) Herşey nesnelerden oluşur. Nasıl mı? hemen basit bir değişken oluşturup içine bir tekst yazalım.

    mesaj = "Merhaba"

`mesaj` değişkeninin türü ne?

    mesaj.class # => String

Bu değişken **String** nesnesinden türemiş. Peki, **String** nereden geliyor?

    mesaj.class.superclass # => Object

Dikkat ettiyseniz burada `superclass` kullandık. Yani bu hiyeraşideki bir üst sınıfı arıyoruz. Karşımıza ne çıktı? **Object**. Peki acaba **Object** nereden türemiş?

    mesaj.class.superclass.superclass # => BasicObject

Hmmm.. Peki **BasicObject** nereden geliyor?

    mesaj.class.superclass.superclass.superclass # => nil

İşte şu anda dibi bulduk :) Demekki hiyeraşi;

    BasicObject > Object > String

şeklinde bir hiyeraşi söz konusu.

Peki, sayılarda durum ne?

    numara = 1
    numara.class # => Fixnum
    numara.class.superclass # => Integer
    numara.class.superclass.superclass # => Numeric
    numara.class.superclass.superclass.superclass # => Object
    numara.class.superclass.superclass.superclass.superclass # => BasicObject
    numara.class.superclass.superclass.superclass.superclass.superclass # => nil

Ufff... bir an için bitmeyecek sandım :) Çok basit bir sayı tanımlası yaptığımızda bile, arka plandaki işleyiz yukarıdaki gibi oluyor. Yani

    BasicObject > Object > Numeric > Integer > Fixnum

şeklinde yine ana nesne **BasicObject** olmak koşuluyla uzun bir hiyeraşi söz konusu.

Herşey **BasicObject** den türüyor, bu yüzden de aslında herşey bir **Class** dolayısıyla bu durum dile çok ciddi esneklik kazandırıyor.
