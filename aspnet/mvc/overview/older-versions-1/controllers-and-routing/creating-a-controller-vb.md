---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Denetleyici (VB) oluşturma | Microsoft Docs
author: StephenWalther
description: Bu öğreticide, Stephen Walther bir denetleyici bir ASP.NET MVC uygulamasına nasıl ekleyebileceğinizi gösterir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 619fff7a78526cae6c3db0f3c414215bf7db9bb5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393895"
---
<a name="creating-a-controller-vb"></a>Denetleyici (VB) oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Bu öğreticide, Stephen Walther bir denetleyici bir ASP.NET MVC uygulamasına nasıl ekleyebileceğinizi gösterir.


Bu öğreticinin amacı, yeni ASP.NET MVC denetleyicileri nasıl oluşturabileceğinizi açıklar sağlamaktır. Visual Studio denetleyici Ekle menü seçeneğini kullanarak hem bir sınıf dosyası el ile oluşturarak denetleyicileri oluşturmayı öğrenin.

### <a name="using-the-add-controller-menu-option"></a>Kullanarak denetleyici menü seçeneği ekleyin

Visual Studio Çözüm Gezgini penceresinde denetleyicileri klasörü sağ tıklatın ve seçin için yeni bir denetleyicisi oluşturmak için en kolay yolu olan **Ekle, denetleyici** menü seçeneği (bkz. Şekil 1). Bu menü seçeneğini belirleyerek açılır **denetleyici Ekle** iletişim (bkz: Şekil 2).


[![Yeni Proje iletişim kutusu](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Şekil 01**: yeni bir denetleyici ekleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-controller-vb/_static/image2.png))


[![Yeni Proje iletişim kutusu](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Şekil 02**: Denetleyici Ekle iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-controller-vb/_static/image4.png))


Denetleyici adının ilk bölümü vurgulanan bildirimi **denetleyici Ekle** iletişim. Her Denetleyici adı soneki ile sona ermelidir *denetleyicisi*. Örneğin, adında bir denetleyici oluşturabilirsiniz *ProductController* ancak adlı bir denetleyici *ürün*.


Eksik bir denetleyici oluşturursanız *denetleyicisi* denetleyici çağrılacak mümkün olmayacaktır sonra soneki. Bunu yapmayın--bu hata yaptıktan sonra doğduğum sayısız saatler boşa.


**1 - Controllers\ProductController.vb listeleme**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Her zaman denetleyicileri klasöründe denetleyicileri oluşturmanız gerekir. Aksi takdirde, ASP.NET MVC kurallarını ihlal ve diğer geliştiriciler, uygulamanızın anlamak daha zor bir zaman alacaktır.

### <a name="scaffolding-action-methods"></a>Yapı iskelesi eylem yöntemleri

Bir denetleyici oluşturduğunuzda, oluşturma, güncelleştirme ve ayrıntıları eylem yöntemlerine otomatik olarak oluşturma seçeneğiniz vardır (bkz: Şekil 3). Ardından bu seçeneği belirlerseniz listeleme 2 controller sınıfında oluşturulur.


[![Eylem yöntemlerine otomatik olarak oluşturma](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Şekil 03**: eylem yöntemlerine otomatik olarak oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-controller-vb/_static/image6.png))


**2 - Controllers\CustomerController.vb listeleme**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Saptama yöntemleri bu oluşturulan yöntemlerdir. Oluşturma, güncelleştirme ve kendiniz için bir müşterinin ayrıntılarını gösteren fiili mantığı eklemeniz gerekir. Ancak, saptama yöntemleri ile güzel bir başlangıç noktası sağlar.

### <a name="creating-a-controller-class"></a>Denetleyici sınıfı oluşturma

ASP.NET MVC denetleyicisi, yalnızca bir sınıf değil. İsterseniz, uygun Visual Studio denetleyicisi iskele yoksaymak ve el ile bir denetleyici sınıfı oluşturun. Aşağıdaki adımları uygulayın:

1. Menü seçeneği denetleyicileri klasörüne sağ tıklayıp **Ekle, yeni öğe** seçip **sınıfı** şablonu (bkz: Şekil 4).
2. Yeni bir sınıf PersonController.vb adlandırın ve tıklatın **Ekle** düğmesi.
3. Temel System.Web.Mvc.Controller sınıfından (3 listeleme bakın) sınıfından devralan elde edilen sınıf dosyasını değiştirin.


[![Yeni bir sınıf oluşturma](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Şekil 04**: yeni bir sınıf oluşturursunuz ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-controller-vb/_static/image8.png))


**3 - Controllers\PersonController.vb listeleme**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Denetleyici listeleme 3'te, "Hello World!" dizesini döndürür İNDİS() adlı bir eylem kullanıma sunar. Bu denetleyici eylemi, uygulamanızı çalıştıran ve aşağıdaki gibi bir URL isteyen çağırabilirsiniz:

`http://localhost:40071/Person`

> [!NOTE]
> 
> ASP.NET Geliştirme Sunucusu bir rastgele bağlantı noktası numarası (örneğin, 40071) kullanır. Bir denetleyici çağırmak için bir URL girerken, doğru bağlantı noktası numarası girmeniz gerekir. ASP.NET Geliştirme Sunucusu (sağ alt ekranınızın) Windows bildirim alanındaki simgenin üzerine farenizi gelerek, bağlantı noktası numarasını belirleyebilirsiniz.
> 
> [!div class="step-by-step"]
> [Önceki](adding-dynamic-content-to-a-cached-page-vb.md)
> [İleri](creating-an-action-vb.md)
