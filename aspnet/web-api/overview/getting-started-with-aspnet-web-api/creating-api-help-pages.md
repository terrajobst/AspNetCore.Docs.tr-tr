---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "ASP.NET Web API Yardım sayfası oluşturma | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 18d04492529e96b6c0e14f1d7a30378b4832f4c8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-help-pages-for-aspnet-web-api"></a>ASP.NET Web API Yardım sayfası oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Web API'si oluşturduğunuzda, diğer geliştiriciler API'nizi nasıl bilmesini genellikle bir Yardım sayfası oluşturmak yararlı olacaktır. Tüm belgelerin el ile oluşturabilirsiniz, ancak otomatik olarak mümkün olduğu kadar iyidir.

Bu görev kolaylaştırmak için ASP.NET Web API kitaplığını çalışma zamanında yardım sayfalarına otomatik oluşturmak için sağlar.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>API Yardım sayfası oluşturma

Yükleme [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650). Bu güncelleştirme, Web API projesi şablonuna yardım sayfalarına tümleştirir.

Ardından, yeni bir ASP.NET MVC 4 projesi oluşturun ve Web API projesi şablonu seçin. Proje şablonu adlı bir örnek API denetleyicisi oluşturur `ValuesController`. Şablon API yardım sayfalarına da oluşturur. Tüm kod dosyaları Yardım sayfası, proje alanları klasöründe yer alır.

![](creating-api-help-pages/_static/image2.png)

Uygulamayı çalıştırdığınızda, giriş sayfası API yardım sayfasına bir bağlantı içerir. Giriş sayfasından göreli yolu/Help ' dir.

![](creating-api-help-pages/_static/image3.png)

Bu bağlantıyı bir API Özet sayfasında getirir.

![](creating-api-help-pages/_static/image4.png)

Bu sayfa için MVC görünümü Areas/HelpPage/Views/Help/Index.cshtml tanımlanır. Düzen, giriş, başlık, stil ve benzeri değiştirmek için bu sayfayı düzenleyebilirsiniz.

Ana sayfa API'leri, denetleyici tarafından gruplandırılmış bir tablo parçasıdır. Tablo girişleri kullanarak dinamik olarak üretilen **IApiExplorer** arabirimi. (Ben daha sonra bu arabirimi hakkında daha fazla konuşun.) Yeni bir API denetleyicisi eklerseniz, tablonun çalışma zamanında otomatik olarak güncelleştirilir.

Göreli URI ve HTTP yöntemi "API" sütununda listelenir. Her API belgelerine "Açıklama" sütun içeriyor. Başlangıçta, yalnızca yer tutucu metin belgesidir. Sonraki bölümde, t, XML açıklamalardan belgelerine ekleme göstereceğiz.

Her API örnek istek ve yanıt gövdesi dahil olmak üzere daha ayrıntılı bilgi içeren bir sayfa için bir bağlantı vardır.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Varolan bir projeye yardım sayfalarına ekleme

NuGet Paket Yöneticisi'ni kullanarak, varolan bir Web API projesine yardım sayfalarına ekleyebilirsiniz. Bu seçenek, "Web API'sini" şablonundan farklı proje şablonu başlangıç yararlıdır.

Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**. İçinde [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) penceresinde, aşağıdaki komutlardan birini yazın:

İçin bir **C#** uygulama:`Install-Package Microsoft.AspNet.WebApi.HelpPage`

İçin bir **Visual Basic** uygulama:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

İki paketler, bir C# ve Visual Basic için bir vardır. Projenizi eşleşen bir kullandığınızdan emin olun.

Bu komut gerekli derlemeleri yükler ve (alanları/HelpPage klasöründe bulunur) Yardım sayfalarına için MVC görünümleri ekler. Yardım sayfasına bir bağlantıyı el ile eklemeniz gerekir. / Help URI'dir. Bir razor görünümünde bir bağlantı oluşturmak için aşağıdakileri ekleyin:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Ayrıca, alanları kaydetmek emin olun. Global.asax dosyasına aşağıdaki kodu ekleyin **uygulama\_Başlat** orada değilse yöntemi:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>API belgelerine ekleme

Varsayılan olarak, Yardım sayfaları sahip belgeleri için yer tutucu dize. Kullanabileceğiniz [XML belgeleri yorumları](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx) belgeleri oluşturmak için. Bu özelliği etkinleştirmek için HelpPage/alanları/uygulama dosyasını açın\_Start/HelpPageConfig.cs ve aşağıdaki satırı açıklamadan çıkarın:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

XML belgeleri şimdi etkinleştirin. Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **özellikleri**. Seçin **yapı** sayfası.

![](creating-api-help-pages/_static/image6.png)

Altında **çıkış**, denetleme **XML belge dosyası**. Düzenleme kutusuna "uygulama\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Ardından, kodunu açmak `ValuesController` /Controllers/ValuesControler.cs içinde tanımlanan API denetleyicisi. Bazı belge açıklamaları için denetleyici yöntemleri ekleyin. Örneğin:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> İpucu: XML öğelerini, Visual Studio otomatik olarak düzeltme işareti yöntemi üstündeki satırın getirin ve üç eğik yazarsanız, ekler. Ardından boşlukları doldurabilirsiniz.


Şimdi oluşturmak ve uygulamayı yeniden çalıştırın ve Yardım sayfalarına gidin. Belge dizeleri API tablosunda görünmesi gerekir.

![](creating-api-help-pages/_static/image8.png)

Yardım sayfası dizeleri çalışma zamanında XML dosyasından okur. (Uygulama dağıttığınızda, XML dosyasını dağıtmak emin olun.)

## <a name="under-the-hood"></a>Başlık altında

Yardım sayfalarına üstünde oluşturulan **ApiExplorer** Web API çerçevesi parçası olan sınıf. **ApiExplorer** sınıfı bir Yardım sayfasını oluşturmak için ham madde sağlar. Her API için **ApiExplorer** içeren bir **ApiDescription** API açıklar. Bu amaç için bir "API" HTTP yöntemi ile göreli URL birleşimi tanımlanır. Örneğin, bazı farklı API'leri şunlardır:

- /Api/Products Al
- /Api/ürünler / {id} Al
- / Api/ürünleri sonrası

Bir denetleyici eylemi birden çok HTTP yöntemleri destekliyorsa **ApiExplorer** her yöntem farklı bir API değerlendirir.

API'den gizlemek için **ApiExplorer**, ekleme **ApiExplorerSettings** öznitelik kümesi ve eylem *IgnoreApi* true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Bu öznitelik tüm denetleyicinin dışlamak için denetleyiciye de ekleyebilirsiniz.

Belge dizelerden ApiExplorer sınıfı alır **IDocumentationProvider** arabirimi. Daha önce anlatıldığı gibi Yardımı sayfalarında kitaplığı sağlar bir **IDocumentationProvider** XML belgeleri dizelerden belgeleri alır. Kod içinde /Areas/HelpPage/XmlDocumentationProvider.cs bulunur. Belge başka bir kaynaktan yazarak kendi alabileceğiniz **IDocumentationProvider**. Yukarı wire çağrısı **SetDocumentationProvider** genişletme yöntemi, tanımlanan **HelpPageConfigurationExtensions**

**ApiExplorer** içine otomatik olarak çağırır **IDocumentationProvider** için her API belgelerine dizeleri almak için arabirim. Bunları depolar **belgelerine** özelliği **ApiDescription** ve **ApiParameterDescription** nesneleri.

## <a name="next-steps"></a>Sonraki Adımlar

Burada gösterilen yardım sayfalarına sınırlı değildir. Aslında, **ApiExplorer** Yardım sayfaları oluşturma için sınırlı değildir. Kutudan çıktığında düşünüyorum sağlamak için bazı harika blog yazılarını yao Huang Bağla yazmıştır:

- [ASP.NET Web API Yardım sayfası için basit bir Test istemcisi ekleme](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [ASP.NET Web API Yardım kendini barındıran Services'de çalışma sayfası yapma](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Tasarım zamanı nesil Yardım sayfası (veya istemcisi) için ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Gelişmiş Yardım sayfası özelleştirmeleri](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
