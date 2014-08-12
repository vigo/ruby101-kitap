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

```ruby
2014.class                  # => Fixnum
2_014.class                 # => Fixnum
201.4.class                 # => Float
1.2e3.class                 # => Float
7e4.class                   # => Float
7E-4.class                  # => Float
0664.class                  # => Fixnum
0xfff.class                 # => Fixnum
0b1111.class                # => Fixnum
45678327041234321312.class  # => Bignum
```

Ruby'de sayı işlerinde `_` hiçbir etki yapmaz. Bir şekilde okumayı kolaylaştırmak için kullanılır. Örnekteki **2014** ile **2_014** aynı şeydir. Büyük sayıları yazarken; `1_345_201` gibi bir ifade `1345201`'dir aslında.

Ondalık sayılarda `.` kullanılır. **Octal** yani 8'lik sayı sistemi için direk `0` ile `0664` gibi kullanılır. 16'lık yani **Hexadecimal** sayı sistemi için, css dünyasından tanıdığınız **beyaz** rengini ifade etmek için `$fff` yerine `0xfff` şeklinde bir kullanım mevcut. 2'lik yani **Binary** sayı sistemi için `0b` ile başlamak yeterlidir. **Scientific Notation** ifadeleri için de `1.2e3`ya da `7e4`gibi kullanımlar mümkündür.
