---
title: ASP.NET core'da görünümlere bağımlılık ekleme
author: ardalis
description: ASP.NET Core MVC görünümlere bağımlılık ekleme nasıl desteklediğini öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: 9b437d27a8d391db4533596674d144628a0c10b1
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207068"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a>ASP.NET core'da görünümlere bağımlılık ekleme

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core destekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) görünümlere. Bu, yerelleştirme veya yalnızca görünüm öğeleri doldurmak için gerekli veriler gibi özel görünüm Hizmetleri için yararlı olabilir. Korunacak denemelisiniz [görev ayrımı nettir](http://deviq.com/separation-of-concerns/) denetleyici ve görünüm arasında. Kendi görünümlerinizi görüntüleyin verilerden en iyi şekilde denetleyicisinden geçirilmelidir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Basit bir örnek

Bir görünümü kullanarak bir hizmet ekleyebilir `@inject` yönergesi. Düşünebilirsiniz `@inject` görünümünüzü özellik ekleme ve DI kullanan özellik dolduruluyor.

Sözdizimi `@inject`: `@inject <type> <name>`

Örneği `@inject` eylem:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Bu görünüm listesini görüntüler `ToDoItem` örnekleri, genel istatistiklerini gösteren bir özetiyle birlikte. Özet doldurulur eklenen gelen `StatisticsService`. Bu hizmet bağımlılık ekleme için kayıtlı `ConfigureServices` içinde *Startup.cs*:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` Dizi üzerinde bazı hesaplamalar yapan `ToDoItem` bir depo erişir örnekleri:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

Örnek depoyu bir bellek içi koleksiyon kullanır. Yukarıda gösterilen uygulama (Bu, tüm verilerin bellek içinde çalışır), büyük, uzaktan erişim veri kümeleri için önerilmez.

Örnek verileri modele görünüme bağlı ve görünüme eklenen hizmet görüntüler:

![Toplam öğe listesi görüntülemek için öğeleri, ortalama öncelik ve öncelik düzeyleri ve tamamlama belirten Boole değerleri görevlerinin listesi tamamlandı.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Arama verilerini doldurma

Görünüm ekleme açılır listeleri gibi kullanıcı Arabirimi öğeleri seçeneklerinde doldurmak yararlı olabilir. Cinsiyet, durumunu ve diğer tercihlerinizi belirtmek için seçenekleri içeren bir kullanıcı profili form göz önünde bulundurun. Bir standart MVC yaklaşımı kullanarak form işleme her biri, bu seçenekler için veri erişim Hizmetleri isteyin ve ardından bir model doldurmak için denetleyici içerseydi veya `ViewBag` her bağlanacak seçenek kümesi ile.

Alternatif bir yaklaşım Hizmetleri seçenekleri elde etmek için doğrudan görünümüne ekler. Bu görünüme bu görünüm öğesi oluşturma mantığı taşıma denetleyicisi tarafından gereken kod miktarını azaltır. Profil örneği form geçirmek bir profil düzenleme formu görüntülemek için denetleyici eylemi yeterlidir:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Bu tercihler güncelleştirmek için kullanılan HTML formu açılır listeleri üç özellikleri içerir:

![Profil görünümü adı, cinsiyet, durum ve sık kullanılan renk girişi sağlayan bir formla güncelleştirin.](dependency-injection/_static/updateprofile.png)

Bu listeler, görünüme eklenmiş bir hizmet tarafından doldurulur:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` Yalnızca bu form için gereken verileri sağlamak üzere tasarlanmış bir UI düzeyi hizmeti:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> Bağımlılık ekleme aracılığıyla istek türleri kaydedilecek unutmayın `Startup.ConfigureServices`. Hizmet sağlayıcısı aracılığıyla dahili olarak sorgulanır çünkü bir kaydı türü çalışma zamanında bir özel durum oluşturur. [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).

## <a name="overriding-services"></a>Hizmetleri geçersiz kılma

Yeni hizmet ekleme ek olarak, bu tekniği de bir sayfada daha önce eklenen Hizmetleri geçersiz kılmak için kullanılabilir. Aşağıdaki şekilde ilk örnekte kullanılan sayfasında, kullanılabilir alanların tümünü gösterilmektedir:

![IntelliSense bağlam menüsünde bir türü belirtilmiş @ sembolünü Html, bileşen, StatsService ve Url alanları listeleme](dependency-injection/_static/razor-fields.png)

Gördüğünüz gibi varsayılan alanları dahil `Html`, `Component`, ve `Url` (yanı sıra `StatsService` biz hatalara). Örneği için varsayılan HTML Yardımcıları kendinizinkilerle değiştirildiğinden isteseydiniz, kolayca kullanarak bunu `@inject`:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Var olan hizmetleri genişletmek isterseniz, dan devralan veya mevcut bir uygulama ile kendi sarmalama sırasında yalnızca bu tekniği kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca Bkz.

* Simon Timms Blog: [görünümünüzü arama verilerini alma](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
