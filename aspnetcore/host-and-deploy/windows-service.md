---
title: Bir Windows hizmetinde konak ASP.NET Çekirdeği
author: guardrex
description: Bir Windows hizmetinde bir ASP.NET Core uygulama ana bilgisayar öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 718cc83bb29c0cff323853d22c107e00616b1dd1
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126241"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Bir Windows hizmetinde konak ASP.NET Çekirdeği

Tarafından [Luke Latham](https://github.com/guardrex) ve [zel Dykstra](https://github.com/tdykstra)

Bir ASP.NET Core uygulama IIS olarak kullanmadan Windows üzerinde barındırılabilir bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Bir Windows hizmeti olarak barındırıldığında, uygulama otomatik olarak şunları yapabilecek başladıktan sonra bilgisayarı yeniden başlatır ve insan etkileşimi gerektirmeden çöküyor.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started"></a>Kullanmaya başlayın

Bir hizmet olarak çalıştırmak üzere mevcut bir ASP.NET Core projeyi ayarlamak için aşağıdaki en düşük değişiklikleri gerekir:

1. Proje dosyasında:

   1. Çalışma zamanı tanımlayıcı varlığını onaylamak veya ekleyin  **\<PropertyGroup >** hedef Framework'ü içerir:
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. Bir paket başvurusunu ekleyin [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

1. Aşağıdaki değişiklikleri yapın `Program.Main`:

   * Çağrı [ana bilgisayar. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) yerine `host.Run`.

   * Kod çağırırsa `UseContentRoot`, uygulama için bir yol yerine konumu yayımlanan kullanım `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Uygulamayı yayımlayın. Kullanım [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish) veya [Visual Studio yayımlama profili](xref:host-and-deploy/visual-studio-publish-profiles).

   Komut satırından örnek uygulamayı yayımlamak için proje klasöründen konsol penceresinde aşağıdaki komutu çalıştırın:

   ```console
   dotnet publish --configuration Release
   ```

1. Kullanım [sc.exe](https://technet.microsoft.com/library/bb490995) hizmeti oluşturmak için komut satırı aracı. `binPath` Yürütülebilir dosya adı içeren uygulamanın yürütülebilir dosya yolu bir değerdir. **Eşittir işareti ve yolun başlangıcında tırnak karakteri arasındaki boşluğu gereklidir.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   Proje klasöründe yayımlanan bir hizmet için yolunu kullanın *yayımlama* hizmeti oluşturmak için klasör. Aşağıdaki örnekte hizmetidir:

   * Adlı **MyService**.
   * Yayımlanan *c:\\my_services\\AspNetCoreService\\bin\\sürüm\\&lt;TARGET_FRAMEWORK&gt;\\Yayımlama* klasör.
   * Adlı bir uygulama tarafından yürütülebilir temsil *AspNetCoreService.exe*.

   Yönetici ayrıcalıklarıyla bir komut kabuğu'nu açın ve aşağıdaki komutu çalıştırın:

   ```console
   sc create MyService binPath= "c:\my_services\aspnetcoreservice\bin\release\<TARGET_FRAMEWORK>\publish\aspnetcoreservice.exe"
   ```
   
   > [!IMPORTANT]
   > Alanı arasında mevcut olduğundan emin olun `binPath=` bağımsız değişkeni ve değeri.
   
   Yayımlama ve farklı bir klasör hizmetini başlatmak için:
   
   1. Kullanım [--çıkış &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) seçeneği `dotnet publish` komutu.
   1. İle hizmet oluşturma `sc.exe` çıkış klasörü yolu kullanarak komutu. Sağlanan yol hizmetin yürütülebilir dosyanın adını dahil `binPath`.

1. Hizmetle başlar `sc start <SERVICE_NAME>` komutu.

   Örnek uygulama hizmetini başlatmak için aşağıdaki komutu kullanın:

   ```console
   sc start MyService
   ```

   Komut hizmetini başlatmak için birkaç saniye sürer.

1. `sc query <SERVICE_NAME>` Komutu, durumu belirlemek için hizmet durumunu denetlemek için kullanılabilir:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Örnek uygulama hizmeti durumunu denetlemek için aşağıdaki komutu kullanın:

   ```console
   sc query MyService
   ```

1. Hizmet olduğunda `RUNNING` durum ve hizmeti bir web uygulaması ise, uygulama kendi yolundaki göz atın (varsayılan olarak, `http://localhost:5000`, hangi yönlendirir `https://localhost:5001` kullanırken [HTTPS yeniden yönlendirmesi Ara](xref:security/enforcing-ssl)).

   Örnek uygulama hizmeti için uygulamaya Gözat `http://localhost:5000`.

1. Hizmet ile Durdur `sc stop <SERVICE_NAME>` komutu.

   Aşağıdaki komut örnek uygulama hizmetini durdurur:

   ```console
   sc stop MyService
   ```

1. Hizmet ile bir hizmeti durdurmak için kısa bir gecikmeyle, kaldırma `sc delete <SERVICE_NAME>` komutu.

   Örnek uygulama hizmetinin durumunu kontrol edin:

   ```console
   sc query MyService
   ```

   Örnek uygulama hizmeti olduğunda `STOPPED` durum, örnek uygulama hizmetini kaldırmak için aşağıdaki komutu kullanın:

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Hizmet dışında çalıştırmak için bir yol sağlama

Test ve çağıran kodu eklemek için her zamanki olacak şekilde bir hizmet dışında çalıştırılırken hata ayıklamak daha kolay `RunAsService` yalnızca belirli koşullar altında. Örneğin, uygulama ile bir konsol uygulaması olarak çalıştırabilirsiniz bir `--console` komut satırı bağımsız değişkeni ya da hata ayıklayıcısı ekli varsa:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

ASP.NET Core yapılandırma komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden `--console` anahtar bağımsız değişkenler için geçirilmeden önce kaldırılır [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Durdurma ve başlatma olaylarını işleme

İşlenecek [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), ve [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) olayları, aşağıdaki ek değişiklikleri yapın:

1. Türeyen bir sınıf oluşturun [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Create genişletme yöntemi için [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) özel geçen `WebHostService` için [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. İçinde `Program.Main`, yeni uzantı metodu çağırma `RunAsCustomService`, yerine [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Varsa özel `WebHostService` kod bağımlılık ekleme (örneğin, bir Günlükçü) hizmetinden gerektirir, buradan edinin [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) özelliği:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy sunucusu ve yük dengeleyici senaryoları

Internet'ten veya bir şirket ağı isteklerle etkileşim ve bir proxy'nin arkasında veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir. Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="kestrel-endpoint-configuration"></a>Kestrel uç nokta yapılandırması

HTTPS yapılandırma ve SNI destek dahil olmak üzere Kestrel uç noktasını yapılandırma hakkında bilgi için bkz: [Kestrel uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).
