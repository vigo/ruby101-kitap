# Ranges

Başı, sonu olan, tanımlanan belli bir aralıktaki değerleri gösteren nesneler `Range` sınıfındadır. Örneğin 0'la 5 arasındaki sayılar **Range** olarak ifade edilebilir;

```ruby
(0..5)           # => 0..5
(0..5).class     # => Range
(0..5).to_a      # => [0, 1, 2, 3, 4, 5]
(0..5).to_a.join # => "012345"
(0..5).each      # => #<Enumerator: 0..5:each>
```

`(0..5)` ifadesinde 0 ve 5 dahil olmak üzere bir aralık tanımladık. İstediğimiz gibi işleyebiliriz.

```ruby
(-10..0).to_a      # => [-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0]
(0..-10).to_a      # => []
("a".."e").to_a    # => ["a", "b", "c", "d", "e"]
```

Eğer `..` yerine `...` kullanırsak, yani `(0..5)` dersek, 5 hariç demiş oluruz;

```ruby
(0...5).to_a         # => [0, 1, 2, 3, 4]
("a"..."e").to_a     # => ["a", "b", "c", "d"]
```

Son değer hariç mi dahil mi anlamak için `exclude_end?` method'unu kullanırız;

```ruby
(5..10).exclude_end?          # => false
(5...10).exclude_end?         # => true
```

`Range` aslında bir `Class`'dır ve her Class gibi;

```ruby
r = Range.new(0,2)     # => 0..2
r.to_a                 # => [0, 1, 2]
```

kullanılabilir. `==` ya da `eql?` method'u ile karşılaştırılabilir;

```ruby
(0..2) == (0..2)            # => true
(0..2).eql?(0..2)           # => true
(0..2) == (0...2)           # => false
(0..2) == Range.new(0,2)    # => true
(0..2).eql?(Range.new(0,2)) # => true
```

**begin**, **first**, **cover?**, **include?**, **member?**, **end**, **last**

Aralığın başlama değerini almak için `begin` ya da `first` kullanabildiğimiz gibi, `first`'e parametre geçerek, ilk N değeri de okuyabiliriz;

```ruby
(5..10).begin               # => 5
(5..10).first               # => 5
(5..10).first(2)            # => [5, 6]  # ilk 2 değeri ver
```

Belirlediğimiz aralık içinde sorgu yapmak için `cover?` ya da `include?` ya da `member?` method'unu kullanırız. `cover?` kullanırken eğer verdiğimiz değer aralık içindeyse sonuç `true` döner;

```ruby
(5..10).cover?(6)               # => true
(5..10).cover?(4)               # => false
(5..10).cover?(11)              # => false
(5..10).cover?(9)               # => true
("a".."z").cover?("b")          # => true
("a".."z").cover?("1")          # => false
("a".."z").cover?(1)            # => false
("a".."z").cover?("abc")        # => true
```

`include?` ile `cover?` arasındaki fark ise şudur; `cover?`a verilen parametredeki değer, örnekteki `abc` teker teker range içinde var mı? yani `a` var mı? `b` var mı? `c` var mı? şeklinde olurken, `include?` da `abc` var mı? şeklindedir;

```ruby
("a".."z").cover?("abc")        # => true
("a".."z").include?("abc")      # => false

("a".."z").cover?("a")        # => true
("a".."z").include?("a")      # => true
```

Tahmin edeceğiniz gibi, `end` ile son değeri alırız.

```ruby
(5..10).end                 # => 10
(5...10).end                # => 10
```

`last` ile de aynı `first` deki gibi son ya da son N değeri okuruz;

```ruby
(5..10).last                  # => 10
(5..10).last(3)               # => [8, 9, 10]
```

**min**, **max**, **size**

`min` ve `max` ile tanımlı aralıktaki en büyük/küçük değeri alırız, `size` bize boyu verir;

```ruby
(5..10).min        # => 5
(5..10).max        # => 10
(5..10).size       # => 6
```

**step** ile kaçar kaçar artacağını veririz;

```ruby
r = Range.new(0, 5)
r.step(2) # => #<Enumerator: 0..5:step(2)>
r.step(2).to_a # => [0, 2, 4]
```
