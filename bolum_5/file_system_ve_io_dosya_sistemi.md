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

**absolute_path**

```ruby
File.absolute_path("~")               # => "/~"
File.absolute_path(".gitignore", "~") # => "/~/.gitignore"
```

**atime**, **ctime**

wip

**basename**, **dirname**

wip

**chmod**, **chown**

wip

**delete**

wip

## IO

wip...