---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Sayfa düzenleri görünümü ana sayfa (C#) oluşturma | Microsoft Docs
author: microsoft
description: Bu öğreticide, birden çok sayfa için ortak bir sayfa düzeni uygulamanızda görünümü ana sayfalar yararlanarak oluşturmayı öğrenin. Kullanabileceğiniz bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 82500a311e1110713a60604027d018ba16539b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871252"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>Sayfa düzenleri oluşturma görünümü ana sayfa (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> Bu öğreticide, birden çok sayfa için ortak bir sayfa düzeni uygulamanızda görünümü ana sayfalar yararlanarak oluşturmayı öğrenin. Örneğin, iki sütun sayfa düzeni tanımlamak ve web uygulamanızda tüm sayfalar için iki sütun düzeni kullanmak için ana görünüm sayfası kullanabilirsiniz.


## <a name="creating-page-layouts-with-view-master-pages"></a>Sayfa düzenleri görünümü ana sayfa oluşturma

Bu öğreticide, birden çok sayfa için ortak bir sayfa düzeni uygulamanızda görünümü ana sayfalar yararlanarak oluşturmayı öğrenin. Örneğin, iki sütun sayfa düzeni tanımlamak ve web uygulamanızda tüm sayfalar için iki sütun düzeni kullanmak için ana görünüm sayfası kullanabilirsiniz.

Ayrıca görünümün, uygulamanızda birden çok sayfa arasında ortak içeriği paylaşmak için ana sayfalar yararlanabilirsiniz. Örneğin, bir görünüm ana sayfasında Web sitesi logosu, Gezinti bağlantıları ve başlık reklamları yerleştirebilirsiniz. Böylece, uygulamanızda her sayfada bu içerik otomatik olarak görüntüler.

Bu öğreticide, yeni bir görünüm ana sayfası oluşturun ve ana sayfasını temel alan yeni bir görünüm içerik sayfası oluşturun öğrenin.

### <a name="creating-a-view-master-page"></a>Bir görünüm ana sayfa oluşturma

İki sütun düzeni tanımlayan bir görünüm ana sayfa oluşturarak başlayalım. Yeni bir görünüm ana sayfası bir MVC projesini görünümler/paylaşılan klasörünü sağ tıklayarak menü seçeneğini belirleyerek eklediğiniz **Ekle, yeni öğe**, seçerek **MVC görünümü ana sayfa** şablonu (bkz: Şekil 1).


[![Bir görünüm ana sayfası ekleme](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Şekil 01**: bir görünüm ana sayfası ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


Birden çok görünüm ana sayfa bir uygulama oluşturabilirsiniz. Her görünüm ana sayfa farklı sayfa düzeni tanımlayabilirsiniz. Örneğin, iki sütun düzeni sağlamak için belirli sayfaları ve diğer sayfaları üç sütun düzeni olmasını isteyebilirsiniz.

Bir görünüm ana sayfa gibi standart bir ASP.NET MVC görünümü çok arar. Ancak, normal görünümünden farklı olarak, bir veya daha fazla görünüm ana sayfa içeren `<asp:ContentPlaceHolder>` etiketler. `<contentplaceholder>` Etiketler tek tek bir içerik sayfasındaki kılınabilir ana sayfa alanları işaretlemek için kullanılır.

Örneğin, iki sütun düzeni listeleme 1 görüntüle ana sayfasında tanımlar. İki içeren `<contentplaceholder>` etiketler. Bir `<ContentPlaceHolder>` her sütun için.

**Kod 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Ana sayfa listeleme 1 içeren iki görünüm gövdesi `<div>` iki sütunlara karşılık gelen etiketleri. Geçişli stil sayfası sütun sınıfı hem de uygulanır `<div>` etiketler. Bu sınıf ana sayfanın en üstünde bildirilen stil sayfası tanımlandı. Tasarım görünümüne geçiş yaparak görünümü ana sayfa nasıl işlenir önizleyebilirsiniz. Kaynak Kodu Düzenleyicisi sol alt Tasarım sekmesini tıklatın (bkz: Şekil 2).


[![Bir ana sayfa Tasarımcısı'nda Önizleme](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Şekil 02**: Tasarımcısı'nda bir ana sayfa Önizleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>İçerik sayfasını görüntüle oluşturma

Bir görünüm ana sayfa oluşturduktan sonra içerik sayfalarının görünüm ana sayfasında göre bir veya daha fazla görünüm oluşturabilirsiniz. Örneğin, giriş denetleyicisi için dizin görünümü içerik sayfası görünümler/giriş klasörünü sağ tıklayarak oluşturabileceğiniz seçme **Ekle, yeni öğe**, seçme **MVC görünüm içerik sayfası** girme şablonu, Index.aspx, adı ve tıklayarak **Ekle** (bkz: Şekil 3) düğmesine tıklayın.


[![Bir görünümü içerik sayfası ekleme](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Şekil 03**: görünümü içerik sayfası ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Ekle düğmesini tıklattıktan sonra yeni bir iletişim kutusu görünümü içerik sayfayla ilişkilendirilecek bir görünüm ana sayfa seçmenize olanak sağlayan görünür (Şekil 4'e bakın). Önceki bölümde oluşturduğumuz Site.master görünüm ana sayfasına gidebilirsiniz.


[![Bir ana sayfa seçme](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Şekil 04**: bir ana sayfa seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Site.master ana sayfasını temel alan yeni bir görünüm içerik sayfası oluşturduktan sonra listeleme 2 dosyasında alın.

**Kod 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Bu görünüm içerir bildirimi bir `<asp:Content>` her biri için karşılık gelen etiketi `<asp:ContentPlaceHolder>` görünüm ana sayfa etiketler. Her `<asp:Content>` etiketi içeren belirli işaret eden bir ContentPlaceHolderID özniteliği `<asp:ContentPlaceHolder>` , bu geçersiz kılar.

Ayrıca, içerik görünümü sayfası listeleme 2'deki normal açma ve kapatma HTML etiketleri herhangi birini içermediğinden emin dikkat edin. Örneğin, bunu açma ve kapatma içermiyor `<html>` veya `<head>` etiketler. Tüm normal açma ve kapatma etiketleri görüntüle ana sayfasında bulunur.

Bir görünümü içerik sayfasında görüntülenmesini istediğiniz herhangi bir içerik içinde yerleştirilmelidir bir `<asp:Content>` etiketi. Herhangi bir HTML veya bu etiketler dışında diğer içerik yerleştirirseniz, sayfayı görüntülemeye çalıştığınızda bir hata alırsınız.

Geçersiz kılma gerekmez her `<asp:ContentPlaceHolder>` bir ana sayfa içerik görünümü sayfasındaki etiketi. Yalnızca geçersiz kılmak gereken bir `<asp:ContentPlaceHolder>` etiket etiket belirli içerikle değiştirmek istediğinizde.

Örneğin, değiştirilmiş dizin görünümünün listeleme 3'te yalnızca iki içeren `<asp:Content>` etiketler. Her biri `<asp:Content>` etiketleri bazı metinleri içerir.

**Kod 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Listeleme 3 görünümünde istendiğinde, Şekil 5'te sayfasını işler. Görünümü iki sütun içeren bir sayfa işleyen dikkat edin. Ayrıca, içerik sayfasını görüntüle içerikten görünümü ana sayfa içeriğini birleştirilir fark


[![Dizin içerik sayfasını görüntüle](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Şekil 05**: dizin görünümü içerik sayfası ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Görünüm ana sayfa içeriğini değiştirme

Neredeyse karşılaştığınız bir sorunu hemen görünümü ana sayfalar ile çalışırken, farklı görünüm içerik sayfaları istendiğinde, ana sayfa içeriğini görüntüleme değiştirme sorunudur. Örneğin, her sayfanın benzersiz bir başlığı olması için web uygulamanızda istiyorsunuz. Ancak, başlık görünümü ana sayfa yer alan ve içerik sayfasını görüntüle bildirilir. Bu nedenle, nasıl her içerik sayfasını görüntüle sayfa başlığı özelleştirdiğiniz?

Bir içerik sayfasını görüntüle tarafından görüntülenen Başlık değiştirebileceğiniz iki yolu vardır. İlk olarak, sayfa başlığını başlık özniteliğine atayabilirsiniz `<%@ page %>` yönergesi bildirilen bir görünüm içerik sayfasının en üstünde. Örneğin, sayfa başlığı "Süper harika Web sitesi" dizini görünümüne atamak istiyorsanız, aşağıdaki yönergesi dizin görünümünün üstündeki içerebilir:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Dizin görünümünün tarayıcıya işlendiğinde istenilen başlığı tarayıcının başlık çubuğunda görünür:


[![Tarayıcının başlık çubuğu](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


Ana görünüm sayfası çalışması başlık özniteliği de getirmelidir önemli bir gereksinimi yoktur. Görünüm ana sayfa içermelidir bir `<head runat="server">` yerine normal bir etiket `<head>` üstbilgisinde etiketi. Varsa `<head>` etiketinin runat içermez başlık görünmez sonra = "server" özniteliği. Ana sayfa içerir gerekli varsayılan görünüm `<head runat="server">` etiketi.

Değiştirmek istediğiniz bölgeyi sarmalamak için ana sayfa içeriği tek tek görünüm içerik sayfasından değiştirme için alternatif bir yaklaşım olan bir `<asp:ContentPlaceHolder>` etiketi. Örneğin, yalnızca başlık, aynı zamanda bir ana görünüm sayfası tarafından oluşturulan meta etiketleri değiştirmek istediğiniz düşünün. Ana görünüm sayfası 4 listeleme içeren bir `<asp:ContentPlaceHolder>` içinde etiketi kendi `<head>` etiketi.

**4 listeleme – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Dikkat `<asp:ContentPlaceHolder>` listeleme 4 etiketinde varsayılan içerik içerir: varsayılan başlık ve varsayılan meta etiketler. Bu geçersiz kılmaz varsa `<asp:ContentPlaceHolder>` varsayılan içerik görüntülenir sonra bir tek tek görünüm içerik sayfasındaki etiketi.

Geçersiz kılmaları listeleme 5'teki içerik görünümü sayfası `<asp:ContentPlaceHolder>` özel bir başlık ve özel meta etiketler görüntülemek için etiketi.

**5 listeleme – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Özet

Bu öğretici ana sayfalar görüntülemek ve içerik sayfalarını görüntülemek için temel bir giriş sağlanan. İçin yeni bir görünüm ana sayfalar ve bunlar üzerinde temel içerik sayfalarını görüntüleme oluşturun öğrendiniz. Ayrıca belirli Görünüm İçerik sayfasından bir görünüm ana sayfasının içeriği nasıl değiştirebileceğiniz incelendi.

> [!div class="step-by-step"]
> [Önceki](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [sonraki](passing-data-to-view-master-pages-cs.md)
