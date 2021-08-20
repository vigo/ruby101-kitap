# Number (Sayılar)

Sayılar, temel öğe olmayıp, direk nesneden türemişlerdir. Türedikleri nesne de
Ruby'nin sayılar için olan **base class**'ıdır.

Örneğin **3** sayısına bakalım:

```ruby
3.class # => Fixnum
3.class.superclass # => Integer
3.class.superclass.superclass # => Numeric
3.class.superclass.superclass.superclass # => Object
```

`Numeric` sınıfıdır asıl olan. İlk türediği sınıfı `Fixnum`dır. Örnekte
gördüğünüz gibi;

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

Ruby'de sayı işlerinde `_` hiçbir etki yapmaz. Bir şekilde okumayı
kolaylaştırmak için kullanılır. Örnekteki **2014** ile **2_014** aynı şeydir.
Büyük sayıları yazarken; `1_345_201` gibi bir ifade `1345201`'dir aslında.

Ondalık sayılarda `.` kullanılır. **Octal** yani 8'lik sayı sistemi için direk
`0` ile `0664` gibi kullanılır. 16'lık yani **Hexadecimal** sayı sistemi için,
css dünyasından tanıdığınız **beyaz** rengini ifade etmek için `$fff` yerine
`0xfff` şeklinde bir kullanım mevcut. 2'lik yani **Binary** sayı sistemi için
`0b` ile başlamak yeterlidir. **Scientific Notation** ifadeleri için de
`1.2e3`ya da `7e4`gibi kullanımlar mümkündür.

## Number Method'ları

**5** sayısı `Fixnum` sınıfındandır ve neticede üst sınıfları;

    Numeric -> Integer -> Fixnum

şeklinde olduğu için (_en üsttte Numeric_) ilgili üst sınıfların method'ları
da `Fixnum` tarafından kullanılabilir durumdadır.

Her zamanki gibi, **acaba bu sınıfa ait methodlar neymiş?** dediğimiz an bir
ton method gelir karşımıza;

```ruby
5.methods # => [
    :to_s, 
    :inspect, 
    :-@, 
    :+, 
    :-, 
    :*, 
    :/, 
    :div, 
    :%, 
    :modulo, 
    :divmod, 
    :fdiv, 
    :**, 
    :abs, 
    :magnitude,
    :==,
    :===,
    :<=>,
    :>,
    :>=,
    :<,
    :<=,
    :~,
    :&,
    :|,
    :^,
    :[],
    :<<,
    :>>,
    :to_f,
    :size,
    :bit_length,
    :zero?,
    :odd?,
    :even?,
    :succ,
    :integer?,
    :upto,
    :downto,
    :times,
    :next,
    :pred,
    :chr,
    :ord,
    :to_i,
    :to_int,
    :floor,
    :ceil,
    :truncate,
    :round,
    :gcd,
    :lcm,
    :gcdlcm,
    :numerator,
    :denominator,
    :to_r,
    :rationalize,
    :singleton_method_added,
    :coerce,
    :i,
    :+@,
    :eql?,
    :remainder,
    :real?,
    :nonzero?,
    :step,
    :quo,
    :to_c,
    :real,
    :imaginary,
    :imag,
    :abs2,
    :arg,
    :angle,
    :phase,
    :rectangular,
    :rect,
    :polar,
    :conjugate,
    :conj,
    :between?,
    :nil?,
    :=~,
    :!~,
    :hash,
    :class,
    :singleton_class,
    :clone,
    :dup,
    :taint,
    :tainted?,
    :untaint,
    :untrust,
    :untrusted?,
    :trust,
    :freeze,
    :frozen?,
    :methods,
    :singleton_methods,
    :protected_methods,
    :private_methods,
    :public_methods,
    :instance_variables,
    :instance_variable_get,
    :instance_variable_set,
    :instance_variable_defined?,
    :remove_instance_variable,
    :instance_of?,
    :kind_of?,
    :is_a?,
    :tap,
    :send,
    :public_send,
    :respond_to?,
    :extend,
    :display,
    :method,
    :public_method,
    :singleton_method,
    :define_singleton_method,
    :object_id,
    :to_enum,
    :enum_for,
    :equal?,
    :!,
    :!=,
    :instance_eval,
    :instance_exec,
    :__send__,
    :__id__,
]
```

Bunların içinden en sık kullanılanlara ve kullanım şekillerine değineceğim.

```ruby
5.to_s # => "5"
# Sayısal değeri String'e çevirdik

-5.abs # => 5
# Mutlak değer

5.zero? # => false
# Sıfır mı?

5.even? # => false
# Çift sayı mı?

5.odd?  # => true
# Tek sayı mı?

5.next # => 6
# Sonraki sayı?

5.pred # => 4
# Önceki sayı?

3.14.floor # => 3
# Taban değeri

3.14.ceil # => 4
# Tavan değeri

1.49.round # => 1
1.51.round # => 2
# Yuvarlama

1.bit_length # => 1
15.bit_length # => 4
255.bit_length # => 8
# Bit cinsinden uzunluğu/boyu

1.size        # => 8
10.size       # => 8
10242048.size # => 8
1024204810242048102420481024.size # => 12
# Byte cinsinden kapladığı yer
```

`upto`, `downto` gibi iterasyonla ilgili olanları **Enumeration ve Iteration**
bölümünde göreceğiz!
