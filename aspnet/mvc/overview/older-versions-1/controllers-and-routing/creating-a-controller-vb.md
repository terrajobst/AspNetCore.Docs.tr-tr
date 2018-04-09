---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Bir denetleyici (VB) oluşturma | Microsoft Docs
author: StephenWalther
description: Bu öğreticide, Stephen Walther bir denetleyici için bir ASP.NET MVC uygulamasını nasıl ekleyebileceğiniz gösterilmektedir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: e9a2bbcb09672f5247429064908cd4d2ef67f518
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-controller-vb"></a>Bir denetleyici (VB) oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Bu öğreticide, Stephen Walther bir denetleyici için bir ASP.NET MVC uygulamasını nasıl ekleyebileceğiniz gösterilmektedir.


Bu öğretici, yeni ASP.NET MVC denetleyicileri nasıl oluşturabileceğinizi açıklamak için hedefidir. Visual Studio denetleyici Ekle menü seçeneğini kullanarak hem bir sınıf dosyası el ile oluşturarak denetleyicileri oluşturmayı öğrenin.

### <a name="using-the-add-controller-menu-option"></a>Kullanarak denetleyici menü seçeneği ekleyin

Visual Studio Çözüm Gezgini penceresinde denetleyicileri klasörü sağ tıklatın ve seçmek için yeni bir denetleyicisi oluşturmak için en kolay yolu olan **Ekle, denetleyici** menü seçeneği (bkz: Şekil 1). Bu menü seçeneğini seçerek açılır **denetleyici Ekle** iletişim (bkz: Şekil 2).


[![Yeni Proje iletişim kutusu](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Şekil 01**: yeni bir denetleyicisi ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-controller-vb/_static/image2.png))


[![Yeni Proje iletişim kutusu](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Şekil 02**: Denetleyici Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-controller-vb/_static/image4.png))


Denetleyici adı ilk bölümü vurgulanan bildirimi **denetleyici Ekle** iletişim. Her Denetleyici adı son eki ile bitmelidir *denetleyicisi*. Örneğin, adlı bir denetleyicisi oluşturabilirsiniz *ProductController* ancak adlı bir denetleyicisi *ürün*.


Eksik bir denetleyici oluşturursanız *denetleyicisi* denetleyicisi çağırma açamayacaksınız sonra soneki. Bunu yapmayın--ı my ömrü sayısız saatleri bu hata yaptıktan sonra küçülttüğü iyi bir şekilde.


**1 - Controllers\ProductController.vb listeleme**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Her zaman denetleyicileri klasöründe denetleyicileri oluşturmanız gerekir. Aksi takdirde, ASP.NET MVC kurallarını ihlal ve diğer geliştiriciler, uygulamanızın anlamak daha zor bir zaman gerekir.

### <a name="scaffolding-action-methods"></a>Yapı iskelesi eylem yöntemleri

Bir denetleyici oluşturduğunuzda, oluşturma, güncelleştirme ve ayrıntılarını eylem yöntemlerine otomatik olarak oluşturmak için seçeneğiniz vardır (Şekil 3 bakın). Bu seçeneği belirlerseniz listeleme 2 denetleyicisi sınıfında oluşturulur.


[![Eylem yöntemleri otomatik olarak oluşturma](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Şekil 03**: eylem yöntemlerine otomatik olarak oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-controller-vb/_static/image6.png))


**2 - Controllers\CustomerController.vb listeleme**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Bu oluşturulan yöntemler saplama yöntemleridir. Oluşturma, güncelleştirme ve bir müşteri için kendiniz ayrıntıları gösteren fiili mantığı eklemeniz gerekir. Ancak, saplama yöntemleri ile iyi bir başlangıç noktası sağlar.

### <a name="creating-a-controller-class"></a>Denetleyici sınıfı oluşturma

ASP.NET MVC denetleyicisi yalnızca bir sınıftır. İsterseniz, uygun Visual Studio denetleyicisi yapı iskelesi yoksay ve denetleyici sınıfını el ile oluşturun. Aşağıdaki adımları uygulayın:

1. Denetleyicileri klasörünü sağ tıklatın ve menü seçeneğini belirleyin **Ekle, yeni öğe** seçip **sınıfı** şablonu (Şekil 4'e bakın).
2. Yeni sınıf PersonController.vb ve tıklayın **Ekle** düğmesi.
3. Sonuçta elde edilen sınıf dosyasını sınıf taban System.Web.Mvc.Controller (3 listeleme bakın) sınıftan şekilde değiştirin.


[![Yeni bir sınıf oluşturma](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Şekil 04**: yeni bir sınıf oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-controller-vb/_static/image8.png))


**3 - Controllers\PersonController.vb listeleme**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

"Hello World!" dizesini döndürür İNDİS() adlı bir eylem listeleme 3'te denetleyicisi sunar. Bu denetleyici eylemi, uygulamanızı çalıştıran ve aşağıdaki gibi bir URL isteyen çağırabilirsiniz:

`http://localhost:40071/Person`

> [!NOTE]
> 
> ASP.NET Geliştirme Sunucusu, bir rastgele bağlantı noktası numarası (örneğin, 40071) kullanır. Bir denetleyici çağırmak için bir URL girerken, doğru bağlantı noktası numarası girmeniz gerekir. ASP.NET Geliştirme Sunucusu (sağ alt ekranınızın) Windows bildirim alanındaki simgenin üzerine farenizi gelerek, bağlantı noktası numarasını belirleyebilirsiniz.
> 
> [!div class="step-by-step"]
> [Önceki](adding-dynamic-content-to-a-cached-page-vb.md)
> [sonraki](creating-an-action-vb.md)
