---
title : Tarayıcıda Renkli Konsol Mesajları Basmak
tags  : ['devtools', 'console']
---
{% capture image_path %}{{site.url}}/assets/images/content/2015-10-23-tarayicida-renkli-konsol-mesajlari-basmak/{% endcapture %}

<p class="lead">Alert popülerliğini yitirdikten sonra hepimiz console.log'a sardık. Artık o yokken nasıl kod yazıyorduk hatırlamıyoruz bile. Ancak siyah beyaz loglardan ötesi de mümkün.</p>
<!--more-->

> Hikaye dinlemek istemeyenler doğrudan uygulama kısmına atlayabilirler.

# Hikaye
Şu an çalıştığım projede geliştirdiğimiz yapıyı birçok backend developer kullanıyor. Bu nedenle pek çok hata geri dönüşü alıyoruz. Herkes de doğal olarak kendi bilgisayarıyla geliyor 'abi bu çalışmıyor' diye. Her geri dönüşte açıp debugging yapmak artık zor gelmeye başlayınca; sistemin her adımı loglayacağı bir yapı geliştireyim dedim.

Fikir basit. Her adımda loglama olacaktı. Sunucuya request mi atıldı logla, cevap mı geldi logla, plugin mi çalıştırılacak logla, sayfa içeriği mi değişiyor logla, ne oluyorsa önce logla! Sonra 'biri bu çalışmıyor' deyince de ilk iş aç loglara bak hangi adıma kadar gelmiş, nerede patlamış diye.

Ancak art arda onlarca log basınca konsol mide bulandırıcı bir hal aldı. Ne bir bakışta loglar birbirinden ayırtedilebiliyordu ne de insanın konsola bakası geliyordu. Şu konsola bir renk katmak gerekliydi.

# Uygulama

İlk söylenmesi gereken konsol mesajlarının `console.log`'dan ibaret olmadığı.

### console.error
{% highlight js lineno %} console.error('Çok pis bir olay oldu!'); {% endhighlight %}
!["console.error çıktısı"]({{image_path}}console.error.output.png "console.error çıktısı"){: .border-wrapper }

Kritik bir hata oluşan durumlarda eğer hata fırlatmak istemiyorsanız, kullanılabilecek konsol mesajıdır. Aynı zamanda mesajın solundaki ok simgesine dikkat edin, simgeye tıklayarak callstack'i görüntüleyebilirsiniz.

### console.info
{% highlight js lineno %} console.info('Sana bir haberim var!'); {% endhighlight %}
!["console.info çıktısı"]({{image_path}}console.info.output.png "console.info çıktısı"){: .border-wrapper }

Beklenen bir şey olduğunda veya olmasaydı da olurdu ama olması çok güzel dedirten şeylerde kullanılabilecek, coşku ifade eden mesaj tipi.

### console.log
{% highlight js lineno %} console.log('Şu durum gerçekleşti.'); {% endhighlight %}
!["console.log çıktısı"]({{image_path}}console.log.output.png "console.log çıktısı"){: .border-wrapper }

Bildiğiniz console.log. Sıradan işlemleri, anlık durumu loglamak için kullanılmalıdır.

### console.warn
{% highlight js lineno %} console.warn('Bu sadece bir uyarıydı!'); {% endhighlight %}
!["console.warn çıktısı"]({{image_path}}console.warn.output.png "console.warn çıktısı"){: .border-wrapper }

Sistemi patlatmaz ama olmasaydı daha iyiydi, bak buna dikkat et, yapma böyle şeyler demek istediğiniz anlarda kızlarımızın 'neyse' sözünü söylerken kullandıklarını tonlama ile beliren mesaj. Bi uyarı gençler. 

Buraya kadar tamam.

Şimdi gelelim işin <b class="red">r</b><b class="blue">e</b><b class="green">n</b><b class="magenta">k</b><b class="brown">l</b><b class="orange">i</b> kısmına. Yukarıdaki loglama metodlarının her biri ilk parametre olarak verdiğiniz string'in içerisinde formatlayıcı kullanmanıza olanak sağlıyor. Bu formatlayıcıların tam listesine bu yazının sonundaki linkten erişebilirsiniz. Biz burada css stili formatlayıcısından bahsedeceğiz.

### Stil formatlayıcısı %c

Bu formatlayıcının her biri kendine ait bir stil parametresi istiyor. Bu parametreleri de formatlayıcı sırasına göre göndermelisiniz. Birkaç örnek verelim;

{% highlight js lineno %} console.log('%cBu yazı kırmızı.', 'color:red'); {% endhighlight %}
!["konsol stil formatlama örnek 1"]({{image_path}}console.css.1.png "konsol stil formatlama örnek 1"){: .border-wrapper }

Stiller bir arada kullanılabilir.

{% highlight js lineno %} console.log('%cKırmızı, %cbold, %ckırmızı ve bold.', 'color:red', 'font-weight:bold', 'color:red; font-weight:bold;'); {% endhighlight %}
!["konsol stil formatlama örnek 2"]({{image_path}}console.css.2.png "konsol stil formatlama örnek 2"){: .border-wrapper }

Yeni stil vermeden sadece stili sıfırlamak için stil parametresi boş string olarak verilmiş bir stil formatlayıcısı kullanmalısınız.

{% highlight js lineno %} console.log('%cMavi ve bold %cancak varsayılan.', 'color:blue; font-weight:bold', ''); {% endhighlight %}
!["konsol stil formatlama örnek 3"]({{image_path}}console.css.3.png "konsol stil formatlama örnek 3"){: .border-wrapper }

Son parametre olarak gönderdiğimiz boş string `''`'e dikkat edin. O aslında ikinci `%c` formatlayıcımıza ait hiçbir stil ayarının olmadığını söylüyor. Böylece yazı oradan sonra varsayılan ayarlarına geri dönüyor.

Ne yazık ki formatlama sadece ilk parametrede çalışıyor, yani aşağıdaki formatlama denememiz çalışmayacak.

{% highlight js lineno %} console.log('birinci', '%c ikinci', 'color:blue; font-weight:bold'); {% endhighlight %}
!["konsol stil formatlama örnek 4"]({{image_path}}console.css.4.png "konsol stil formatlama örnek 4"){: .border-wrapper }

Buradan sonra olay yaratıcılığınıza kalmış. Kullanabileceğiniz stiller sadece `color` ve `font-weight`'ten ibaret değil. Birkaç deneme yapmayı ihmal etmeyin!

# Hazırı Var
Bu zorlu kullanımı kolaylaştırmak için kendinize basit bir araç geliştirebilirsiniz, ancak yapılmışı da var; <a href="https://github.com/adamschwartz/log" target="_blank">adamschwartz/log<a>.

# Tarayıcı Desteği
Burada anlatılanlar Google Chrome ve Firefox Firebug üzerinde çalışır. Konsolların CSS'i destekleyip desteklemediğini şu an için öğrenmenin bir yolu olmadığından tarayıcı tespit edilerek kullanılması tavsiye edilir.

# Kaynakça
Bu anlatılanlarla ilgili daha detaylı bilgiye <a href="https://developers.google.com/web/tools/chrome-devtools/debug/console/console-write#string-substitution-and-formatting" target='_blank'>Chrome Devtools / String Substitution and Formatting</a> sayfası altından erişebilirsiniz. Aynı sayfadan konsolun farklı yeteneklerine de göz gezdirebilirsiniz. Tek olayı mesaj basmak değil.

# Sonuç
Sonuç olarak bu renkler benim hikayemde epey işime yaradı. Konsolu incelemediğim zamanlarda bile göze çarpan bold, renkli mesajlar veya uyarı mesajları dikkat çekmeyi başarıyor. Başlangıçta biraz boşa iş gibi gelse de, doğru kurgulanmış bir durum loglama yapısı hataları bulmayı epey kolaylaştırabiliyor.