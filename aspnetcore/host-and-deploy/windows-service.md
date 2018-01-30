---
title: Bir Windows hizmeti ana bilgisayar
author: tdykstra
description: "Bir ASP.NET Core uygulamayı bir Windows hizmetinde barındırma öğrenin."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Konak bir Windows hizmetinde bir ASP.NET Core uygulama

By [Tom Dykstra](https://github.com/tdykstra)

IIS kullanarak çalıştırmak için olmayan bir ASP.NET Core uygulama Windows üzerinde barındırmak için önerilen yöntem bir [Windows hizmeti](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). Bu şekilde otomatik olarak yeniden başlatmalar ve çökme (Crash), sonra oturum birine beklemeden başlatabilirsiniz.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)). Bkz: [sonraki adımlar](#next-steps) bölümü çalıştırmak yönergeler için.

## <a name="prerequisites"></a>Önkoşullar

* Uygulama, .NET Framework çalışma zamanı modülü çalıştırmanız gerekir.  İçinde *.csproj* dosyası, uygun değerlerini belirtin [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) ve [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Örnek buradadır:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Visual Studio'da bir proje oluşturma sırasında kullanmak **ASP.NET Core uygulama (.NET Framework)** şablonu.

* Uygulama (yalnızca bir iç ağ üzerinden) Internet'ten gelen istekleri alırsa, kullanmalısınız [WebListener](xref:fundamentals/servers/weblistener) web sunucusu yerine [Kestrel](xref:fundamentals/servers/kestrel).  Kestrel IIS ile kenar dağıtımlar için kullanılması gerekir.  Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="getting-started"></a>Başlarken

Bu bölümde, mevcut bir ASP.NET Core projeyi bir hizmet olarak çalıştırmak için gerekli minimum değişiklikler açıklanmaktadır.

* NuGet paket yüklemesi [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Aşağıdaki değişiklikleri yapın `Program.Main`:
  
  * Çağrı `host.RunAsService` yerine `host.Run`.
  
  * Kod çağırırsa `UseContentRoot`, yerine yayımlama konumu için bir yol kullanın`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Bir klasöre uygulama yayımlayın.

  Kullanım [dotnet yayımlama](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) veya [Visual Studio yayımlama profili](xref:host-and-deploy/visual-studio-publish-profiles) bir klasöre yayımlar.

* Test oluşturma ve hizmeti başlatılıyor.

  Kullanmak için bir yönetici komut istemi penceresi açın [sc.exe](https://technet.microsoft.com/library/bb490995) oluşturmak ve bir hizmeti başlatmak için komut satırı aracı.  
  
  Hizmet Hizmetim adlı, uygulaması'na yayımlamak `c:\svc`ve uygulama AspNetCoreService adlı, komutları şuna benzer:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  `binPath` Yürütülebilir filename de dahil olmak üzere uygulamanın yürütülebilir dosya yolu bir değerdir.

  ![Konsol penceresi oluşturma ve örnek başlatma](windows-service/_static/create-start.png)

  Bu komutlar bitirdikten sonra aynı yol bir konsol uygulaması çalıştırırken göz atın (varsayılan olarak, `http://localhost:5000`)

  ![Bir hizmet olarak çalışıyor](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Hizmet dışında çalıştırmak için bir yol sağlama

Test ve çağıran kodu eklemek için her zamanki olacak şekilde bir hizmet dışında çalıştırılırken hata ayıklamak daha kolay `host.RunAsService` yalnızca belirli koşullar altında.  Örneğin, uygulama ile bir konsol uygulaması olarak çalıştırabilirsiniz bir `--console` hata ayıklayıcı bağlı değilse veya komut satırı bağımsız değişkeni.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Durdurma ve başlatma olaylarını işleme

İşlenecek `OnStarting`, `OnStarted`, ve `OnStopping` olayları, aşağıdaki ek değişiklikleri yapın:

* Türeyen bir sınıf oluşturun `WebHostService`.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Create genişletme yöntemi için `IWebHost` özel geçen `WebHostService` için `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* İçinde `Program.Main` değişiklik yerine yeni uzantı metodu çağırma `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Varsa özel `WebHostService` kod gerekir (örneğin, bir Günlükçü) bağımlılık ekleme alanından bir hizmet almak için elde `Services` özelliği `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Sonraki adımlar

[Örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) bu eşlik makaledir kod örnekleri önceki gösterildiği gibi değiştirilmiş basit bir MVC web uygulaması.  Bunu bir hizmet olarak çalıştırmak için aşağıdakileri yapın:

* Yayımla *c:\svc*.

* Bir yönetici penceresini açın.

* Aşağıdaki komutları girin:

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * Bir tarayıcıda çalıştığını doğrulamak için http://localhost: 5000 için gidin.

Uygulamayı bir hizmet olarak çalıştırırken istendiği şekilde başlamazsa, hata iletileri erişilebilir hale getirmek için hızlı bir şekilde oturum açma sağlayıcısı gibi eklemektir [Windows olay günlüğü sağlayıcısı](xref:fundamentals/logging/index#eventlog).

## <a name="acknowledgments"></a>İlgili kaynaklar

Bu makalede önceden yayımlanan kaynakları yardımıyla yazıldı. Erken ve bunların en yararlı bunlar olan:

* [ASP.NET Core Windows hizmeti olarak barındırma](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Bir Windows hizmetinde ASP.NET çekirdek barındırmak nasıl](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
