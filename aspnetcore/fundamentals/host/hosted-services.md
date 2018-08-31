---
title: ASP.NET core'da barındırılan hizmetler ile arka plan görevleri
author: guardrex
description: ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: cc8f7fa00436a847ab1d1ba0976fb5e3899576ee
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312134"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>ASP.NET core'da barındırılan hizmetler ile arka plan görevleri

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core, arka plan görevleri olarak uygulanabilir *barındırılan hizmetleri*. Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır [Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi. Bu konu başlığı altında üç barındırılan hizmet örnekleri sağlar:

* Bir zamanlayıcıyı temel çalışan arka plan görev.
* Kapsamlı bir hizmet etkinleştirir, barındırılan hizmeti. Kapsamlı hizmet, bağımlılık ekleme kullanabilirsiniz.
* Sırayla çalışır kuyruğa alınmış arka plan görevleri.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulama, iki sürümde sağlanır:

* Web ana bilgisayar &ndash; Web ana bilgisayarı, web uygulamalarını barındırmak için kullanışlıdır. Bu konuda gösterilen örnek kod örnek Web ana bilgisayarı sürümünden ' dir. Daha fazla bilgi için [Web ana bilgisayarı](xref:fundamentals/host/web-host) konu.
* Genel konak &ndash; ASP.NET Core 2.1 içinde genel ana bilgisayar içindir. Daha fazla bilgi için [genel ana bilgisayar](xref:fundamentals/host/generic-host) konu.

## <a name="ihostedservice-interface"></a>Ihostedservice arabirimi

Barındırılan hizmetler uygulama [Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi. Ana bilgisayar tarafından yönetilen nesneler için iki yöntem arabirimin tanımlar:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync)  -  `StartAsync` arka plan görevini başlatmak için mantığı içerir. Kullanırken [Web ana bilgisayarı](xref:fundamentals/host/web-host), `StartAsync` sunucu yeniden başlatıldıktan sonra çağrılır ve [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) tetiklenir. Kullanırken [genel ana bilgisayar](xref:fundamentals/host/generic-host), `StartAsync` önce çağrılır `ApplicationStarted` tetiklenir.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -konak normal şekilde kapatılmasını gerçekleştirirken tetiklendi. `StopAsync` Son arka plan görevinin ve herhangi bir yönetilmeyen kaynakların dispose mantığını içerir. Uygulama beklenmedik bir şekilde (örneğin, uygulamanın işlemi başarısız olur), kapanırsa `StopAsync` değil olarak adlandırılabilir.

Barındırılan hizmet uygulamasını başlangıçta ve düzgün biçimde kapatma uygulama kapatma sırasında bir kez etkinleştirilir. Zaman [IDisposable](/dotnet/api/system.idisposable) olan hizmet kapsayıcı uygulanan, kaynakları silinmediğinde. Arka plan görevi yürütme sırasında bir hata oluşturulursa `Dispose` çağrılmalıdır bile `StopAsync` adı değil.

## <a name="timed-background-tasks"></a>Zamanlanmış arka plan görevleri

Zamanlanmış arka plan görevi kullanır [süre System.Threading.Timer](/dotnet/api/system.threading.timer) sınıfı. Görev Zamanlayıcı tetiklendikten `DoWork` yöntemi. Zamanlayıcıyı devre dışı `StopAsync` ve hizmet kapsayıcısı üzerinde silinmediğinde `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Hizmet kayıtlı `Startup.ConfigureServices` ile `AddHostedService` genişletme yöntemi:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Kapsamlı bir hizmet bir arka plan görevi olarak kullanma

Kapsamlı Hizmetleri içinde kullanmak için bir `IHostedService`, bir kapsam oluşturun. Kapsam, bir barındırılan hizmet için varsayılan olarak oluşturulur.

Kapsamlı bir arka plan görev hizmeti arka plan görev mantığını içerir. Aşağıdaki örnekte, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) hizmetinde eklenmiş olur:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Barındırılan hizmet çağırmak için kapsamlı bir arka plan görev hizmeti çözümlemek için bir kapsam oluşturur, `DoWork` yöntemi:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Hizmetleri kayıtlı `Startup.ConfigureServices`. `IHostedService` Uygulaması ile kayıtlı `AddHostedService` genişletme yöntemi:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Sıraya alınan arka plan görevleri

Arka plan görev sırası, .NET tabanlı 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([ASP.NET Core 3.0 için yerleşik olarak geçici olarak zamanlanmış](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

İçinde `QueueHostedService`, arka plan görevleri sırasındaki sıradan çıkarılan ve olarak yürütülen bir [BackgroundService](/dotnet/api/microsoft.extensions.hosting.backgroundservice), uzun çalışan bir uygulama için bir temel sınıf olan `IHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=16,20)]

Hizmetleri kayıtlı `Startup.ConfigureServices`. `IHostedService` Uygulaması ile kayıtlı `AddHostedService` genişletme yöntemi:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

Dizin Sayfası model sınıfında `IBackgroundTaskQueue` oluşturucuya eklenen ve atanan `Queue`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Zaman **Görev Ekle** düğmesi seçili dizin sayfasında `OnPostAddTask` yöntemi yürütülür. `QueueBackgroundWorkItem` İş öğesini kuyruğa çağrılır:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Ihostedservice ve BackgroundService sınıfı ile mikro hizmetler içindeki arka plan görevlerini uygulama](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [Süre System.Threading.Timer](/dotnet/api/system.threading.timer)
