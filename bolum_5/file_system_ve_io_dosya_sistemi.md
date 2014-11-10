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

Path içinden dosya adını almak için `basename` kullanırız. Eğer parametre olarak atacağımız şeyi (*örneğin extension olarak .gif, .rb gibi*) geçersek bize sadece dosyanın adını verir.

```ruby
File.basename("/Users/vigo/test.rb")        # => "test.rb"
File.basename("/Users/vigo/test.rb", ".rb") # => "test"
```

Bu işin tersini de `dirname` ile yaparız, yani directory adı gerekince:

```ruby
File.dirname("/Users/vigo/test.rb") # => "/Users/vigo"
```

şekinde kullanırız.

**chmod**, **chown**

Her iki komut da Unix'den gelir. **Change mod** ve **Change owner** işlerini yapmamızı sağlar. `chmod` ile Unix izinlerini ayarlarız:

```bash
-rw-r--r-- 1 vigo wheel 0 Aug 30 19:19 file-01.txt
||||||||||
|||||||||+--- Others, Execute    (x) 1 = 2^0
||||||||+---- Others, Write      (w) 2 = 2^1
|||||||+----- Others, Read       (r) 4 = 2^2
||||||+------ Group, Execute     (x) 1 = 2^0
|||||+------- Group, Write       (w) 2 = 2^1
||||+-------- Group, Read        (r) 4 = 2^2
|||+--------- Owner/User Execute (e) 1 = 2^0
||+---------- Owner/User Write   (w) 2 = 2^1
|+----------- Owner/User Read    (r) 4 = 2^2
+------------ Is Directory?      (d)
```

`file-01.txt` dosyasında, User (yani dosyanın sahibi) **R**ead ve **W**rite hakkına sahiptir. Group ve Others ise sadece **R**ead hakkına sahiptir. Bu durumda varolan bu dosyanin **chmod** değeri:

```bash
Owner/User  : Read, Write  => 4 + 2 = 6
Group       : Read         => 4     = 4
Others      : Read         => 4     = 4
----------------------------------------
644 unix file permission
```

şeklindedir. Hatta Terminal'den; `stat -f '%A' file-01.txt` yaparsak **644** olduğunu da görebiliriz. Şimdi bu dosyayı Ruby ile sadece sahibi tarafından okunur ve yazılır yapıp, başka hiçbir kimse tarafından okunamaz ve yazılamaz hale getirelim:

```ruby
File.chmod(0600, "file-01.txt")
```


**delete**

wip

## IO

wip...