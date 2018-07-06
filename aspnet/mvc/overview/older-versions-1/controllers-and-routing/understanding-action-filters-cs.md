---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Eylem filtrelerini (C#) Anlama | Microsoft Docs
author: microsoft
description: Bu öğreticide eylem filtrelerini açıklamak için hedefidir. Bir eyleme eylem filtresi bir denetleyici eylemi--ya da tüm bir denetleyiciye uygulanan bir özniteliktir...
ms.author: aspnetcontent
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: aaec90a14e53f0173ea09fbdbaa591ec0eb680b1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810169"
---
<a name="understanding-action-filters-c"></a>Eylem filtrelerini (C#) Anlama
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> Bu öğreticide eylem filtrelerini açıklamak için hedefidir. Bir eyleme eylem filtresi bir denetleyici eylemi--veya eylemin yürütüldüğü şeklini değiştirir tüm controller--uygulayabileceğiniz bir özniteliktir.


## <a name="understanding-action-filters"></a>Eylem filtrelerini anlama

Bu öğreticide eylem filtrelerini açıklamak için hedefidir. Bir eyleme eylem filtresi bir denetleyici eylemi--veya eylemin yürütüldüğü şeklini değiştirir tüm controller--uygulayabileceğiniz bir özniteliktir. ASP.NET MVC çerçevesi, birkaç eylem filtrelerini içerir:

- OutputCache – belirtilen bir süre için bir denetleyici eylemi çıktısı bu eylem filtresi önbelleğe alır.
- HandleError – Bu eylem filtresi, bir denetleyici eylemi yürütüldüğünde yükseltilmiş hatalarını ele alır.
- Yetkilendirme: Bu eylem filtresi, belirli kullanıcı veya rol için erişimi kısıtlamak sağlar.

Kendi özel eylem filtreleri de oluşturabilirsiniz. Örneğin, bir özel kimlik doğrulama sistemi uygulamak için bir özel eylem filtresi oluşturmak isteyebilirsiniz. Veya, denetleyici eylem tarafından döndürülen görünüm verileri değiştiren bir eylem filtresi oluşturmak isteyebilirsiniz.

Bu öğreticide, bir eyleme eylem filtresi sıfırdan oluşturmak nasıl öğrenin. Farklı işleme eylem aşamalarında Visual Studio çıkış penceresine oturum günlüğü bir eylem filtresi oluştururuz.

### <a name="using-an-action-filter"></a>Bir eylem filtresini kullanma

Bir eyleme eylem filtresi bir özniteliktir. Ayrı ayrı denetleyicisinin eylem ya da tüm bir denetleyicinin eylem filtrelerin çoğu uygulayabilirsiniz.

Örneğin, adlı bir eylem listeleme 1 veri denetleyicisi sunan `Index()` geçerli saati döndürür. Bu eylem ile donatılmış `OutputCache` eylem filtresi. Bu filtre, 10 saniye boyunca önbelleğe alınacak eylem tarafından döndürülen değeri neden olur.

**Kod 1 – `Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Art arda çağırma `Index()` URL/Data/dizin tarayıcınızın adres çubuğuna girerek ve yenileme eylemi düğmesine birden çok kez aynı anda 10 saniye boyunca görür. Çıkışı `Index()` eylem 10 (bkz. Şekil 1) saniye için önbelleğe alınır.


[![Önbelleğe alınan saati](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Şekil 01**: zaman önbelleğe ([tam boyutlu görüntüyü görmek için tıklatın](understanding-action-filters-cs/_static/image3.png))


1 ' bir tek eylem filtresi – listeleme `OutputCache` eylem filtresi – uygulanan `Index()` yöntemi. Gerekirse, aynı eylemi birden çok eylem filtre uygulayabilirsiniz. Örneğin, her ikisini de uygulamak isteyebilirsiniz `OutputCache` ve `HandleError` aynı eylem için eylem filtreleri.

1 ' listeleme `OutputCache` eylem filtresi uygulandığı `Index()` eylem. Bu öznitelik için de uygulanabilir `DataController` sınıfının kendisini. Bu durumda, denetleyici tarafından kullanıma sunulan herhangi bir eylem tarafından döndürülen sonuç 10 saniye boyunca önbelleğe alınması.

### <a name="the-different-types-of-filters"></a>Farklı türde filtreler

ASP.NET MVC çerçevesi, filtre dört farklı türlerini destekler:

1. Yetkilendirme filtreleri – uygular `IAuthorizationFilter` özniteliği.
2. Eylem filtreleri – uygular `IActionFilter` özniteliği.
3. Sonuç filtreleri – uygular `IResultFilter` özniteliği.
4. Özel durum filtreleri – uygular `IExceptionFilter` özniteliği.

Filtreler, yukarıda listelenen sırayla yürütülür. Örneğin, yetkilendirme filtreleri önce eylem filtreleri her zaman yürütülür ve özel durum filtreleri filtre her başka bir türden sonra her zaman yürütülür.

Yetkilendirme filtreleri, kimlik doğrulama ve yetkilendirme denetleyici eylemleri uygulamak için kullanılır. Örneğin, Authorize filtre bir yetkilendirme filtresi örneğidir.

Eylem filtreleri önce ve denetleyici Eylem yürütüldükten sonra yürütülen mantığı içerir. Bir denetleyici eylemi döndüren görünüm verileri değiştirmek için bir eyleme eylem filtresi örneği için kullanabilirsiniz.

Sonuç filtreleri önce ve bir görünüm sonucu yürütüldükten sonra yürütülen mantığı içerir. Örneğin, bir görünüm sonucu değiştirmek isteyebilirsiniz görünümü tarayıcıya işlenmeden önce sağ.

Özel durum filtreleri çalıştırmak için filtre son türüdür. Denetleyici eylem veya denetleyici eylem sonuçlarını tarafından oluşturulan hataları işlemek için bir özel durum filtresini kullanabilirsiniz. Özel durum filtreleri, hataları günlüğe kaydetmek için de kullanabilirsiniz.

Farklı her filtre türü, belirli bir düzende yürütülür. Aynı türde filtrelerinin yürütüldüğü sırayı denetlemek istiyorsanız, filtrenin sırası özelliği ayarlayabilirsiniz.

Tüm eylem filtrelerini temel sınıfı olan `System.Web.Mvc.FilterAttribute` sınıfı. Belirli türde bir filtre uygulamak istediğiniz sonra filtre temel sınıfından devralır ve bir veya daha fazlasını uygulayan bir sınıf oluşturmak için ihtiyacınız `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, veya `IExceptionFilter` arabirimleri.

### <a name="the-base-actionfilterattribute-class"></a>Temel ActionFilterAttribute sınıfı

ASP.NET MVC çerçevesi, bir özel eylem filtresi uygulamak daha kolay hale getirmek için bir temel içerir `ActionFilterAttribute` sınıfı. Her ikisi de bu sınıfın uyguladığı `IActionFilter` ve `IResultFilter` arabirimleri ve devralınan `Filter` sınıfı.

Terimleri burada tamamen tutarlı değil. Teknik olarak ActionFilterAttribute sınıfından devralan bir sınıf hem bir eyleme eylem filtresi hem de bir sonuç Filtresi ' dir. Ancak, gevşek anlamda kelime eylem filtresi herhangi bir türde ASP.NET MVC çerçevesi bir filtrede başvurmak için kullanılır.

Temel `ActionFilterAttribute` sınıfında aşağıdaki yöntemleri geçersiz kılabilirsiniz:

- Bir denetleyici eylemi yürütülmeden önce OnActionExecuting – bu yöntem çağrılır.
- Denetleyici Eylem yürütüldükten sonra OnActionExecuted – bu yöntem çağrılır.
- Denetleyici eylem sonucu yürütülmeden önce OnResultExecuting – bu yöntem çağrılır.
- Denetleyici eylem sonucu yürütüldükten sonra OnResultExecuted – bu yöntem çağrılır.

Sonraki bölümde, bu farklı yöntemlerinin her birini nasıl uygulayabileceğiniz göreceğiz.

### <a name="creating-a-log-action-filter"></a>Günlük eylem filtresi oluşturma

Bir özel eylem filtresi nasıl oluşturabileceğinizi göstermek için Visual Studio çıkış penceresine bir denetleyici eylemi işleme aşamalarını günlükleri bir özel eylem filtresi oluşturacağız. Bizim `LogActionFilter` listeleme 2'de yer alır.

**Kod 2 – `ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

Listeleme 2 `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, ve `OnResultExecuted()` tüm yöntemleri çağırmak `Log()` yöntemi. Yöntemin adı ve geçerli bir rota verilerini geçirilir `Log()` yöntemi. `Log()` Yöntemi, Visual Studio çıkış penceresinde bir ileti yazar (bkz: Şekil 2).


[![Visual Studio çıkış penceresine yazma](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Şekil 02**: Visual Studio çıkış penceresine yazma ([tam boyutlu görüntüyü görmek için tıklatın](understanding-action-filters-cs/_static/image6.png))


Giriş denetleyicisine listeleme 3'te nasıl bir tüm denetleyicinin sınıf için günlüğü eylem filtresi uygulayabilirsiniz gösterilmektedir. Her giriş denetleyici tarafından kullanıma sunulan eylemlerden herhangi birini çağrılır – ya da `Index()` yöntemi veya `About()` yöntemi – Visual Studio çıkış penceresinde eylem günlüğe kaydedilen işleme aşamalarını.

**Kod 3 – `Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Özet

Bu öğreticide, ASP.NET MVC eylem filtreleri için eklenmiştir. Filtreler dört farklı türleri hakkında bilgi edindiniz: Yetkilendirme filtreleri, eylem filtreleri, sonuç filtreleri ve özel durum filtreleri. Ayrıca temel hakkında bilgi edindiniz `ActionFilterAttribute` sınıfı.

Son olarak, basit bir eylem filtresinin uygulama hakkında bilgi edindiniz. Visual Studio çıkış penceresine bir denetleyici eylemi işleme aşamalarını günlükleri bir günlüğü eylem filtresi oluşturduk.

> [!div class="step-by-step"]
> [Önceki](asp-net-mvc-routing-overview-cs.md)
> [İleri](improving-performance-with-output-caching-cs.md)
