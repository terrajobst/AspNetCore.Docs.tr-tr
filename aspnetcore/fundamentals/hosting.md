---
title: ASP.NET çekirdek barındırma
author: guardrex
description: Uygulama başlatma ve ömür boyu yönetimi için sorumlu ASP.NET Core web ana bilgisayar hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 9d9b35153e8d771086e3171a224cb62d5d9cc14e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="hosting-in-aspnet-core"></a>ASP.NET çekirdek barındırma

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*. Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur. En az bir sunucu ve bir istek işleme ardışık düzenine konak yapılandırır.

## <a name="setting-up-a-host"></a>Bir konak ayarlıyor

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Bu genellikle uygulamanın giriş noktası gerçekleştirilen `Main` yöntemi. Proje şablonları içinde `Main` bulunan *Program.cs*. Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlatmak için:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder` Aşağıdaki görevleri gerçekleştirir:

* Yapılandırır [Kestrel](servers/kestrel.md) web sunucusu olarak. Kestrel varsayılan seçenekleri için bkz [Kestrel seçenekleri Kestrel web server ASP.NET Core uygulamasında bölümünü](xref:fundamentals/servers/kestrel#kestrel-options).
* İçerik kök tarafından döndürülen yola ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Gelen isteğe bağlı yapılandırma yükler:
  * *appsettings.json*.
  * *appsettings.{Environment}.json*.
  * [Kullanıcı parolaları](xref:security/app-secrets) uygulama çalıştırıldığında `Development` ortamı.
  * Ortam değişkenleri.
  * Komut satırı bağımsız değişkenleri.
* Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktısı için. Günlük kaydı içerir [günlüğü filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.
* IIS çalıştırırken etkinleştirir [IIS tümleştirme](xref:host-and-deploy/iis/index). Taban yol ve sunucunun dinlediği üzerinde kullanırken bağlantı noktasını yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module). Modül IIS ve Kestrel arasında ters bir proxy oluşturur. Ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors). IIS, varsayılan seçenekleri için bkz: [IIS seçenekleri konak ASP.NET Core IIS ile Windows bölümünü](xref:host-and-deploy/iis/index#iis-options).
* Ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise. Daha fazla bilgi için bkz: [kapsam doğrulama](#scope-validation).

*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler. Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır. Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).

Uygulama yapılandırması hakkında daha fazla bilgi için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).

> [!NOTE]
> Statik kullanmaya alternatif olarak `CreateDefaultBuilder` yöntemi, bir ana bilgisayardan oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core ile desteklenen bir yaklaşımdır 2.x. Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Bir ana bilgisayar oluşturma uygulamanın giriş noktası genellikle gerçekleştirilir `Main` yöntemi. Proje şablonları içinde `Main` bulunan *Program.cs*:

[!code-csharp[](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder` gerektiren bir [IServer uygulayan sunucu](servers/index.md). Yerleşik sunucularıdır [Kestrel](servers/kestrel.md) ve [HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 sürümünden önce HTTP.sys çağrıldı [WebListener](xref:fundamentals/servers/weblistener)). Bu örnekte, [UseKestrel genişletme yöntemi](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Kestrel sunucuyu belirtir.

*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler. İçin varsayılan içerik kök elde `UseContentRoot` tarafından [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır. Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).

IIS ters proxy kullanmak için arama [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) konak oluşturmanın bir parçası olarak. `UseIISIntegration` yapılandırmaz bir *server*gibi [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) yapar. `UseIISIntegration` Taban yol ve sunucunun dinlediği üzerinde kullanırken bağlantı noktasını yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Kestrel ve IIS arasında ters Ara sunucu oluşturmak için. IIS ASP.NET Core ile kullanmak için `UseKestrel` ve `UseIISIntegration` belirtilmesi gerekir. `UseIISIntegration` yalnızca IIS veya IIS Express çalıştırırken etkinleştirir. Daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) ve [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).

Bir ana bilgisayar (ve ASP.NET Core uygulama) yapılandırır en az bir uygulama sunucusu ve yapılandırma uygulamanın istek ardışık belirtme içerir:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

* * *
Bir konak ayarlıyor zaman [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri sağlanabilir. Varsa bir `Startup` sınıfı belirtilmediyse, tanımlamanız gerekir bir `Configure` yöntemi. Daha fazla bilgi için bkz: [ASP.NET Core uygulama başlangıç](startup.md). Birden çok çağrılar `ConfigureServices` birbirine ekleyin. Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarlarını değiştirin.

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:

* Ortam değişkenleri biçiminde içeren konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`. Örneğin, `ASPNETCORE_URLS`.
* Açık yöntemleri gibi `CaptureStartupErrors`.
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar. Bir değerle ayarlarken `UseSetting`, değer, dize türünden bağımsız olarak olarak ayarlanır.

Ana bilgisayar son hangi seçeneği bir değer ayarlar kullanır. Daha fazla bilgi için bkz: [geçersiz kılınıyor yapılandırma](#overriding-configuration) sonraki bölümde.

### <a name="capture-startup-errors"></a>Başlangıç hatalarını yakalama

Bu ayar, başlangıç hatalarını yakalama denetler.

**Anahtar**: captureStartupErrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: varsayılan olarak `false` Kestrel IIS, varsayılan olduğu arkasında ile uygulamanın çalıştığı sürece `true`.  
**Kullanılarak ayarlanan**: `CaptureStartupErrors`  
**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`

Zaman `false`, çıkma konak başlangıç sonucunda sırasında hatalar. Zaman `true`, ana bilgisayar başlatma sırasında özel durumları yakalar ve sunucu başlatmaya çalışır.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Kök içerik

Bu ayar, ASP.NET Core MVC görünümler gibi içerik dosyaları aranıyor başladığı belirler. 

**Anahtar**: contentRoot  
**Tür**: *dize*  
**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasöre.  
**Kullanılarak ayarlanan**: `UseContentRoot`  
**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`

İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root). Yol yoksa, konağı başlatmak başarısız olur.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Ayrıntılı hataları

Ayrıntılı hataları yakalanan belirler.

**Anahtar**: detailedErrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: yanlış  
**Kullanılarak ayarlanan**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`

Etkin olduğunda (veya ne zaman <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumları yakalar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Ortam

Uygulamanın ortamını ayarlar.

**Anahtar**: ortamı  
**Tür**: *dize*  
**Varsayılan**: üretim  
**Kullanılarak ayarlanan**: `UseEnvironment`  
**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`

Ortam herhangi bir değere ayarlanabilir. Framework tanımlı değerler `Development`, `Staging`, ve `Production`. Değerleri büyük küçük harfe duyarlı değildir. Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni. Kullanırken [Visual Studio](https://www.visualstudio.com/), ortam değişkenleri kümesinde *launchSettings.json* dosya. Daha fazla bilgi için bkz: [çalışma ile birden çok ortamları](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Barındırma başlangıç derlemeler

Uygulamanın barındırma başlangıç derlemeleri ayarlar.

**Anahtar**: hostingStartupAssemblies  
**Tür**: *dize*  
**Varsayılan**: boş dize  
**Kullanılarak ayarlanan**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Başlangıçta yüklemek için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış dize. Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.

Yapılandırma değeri için boş bir dize Varsayılanları rağmen barındırma başlangıç derlemeler her zaman uygulamanın derleme ekleyin. Barındırma başlangıç derlemeleri sağlandığında, uygulama başlatma sırasında ortak hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Bu özellik ASP.NET Core kullanılamıyor 1.x.

---

### <a name="prefer-hosting-urls"></a>URL'leri barındırma tercih

Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.

**Anahtar**: preferHostingUrls  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: true  
**Kullanılarak ayarlanan**: `PreferHostingUrls`  
**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`

Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Bu özellik ASP.NET Core kullanılamıyor 1.x.

---

### <a name="prevent-hosting-startup"></a>Başlangıç barındırma engelle

Uygulamanın derlemesi tarafından yapılandırılan başlangıç derlemeleri barındırma dahil olmak üzere başlangıç derlemeleri barındırma otomatik yüklenmesini engeller. Bkz: [platforma özgü yapılandırma kullanarak uygulama özellik ekleme](xref:host-and-deploy/platform-specific-configuration) daha fazla bilgi için.

**Anahtar**: preventHostingStartup  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: yanlış  
**Kullanılarak ayarlanan**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`

Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Bu özellik ASP.NET Core kullanılamıyor 1.x.

---

### <a name="server-urls"></a>Sunucu URL'leri

IP adresi veya ana bilgisayar adresleriyle bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri gösterir.

**Anahtar**: URL'leri  
**Tür**: *dize*  
**Varsayılan**: http://localhost:5000  
**Kullanılarak ayarlanan**: `UseUrls`  
**Ortam değişkeni**: `ASPNETCORE_URLS`

Bir noktalı virgülle ayrılmış ayarlayın (;) URL listesi önekleri sunucu yanıt. Örneğin, `http://localhost:123`. Kullan "\*" sunucu IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokolü kullanarak isteklerini dinleme yapması gerektiğini belirtmek için (örneğin, `http://*:5000`). Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir. Desteklenen biçimler sunucular arasında farklılık gösterir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel kendi uç nokta yapılandırması API vardır. Daha fazla bilgi için bkz: [Kestrel web server ASP.NET Core uygulamasında](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Kapatma zaman aşımı

Web ana bilgisayarı kapatmak için beklenecek süreyi belirtir.

**Anahtar**: shutdownTimeoutSeconds  
**Tür**: *int*  
**Varsayılan**: 5  
**Kullanılarak ayarlanan**: `UseShutdownTimeout`  
**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

Anahtar kabul rağmen bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi geçen bir [TimeSpan](/dotnet/api/system.timespan). Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.

Sırasında zaman aşımı süresi, barındırma:

* Tetikleyiciler [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).
* Barındırılan hizmetler, durdurmak için başarısız hizmetler için herhangi bir hata günlüğü durdurmaya çalışır.

Tüm barındırılan Hizmetleri Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında kalan tüm etkin Hizmetleri durduruldu. İşleme tamamlamadınız olsa bile hizmetlerini durdurun. Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımı süresini artırın.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Bu özellik ASP.NET Core kullanılamıyor 1.x.

---

### <a name="startup-assembly"></a>Başlangıç derleme

Aranacak derleme belirler `Startup` sınıfı.

**Anahtar**: startupAssembly  
**Tür**: *dize*  
**Varsayılan**: uygulamanın derleme  
**Kullanılarak ayarlanan**: `UseStartup`  
**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`

Ada göre derleme (`string`) veya türü (`TStartup`) başvurulabilir. Birden çok `UseStartup` yöntemleri çağrılmadan, son önceliklidir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Kök web

Uygulamanın statik varlıklar için göreli yolunu ayarlar.

**Anahtar**: webroot  
**Tür**: *dize*  
**Varsayılan**: belirtilmezse, varsayılan "(Content Root)/wwwroot olur", yol varsa. Yol yoksa, yok dosya sağlayıcısı kullanılır.  
**Kullanılarak ayarlanan**: `UseWebRoot`  
**Ortam değişkeni**: `ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Yapılandırma geçersiz kılma

Kullanım [yapılandırma](xref:fundamentals/configuration/index) ana bilgisayarı yapılandırmak için. Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hosting.json* dosya. Herhangi bir yapılandırma aşağıdaki konumdan yüklendi *hosting.json* dosyası tarafından komut satırı bağımsız değişkenleri geçersiz. Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`. `GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`). `UseConfiguration` Yöntemi bekliyor eşleşecek şekilde anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`). Bölüm adı anahtarlar varlığını ana bilgisayar yapılandırmalarını bölümün değerleri engeller. Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir. Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).

Ana bilgisayar üzerinde belirli bir URL çalıştırmak belirtmek için istenen değeri bir komut isteminden yürütülürken geçirilebilir [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run). Komut satırı bağımsız değişkeni geçersiz kılmaları `urls` değeri *hosting.json* dosya ve sunucunun dinlediği bağlantı noktası 8080 üzerinde:

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a>Ana bilgisayar başlatma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**Çalıştır**

`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatma kadar çağıran iş parçacığı engeller:

```csharp
host.Run();
```

**Start**

Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

Uygulamayı başlatın ve önceden yapılandırılmış Varsayılanları birini kullanarak yeni bir ana bilgisayar Başlat `CreateDefaultBuilder` statik kolaylık yöntemini kullanarak. Bu yöntemler konsol çıktısı olmadan ve sunucuyu Başlat [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için bir sonu bekleyin:

**Başlangıç (RequestDelegate uygulama)**

İle başlayan bir `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için `WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller. Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.

**Başlangıç (dize url, RequestDelegate uygulama)**

Bir URL ile başlatın ve `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Aynı sonucu verir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt `http://localhost:8080`.

**Başlangıç (Eylem<IRouteBuilder> routeBuilder)**

Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Aşağıdaki tarayıcı isteklerini örnekle kullanın:

| İstek                                    | Yanıt                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Merhaba, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | "Ooops!" dizesini içeren bir özel durum oluşturur |
| `http://localhost:5000/throw`              | Aykırı dizesiyle "Uh!!" |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Merhaba Dünya!                             |

`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller. Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.

**Başlangıç (url, eylem dize<IRouteBuilder> routeBuilder)**

Bir örneği ile bir URL kullanın `IRouteBuilder`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Aynı sonucu verir **Başlat (Eylem<IRouteBuilder> routeBuilder)**, uygulama dışında yanıt adresindeki `http://localhost:8080`.

**StartWith (Eylem<IApplicationBuilder> uygulama)**

Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için `WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller. Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.

**StartWith (url, eylem dize<IApplicationBuilder> uygulama)**

Bir URL ve yapılandırmak için bir temsilci sağlayan bir `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Aynı sonucu verir **StartWith (Eylem<IApplicationBuilder> uygulama)**, uygulama dışında yanıt `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Çalıştır**

`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığı engeller:

```csharp
host.Run();
```

**Start**

Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment arabirimi

[IHostingEnvironment arabirimi](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar. Kullanmak [Oluşturucu ekleme](xref:fundamentals/dependency-injection) almak için `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

A [kurala dayalı yaklaşım](xref:fundamentals/environments#startup-conventions) ortamına bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir. Alternatif olarak, Ekle `IHostingEnvironment` içine `Startup` kullanılmak üzere Oluşturucusu `ConfigureServices`:

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> Ek olarak `IsDevelopment` genişletme yöntemi, `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri. Bkz: [çalışma ile birden çok ortamları](xref:fundamentals/environments) Ayrıntılar için.

`IHostingEnvironment` Hizmet aynı zamanda eklenen doğrudan `Configure` yöntemi işleme ardışık ayarlamak için:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

`IHostingEnvironment` içine eklenen `Invoke` özel oluştururken yöntemi [Ara](xref:fundamentals/middleware/index#writing-middleware):

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime arabirimi

[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar. Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.

| İptal belirteci    | Ne zaman tetiklendi&#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | Ana bilgisayar tam olarak başlatıldı. |
| `ApplicationStopping` | Konak bir kapama gerçekleştiriyor. İstekleri hala işliyor olabilir. Bu olay tamamlanana kadar kapatma engeller. |
| `ApplicationStopped`  | Konak bir kapama üzeredir. Tüm isteklerin işlenmesi. Bu olay tamamlanana kadar kapatma engeller. |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) uygulama sonlandırılması ister. Aşağıdaki sınıf kullanır `StopApplication` düzgün biçimde kapatma bir uygulama için zaman sınıfının `Shutdown` yöntemi çağrılır:

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a>Kapsam doğrulama

ASP.NET Core 2.0 veya sonraki sürümlerde, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.

Zaman `ValidateScopes` ayarlanır `true`, varsayılan hizmet sağlayıcısı doğrulamak üzere denetler:

* Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök servis sağlayıcısı'ndan çözülmüş değil.
* Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değil.

Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) olarak adlandırılır. Kök hizmet sağlayıcısının ömrü zaman sağlayıcı uygulamayla başlatır ve uygulamayı kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.

Kapsamlı Hizmetleri oluşturuldukları kapsayıcı tarafından elden. Kapsamlı bir hizmet kök kapsayıcısında oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından atıldı çünkü hizmetin ömrü tekliye etkili bir şekilde yükseltildi. Hizmet kapsamları doğrulama yakalar bu durumlarda, `BuildServiceProvider` olarak adlandırılır.

Her zaman üretim ortamında da dahil olmak üzere kapsamları doğrulamak için yapılandırma [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) ile [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) konak oluşturucu üzerinde:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a>Sorun giderme System.ArgumentException

**ASP.NET çekirdeği 2.0 yalnızca uygular**

Bir konak ekleyerek yerleşik `IStartup` doğrudan çağırmak yerine bağımlılık ekleme kapsayıcısını `UseStartup` veya `Configure`:

```csharp
services.AddSingleton<IStartup, Startup>();
```

Konak bu şekilde oluşturulduysa, aşağıdaki hata oluşabilir:

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

Bu kaynaklanır [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (geçerli derleme) taramak için gerekli `HostingStartupAttributes`. Uygulamayı el ile yerleştirir, `IStartup` bağımlılık ekleme kapsayıcısına aşağıdaki çağrısı ekleyin `WebHostBuilder` ile belirtilen derleme adı:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

Alternatif olarak, bir kukla eklemek `Configure` için `WebHostBuilder`, hangi kümeleri `applicationName`(`ApplicationKey`) otomatik olarak:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**Not**: uygulama çağırdığınızda değil yalnızca gerekli ASP.NET Core 2.0 sürümüyle ve yalnızca budur `UseStartup` veya `Configure`.

Daha fazla bilgi için bkz: [duyuruları: Microsoft.Extensions.PlatformAbstractions süredir (Açıklama) kaldırıldı](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) ve [StartupInjection örnek](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS ile Windows’da barındırma](xref:host-and-deploy/iis/index)
* [Nginx ile Linux’ta barındırma](xref:host-and-deploy/linux-nginx)
* [Apache ile Linux’ta barındırma](xref:host-and-deploy/linux-apache)
* [Bir Windows hizmeti ana bilgisayar](xref:host-and-deploy/windows-service)
