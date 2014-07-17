# Ön Tanımlı Değişkenler
Ruby, bir kısım ön tanımlı yani **Predefined** değişkenlerle geliyor. Değişkenlerin ne olduğunu;

```ruby
puts global_variables
```
şeklinde görebiliriz. Önden bu bilgileri vermek zorundayım, daha geniş kullanımı ve tam olarak ne işe yaradıklarını ileriki bölümlerde daha iyi anlayacaksınız.

| Değişken | Açıklama |
| -- | -- |
| $! | `raise` ile atanan **exception** bilgisidir. `rescue`ile erişilir. |
| $@ | -- |
| $& | -- |
| $\` | -- |
| $' | -- |
| $+ | -- |
| $1, $2, ..., $9 | -- |
| $~ | -- |
| $= | -- |
| $/ | -- |
| $\ | -- |
| $, | -- |
| $; | -- |
| $. | -- |
| $< | -- |
| $> | -- |
| $_ | -- |
| $0 | -- |
| $* | -- |
| $$ | -- |
| $? | -- |
| $: | -- |
| $" | -- |
| $DEBUG | -- |
| $KCODE | -- |
| $default | -- |
| $F | -- |
| $FILENAME | -- |
| $LOAD_PATH | -- |
| $SAFE | -- |
| $stdin | -- |
| $stdout | -- |
| $stderr | -- |
| $VERBOSE | -- |
| $-0 | -- |
| $-a | -- |
| $-d | -- |
| $-F | -- |
| $-i | -- |
| $-I | -- |
| $-l | -- |
| $-p | -- |
| $-K | -- |
| $-v | -- |
| $-w | -- |
| $-W | -- |
| $LOADED_FEATURES | -- |
| $PROGRAM_NAME | -- |


# Pseudo (Gerçek Olmayan) Değişkenler
**Değişken** (_Variable_) gibi görünen ama **Sabit** (_Constant_) gibi davranan ve kesinlikle **değer ataması yapılamayan** şeylerdir.

| Değişken | Açıklama |
| -- | -- |
| self | Alıcı nesnenin o anki aktif metodu. Yani bu bir **Class** ise kendisi...  |
| nil | Tanımı olmayan (_Undefined_) şeylerin değeri. |
| true | Mantıksal (_Boolean_) işlem, anlayacağınız gibi **true** yani **1** |
| false | **true**'nun tersi (_Boolean_) yani **0** |
| \_\_FILE\_\_ | Çalışan kaynak kod dosyasının adı |
| \_\_LINE\_\_ | Çalışan kaynak kod dosyasındaki, o anki aktif satırın numarası |
