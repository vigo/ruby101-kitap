# File System ve IO (Dosya Sistemi)

## File

`IO` sınıfından özelliklerler içeren `File` sınıfı, fiziki dosyalarla işlem yapmamızı sağlan özellikleri sunar bize. Ruby'nin üzerinde çalıştığı işletim sistemine göre de **file permission** yani dosya üzerindeki yetki sistemi de devrededir.

Default olarak gelen Constant'ları:

```ruby
File.constants # => [:Separator, :SEPARATOR, :ALT_SEPARATOR, :PATH_SEPARATOR, :Constants, :Stat, :WaitReadable, :WaitWritable, :EAGAINWaitReadable, :EAGAINWaitWritable, :EWOULDBLOCKWaitReadable, :EWOULDBLOCKWaitWritable, :EINPROGRESSWaitReadable, :EINPROGRESSWaitWritable, :SEEK_SET, :SEEK_CUR, :SEEK_END, :RDONLY, :WRONLY, :RDWR, :APPEND, :CREAT, :EXCL, :NONBLOCK, :TRUNC, :NOCTTY, :BINARY, :SYNC, :DSYNC, :NOFOLLOW, :LOCK_SH, :LOCK_EX, :LOCK_UN, :LOCK_NB, :NULL, :FNM_NOESCAPE, :FNM_PATHNAME, :FNM_DOTMATCH, :FNM_CASEFOLD, :FNM_EXTGLOB, :FNM_SYSCASE]

File::ALT_SEPARATOR  # => nil # Bu Ruby’nin çalıştığı platforma özeldir
File::PATH_SEPARATOR # => ":"
File::SEPARATOR      # => "/"
File::Separator      # => "/"
```

Örneğin Windows'da çalışan Ruby'de `SEPARATOR` ters slash `\` şeklinde gelecektir.

## Public Class Method'ları

**absolute_path**, **expand_path**

String olarak verilen path bilgisini **absolute path**'e çevirir. Eğer ikinci parametre verilmezse **CWD** (*current working directory*) yani o an için içinde çalıştığınız directory bilgisi kullanılır.

```ruby
File.absolute_path("~")               # => "/~"
File.absolute_path(".gitignore", "~") # => "/~/.gitignore"
```

`expand_path` de bir nevi absolute path'e çevirir:

```ruby
File.expand_path("~/.gitignore") # => "/Users/vigo/.gitignore"
```

Keza daha kompleks path bulma işlerinde de kullanılır. Bu durumda `__FILE__` sabiti hayatımızı kolaylaştırır. O an Ruby script'inin çalıştığı dosyanın path'i `__FILE__` sabitindedir. Örneğin aşağıdaki gibi bir directory yapısı olsa:

    proje/
    ├── lib
    │   └── users.rb
    └── main.rb

ve `lib/users.rb` içinden, dışarıda bulunan `main.rb` dosyasının path'ine ulaşmak istesek;

```ruby
File.expand_path("../../main.rb", __FILE__)
```

şeklinde kullanırız.

**atime**, **ctime**

Dosyaya son erişilen tarihi `atime` ile, dosyada yapılmış olan son değişiklik tarihini de `ctime` ile alırız:

```ruby
File.atime("/Users/vigo/.gitignore") # => 2014-11-05 11:45:10 +0200
File.ctime("/Users/vigo/.gitignore") # => 2014-08-04 11:33:14 +0300
```

**basename**, **dirname**

wip

**chmod**, **chown**

wip

**delete**

wip

## IO

wip...