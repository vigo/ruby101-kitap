[![Documentation Status](https://readthedocs.org/projects/ruby-101-kitab/badge/?version=latest)](https://ruby-101-kitab.readthedocs.io/tr/latest/?badge=latest)

# Ruby 101 Kitabı

https://github.com/vigo/ruby101-kitap

<a target="_blank" href="https://www.patreon.com/vigoo"><img src="https://c5.patreon.com/external/logo/become_a_patron_button@2x.png" width="160"></a>

Ruby öğrenmeyen kalmasın amacıyla kendi halimde bir kitap yazmaya başladım.
Biliyorum biraz eskidi ama genel olarak hemen hemen her şey halen geçerli.

`mkdocs` entegrasyonu yaptım, rahat rahat web üzerinden okunabiliyor artık!

---

## Lisans

Bu proje MIT lisansı kullanmaktadır.

Bu proje, işbirliği için güvenli ve davetkar bir alan olarak tasarlanmıştır ve
katkıda bulunanların [davranış kurallarına][coc] uymaları beklenir.

---

## Katkı Yapın

1. `fork` (https://github.com/vigo/ruby101-kitap/fork)
1. Yeni bir `branch` açın (`git checkout -b duzeltmeler`)
1. `commit` edin (`git commit -am 'imla hataları'`)
1. `branch`’inizi `push` edin (`git push origin duzeltmeler`)
1. ve **Pull Request** açın!

---

## Rakefile

```bash
$ rake -T

rake build   # Build docs
rake deploy  # Deploy to github
rake serve   # Run docs server
```

[coc]: https://github.com/vigo/ruby101-kitap/blob/main/CODE_OF_CONDUCT.md