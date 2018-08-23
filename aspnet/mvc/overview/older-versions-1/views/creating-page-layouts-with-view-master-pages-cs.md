---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: (C#) görünüm ana sayfalarıyla sayfa düzenleri oluşturma | Microsoft Docs
author: microsoft
description: Bu öğreticide, görünüm ana sayfalarına yararlanarak uygulamanızda ortak bir sayfa düzeni için birden çok sayfa oluşturma konusunda bilgi edinin. Kullanabileceğiniz bir...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: acf304d0f1cf349f7e9c30acb9271e602a47d3d9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752749"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>(C#) görünüm ana sayfalarıyla sayfa düzenleri oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> Bu öğreticide, görünüm ana sayfalarına yararlanarak uygulamanızda ortak bir sayfa düzeni için birden çok sayfa oluşturma konusunda bilgi edinin. Örneğin, iki sütunlu sayfa düzeni tanımlamak ve web uygulamanızda tüm sayfalar için iki sütunlu düzeni kullanmak için ana görünüm sayfası kullanabilirsiniz.


## <a name="creating-page-layouts-with-view-master-pages"></a>Görünüm ana sayfalarıyla sayfa düzenleri oluşturma

Bu öğreticide, görünüm ana sayfalarına yararlanarak uygulamanızda ortak bir sayfa düzeni için birden çok sayfa oluşturma konusunda bilgi edinin. Örneğin, iki sütunlu sayfa düzeni tanımlamak ve web uygulamanızda tüm sayfalar için iki sütunlu düzeni kullanmak için ana görünüm sayfası kullanabilirsiniz.

Ayrıca görünümün birden çok sayfada uygulamanızda ortak içerik paylaşmak için ana sayfalar yararlanabilirsiniz. Örneğin, bir görünüm ana sayfaya, Web sitesi logosu, gezinti bağlantılarının ve başlık reklamları yerleştirebilirsiniz. Bu şekilde, uygulamanızdaki her sayfada bu içerik otomatik olarak görüntülenebilir.

Bu öğreticide, yeni bir görünüm ana sayfası oluşturun ve ana sayfasını temel alan yeni bir görünüm içerik sayfası oluşturmayı öğrenin.

### <a name="creating-a-view-master-page"></a>Görünüm ana sayfası oluşturma

İki sütunlu düzeni tanımlayan bir görünümü ana sayfası oluşturarak başlayalım. Yeni bir görünüm ana sayfası bir MVC projesi için görünümler/paylaşılan klasörünü sağ tıklayarak menü seçeneğini belirleyerek eklediğiniz **Ekle, yeni öğe**, seçerek **MVC görünüm ana sayfa** şablonu (bkz. Şekil 1).


[![Görünüm ana sayfası ekleme](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Şekil 01**: görünüm ana sayfası ekleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


Bir uygulamada birden fazla ana görünüm sayfası oluşturabilirsiniz. Her görünüm ana sayfası farklı sayfa düzeni tanımlayabilirsiniz. Örneğin, iki sütunlu düzeni için belirli sayfaları ve diğer sayfalara üç sütunlu düzeni isteyebilirsiniz.

Görünüm ana sayfası gibi standart bir ASP.NET MVC görünüm çok arar. Ancak, normal görünümünden farklı olarak, bir veya daha fazla görünüm ana sayfası içeren `<asp:ContentPlaceHolder>` etiketler. `<contentplaceholder>` Etiketler tek tek bir içerik sayfasındaki geçersiz kılınabilir ana sayfa alanlarının işaretlemek için kullanılır.

Örneğin, iki sütunlu düzeni listeleme 1 görünüm ana sayfasını tanımlar. İki tane `<contentplaceholder>` etiketler. Bir `<ContentPlaceHolder>` her sütun için.

**Kod 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Ana sayfa 1 listeleme içeren iki görünüm gövdesi `<div>` iki sütunlara karşılık gelen etiketleri. Geçişli stil sayfası sütun sınıf her ikisi de uygulanır `<div>` etiketler. Bu sınıf ana sayfanın en üstündeki bildirilen stil sayfası içinde tanımlanır. Tasarım görünümüne geçerek görünüm ana sayfasını nasıl işlenir önizleyebilirsiniz. Kaynak kod düzenleyicisinin alt sol tasarım sekmesine tıklayın (bkz: Şekil 2).


[![Bir ana sayfa tasarımcıyı Önizleme](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Şekil 02**: bir ana sayfa tasarımcıyı Önizleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Bir görünüm içerik sayfası oluşturma

Görünüm ana sayfası oluşturduktan sonra içerik sayfalarının görünüm ana sayfasını temel alan bir veya daha fazla görünüm oluşturabilirsiniz. Görünümler/giriş klasörünü sağ tıklayarak dizini bir görünüm içerik sayfası için giriş denetleyicisine gibi oluşturabilirsiniz seçerek **Ekle, yeni öğe**u seçerek **MVC görünüm içerik sayfası** girme şablonu ' % s'adı, Index.aspx tıklayıp **Ekle** (bkz: Şekil 3) düğmesini.


[![Bir görünüm içerik sayfası ekleme](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Şekil 03**: görünüm içerik sayfası ekleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Ekle düğmesine tıkladıktan sonra Görünüm içerik sayfası ile ilişkilendirmek için bir ana görünüm sayfası seçmenize olanak sağlayan yeni bir iletişim kutusu görünür (bkz: Şekil 4). Önceki bölümde oluşturduğumuz Site.master görünüm ana sayfasına gidebilirsiniz.


[![Ana sayfa seçme](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Şekil 04**: bir ana sayfa seçme ([tam boyutlu görüntüyü görmek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Site.master ana sayfasını temel alan yeni bir görünüm içerik sayfası oluşturduktan sonra 2 listeleme dosyasında alın.

**Kod 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Bu görünüm içeren bildirimi bir `<asp:Content>` her biri için karşılık gelen etiket `<asp:ContentPlaceHolder>` görünüm ana sayfasındaki etiketler. Her `<asp:Content>` etiketi içeren belirli işaret eden bir ContentPlaceHolderID özniteliği `<asp:ContentPlaceHolder>` , onu geçersiz kılar.

Ayrıca, içerik görünümü sayfası listeleme 2 normal açılış ve kapanış HTML etiketleri herhangi birini içermediğinden emin dikkat edin. Örneğin, bu açılış ve kapanış içermiyor `<html>` veya `<head>` etiketler. Normal açılış ve kapanış etiketlerinin tümünün görünüm ana sayfada yer alır.

Bir görünüm içerik sayfası görüntülemek istediğiniz herhangi bir içerik içinde yerleştirilmelidir bir `<asp:Content>` etiketi. Herhangi bir HTML veya bu etiketleri dışında diğer içerik yerleştirirseniz, sayfayı görüntülemeye çalıştığınızda bir hata alırsınız.

Geçersiz kılma gerekmez her `<asp:ContentPlaceHolder>` bir ana sayfa içerik görünümü sayfasında etiketi. Yalnızca geçersiz kılmak gereken bir `<asp:ContentPlaceHolder>` etiketi ile belirli bir içeriğe etiket değiştirmek istediğinizde.

Örneğin, yalnızca iki listeleme 3'te değiştirilmiş Index görünümünü içerir `<asp:Content>` etiketler. Her biri `<asp:Content>` etiketleri, bazı metinleri içerir.

**Kod 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

3 liste görünümünde istendiğinde, Şekil 5'te sayfasını işler. Görünüm iki sütuna sahip bir sayfa işler dikkat edin. Ayrıca, içerik görünümü içerik sayfasından ana görünüm sayfası içerikle birleştirilir fark


[![Dizin görünüm içerik sayfası](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Şekil 05**: dizin görünüm içerik sayfası ([tam boyutlu görüntüyü görmek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Görünüm ana sayfa içeriğini değiştirme

Neredeyse karşılaştığınız bir sorun, farklı bir görünüm içerik sayfalarını istendiğinde görünüm ana sayfa içeriği değiştirme sorununu hemen görünüm ana sayfalarıyla çalışırken. Örneğin, her sayfada web uygulamanıza benzersiz bir başlık olmasını istersiniz. Ancak, ana sayfa görünümü ve görünüm içerik sayfası başlığı bildirilir. Bu nedenle, nasıl sayfa başlığının her görünüm içerik sayfası için özelleştirdiğiniz?

Görünüm içerik sayfası tarafından gördüğü başlık değiştirebileceğiniz iki yolu vardır. İlk olarak, bir sayfa başlığı başlık özniteliğiyle atayabilirsiniz `<%@ page %>` yönergesi bildirilen bir görünüm içerik sayfası üstünde. Örneğin, dizin görünümü sayfa başlığı "Süper harika Web sitesi" atamak istiyorsanız, aşağıdaki yönerge Index görünümünü üst kısmındaki şunları içerebilir:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Dizin görünümünün tarayıcıya işlendiğinde, istenen başlık tarayıcının başlık çubuğunda görünür:


[![Tarayıcının başlık çubuğu](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


Bir ana görünüm sayfası, sırayla çalışmak üzere title özniteliği için karşılaması gereken önemli bir gereksinim yoktur. Görünüm ana sayfası içermelidir bir `<head runat="server">` yerine normal bir etiket `<head>` üstbilgisi etiketi. Varsa `<head>` etiketi runat içermez başlığı görünmez sonra = "server" özniteliği. Ana sayfa içerir gerekli varsayılan görünüm `<head runat="server">` etiketi.

Tek görünüm içerik sayfasından ana sayfa içeriği değiştirme için alternatif bir yaklaşım sarmaktır değiştirmek için istediğiniz bölgede bir `<asp:ContentPlaceHolder>` etiketi. Örneğin, yalnızca başlığı, aynı zamanda bir ana görünüm sayfası tarafından oluşturulan meta etiketleri değiştirmek istediğinizi varsayalım. Ana görünüm sayfası 4 listesi içeren bir `<asp:ContentPlaceHolder>` içinde etiketi kendi `<head>` etiketi.

**4 listeleme – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Dikkat `<asp:ContentPlaceHolder>` listeleme 4'te etiketi varsayılan içerik içerir: varsayılan başlık ve varsayılan meta etiketler. Bu geçersiz kılmazsanız `<asp:ContentPlaceHolder>` varsayılan içerik görüntülenecek sonra tek tek görünüm içerik sayfası etiketleyin.

Geçersiz kılmaları listeleme 5'teki içerik görünümü sayfası `<asp:ContentPlaceHolder>` özel bir başlık ve özel meta etiketler görüntülemek için etiket.

**5 listeleme – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Özet

Bu öğreticide, ana sayfalar ve içerik sayfalarını görüntülemek için temel bir giriş sağlanan. Ana sayfalar yeni görünüm oluşturma ve bunları temel alan içerik sayfalarını görüntüleme oluşturmayı öğrendiniz. Ayrıca belirli görünüm içerik sayfası görünüm ana sayfadan içerik nasıl değiştirebileceğiniz incelenir.

> [!div class="step-by-step"]
> [Önceki](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [İleri](passing-data-to-view-master-pages-cs.md)
