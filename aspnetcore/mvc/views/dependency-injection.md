---
title: "Görünümler içinde bağımlılık ekleme"
author: ardalis
description: 
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: 4586f50bc663b7269914dfff28b61342e3991a48
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-into-views"></a>Görünümler içinde bağımlılık ekleme

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET çekirdeği destekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) görünümleri içine. Bu, yerelleştirme veya yalnızca görünüm öğeleri yerleştirmek için gereken verileri gibi görünüm özgü Hizmetleri için yararlı olabilir. Korumak denemelisiniz [sorunları ayrılması](http://deviq.com/separation-of-concerns/) görünümleri ve denetleyicilerini arasında. Kendi görünümlerinizi görüntülemek verilerin çoğu denetleyicisinden geçirilmesi.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Basit bir örnek

Bir görünümü kullanarak bir hizmet ekleyemezsiniz `@inject` yönergesi. Düşünebilirsiniz `@inject` görünümünüze özellik ekleme ve DI kullanan özellik doldurmak olarak.

Sözdizimi `@inject`:`@inject <type> <name>`

Bir örneği `@inject` eylem:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Bu görünüm bir listesini görüntüler `ToDoItem` genel istatistiklerini gösteren bir Özet birlikte örnekleri. Özet doldurulur eklenen gelen `StatisticsService`. Bu hizmet bağımlılık ekleme için kayıtlı `ConfigureServices` içinde *haline*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` Kümesi üzerinde bazı hesaplamalar gerçekleştirir `ToDoItem` depo erişen örnekleri:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

Örnek deposu, bir bellek içi koleksiyonu kullanır. Yukarıda gösterilen uygulama (tüm verilerin bellekte faaliyet) büyük, uzaktan erişilen veri kümeleri için önerilmez.

Örnek verileri modele görünüme bağlı ve görünüme eklenen hizmet görüntüler:

![Toplam öğeleri listeleme görüntülemek için öğeleri, ortalama öncelik ve bunların öncelik düzeyleri ve tamamlanma gösteren Boole değerleri ile görevlerin bir listesi tamamlandı.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Arama veri doldurma

Görünüm ekleme açılır listeleri gibi kullanıcı Arabirimi öğeleri seçeneklerinde doldurmak yararlı olabilir. Cinsiyetiniz, durumu ve diğer tercihlerinizi belirlemek için seçenekleri içeren bir kullanıcı profili form göz önünde bulundurun. Standart bir MVC yaklaşım kullanarak formu işlemeye veri erişim Hizmetleri, bu seçenekleri her istemek ve bir modeli doldurmak için denetleyici duyar veya `ViewBag` her bağlanması için seçenekler kümesi.

Alternatif bir yaklaşım Hizmetleri seçenekleri elde etmek için doğrudan görünümüne yerleştirir. Bu görünüm öğesi yapım mantığı görünüme taşıma denetleyicisi tarafından gerekli kod miktarını azaltır. Profil örneği form geçirmek bir profil düzenleme formu görüntülemek için denetleyici eylemi yalnızca gerekir:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Bu tercihler güncelleştirmek için kullanılan HTML formu açılır listeleri üç özellikleri içerir:

![Profil görünümü adını, cinsiyetiniz, durumu ve sık kullanılan renk girişi sağlayan bir formla güncelleştirin.](dependency-injection/_static/updateprofile.png)

Bu listeler görünüme eklenen hizmet tarafından doldurulur:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` Yalnızca bu form için gereken verileri sağlamak üzere tasarlanmış bir kullanıcı Arabirimi düzeyi hizmeti:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> Bağımlılık ekleme aracılığıyla istek türlerinin kaydetmeyi unutmayın `ConfigureServices` yönteminde *haline*.

## <a name="overriding-services"></a>Hizmetleri geçersiz kılma

Yeni hizmetler injecting yanı sıra bu tekniği de daha önce eklenen Hizmetleri sayfasında geçersiz kılmak için kullanılabilir. Aşağıdaki şekilde tüm kullanılabilir alanlar ilk örnekte kullanılan sayfasında gösterilmektedir:

![IntelliSense bağlam menüsünde yazılmış bir @ simgesinden Html, bileşen, StatsService ve Url alanlarını listeleme](dependency-injection/_static/razor-fields.png)

Gördüğünüz gibi varsayılan alanlar `Html`, `Component`, ve `Url` (yanı sıra `StatsService` biz hatalara). Örneği için varsayılan HTML Yardımcıları kendi ile değiştirmek istiyorsanız, kolayca bunu kullanarak yapabilirsiniz, `@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Var olan hizmetleri genişletmek istiyorsanız, içinden devralma veya var olan bir uygulama ile kendi kaydırma sırasında yalnızca bu tekniği kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca Bkz.

* Simon Timms Blog: [, görünüme arama verilerini alma](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
