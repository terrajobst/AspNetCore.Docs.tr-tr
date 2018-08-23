---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: 'Yineleme #2 – uygulamanın güzel (VB) görünmesini olun | Microsoft Docs'
author: microsoft
description: Bu yineleme, varsayılan ASP.NET MVC görünüm ana sayfası değiştirme ve geçişli stil sayfası biz uygulamanın görünümünü geliştirin.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f27cbab17effc3b44649e06409893e6be09b011
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751977"
---
<a name="iteration-2--make-the-application-look-nice-vb"></a>Yineleme #2 – uygulamanın güzel (VB) görünmesini olun
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indir](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> Bu yineleme, varsayılan ASP.NET MVC görünüm ana sayfası değiştirme ve geçişli stil sayfası biz uygulamanın görünümünü geliştirin.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Bir kişi yönetimi ASP.NET MVC uygulama (VB)
  

Bu öğretici serisinde, tamamlanması bir tüm kişi yönetimi uygulaması ekleriz. Kişi Yöneticisi uygulama bilgilerini – adları, telefon numaraları ve e-posta adresleri – bir listesi için kişilerin depolamanızı sağlar.

Birden çok yineleme üzerinde uygulama ekleriz. Her yineleme ile biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı amacı, her değişikliğin nedenini anlamak etkinleştirmektir.

- Yineleme #1 – Uygulamayı oluşturma. İlk yinelemede Kişi Yöneticisi basit şekilde olası oluştururuz. Temel veritabanı işlemleri için destek ekliyoruz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 – uygulamanın güzel görünmesini olun. Bu yineleme, varsayılan ASP.NET MVC görünüm ana sayfası değiştirme ve geçişli stil sayfası biz uygulamanın görünümünü geliştirin.

- Yineleme #3 – form doğrulaması ekleme. Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de e-posta adresi ve telefon numaralarını doğrulayın.

- Yineleme #4 – uygulamanın gevşek olun. Bu dördüncü yinelemede biz Bakım ve değişiklik kişi yöneticisi uygulamayı kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, biz uygulamamız depo deseni ve bağımlılık ekleme modelini kullanmak için yeniden düzenleyin.

- Yineleme #5 – birim testleri oluşturma. Beşinci yinelemede uygulamamız Bakım ve değişiklik birim testleri ekleyerek daha kolay vermiyoruz. Biz, bizim veri modeli sınıfları Sahne ve yapı denetleyicilerini ve Doğrulama mantığı birim testleri.

- Yineleme #6 – test odaklı geliştirme kullanma. Bu altıncı yinelemede yeni işlevsellik uygulamamız için ilk birim testleri yazma ve birim testlerini karşı kod yazma ekleriz. Bu yineleme, kişi grupları ekleriz.

- Yineleme #7 – Ajax işlevselliği ekleme. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.

## <a name="this-iteration"></a>Bu yineleme

Kişi Yöneticisi uygulama görünümünü amacı, bu yineleme artırmaktır. Kişi Yöneticisi, şu anda varsayılan ASP.NET MVC görünüm ana sayfası ve geçişli stil sayfası (bkz. Şekil 1) kullanır. Bu rsquo hatalı bakın, ancak yalnızca her diğer ASP.NET MVC Web sitesi gibi aramak için Kişi Yöneticisi t istediğiniz görmüyorum. Bu dosyalar özel dosyaları ile değiştirmek istiyorsunuz.


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Şekil 01**: bir ASP.NET MVC uygulamasını varsayılan görünümünü ([tam boyutlu görüntüyü görmek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image2.png))


Bu yineleme, ı uygulamamız görsel tasarımını geliştirmek için iki yaklaşım ele alınmıştır. İlk olarak, ı, boş bir ASP.NET MVC tasarım şablonu indirmek için ASP.NET MVC tasarım galeri yararlanmak gösterilmektedir. ASP.NET MVC Tasarım Galerisi, herhangi bir iş yapmadan profesyonel web uygulaması oluşturmanıza olanak sağlar.

Kişi Yöneticisi uygulama için bir şablon ASP.NET MVC tasarım galerideki kullanmayacak şekilde verdi. Bunun yerine, bir profesyonel tasarım şirketi tarafından oluşturulan özel bir tasarım var. Bu öğreticinin ikinci bölümünde son ASP.NET MVC tasarım oluşturmak için profesyonel tasarım şirketinin ile nasıl çalıştım açıklanmaktadır.

## <a name="the-aspnet-mvc-design-gallery"></a>ASP.NET MVC Tasarım Galerisi

ASP.NET MVC Tasarım Galerisi, Microsoft tarafından sağlanan ücretsiz bir kaynaktır. ASP.NET MVC galeri şu adresten bulunur:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC tasarım galeri için bir ASP.NET MVC projesinde özellikle kullanılarak oluşturulan ücretsiz Web sitesi tasarımları koleksiyonunu barındırır. Tasarım, topluluk üyeleri tarafından yüklenir. Galeri yayımlanacağını ve Ziyaretçiler, sık kullanılan tasarımları için oy verin (bkz: Şekil 2).


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Şekil 02**: ASP.NET MVC Tasarım Galerisi ([tam boyutlu görüntüyü görmek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image4.png))


Bu öğreticide yazdığım gibi galerisinde en popüler tasarım Ekim David Hauser tarafından adlandırılan bir tasarım olur. Bu tasarım, aşağıdaki adımları izleyerek bir ASP.NET MVC projesi için kullanabilirsiniz:

1. Tıklayın **indirme** October.zip dosyayı bilgisayarınıza indirmek için düğme.
2. İndirilen October.zip dosyasını sağ tıklatıp **Engellemeyi Kaldır** (bkz: Şekil 3) düğmesi.
3. Ekim adlı bir klasöre dosyanın sıkıştırmasını açın.
4. Ekim klasördeki DesignTemplate klasöründen tüm dosyaları seçin, dosyaları sağ tıklayın ve menü seçeneğini **kopyalama**.
5. Visual Studio Çözüm Gezgini penceresinde ContactManager proje düğümüne sağ tıklayın ve menü seçeneğini **Yapıştır** (bkz: Şekil 4).
6. Visual Studio menü seçeneğini **düzenlemek, Bul ve Değiştir hızlı Değiştir** değiştirin *[MyProjectName]* ile *ContactManager* (bkz: Şekil 5).


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Şekil 03**: indirilen Web'den dosya kaldırma ([tam boyutlu görüntüyü görmek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image6.png))


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Şekil 04**: Çözüm Gezgini'nde dosyalarının üzerine yazarak ([tam boyutlu görüntüyü görmek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image8.png))


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Şekil 05**: [ProjeAdı] ile ContactManager değiştirme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image10.png))


Bu adımları tamamladıktan sonra web uygulamanızın yeni tasarım kullanır. Şekil 6 sayfasında Kişi Yöneticisi uygulama ile Ekim Tasarım görünümünü gösterir.


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Şekil 06**: Ekim şablonuyla ContactManager ([tam boyutlu görüntüyü görmek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Özel ASP.NET MVC tasarımı oluşturma

ASP.NET MVC tasarım galeri iyi bir seçim farklı tasarım stili vardır. Galeri özelleştirme, ASP.NET MVC uygulamaları için sorunsuz bir yol sağlar. Ve tabii, Galeri tamamen ücretsiz olmalarının büyük avantajı vardır.

Ancak, Web siteniz için tamamen benzersiz bir tasarım oluşturmak gerekebilir. Bu durumda, bir Web sitesi tasarım şirketinin ile çalışmak için mantıklıdır. Kişi Yöneticisi uygulama tasarımı için bu yaklaşımı benimsemeye karar.

Oluşturan kişi yöneticisi yineleme # 1 daraltılmış ve proje için tasarım şirket gönderilen bildirimi. Etmedi t sunar ancak bu bir sorun, Visual Studio (shame bunlardaki!), sahibi değildi. Bunlar Microsoft Visual Web Developer'ndan ücretsiz karşıdan sunmayı başarabilseydiniz nasıl olurdu [ https://www.asp.net ](https://www.asp.net) Web sitesine gidin ve Visual Web Developer kişi yöneticisi uygulamayı açın. Birkaç gün içinde Şekil 7'de tasarım üretilen.


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Şekil 07**: ASP.NET MVC Kişi Yöneticisi tasarım ([tam boyutlu görüntüyü görmek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image14.png))


Yeni Tasarım iki ana dosyaları toplamda: yeni bir geçişli stil sayfası dosyası ve yeni bir görünüm ana sayfa dosyası. Görünüm ana sayfa düzeni ve paylaşılan içerik için bir ASP.NET MVC uygulamasındaki görünümleri içerir. Örneğin, ana görünüm sayfası üst bilgi, gezinme sekmeleri ve görünen alt Şekil 7'de içerir. Ben mevcut Site.Master görünüm ana sayfası görünümler/paylaşılan klasörde yeni tasarım şirketinin Site.Master dosyasından ile üzerine yazdı,

Bir tasarım şirketinin bir geçişli stil sayfası yeni ve görüntü kümesi de oluşturulur. Bu yeni dosyaları içerik klasörüne yerleştirilir ve mevcut Site.css dosyanın üzerine yazdı bildirimi. Tüm statik içeriği, içerik klasörüne yerleştirmeniz gerekir.

Kişi Yöneticisi için yeni tasarım yönelik düzenleme ve kişileri silme görüntülerinin içerdiğine dikkat edin. Düzenleme ve silme görüntü kişilerin HTML tablodaki her bir kişinin yanında görünür.

İlk olarak, HTML ile işlenmiş olan bu bağlantıları. Bu gibi ActionLink() yardımcı:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

Html.ActionLink() yöntemi (güvenlik nedeniyle bağlantı metni HTML yöntemi kodlar) görüntülerini desteklemiyor. Bu nedenle, ı şöyle Url.Action() çağrılarıyla Html.ActionLink() çağrıları değiştirildi:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

Tüm bir HTML Köprü Html.ActionLink() yöntemi oluşturur. Url.Action() yöntem, diğer taraftan, yalnızca olmadan URL işler &lt;bir&gt; etiketi.

Ayrıca, yeni tasarım hem seçili hem de seçilmemiş sekmeleri içerdiğine dikkat edin. Örneğin, Şekil 8 ' deki **yeni kişi oluşturun** sekmesi seçiliyken ve **Kişilerim** sekmesi seçili değil.


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Şekil 08**: Seçili ve seçili sekmeler ([tam boyutlu görüntüyü görmek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image16.png))


Hem seçili hem de seçilmemiş sekmeleri işleme desteklemek için MenuItemHelper adlı özel bir HTML Yardımcısı oluşturdum. Bu yardımcı yöntem ya da işleyen bir &lt;li&gt; etiketi veya bir &lt;li sınıfı "Seçili" =&gt; olup geçerli denetleyici ve eylem karşılık gelen yardımcıya geçirilen denetleyici ve Eylem adına göre etiket. MenuItemHelper kodunu listeleme 1'de yer alır.

**1 – Helpers\MenuItemHelper.vb listeleme**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

MenuItemHelper TagBuilder sınıfı dahili olarak oluşturmak için kullandığı &lt;li&gt; HTML etiketi. Yeni bir HTML etiket oluşturmak ihtiyaç duyduğunuzda kullanabileceğiniz çok kullanışlı yardımcı program sınıfı TagBuilder sınıftır. Öznitelikleri ekleme, CSS sınıfları ekleme, kimlikleri oluşturma ve değiştirme s etiketi için yöntemleri içeren iç HTML.

## <a name="summary"></a>Özet

Bu yineleme, ASP.NET MVC uygulamamıza görsel tasarımını geliştirdik. İlk olarak, ASP.NET MVC Tasarım Galerisi'ne eklenmiştir. ASP.NET MVC Tasarım Galerisi'nden, ASP.NET MVC uygulamalarınızda kullanabileceğiniz ücretsiz bir tasarım şablonları indirmek öğrendiniz.

Ardından, varsayılan geçişli stil sayfası dosyası ve ana görünüm sayfası dosyası'nı değiştirerek bir özel tasarım nasıl oluşturabileceğinize dair ele almıştık. Yeni Tasarım desteklemek için Kişi Yöneticisi uygulamamız için bazı küçük değişiklikler yapmak vardı. Örneğin, seçilen ve seçili sekmeleri görüntüler MenuItemHelper adlı yeni bir HTML Yardımcısı ekledik.

Bir sonraki yinelemede biz doğrulama çok önemli konusunu ele alalım. Bir kullanıcı bir kişi s gibi gerekli değerleri sağlamadan önce yeni kişi oluşturun ve Soyadı uygulamamıza bir doğrulama kodu ekleriz.

> [!div class="step-by-step"]
> [Önceki](iteration-1-create-the-application-vb.md)
> [İleri](iteration-3-add-form-validation-vb.md)
