---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: URL yönlendirme | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.5 ve Visual Studio 2013 Express için kullandığımız bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 556ef01304d0b5a3cca3606d71ef055ce4b2dc5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389156"
---
<a name="url-routing"></a>URL yönlendirme
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Bir Visual Studio 2013'ün [C# kaynak kodu ile proje](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici serisinin eşlik etmek üzere hazırdır.


Bu öğreticide, Wingtip Toys örnek uygulama, URL yönlendirme desteklemek için değiştirir. Yönlendirme kolay, unutmayın daha kolay ve daha iyi arama motorları tarafından desteklenen URL'lerini kullanacak şekilde web uygulamanızı sağlar. Bu öğreticide, önceki öğreticide "Üyelik ve yönetim" oluşturur ve Wingtip Toys öğretici serisinin bir parçasıdır.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Nasıl bir ASP.NET Web Forms uygulaması için rotalar kaydedin.
- Yollar bir web sayfasına ekleme.
- Nasıl yollar desteklemek için bir veritabanından verileri seçin.

## <a name="aspnet-routing-overview"></a>ASP.NET yönlendirmeye genel bakış

URL yönlendirme kabul etmek için bir uygulama yapılandırmanıza olanak tanır fiziksel dosyaları eşlemeyin URL'leri isteyin. Yalnızca web sitenizde bir sayfayı bulmak için kendi tarayıcıya kullanıcının girdiği URL bir istek URL'sidir. Anlamsal olarak kullanıcılar için anlamlı olan ve arama motoru iyileştirmesi (SEO) yardımcı olan URL'ler tanımlamak için yönlendirme kullanır.

Varsayılan olarak, Web Forms şablonu içerir [ASP.NET kolay URL'leri](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Temel yönlendirme işin çoğunu kullanarak uygulanacak *kolay URL'leri*. Ancak, bu öğreticide, özelleştirilmiş yönlendirme özelliklerini ekleyeceksiniz.

Wingtip Toys örnek uygulama, URL yönlendirme özelleştirmeden önce aşağıdaki URL'yi kullanarak bir ürüne bağlayabilirsiniz:

`https://localhost:44300/ProductDetails.aspx?productID=2`

URL yönlendirme özelleştirerek Wingtip Toys örnek uygulamanın bir okunmasını URL'yi kullanarak aynı ürün için bağlantı:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Yollar

Bir rota için bir işleyici eşlenmiş bir URL deseni şudur. İşleyici, bir Web Forms uygulaması bir .aspx dosyası gibi bir fiziksel dosya olabilir. Bir işleyici isteği işleyen bir sınıf da olabilir. Bir rota tanımlamak için URL deseni, işleyici ve isteğe bağlı olarak yol için bir ad belirterek rota sınıfının bir örneğini oluşturun.

Ekleyerek uygulamayı rotası ekleyin `Route` statik nesne `Routes` özelliği `RouteTable` sınıfı. Yollar özelliği bir `RouteCollection` uygulama için tüm yolları depolayan bir nesne.

### <a name="url-patterns"></a>URL desenleri

Bir URL deseni, değişmez değerler ve değişken yer tutucuları (URL parametreleri adlandırılır) içerebilir. Değişmez değerler ve yer tutucu URL'nin eğik çizgiyle ayrılmış segmenti bulunur (`/`) karakter.

Web uygulamanız için bir istek yapıldığında, URL kesimleri ve yer tutucu ayrıştırılır ve değişken değerleri için istek işleyicisi sağlanır. Bu işlem, bir sorgu dizesi verileri ayrıştırılır ve istek işleyicisine geçirilen şekilde benzerdir. Her iki durumda da, değişken bilgileri URL'de bulunan ve anahtar-değer çiftleri biçiminde işleyicisine geçirilen. Sorgu için anahtarlar ve değerler hem URL'de dizelerdir. Yollar için URL deseninde tanımlanan yer tutucu adlarını anahtarlarıdır ve yalnızca URL'ye değerlerdir.

Bir URL deseni yer tutucuları küme ayraçları içine alarak tanımladığınız ( `{` ve `}` ). Birden fazla yer tutucu bir segmente tanımlayabilirsiniz, ancak yer tutucuları değişmez değer ayrılmış olması gerekir. Örneğin, `{language}-{country}/{action}` geçerli bir rota modelidir. Ancak, `{language}{country}/{action}` değişmez değer ya da yer tutucuları bölücüyü olduğundan geçerli bir düzen değil. Bu nedenle, yönlendirme değeri dil yer tutucusu için için Ülke yer tutucu değerini ayırmak yeri belirlenemiyor.

### <a name="mapping-and-registering-routes"></a>Eşleştirme ve yollar kaydediliyor

Wingtip Toys örnek uygulamanın sayfaları yolları içerebilir önce uygulama başladığında yolları kaydetmeniz gerekir. Yollar kaydetmek için değiştireceğiniz `Application_Start` olay işleyicisi.

1. İçinde **Çözüm Gezgini**Visual Studio bulma ve açma *Global.asax.cs* dosya.
2. Sarı ile vurgulanmış kodu ekleyin *Global.asax.cs* aşağıdaki gibi:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Wingtip Toys örneği uygulama başladığında, çağrı `Application_Start` olay işleyicisi. Bu olay işleyicisi sonunda `RegisterCustomRoutes` yöntemi çağrılır. `RegisterCustomRoutes` Yöntemi çağırarak her bir rota ekler `MapPageRoute` yöntemi `RouteCollection` nesne. Yol, yönlendirme URL'sini ve fiziksel bir URL, bir rota adı kullanılarak tanımlanır.

İlk parametresi ("`ProductsByCategoryRoute`") rota adı. Rota gerektiğinde çağırmak için kullanılır. İkinci parametresi ("`Category/{categoryName}`") dinamik URL tabanlı kodu kolay değiştirme tanımlar. Bir veri denetim verilerini temel alarak oluşturulan bağlantılarla birlikte doldurulurken bu yolu kullanın. Bir rota şu şekilde gösterilir:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Bir dinamik değerin ayraç içinde belirtilen yol'öğesinin ikinci parametresi içerir (`{ }`). Bu durumda, `categoryName` uygun yönlendirme yolunu belirlemek için kullanılan bir değişkendir.

> [!NOTE] 
> 
> **Optional**
> 
> Kodunuzu taşıyarak yönetilmesi daha kolay bulabilirsiniz `RegisterCustomRoutes` ayrı bir sınıf için yöntemi. İçinde *mantıksal* klasörü, ayrı bir oluşturma `RouteActions` sınıfı. Yukarıdaki taşıma `RegisterCustomRoutes` yönteminden *Global.asax.cs* yeni dosyaya `RoutesActions` sınıfı. Kullanım `RoleActions` sınıfı ve `createAdmin` yöntemi çağırmak nasıl bir örnek olarak `RegisterCustomRoutes` yönteminden *Global.asax.cs* dosya.


Ayrıca fark etmiş `RegisterRoutes` yöntemi kullanarak `RouteConfig` nesne başına `Application_Start` olay işleyicisi. Varsayılan yönlendirme uygulamak için bu çağrı yapılır. Visual Studio Web Forms şablonunu kullanarak uygulama oluştururken varsayılan kod olarak eklendi.

## <a name="retrieving-and-using-route-data"></a>Alma ve rota verilerini kullanma

Yukarıda belirtildiği gibi yollar tanımlanabilir. Eklediğiniz kod `Application_Start` olay işleyicisinde *Global.asax.cs* tanımlanabilir yolları dosya yükler.

### <a name="setting-routes"></a>Rotalar ayarlama

Yollar ek kod eklemenizi gerektirir. Bu öğreticide, model bağlama almak için kullanacağı bir `RouteValueDictionary` veri denetimi verilerini kullanarak yolları oluştururken kullanılan nesne. `RouteValueDictionary` Nesne ürünlerin belirli bir kategoriye ait ürün adlarının bir listesini içerir. Veri ve rota tabanlı her ürün için bir bağlantı oluşturulur.

#### <a name="enable-routes-for-categories-and-products"></a>Kategoriler ve ürünler için rotalar etkinleştir

Ardından, uygulamayı kullanacak şekilde güncelleştireceksiniz `ProductsByCategoryRoute` her bir ürün kategorisini bağlantısı için içerecek şekilde doğru yolu belirlemek için. Ayrıca güncelleştireceksiniz *ProductList.aspx* sayfasına her ürün için gönderilmiş bir bağlantı içerir. Değişiklikten önce oldukları gibi bağlantıları artık URL yönlendirme kullanır ancak bağlantılar görüntülenir.

1. İçinde **Çözüm Gezgini**açın *Site.Master* zaten açık değilse, sayfa.
2. Güncelleştirme **ListView** adlı Denetim "`categoryList`" sarı ile vurgulanmış değişikliklerle birlikte, bu nedenle işaretleme şu şekilde görünür:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. İçinde **Çözüm Gezgini**açın *ProductList.aspx* sayfası.
4. Güncelleştirme `ItemTemplate` öğesinin *ProductList.aspx* biçimlendirme gibi görünecek şekilde, sarı ile vurgulanmış güncelleştirmeleri sayfası:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Arka plan kod, açık *ProductList.aspx.cs* ve şu ad alanı vurgulanmış sarı ekleyin:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Değiştirin `GetProducts` arka plan kod yöntemi (*ProductList.aspx.cs*) aşağıdaki kod ile:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Ürün ayrıntılarını kod ekleyin

Şimdi, arka plan kod güncelleştir (*ProductDetails.aspx.cs*) için *ProductDetails.aspx* rota verilerini kullanmak için sayfa. Dikkat yeni `GetProduct` yöntemi de eski kolay olmayan, yönlendirilmeyen URL'yi kullanır bozulmasına neden olan bir bağlantı kullanıcının sahip olduğu durum için bir sorgu dizesi değerini kabul eder.

1. Değiştirin `GetProduct` arka plan kod yöntemi (*ProductDetails.aspx.cs*) aşağıdaki kod ile:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Güncelleştirilmiş yollarını görmek için uygulamayı şimdi çalıştırabilirsiniz.

1. Tuşuna **F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Tarayıcı açılır ve gösterir *Default.aspx* sayfası.
2. Tıklayın **ürünleri** sayfanın üstündeki bağlantısı.  
 Tüm ürünler hakkında görüntülenen *ProductList.aspx* sayfası. (Bağlantı noktası numaranızı kullanılarak) aşağıdaki URL için tarayıcının görüntülenir:  
    `https://localhost:44300/ProductList`
3. Ardından, **otomobiller** sayfanın üst kısmındaki kategori bağlantı.  
 Yalnızca otomobiller görüntülenir *ProductList.aspx* sayfası. (Bağlantı noktası numaranızı kullanılarak) aşağıdaki URL için tarayıcının görüntülenir:  
    `https://localhost:44300/Category/Cars`
4. İlk araba adını içeren bağlantı sayfada listelenen tıklayın ("**dönüştürülebilir araba**") ürün ayrıntılarını görüntülemek için.  
 (Bağlantı noktası numaranızı kullanılarak) aşağıdaki URL için tarayıcının görüntülenir:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Ardından, tarayıcıya (bağlantı noktası numaranızı kullanılarak) aşağıdaki yönlendirilmeyen URL'yi girin:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Kod, bir kullanıcı işareti bağlantı sahip olduğu durum için bir sorgu dizesi içeren bir URL hala tanır.

## <a name="summary"></a>Özet

Bu öğreticide, kategoriler ve ürünler için yollar ekledik. Model bağlama kullanan veri denetimleri ile yolların nasıl tümleştirilebilir öğrendiniz. Sonraki öğreticide, genel hata işleme uygular.

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET kolay URL'leri](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Azure App Service'e bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET Web Forms uygulaması dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Önceki](membership-and-administration.md)
> [İleri](aspnet-error-handling.md)
