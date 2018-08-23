---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: ASP.NET Web API Yardım sayfaları oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 2d8758034ca4339ed7e9699cf2f2643bfab87ba4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752359"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>ASP.NET Web API Yardım sayfaları oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Web API'si oluşturma, böylece diğer geliştiriciler, API'nin nasıl çağrılacağını bilir, genellikle bir Yardım sayfasını oluşturmak kullanışlıdır. Tüm belgelerini el ile oluşturabilirsiniz, ancak mümkün olduğunca otomatik iyidir.

Bu görevi kolaylaştırmak için ASP.NET Web API kitaplık çalışma zamanında otomatik olarak oluşturmak için Yardım sayfaları sağlar.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>API Yardım sayfaları oluşturma

Yükleme [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650). Bu güncelleştirme ile Web API proje şablonunu yardım sayfalarına tümleştirir.

Ardından, yeni bir ASP.NET MVC 4 projesi oluşturun ve Web API proje şablonunu seçin. Proje şablonu adlı bir örnek API denetleyicisi oluşturur `ValuesController`. Şablon API Yardım sayfaları da oluşturur. Tüm kod dosyaları Yardım sayfası proje alanlarını klasöründe yerleştirilir.

![](creating-api-help-pages/_static/image2.png)

Uygulamayı çalıştırdığınızda giriş sayfasının API Yardım sayfası için bir bağlantı içerir. Giriş sayfasından, göreli yol/Help ' dir.

![](creating-api-help-pages/_static/image3.png)

Bu bağlantı için bir API Özet sayfasında getirir.

![](creating-api-help-pages/_static/image4.png)

Bu sayfa için MVC görünümü Areas/HelpPage/Views/Help/Index.cshtml içinde tanımlanır. Düzen, giriş, başlık, stillerini ve benzeri değiştirmek için bu sayfayı düzenleyebilirsiniz.

Ana sayfanın parçası, API, denetleyici tarafından gruplandırılmış bir tablodur. Tablo girişleri kullanarak dinamik olarak oluşturulan **IApiExplorer** arabirimi. (Ben daha sonra bu arabirimi hakkında daha fazla konuşacağız.) Yeni bir API denetleyicisi eklerseniz, tablo, çalışma zamanında otomatik olarak güncelleştirilir.

Göreli URI ve HTTP yöntemi "API" sütunu listeler. "Description" sütunu, her API belgelerini içerir. Başlangıçta, yer tutucu metnini belgelerdir. Sonraki bölümde, ı, belgeleri XML açıklamalarını ekleme göstereceğiz.

Her API, örnek istek ve yanıt gövdeleri dahil olmak üzere daha ayrıntılı bilgi içeren bir sayfa için bir bağlantı içerir.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Mevcut bir projeyi yardım sayfalarına ekleme

NuGet Paket Yöneticisi'ni kullanarak yardım sayfalarına varolan bir Web API projesine ekleyebilirsiniz. Bu seçenek, bir "Web API" şablonu değerinden farklı proje şablonundan başlattığınız yararlıdır.

Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**. İçinde [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) penceresinde, aşağıdaki komutlardan birini yazın:

İçin bir **C#** uygulama: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

İçin bir **Visual Basic** uygulama: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

İki paket, bir C# ve Visual Basic için bir tane vardır. Projenizi eşleşen bir kullandığınızdan emin olun.

Bu komut, gerekli bütünleştirilmiş kodları yükler ve MVC görünümleri (alanlar/HelpPage klasöründe bulunur) Yardım sayfaları için ekler. Yardım sayfasına bir bağlantıyı el ile eklemeniz gerekir. / Help URI'dir. Razor Görünümü'nde bir bağlantı oluşturmak için aşağıdakileri ekleyin:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Ayrıca, alanları kaydedilecek emin olun. Global.asax dosyasında aşağıdaki kodu ekleyin **uygulama\_Başlat** orada değilse yöntemi:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>API belgelerine ekleme

Varsayılan olarak, Yardım sayfaları, belgeler için yer tutucu dizeleri vardır. Kullanabileceğiniz [XML belgeleri yorumları](https://msdn.microsoft.com/library/b2s063f7.aspx) belgeleri oluşturmak için. Bu özelliği etkinleştirmek için ' % s'dosyasını HelpPage/alanlar/uygulama açın\_Start/HelpPageConfig.cs ve aşağıdaki satırı açıklamadan çıkarın:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

XML belgeleri artık etkinleştirin. Çözüm Gezgini'nde projeye sağ tıklayıp seçin **özellikleri**. Seçin **derleme** sayfası.

![](creating-api-help-pages/_static/image6.png)

Altında **çıkış**, kontrol **XML belge dosyası**. Düzenleme kutusuna "uygulama\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Ardından, kodunu açmak `ValuesController` /Controllers/ValuesControler.cs içinde tanımlanan API denetleyicisi. Bazı belge açıklamaları için denetleyici yöntemleri ekleyin. Örneğin:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> İpucu: XML öğeleri, Visual Studio otomatik olarak giriş işaretini satırın yöntem yukarıda getirin ve üç eğik çizgi yazın, ekler. Ardından, boşlukları doldurabilirsiniz.


Artık derleme ve uygulamayı yeniden çalıştırın ve Yardım sayfalarına gidin. Belge dizeleri API tabloda görüntülenmesi gerekir.

![](creating-api-help-pages/_static/image8.png)

Yardım sayfasına dizeleri çalışma zamanında XML dosyasından okur. (Uygulama dağıttığınızda, XML dosyasını dağıtmak emin olun.)

## <a name="under-the-hood"></a>Başlık altında

Üst kısmındaki yardım sayfalarına yerleşik **ApiExplorer** Web API çerçevesi parçası olan sınıf. **ApiExplorer** sınıfı, bir Yardım sayfasını oluşturmak için ham madde sağlar. Her API için **ApiExplorer** içeren bir **ApiDescription** API'sini açıklayan. Bu amaç için bir "API" HTTP yöntemi ile göreli URL birleşimi tanımlanır. Örneğin, bazı ayrı API şunlardır:

- /Api/Products Al
- Alma/API'si/ürünler / {id}
- / Api/ürünleri gönderin

Bir denetleyici eylemi birden çok HTTP yöntemleri destekliyorsa **ApiExplorer** her yöntem farklı bir API olarak değerlendirir.

API'den gizlemek için **ApiExplorer**, ekleme **ApiExplorerSettings** öznitelik kümesi ve eylem *IgnoreApi* true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Ayrıca, bu özniteliği denetleyiciye tüm denetleyicinin hariç tutmak için ekleyebilirsiniz.

Belgeleri dizelerden ApiExplorer sınıfı alır **IDocumentationProvider** arabirimi. Daha önce bahsettiğim gibi Yardım sayfaları kitaplığı sağlayan bir **IDocumentationProvider** XML belgeleri dizelerden belgeleri alır. Kod içinde /Areas/HelpPage/XmlDocumentationProvider.cs bulunur. Belge başka bir kaynaktan yazarak kendi alabileceğiniz **IDocumentationProvider**. Yukarı wire çağrısı **SetDocumentationProvider** genişletme yöntemi, tanımlanan **HelpPageConfigurationExtensions**

**ApiExplorer** otomatik olarak içine yapılan çağrılar **IDocumentationProvider** her bir API için belgeler dizelerini almak için arabirim. Bu depolar **belgeleri** özelliği **ApiDescription** ve **ApiParameterDescription** nesneleri.

## <a name="next-steps"></a>Sonraki Adımlar

Burada gösterilen yardım sayfalarına sınırlı değildir. Aslında, **ApiExplorer** Yardım sayfaları oluşturma için sınırlı değildir. Kullanıma hazır düşünmek başlamanızı sağlayacak bazı harika blog yazılarını yazılmış yao Huang Bağla:

- [ASP.NET Web API Yardım sayfası için basit bir Test istemcisi ekleme](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [ASP.NET Web API Yardım şirket içinde barındırılan hizmetleri üzerinde çalışma sayfası yapma](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Yardım sayfası (veya istemci) ASP.NET Web API'si için tasarım zamanı oluşturma](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Gelişmiş Yardım sayfası özelleştirmeleri](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
