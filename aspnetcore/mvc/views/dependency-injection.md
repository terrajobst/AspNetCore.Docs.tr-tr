---
title: ASP.NET Core görünümlere bağımlılık ekleme
author: ardalis
description: ASP.NET Core MVC görünümlerine bağımlılık ekleme işlemini nasıl desteklediğini öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: 6241bb8e262f64e2e30721bc5fe6f8f1be84b60d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656102"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a>ASP.NET Core görünümlere bağımlılık ekleme

[Steve Smith](https://ardalis.com/) tarafından

ASP.NET Core, görünümlere [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini destekler. Bu, yalnızca görünüm öğelerini doldurmak için gereken yerelleştirme veya veriler gibi görünüme özgü hizmetler için yararlı olabilir. Denetleyicileriniz ve görünümleriniz arasındaki [kaygıların ayrılmasını](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) korumayı denemelisiniz. Görünümlerinizin görüntüleyeceği verilerin çoğu denetleyiciden geçirilmelidir.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="configuration-injection"></a>Yapılandırma ekleme

*appSettings. JSON* değerleri doğrudan bir görünüme eklenebilir.

Bir *appSettings. JSON* dosyası örneği:

```json
{
   "root": {
      "parent": {
         "child": "myvalue"
      }
   }
}
```

`@inject`için sözdizimi: `@inject <type> <name>`

`@inject`kullanarak bir örnek:

```csharp
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration
@{
   string myValue = Configuration["root:parent:child"];
   ...
}
```

## <a name="service-injection"></a>Hizmet ekleme

Bir hizmet `@inject` yönergesini kullanarak bir görünüme eklenebilir. `@inject`, görünüme özellik ekleme ve dı kullanarak özelliği doldurma olarak düşünebilirsiniz.

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Bu görünüm, genel istatistikleri gösteren bir Özet ile birlikte `ToDoItem` örneklerinin bir listesini görüntüler. Özet, eklenen `StatisticsService`doldurulur. Bu hizmet, *Startup.cs*içinde `ConfigureServices` bağımlılık ekleme için kaydedilir:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService`, bir depo aracılığıyla eriştiği `ToDoItem` örnekleri kümesi üzerinde bazı hesaplamalar yapar:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

Örnek depo, bellek içi bir koleksiyon kullanır. Yukarıda gösterilen uygulama (bellekteki tüm veriler üzerinde çalışır), büyük ve uzaktan erişilen veri kümeleri için önerilmez.

Örnek, görüntüleme ile bağlantılı modeldeki verileri ve görünüme eklenen hizmeti görüntüler:

![Toplam öğe, tamamlanan öğe, ortalama öncelik ve tamamlanma düzeylerine sahip bir görev listesi ve tamamlanma belirten Boole değerlerini görüntülemek için.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Arama verileri dolduruluyor

Görünüm ekleme, açılan listeler gibi kullanıcı arabirimi öğelerindeki seçenekleri doldurmak için yararlı olabilir. Cinsiyet, eyalet ve diğer tercihlerin belirtilmesine yönelik seçenekleri içeren bir kullanıcı profili formu düşünün. Standart bir MVC yaklaşımı kullanarak bu tür bir formu işlemek, denetleyicinin Bu seçenek kümelerinin her biri için veri erişim Hizmetleri istemesi ve ardından bir modeli veya `ViewBag`, bağlanacak her seçenek kümesiyle doldurmasını gerektirir.

Alternatif yaklaşım, seçenekleri almak için Hizmetleri doğrudan görünüme çıkarır. Bu, denetleyicinin gerektirdiği kod miktarını en aza indirir ve bu görünüm öğesi oluşturma mantığını görünümün içine taşır. Bir profil düzenlemesi formunu görüntülemeye yönelik denetleyici eylemi yalnızca profil örneği formunu geçirmeniz gerekir:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Bu tercihleri güncelleştirmek için kullanılan HTML formu, özelliklerin üç yanındaki açılan listeleri içerir:

![Ad, cinsiyet, eyalet ve sık kullanılan renk girişine izin veren bir formla profil görünümünü güncelleştirin.](dependency-injection/_static/updateprofile.png)

Bu listeler, görünüme eklenen bir hizmet tarafından doldurulur:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService`, yalnızca bu form için gereken verileri sağlamak üzere tasarlanan bir UI düzeyi hizmettir:

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> `Startup.ConfigureServices`, bağımlılık ekleme yoluyla istediğiniz türleri kaydetmeyi unutmayın. Kaydedilmemiş bir tür, hizmet sağlayıcısı, [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice)aracılığıyla dahili olarak sorgulandığından çalışma zamanında bir özel durum oluşturur.

## <a name="overriding-services"></a>Hizmetleri geçersiz kılma

Bu teknik, ekleme yeni hizmetlere ek olarak, bir sayfada önceden eklenen Hizmetleri geçersiz kılmak için de kullanılabilir. Aşağıdaki şekilde, ilk örnekte kullanılan sayfada bulunan tüm alanlar gösterilmektedir:

![HTML, bileşen, StatsService ve URL alanları için belirlenmiş bir @ symbol üzerinde IntelliSense bağlamsal menüsü](dependency-injection/_static/razor-fields.png)

Gördüğünüz gibi, varsayılan alanlar `Html`, `Component`ve `Url` (Ayrıca eklediğimiz `StatsService`) içerir. Örneğin, varsayılan HTML yardımcılarını kendi kendinize değiştirmek istiyorsanız, `@inject`kullanarak kolayca bunu yapabilirsiniz:

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Mevcut hizmetleri genişletmek istiyorsanız, bu tekniği kullanarak var olan uygulamadan devralma veya kaydırma yaparken bu tekniği kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca Bkz.

* Simon Timms blogu: [görünümünüze arama verileri alma](https://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
