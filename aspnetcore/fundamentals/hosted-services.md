---
title: ASP.NET Core barındırılan hizmetleri ile arka plan görevleri
author: guardrex
description: ASP.NET Core arka plan görevlerini barındırılan hizmetleri ile uygulama öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosted-services
ms.openlocfilehash: 89e595fb6ef38745d7377fdaaf1780c64320efe3
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2018
ms.locfileid: "29374697"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>ASP.NET Core barındırılan hizmetleri ile arka plan görevleri

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET çekirdek arka plan görevleri olarak uygulanabilir *barındırılan hizmetlere*. Barındırılan hizmet uygulayan arka plan görevi mantığı ile bir sınıftır [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi. Bu konuda üç barındırılan hizmet örnekleri sağlar:

* Bir süreölçer çalıştıran arka plan görevi.
* Kapsamlı bir hizmet etkinleştirir barındırılan hizmet. Kapsamlı hizmet bağımlılık ekleme kullanabilirsiniz.
* Sırayla çalışır sıraya alınan arka plan görevleri.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="ihostedservice-interface"></a>IHostedService interface

Barındırılan hizmetler uygulama [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi. Ana bilgisayar tarafından yönetilen nesneler için iki yöntem arabirimi tanımlar:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) -sunucu başlatıldıktan sonra çağrılır ve [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) tetiklenir. `StartAsync` arka plan görevi başlatmak için mantığını içerir.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -ana bilgisayar normal şekilde kapatılmasını gerçekleştirirken tetiklendi. `StopAsync` Son arka plan görevi ve yönetilmeyen kaynakları dispose mantığını içerir. Uygulama beklenmedik biçimde (örneğin, uygulamanın işlemi başarısız olur), kapatılırsa `StopAsync` çağrılmaz.

Barındırılan hizmet uygulaması başlangıçta ve düzgün biçimde kapatma uygulama kapatma sırasında bir kez etkinleştirilmiş bir singleton ' dir. Zaman [IDisposable](/dotnet/api/system.idisposable) olan hizmet kapsayıcısı uygulanan kaynakları silinmediğinde. Arka plan Görev yürütülürken bir hata oluşursa `Dispose` çağrılmalıdır olsa bile `StopAsync` olarak adlandırılmaz.

## <a name="timed-background-tasks"></a>Zamanlanmış arka plan görevleri

Zamanlanmış arka plan görevi kullanır [süre System.Threading.Timer](/dotnet/api/system.threading.timer) sınıfı. Görev Zamanlayıcı tetikler `DoWork` yöntemi. Zamanlayıcı devre dışı bırakıldı `StopAsync` ve hizmet kapsayıcısı üzerinde silinmediğinde `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Hizmet kayıtlı `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Kapsamlı bir arka plan görevi hizmetinde kullanma

Kapsamlı Hizmetleri içinde kullanmak için bir `IHostedService`, bir kapsamı oluşturun. Kapsam bir barındırılan hizmet için varsayılan olarak oluşturulur.

Kapsamlı bir arka plan görev hizmeti arka plan görevin mantığını içerir. Aşağıdaki örnekte, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) hizmete eklenmiş:

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

Barındırılan hizmet çağırmak için kapsamlı bir arka plan görev hizmeti çözümlemek için bir kapsam oluşturur, `DoWork` yöntemi:

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Hizmetleri kayıtlı `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Sıraya alınan arka plan görevleri

Arka plan görev sırası .NET tabanlı 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([ASP.NET Core 2.2 yerleşik olarak kesin zamanlanmış](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

İçinde `QueueHostedService`, arka plan görevleri (`workItem`) sırasındaki kuyruktan çıkarıldı ve yürütülemiyor:

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

Hizmetleri kayıtlı `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

Dizin sayfası modeli sınıfında `IBackgroundTaskQueue` oluşturucuya eklenen ve atanan `Queue`:

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

Zaman **Görev Ekle** düğmesi dizin sayfasında, seçili `OnPostAddTask` yöntemi yürütüldüğünde. `QueueBackgroundWorkItem` sıraya alma, iş öğesi çağrılır:

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Mikro IHostedService ve BackgroundService sınıfı ile arka plan görevlerini uygulama](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
