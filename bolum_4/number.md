# Number (Sayılar)

Sayılar, temel öğe olmayıp, direk nesneden türemişlerdir. Türedikleri nesne de Ruby'nin sayılar için olan **base class**'ıdır.

Örneğin **3** sayısına bakalım:

```ruby
3.class # => Fixnum
3.class.superclass # => Integer
3.class.superclass.superclass # => Numeric
3.class.superclass.superclass.superclass # => Object
```

`Numeric` sınıfıdır asıl olan. İlk türediği sınıfı `Fixnum`dır. Örnekte gördüğünüz gibi;

`Fixnum > Integer > Numeric` şeklinde bir hiyeraşi söz konusudur.
