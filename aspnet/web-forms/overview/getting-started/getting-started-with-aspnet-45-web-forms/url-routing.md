---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: URL yönlendirme | Microsoft Docs
author: Erikre
description: Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: a195b36517bcae4bbeaf43fe7386e7787fd00212
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="url-routing"></a>URL yönlendirme
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.


Bu öğreticide, URL yönlendirme desteklemek için Wingtip Toys örnek uygulama değiştirecektir. Yönlendirme kolay, anımsaması kolay ve daha iyi arama motorları tarafından desteklenen URL'leri kullanmak, web uygulamanızı sağlar. Bu öğretici önceki öğreticiyi "Üyeliği ve yönetim" oluşturur ve Wingtip Toys öğretici serinin bir parçasıdır.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Bir ASP.NET Web Forms uygulama için rota nasıl.
- Yollar bir web sayfasına ekleme.
- Yollar desteklemek için bir veritabanından veri seçmek nasıl.

## <a name="aspnet-routing-overview"></a>ASP.NET yönlendirme genel bakış

URL yönlendirme kabul etmek için bir uygulama yapılandırmanıza olanak tanır fiziksel dosyalarına eşleme değil URL'leri isteyin. Yalnızca web sitenizde bir sayfayı bulmak için tarayıcıda kullanıcının girdiği URL isteği URL'dir. Kullanıcılara anlamsal olarak anlamlı ve arama motoru iyileştirmesi (SEO) yardımcı olabilecek URL'leri tanımlamak için yönlendirme kullanın.

Varsayılan olarak, Web Forms şablonu içerir [ASP.NET kolay URL'leri](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Temel yönlendirme işin çoğunu kullanarak uygulanacak *kolay URL'leri*. Ancak, bu öğreticide, özelleştirilmiş Yönlendirme yetenekleri ekleyeceksiniz.

Wingtip Toys örnek uygulama, URL yönlendirme özelleştirmeden önce aşağıdaki URL'yi kullanarak bir ürün bağlayabilirsiniz:

`https://localhost:44300/ProductDetails.aspx?productID=2`

URL yönlendirme özelleştirerek Wingtip Toys örnek uygulama bir okunmasını URL'yi kullanarak aynı ürün Bağla:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Yollar

Bir işleyiciye eşlenmiş bir URL deseni yoldur. İşleyici, bir Web Forms uygulamasında .aspx dosyası gibi fiziksel bir dosya olabilir. Bir işleyici isteği işleyen bir sınıf da olabilir. Bir rota tanımlamak için URL deseni, işleyici ve isteğe bağlı olarak yol için bir ad belirterek rota sınıfının bir örneğini oluşturun.

Rota ekleyerek uygulamaya eklemek `Route` statik nesnesine `Routes` özelliği `RouteTable` sınıfı. Yollar özelliği bir `RouteCollection` uygulama için tüm yolları depolar nesnesi.

### <a name="url-patterns"></a>URL desenlerini

Bir URL deseni değişmez değerler ve değişken yer tutucuları (URL parametre olarak adlandırılır) içerebilir. Değişmez değerler ve yer tutucuları, eğik çizgiyle ayrılmış URL kesimleri bulunur (`/`) karakter.

Web uygulamanız için bir istek yapıldığında, URL kesimleri ve yer tutucular olarak ayrıştırılır ve değişken değerleri istek işleyicisi sağlanır. Bu işlem, bir sorgu dizesi verileri ayrıştırılır ve istek işleyicisine geçirilen şekilde benzer. Her iki durumda da, değişken bilgileri URL'de bulunan ve anahtar-değer çiftleri biçiminde işleyicisine geçirilen. Sorgu dizeleri için URL'de anahtarları ve değerleri şunlardır. Yollar için URL deseni tanımlanan yer tutucu adlarının anahtarları ve yalnızca URL'de değerlerdir.

Bir URL deseni yer tutucuları ayraçlar içinde kapsayan tarafından tanımladığınız ( `{` ve `}` ). Birden fazla yer tutucu bir segmente tanımlayabilirsiniz, ancak yer tutucuları bir hazır değer tarafından ayrılmış olması gerekir. Örneğin, `{language}-{country}/{action}` geçerli bir rota deseni. Ancak, `{language}{country}/{action}` değişmez değer veya yer tutucuları bölücüyü olmadığından geçerli bir düzen değil. Bu nedenle, yönlendirme değeri için dil yer tutucu değerini için Ülke yer tutucu ayırmak nereye belirleyemiyor.

### <a name="mapping-and-registering-routes"></a>Eşleme ve yolları kaydediliyor

Wingtip Toys örnek uygulamasının sayfalara yollar içerebilir önce uygulama başladığında yolları kaydetmeniz gerekir. Yollar kaydetmek için değiştirecek `Application_Start` olay işleyicisi.

1. İçinde **Çözüm Gezgini**Visual Studio bulma ve açma *Global.asax.cs* dosya.
2. Sarı vurgulanmış kodu ekleyin *Global.asax.cs* gibi dosya:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Uygulama başladığında Wingtip Toys örnek, çağıran `Application_Start` olay işleyicisi. Bu olay işleyicisi sonunda `RegisterCustomRoutes` yöntemi çağrılır. `RegisterCustomRoutes` Yöntemi çağrılarak her yol ekler `MapPageRoute` yöntemi `RouteCollection` nesnesi. Yollar bir yönlendirme URL'si ve fiziksel bir URL, bir rota adı kullanılarak tanımlanır.

İlk parametresi ("`ProductsByCategoryRoute`") rota adı. Gerektiğinde rota çağırmak için kullanılır. İkinci parametre ("`Category/{categoryName}`") dinamik URL'sine bağlı kodu kolay değiştirme tanımlar. Oluşturulan verilerine dayalı bağlantılar veri denetimiyle doldurulurken bu yolu kullanın. Bir rota gibi gösterilir:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

İkinci parametre rotanın kaşlı ayraç belirtilen dinamik bir değer içeriyorsa (`{ }`). Bu durumda, `categoryName` uygun yönlendirme yolunu belirlemek için kullanılan bir değişkendir.

> [!NOTE] 
> 
> **Optional**
> 
> Kodunuzu taşıyarak yönetilmesi daha kolay bulabilirsiniz `RegisterCustomRoutes` yöntemi için ayrı bir sınıf. İçinde *mantığı* klasörü, ayrı bir oluşturma `RouteActions` sınıfı. Yukarıdaki taşıma `RegisterCustomRoutes` yönteminden *Global.asax.cs* yeni dosyasına `RoutesActions` sınıfı. Kullanım `RoleActions` sınıfı ve `createAdmin` yöntemi çağırmak nasıl bir örnek olarak `RegisterCustomRoutes` yönteminden *Global.asax.cs* dosya.


Ayrıca fark etmiş `RegisterRoutes` çağrı yöntemini kullanarak `RouteConfig` nesne başındaki `Application_Start` olay işleyicisi. Varsayılan yönlendirme uygulamak için bu çağrı yapılır. Visual Studio'nun Web Forms şablonunu kullanarak uygulama oluşturduğunuzda varsayılan kod olarak dahil.

## <a name="retrieving-and-using-route-data"></a>Alma ve rota verilerini kullanma

Yukarıda belirtildiği gibi yolları tanımlanabilir. İçin eklenen kod `Application_Start` olay işleyicisini *Global.asax.cs* dosya tanımlanabilir yollar yükler.

### <a name="setting-routes"></a>Yollar ayarlama

Yollar ek kod eklemenizi gerektirir. Bu öğreticide, model bağlama almak için kullanacağı bir `RouteValueDictionary` veri denetimi verilerini kullanarak yolları oluşturulurken kullanılan nesne. `RouteValueDictionary` Nesne ürünlerin belirli bir kategoriye ait ürün adlarının bir listesini içerir. Veri ve rota göre her ürün için bir bağlantı oluşturulur.

#### <a name="enable-routes-for-categories-and-products"></a>Kategoriler ve ürünler için yollar etkinleştir

Ardından, uygulamayı kullanacak şekilde güncelleştireceğim `ProductsByCategoryRoute` her ürün kategorisi bağlantısını dahil etmek için doğru yol belirlemek için. Ayrıca güncelleştireceğim *ProductList.aspx* sayfası her ürün için yönlendirilmiş bir bağlantı içerir. Değişiklikten önce oldukları gibi bağlantılar, şimdi URL yönlendirme kullanır ancak bağlantılar görüntülenir.

1. İçinde **Çözüm Gezgini**, açık *Site.Master* zaten açık değilse, sayfa.
2. Güncelleştirme **ListView** adlı Denetim "`categoryList`" sarı ile vurgulanmış değişikliklerle birlikte, bu nedenle işaretleme şu şekilde görünür:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. İçinde **Çözüm Gezgini**, açık *ProductList.aspx* sayfası.
4. Güncelleştirme `ItemTemplate` öğesinin *ProductList.aspx* işaretleme şu şekilde görünmesi sarı ile vurgulanmış güncelleştirmeleri sayfasıyla:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Arka plan kod, açık *ProductList.aspx.cs* ve vurgulanmış sarı aşağıdaki ad alanı Ekle:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Değiştir `GetProducts` arka plan kodu yöntemi (*ProductList.aspx.cs*) aşağıdaki kod ile:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Ürün Ayrıntıları için kod ekleme

Şimdi, arka plan kodu güncelleştir (*ProductDetails.aspx.cs*) için *ProductDetails.aspx* rota verilerini kullanmak için sayfa. Dikkat yeni `GetProduct` yöntemi de eski kolay olmayan, yönlendirilmeyen URL'yi kullanır işareti bağlantı kullanıcının sahip olduğu durum için bir sorgu dizesi değerini kabul eder.

1. Değiştir `GetProduct` arka plan kodu yöntemi (*ProductDetails.aspx.cs*) aşağıdaki kod ile:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Güncelleştirilen yolları görmek için uygulamayı şimdi çalıştırabilirsiniz.

1. Tuşuna **F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Tarayıcı açılır ve gösterir *Default.aspx* sayfası.
2. Tıklatın **ürünleri** sayfanın üst kısmındaki bağlantı.  
 Tüm ürünleri görüntülenir *ProductList.aspx* sayfası. (Bağlantı noktası numarası kullanarak) aşağıdaki URL için tarayıcıdan görüntülenir:  
    `https://localhost:44300/ProductList`
3. Bundan sonra öğesini **araba** kategori bağlantısını sayfanın üstüne yakın.  
 Yalnızca araba görüntülenir *ProductList.aspx* sayfası. (Bağlantı noktası numarası kullanarak) aşağıdaki URL için tarayıcıdan görüntülenir:  
    `https://localhost:44300/Category/Cars`
4. İlk arabanızın adını içeren bağlantı sayfada listelenen tıklatın ("**dönüştürülebilir araba**") ürün ayrıntıları görüntülemek için.  
 (Bağlantı noktası numarası kullanarak) aşağıdaki URL için tarayıcıdan görüntülenir:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Ardından, (bağlantı noktası numarası kullanarak) aşağıdaki yönlendirilmeyen URL tarayıcısına girin:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Kod bir kullanıcı işareti eklediğiniz bir bağlantıya sahip olduğu durum için bir sorgu dizesi içeren bir URL hala tanır.

## <a name="summary"></a>Özet

Bu öğreticide, kategoriler ve ürünler için rota eklediniz. Model bağlama kullanan veri denetimleri ile yolların nasıl tümleştirilebilir öğrendiniz. Sonraki öğreticide genel hata işleme gerçekleştireceksiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET kolay URL'leri](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Güvenli ASP.NET Web Forms uygulama üyeliği, OAuth ve SQL veritabanı ile Azure App Service'e dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Önceki](membership-and-administration.md)
> [sonraki](aspnet-error-handling.md)
