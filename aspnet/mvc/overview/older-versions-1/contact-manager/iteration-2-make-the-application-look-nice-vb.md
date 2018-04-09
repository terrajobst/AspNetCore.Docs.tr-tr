---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: 'Yineleme #2 – olun iyi (VB) Ara uygulama | Microsoft Docs'
author: microsoft
description: Bu yinelemede biz ana görünüm sayfası, ASP.NET MVC varsayılan değiştirme ve geçişli stil sayfası uygulama görünümünü geliştirir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: 8545351b099e52533789b372903cd493f533f834
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="iteration-2--make-the-application-look-nice-vb"></a>Yineleme #2 – olun iyi (VB) Ara uygulama
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indirme](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> Bu yinelemede biz ana görünüm sayfası, ASP.NET MVC varsayılan değiştirme ve geçişli stil sayfası uygulama görünümünü geliştirir.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Bir kişi yönetim ASP.NET MVC uygulaması (VB) oluşturma
  

Bu öğretici serisinde tamamlamak için tüm kişi yönetim uygulamanın başından oluşturun. Kişi Yöneticisi uygulama iletişim bilgilerini – adları, telefon numaraları ve e-posta adresleri – listesini kişilerin depolamanıza olanak sağlar.

Biz uygulamayı birden çok kez oluşturun. Her bir yineleme, biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı, her değişiklik nedeni anlamak etkinleştirmek için hedefidir.

- Yineleme #1 – uygulama oluşturun. İlk yinelemede Contact Manager en basit yolu olası oluşturuyoruz. Temel veritabanı işlemleri için destek eklediğimiz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 – iyi Ara uygulama olun. Bu yinelemede biz ana görünüm sayfası, ASP.NET MVC varsayılan değiştirme ve geçişli stil sayfası uygulama görünümünü geliştirir.

- Yineleme #3 – Ekle form doğrulama. Üçüncü yinelemede temel form doğrulama ekleyin. Biz, kişilerin gerekli form alanları tamamlamadan bir form gönderme engelleyin. Biz de e-posta adresleri ve telefon numaralarını doğrulayın.

- Yineleme #4 – gevşek uygulama olun. Bu üçüncü yinelemede biz Bakım ve ilgili kişi Yöneticisi uygulaması değişiklik kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, uygulamamız havuz deseni ve bağımlılık ekleme düzeni kullanmak üzere yeniden.

- Yineleme #5 – birim testleri oluşturma. Beşinci yinelemede biz uygulamamız korumak ve birim testleri ekleyerek değiştirmek kolaylaştırır. Biz bizim veri modeli sınıflarını mock ve bizim denetleyicileri ve Doğrulama mantığı için birim testleri oluşturma.

- Yineleme #6 – teste dayalı geliştirme kullanın. Bu altıncı yinelemede yeni işlevsellik uygulamamız için birim testleri ilk yazma ve birim testleri karşı kod yazma ekleriz. Bu yinelemede kişi grupları ekleyin.

- Yineleme #7 – Ekle Ajax işlevselliği. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.

## <a name="this-iteration"></a>Bu yineleme

Bu yineleme Kişi Yöneticisi uygulaması görünümünü iyileştirmek için hedefidir. Şu anda, kişinin Yöneticisi'ni varsayılan ASP.NET MVC görünümü ana sayfa ve geçişli stil sayfası (bkz: Şekil 1) kullanır. Bu tan t hatalı bakın, ancak t istediğiniz yalnızca her diğer ASP.NET MVC Web sitesi gibi ilgili kişi yöneticisi güncelleştireceğinizi. Bu dosyalar özel dosyalarla değiştirmek istiyorsunuz.


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Şekil 01**: bir ASP.NET MVC uygulaması varsayılan görünümünü ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image2.png))


Bu yinelemede ı uygulamamız visual tasarımını geliştirmek için iki yaklaşım tartışın. İlk olarak, t, boş bir ASP.NET MVC tasarım şablonu indirmek için ASP.NET MVC Tasarım Galerisi yararlanmak nasıl gösterir. ASP.NET MVC Tasarım Galerisi, herhangi bir iş yapmadan profesyonel web uygulaması oluşturmanıza olanak sağlar.

ASP.NET MVC Tasarım Galerisi'nden Contact Manager uygulaması için bir şablon kullanamazsınız vermiştir. Bunun yerine, profesyonel tasarım şirketi tarafından oluşturulan özel bir tasarım var. Bu öğreticinin ikinci bölümünde, ı son ASP.NET MVC tasarım oluşturmak için bir profesyonel tasarım şirket nasıl çalıştığı açıklanmaktadır.

## <a name="the-aspnet-mvc-design-gallery"></a>ASP.NET MVC Tasarım Galerisi

ASP.NET MVC Tasarım Galerisi, Microsoft tarafından sağlanan ücretsiz bir kaynaktır. ASP.NET MVC galeri şu adresten bulunur:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC Tasarım Galerisi özellikle, bir ASP.NET MVC projesinde kullanılarak oluşturulan ücretsiz Web sitesi tasarımları koleksiyonu barındırır. Tasarımlar topluluk üyeleri tarafından yüklenir. Galeri ziyaretçileri için sık kullanılan kendi tasarımları oy verin (bkz: Şekil 2).


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Şekil 02**: ASP.NET MVC Tasarım Galerisi ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image4.png))


Bu öğretici yazma gibi en popüler galerisinde Ekim David Hauser tarafından adlı bir tasarım tasarımdır. Bu tasarım aşağıdaki adımları tamamlayarak bir ASP.NET MVC proje için kullanabilirsiniz:

1. ' I tıklatın **karşıdan** düğmesini October.zip dosyayı bilgisayarınıza indirin.
2. İndirilen October.zip dosyasını sağ tıklatın ve **Engellemeyi Kaldır** (bkz: Şekil 3) düğmesine tıklayın.
3. Ekim adlı bir klasöre dosyanın sıkıştırmasını açın.
4. Ekim klasöründe bulunan DesignTemplate klasöründen tüm dosyaları seçin, dosyaları sağ tıklatın ve menü seçeneğini belirleyin **kopya**.
5. Visual Studio Çözüm Gezgini penceresinde ContactManager proje düğümüne sağ tıklayın ve menü seçeneğini **Yapıştır** (Şekil 4'e bakın).
6. Visual Studio menü seçeneğini seçmek **düzenleme, bulma ve değiştirme, hızlı Değiştir** ve değiştirme *[MyProjectName]* ile *ContactManager* (bkz. Şekil 5).


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Şekil 03**: web sunucusundan indirilen dosya engellemelerini kaldırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image6.png))


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Şekil 04**: Çözüm Gezgini'nde dosyaların üzerine yazılması ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image8.png))


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Şekil 05**: [ProjectName] ile ContactManager değiştirme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image10.png))


Bu adımları tamamladıktan sonra web uygulamanızın yeni bir tasarım kullanın. Şekil 6 sayfasında Ekim tasarım Contact Manager uygulamasıyla görünümünü gösterir.


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Şekil 06**: ContactManager Ekim şablonuyla ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Özel ASP.NET MVC tasarımı oluşturma

ASP.NET MVC Tasarım Galerisi farklı tasarım stillerini iyi seçimi vardır. Galeri, ASP.NET MVC uygulamalarınızı görünümünü özelleştirmek için sorunsuz bir yol sağlar. Ve doğal olarak, Galeri tamamen ücretsiz olan büyük avantajı vardır.

Ancak, Web siteniz için tamamen benzersiz bir tasarım oluşturmak gerekebilir. Bu durumda, bir Web sitesi tasarım şirketinin ile çalışmak için mantıklıdır. Kişi Manager uygulaması için tasarım için bu yaklaşımı benimsemeye karar.

I yukarı yineleme #1 Contact Manager'dan daraltılmış ve proje tasarım şirkete gönderilir. Etmedi t var ancak bu bir sorun, Visual Studio (shame bunlardaki!), sahibi değildi. Microsoft Visual Web Developer ücretsiz indirebilir [ https://www.asp.net ](https://www.asp.net) Web sitesi ve Visual Web Developer Contact Manager uygulamasında açın. Birkaç gün içinde Şekil 7'deki Tasarım üretilen.


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Şekil 07**: ASP.NET MVC Contact Manager tasarım ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image14.png))


Yeni Tasarım iki ana dosyaları içermektedir: yeni bir geçişli stil sayfası dosyası ve yeni bir görünüm ana sayfa dosyası. Bir görünüm ana sayfa düzeni ve paylaşılan içerik için bir ASP.NET MVC uygulamasındaki görünümler içerir. Örneğin, görünüm ana sayfa üstbilgisi, gezinme sekmeleri ve görünür altbilgisi Şekil 7'de içerir. I varolan Site.Master görüntüle ana sayfasında görünümler/paylaşılan klasörünü tasarım şirketten yeni Site.Master dosyayla üzerine yazdı,

Tasarım şirket ayrıca yeni geçişli stil sayfası ve görüntü kümesi oluşturuldu. Bu yeni dosyaları içerik klasörüne konmuş ve varolan Site.css dosyanın üzerine yazdı bildirimi. Tüm statik içerik içerik klasöründe yerleştirmelisiniz.

Yeni Tasarım kişi yöneticisi için düzenleme ve kişileri silme için görüntüleri içerdiğine dikkat edin. Düzenleme ve silme görüntüyü kişilerin HTML tablosundaki her kişi yanında görüntülenir.

İlk olarak, HTML ile işlendiği bu bağlantıları. ActionLink() yardımcı şuna benzer:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

Html.ActionLink() yöntemi (güvenlik nedeniyle bağlantı metni HTML yöntemi kodlar) görüntülerini desteklemiyor. Bu nedenle, t Html.ActionLink() çağrıları Url.Action() çağrıları şöyle yerine:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

Html.ActionLink() yöntemi, tüm bir HTML Köprü işler. Url.Action() yöntemi, diğer yandan, yalnızca olmadan URL işler &lt;bir&gt; etiketi.

Ayrıca, yeni tasarım seçili ve seçili sekmeleri içerdiğine dikkat edin. Örneğin, Şekil 8'deki **yeni kişi oluşturma** sekmesi seçilmiştir ve **My kişiler** sekmesi seçili değil.


[![Yeni Proje iletişim kutusu](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Şekil 08**: Seçili ve seçili sekmeler ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-2-make-the-application-look-nice-vb/_static/image16.png))


Seçili ve seçili sekmeleri işleme desteklemek için MenuItemHelper adlı özel bir HTML Yardımcısı oluşturdum. Bu yardımcı yöntem ya da işleyen bir &lt;li&gt; etiketi veya bir &lt;li sınıfı "Seçili" =&gt; olup geçerli denetleyici ve eylem karşılık gelen yardımcıya geçirilen denetleyici ve Eylem adına bağlı olarak etiketi. MenuItemHelper kodunu listeleme 1'de yer alır.

**1 – Helpers\MenuItemHelper.vb listeleme**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

MenuItemHelper TagBuilder sınıfı dahili olarak oluşturmak için kullandığı &lt;li&gt; HTML etiketi. Yeni bir HTML etiket oluşturmak ihtiyaç duyduğunuzda kullanabileceğiniz çok yararlı yardımcı program sınıfı TagBuilder sınıftır. Öznitelikleri ekleme, CSS sınıfları ekleme, kimlikleri oluşturma ve s etiketi değiştirme yöntemlerini içeren iç HTML.

## <a name="summary"></a>Özet

Bu yinelemede bizim ASP.NET MVC uygulamasının görsel tasarım geliştirilmiş. İlk olarak, ASP.NET MVC Tasarım Galerisi'ne tanıtıldı. ASP.NET MVC Tasarım Galerisi'nden ASP.NET MVC uygulamalarınızda kullanabileceğiniz ücretsiz tasarım şablonları indirmek öğrendiniz.

Ardından, varsayılan geçişli stil sayfası dosyası ve ana görünüm sayfası dosyası değiştirerek özel bir tasarım nasıl oluşturabileceğinizi açıklanmıştır. Yeni Tasarım desteklemek için biz Contact Manager uygulamamız için bazı küçük değişiklikler yapmak zorunda kaldı. Örneğin, seçili ve seçili sekmeleri görüntüler MenuItemHelper adlı yeni bir HTML Yardımcısı eklendi.

Sonraki yinelemede doğrulama çok önemli konu üstesinden. Bir kullanıcı bir kişi s gibi gerekli değerleri girmeden önce yeni bir kişi oluşturabilir ve Soyadı böylece biz uygulamamız için doğrulama kodu ekleyin.

> [!div class="step-by-step"]
> [Önceki](iteration-1-create-the-application-vb.md)
> [sonraki](iteration-3-add-form-validation-vb.md)
