---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758199"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>ASP.NET Core bir Windows hizmetinde barındırma

Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core uygulaması olarak IIS kullanmadan Windows üzerinde barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Bir Windows hizmeti olarak barındırıldığında, uygulama yeniden başlatma sonrasında otomatik olarak başlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Bir Windows hizmetinde bir projeyi dönüştürmeye

Mevcut bir ASP.NET Core projesini ayarlarsınız bir hizmet olarak çalışacak şekilde ayarlamak için aşağıdaki en düşük değişiklikleri gerekir:

1. Proje dosyasında:

   * Bir Windows varlığını onaylamak [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) veya eklemek `<PropertyGroup>` , hedef Framework'ü içerir:

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      İçin birden fazla RID yayımlamak için:

      * RID noktalı virgülle ayrılmış bir liste sağlar.
      * Özellik adını kullanan `<RuntimeIdentifiers>` (çoğul).

      Daha fazla bilgi için [.NET Core RID Kataloğu](/dotnet/core/rid-catalog).

   * İçin bir paket başvurusu ekleme [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Aşağıdaki değişiklikleri yapın `Program.Main`:

   * Çağrı [ana bilgisayar. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) yerine `host.Run`.

   * Çağrı [UseContentRoot](xref:fundamentals/host/web-host#content-root) ve uygulama için bir yol yerine konumu yayımlanan `Directory.GetCurrentDirectory()`.

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. Kullanarak uygulama yayımlamayı [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish), [Visual Studio yayımlama profilini](xref:host-and-deploy/visual-studio-publish-profiles), veya Visual Studio Code. Visual Studio kullanırken **FolderProfile** ve yapılandırma **hedef konum** seçmeden önce **Yayımla** düğmesi.

   Komut satırı arabirimi (CLI) araçlarını kullanarak örnek uygulamayı yayımlamak için çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) proje klasöründeki bir komut isteminde komutu. RID belirtilmelidir `<RuntimeIdenfifier>` (veya `<RuntimeIdentifiers>`) özelliği proje dosyasının. Aşağıdaki örnekte, uygulama için sürüm yapılandırmasında yayımlanır `win7-x64` çalışma zamanı sırasında oluşturulan bir klasöre *c:\\svc*:

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. Hizmet kullanımı için bir kullanıcı hesabı oluşturma `net user` komutu:

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   Örnek uygulama için bir kullanıcı hesabı adı ile oluşturun. `ServiceUser` ve parola. Aşağıdaki komutta `{PASSWORD}` ile bir [güçlü parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   Bir gruba kullanıcı eklemeniz gerekiyorsa, kullanın `net localgroup` komutu, burada `{GROUP}` grubunun adıdır:

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   Daha fazla bilgi için [hizmeti kullanıcı hesaplarını](/windows/desktop/services/service-user-accounts).

1. Uygulamanın klasörüne yazma/okuma/yürütme erişimi vermek kullanarak [icacls](/windows-server/administration/windows-commands/icacls) komutu:

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * `{PATH}` &ndash; Uygulamanın klasörün yolu.
   * `{USER ACCOUNT}` &ndash; Kullanıcı hesabı (SID).
   * `(OI)` &ndash; Nesne devral bayrağı dosyaları alt izinleri yayar.
   * `(CI)` &ndash; Kapsayıcı devral bayrağı, alt klasörler için izinleri yayar.
   * `{PERMISSION FLAGS}` &ndash; Uygulamanın erişim izinlerini ayarlar.
     * Yazma (`W`)
     * Okuma (`R`)
     * Yürütme (`X`)
     * Tam (`F`)
     * Değiştir (`M`)
   * `/t` &ndash; Yinelemeli olarak mevcut alt klasörler ve dosyalar için geçerlidir.

   Örnek uygulamayı yayımlanan *c:\\svc* klasörü ve `ServiceUser` hesap yazma/okuma/Yürütme izinleri, aşağıdaki komutu kullanın:

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls).

1. Kullanım [sc.exe](https://technet.microsoft.com/library/bb490995) hizmeti oluşturmak için komut satırı aracı. `binPath` Değerdir yürütülebilir dosya adını içeren uygulamanın yürütülebilir dosyanın yolu. **Eşittir işareti ve tırnak karakteri her bir parametre ve değer arasında gerekli bir alandır.**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}` &ndash; Hizmete atanacak ad [Hizmet Denetimi Yöneticisi](/windows/desktop/services/service-control-manager).
   * `{PATH}` &ndash; Hizmet yürütülebilir dosya yolu.
   * `{DOMAIN}` (veya makinenin etki alanı yoksa katılmış yerel makine adını) ve `{USER ACCOUNT}` &ndash; etki alanı (veya yerel makine adı) ve hizmetin altında çalıştığı kullanıcı hesabı. Yapmak **değil** atlamak `obj` parametresi. İçin varsayılan değer `obj` olduğu [LocalSystem hesabı](/windows/desktop/services/localsystem-account) hesabı. Altında bir hizmeti çalıştıran `LocalSystem` hesabı önemli bir güvenlik riski sunar. Her zaman bir hizmeti bir kullanıcı hesabı altında sunucu üzerinde sınırlı ayrıcalıklarla çalıştırın.
   * `{PASSWORD}` &ndash; Kullanıcı hesabı parolası.

   Aşağıdaki örnekte:

   * Adlı hizmetin **MyService**.
   * Yayınlanan hizmet bulunan *c:\\svc* klasör. Uygulama yürütülebilir dosyası adlı *AspNetCoreService.exe*. `binPath` Değerine düz tırnak işaretleri (") içine alınır.
   * Altında çalışacağı `ServiceUser` hesabı. Değiştirin `{DOMAIN}` kullanıcı hesabının etki alanı veya yerel makine adı. İçine `obj` düz tırnak (") değeri. Örnek: barındıran sistemde adlı bir yerel makineye ise `MairaPC`ayarlayın `obj` için `"MairaPC\ServiceUser"`.
   * Değiştirin `{PASSWORD}` ile kullanıcı hesabının parolası. `password` Değerine düz tırnak işaretleri (") içine alınır.

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > Parametreleri eşittir işareti ve parametrelerin değerleri arasında boşluk bulunmadığından emin olun.

1. Hizmetle başlar `sc start {SERVICE NAME}` komutu.

   Örnek uygulama hizmeti başlatmak için aşağıdaki komutu kullanın:

   ```console
   sc start MyService
   ```

   Komut hizmeti başlatmak için birkaç saniye sürer.

1. Hizmet durumunu denetlemek için kullanmak `sc query {SERVICE NAME}` komutu. Durumu aşağıdaki değerlerden biri olarak bildirilir:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Örnek uygulama hizmeti durumunu denetlemek için aşağıdaki komutu kullanın:

   ```console
   sc query MyService
   ```

1. Hizmet olduğunda `RUNNING` durum ve hizmeti bir web uygulaması ise, uygulama, bir yola göz atın (varsayılan olarak, `http://localhost:5000`, hangi yönlendiren `https://localhost:5001` kullanırken [HTTPS yeniden yönlendirmesi ara yazılım](xref:security/enforcing-ssl)).

   Örnek app service için uygulamaya Gözat `http://localhost:5000`.

1. Hizmetle Durdur `sc stop {SERVICE NAME}` komutu.

   Aşağıdaki komut örnek uygulama hizmetini durdurur:

   ```console
   sc stop MyService
   ```

1. Hizmeti ile bir hizmeti durdurmak için bir kısa bir gecikmeyle kaldırmanız `sc delete {SERVICE NAME}` komutu.

   Örnek uygulama hizmeti durumunu kontrol edin:

   ```console
   sc query MyService
   ```

   Örnek uygulama hizmeti olduğunda `STOPPED` durum, örnek uygulama hizmeti kaldırmak için aşağıdaki komutu kullanın:

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a>Uygulama dışında bir hizmet çalıştırma

Çağıran kod eklemek için her zamanki şekilde bir hizmet dışında çalışırken hata ayıklama ve test etmek daha kolay `RunAsService` yalnızca belirli koşullar altında. Örneğin, bir konsol uygulaması olarak uygulama çalıştırabilirsiniz bir `--console` komut satırı bağımsız değişkeni veya hata ayıklayıcı eklenir:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

ASP.NET Core yapılandırma komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden `--console` anahtar için bağımsız değişkenler geçirilmeden önce kaldırılır [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` gelen geçirilen değil `Main` içine `CreateWebHostBuilder` çünkü imzası `CreateWebHostBuilder` olmalıdır `CreateWebHostBuilder(string[])` sırayla [tümleştirme testi](xref:test/integration-tests) düzgün çalışması için.

## <a name="handle-stopping-and-starting-events"></a>Durdurma ve başlatma olaylarını işleme

İşlenecek [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), ve [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) olayları, aşağıdaki ek değişiklikleri yapın:

1. Türetilen bir sınıf oluşturmanız [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Bir genişletme yöntemi için oluşturma [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) özel Geçiren `WebHostService` için [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. İçinde `Program.Main`, yeni bir uzantı metodu çağırma `RunAsCustomService`, yerine [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` gelen geçirilen değil `Main` içine `CreateWebHostBuilder` çünkü imzası `CreateWebHostBuilder` olmalıdır `CreateWebHostBuilder(string[])` sırayla [tümleştirme testi](xref:test/integration-tests) düzgün çalışması için.

Varsa özel `WebHostService` kod bağımlılık ekleme (örneğin, bir Günlükçü) hizmetinden gerektirir,'ndan elde [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) özelliği:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir. Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>HTTPS yapılandırma

Hizmet güvenli bir uç nokta ile yapılandırmak için:

1. Barındırma system, platformun sertifika edinme ve dağıtım mekanizmalarını kullanarak bir X.509 sertifikası oluşturun.
1. Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) sertifikayı kullanmak için.

Hizmet uç noktası güvenli hale getirmek için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmiyor.

## <a name="current-directory-and-content-root"></a>Geçerli dizin ve içerik kök

Geçerli çalışma dizini çağırarak döndürülen `Directory.GetCurrentDirectory()` bir Windows hizmeti için *C:\\WINDOWS\\system32* klasör. *System32* klasör değil bir hizmetin dosyaları (örneğin, ayarları) depolamak için uygun bir konum. Korumak ve bir hizmetin varlıklar ve ayar dosyaları ile erişmek için aşağıdaki yaklaşımlardan birini kullanın [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) kullanırken bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):

* İçerik kök yolu kullanın. `IHostingEnvironment.ContentRootPath` Sağlanan aynı yol `binPath` hizmeti oluşturulduğunda bağımsız değişken. Yerine `Directory.GetCurrentDirectory()` ayarları dosyalara olan yolları oluşturmak için içerik kök yolu kullanın ve uygulamanın içerik kökünde dosyaların bakımını yapar.
* Disk üzerinde uygun bir konumda dosya Store. Mutlak bir yol belirtin `SetBasePath` dosyaları içeren klasör.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)
* <xref:fundamentals/host/web-host>
