---
title: "ASP.NET çekirdek barındırma"
author: guardrex
description: "Uygulama başlatma ve ömür boyu yönetimi için sorumlu ASP.NET Core web ana bilgisayar hakkında bilgi edinin."
keywords: "ASP.NET Core web ana bilgisayarı, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7deccf135ddd21729206ebed58ddc8aca52c1deb
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="hosting-in-aspnet-core"></a>ASP.NET çekirdek barındırma

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*, uygulama başlatma ve ömür boyu yönetimi için sorumlu olduğu. En az bir sunucu ve bir istek işleme ardışık düzenine konak yapılandırır.

## <a name="setting-up-a-host"></a>Bir konak ayarlıyor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Bu genellikle, uygulamanızın giriş noktası gerçekleştirilen `Main` yöntemi. Proje şablonları içinde `Main` bulunan *Program.cs*. Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlatmak için:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder`Aşağıdaki görevleri gerçekleştirir:

* Yapılandırır [Kestrel](servers/kestrel.md) web sunucusu olarak. Kestrel varsayılan seçenekleri için bkz [Kestrel seçenekleri Kestrel web server ASP.NET Core uygulamasında bölümünü](xref:fundamentals/servers/kestrel#kestrel-options).
* İçerik kök ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Gelen isteğe bağlı yapılandırma yükler:
  * *appSettings.JSON*.
  * *appSettings. {Ortam} .json*.
  * [Kullanıcı parolaları](xref:security/app-secrets) uygulama çalıştırıldığında `Development` ortamı.
  * Ortam değişkenleri.
  * Komut satırı bağımsız değişkenleri.
* Yapılandırır [günlüğü](xref:fundamentals/logging/index) ile konsol ve hata ayıklama çıktısı için [günlüğü filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. {Ortam} .json* dosya.
* IIS çalıştırırken etkinleştirir [IIS tümleştirme](xref:publishing/iis) temel yolunu ve bağlantı noktası yapılandırarak sunucu, kullanırken dinleyecek [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module). Modül IIS ve Kestrel arasında ters proxy oluşturur. Ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors). IIS, varsayılan seçenekleri için bkz: [IIS seçenekleri konak ASP.NET Core IIS ile Windows bölümünü](xref:publishing/iis#iis-options).

*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler. Varsayılan içerik kök [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory). Bu uygulama kök klasörden başlatıldığında web projenin kök klasörü içerik kök olarak kullanarak sonuçları (örneğin, arama [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) proje klasöründen). Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).

Bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index) uygulama yapılandırması hakkında daha fazla bilgi için.

> [!NOTE]
> Statik kullanmaya alternatif olarak `CreateDefaultBuilder` yöntemi, bir ana bilgisayardan oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core ile desteklenen bir yaklaşımdır 2.x. Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Bu genellikle, uygulamanızın giriş noktası gerçekleştirilen `Main` yöntemi. Proje şablonları içinde `Main` bulunan *Program.cs*. Aşağıdaki *Program.cs* nasıl kullanılacağı ortaya `WebHostBuilder` konak oluşturmak için:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder`gerektiren bir [IServer uygulayan sunucu](servers/index.md). Yerleşik sunucularıdır [Kestrel](servers/kestrel.md) ve [HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 sürümünden önce HTTP.sys çağrıldı [WebListener](xref:fundamentals/servers/weblistener)). Bu örnekte, [UseKestrel genişletme yöntemi](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Kestrel sunucuyu belirtir.

*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler. Varsayılan içerik kök tarafından sağlanan `UseContentRoot` olan [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Bu uygulama kök klasörden başlatıldığında web projenin kök klasörü içerik kök olarak kullanarak sonuçları (örneğin, arama [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) proje klasöründen). Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).

IIS ters proxy kullanmak için arama [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) konak oluşturmanın bir parçası olarak. `UseIISIntegration`yapılandırmaz bir *server*gibi [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) yapar. `UseIISIntegration`Taban yol ve bağlantı noktası sunucusunun dinlemesi gereken kullanırken yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Kestrel ve IIS arasında ters proxy oluşturmak için. IIS ile ASP.NET Core kullanmak için her ikisini de belirtmelisiniz `UseKestrel` ve `UseIISIntegration`. `UseIISIntegration`yalnızca IIS veya IIS Express çalıştırırken etkinleştirir. Daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) ve [ASP.NET Core modül yapılandırma başvurusu](xref:hosting/aspnet-core-module).

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

---

Bir konak ayarlıyor durumlarda sağlayabilir [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri. Belirtirseniz bir `Startup` sınıfı gerekir tanımlamanız bir `Configure` yöntemi. Daha fazla bilgi için bkz: [ASP.NET Core uygulama başlangıç](startup.md). Birden çok çağrılar `ConfigureServices` birbirine ekleyin. Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarlarını değiştirin.

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar için hangi ile doğrudan da ayarlanabilir çoğu kullanılabilir yapılandırma değerlerini ayarlamak için yöntemler sağlar [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar. Bir değerle ayarlarken `UseSetting`, değer bir dize (olarak tırnak işaretleri) türünden bağımsız olarak ayarlanır.

### <a name="capture-startup-errors"></a>Başlangıç hatalarını yakalama

Bu ayar, başlangıç hatalarını yakalama denetler.

**Anahtar**: captureStartupErrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: varsayılan olarak `false` Kestrel IIS, varsayılan olduğu arkasında ile uygulamanın çalıştığı sürece `true`.  
**Kullanılarak ayarlanan**:`CaptureStartupErrors`

Zaman `false`, çıkma konak başlangıç sonucunda sırasında hatalar. Zaman `true`, ana bilgisayar başlatma sırasında özel durumları yakalar ve sunucu başlatmaya çalışır.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

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
**Kullanılarak ayarlanan**:`UseContentRoot`

İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root). Yol yoksa, konağı başlatmak başarısız olur.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

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
**Kullanılarak ayarlanan**:`UseSetting`

Etkin olduğunda (veya ne zaman <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumları yakalar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

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
**Kullanılarak ayarlanan**:`UseEnvironment`

Ayarlayabileceğiniz *ortamı* için herhangi bir değer. Framework tanımlı değerler `Development`, `Staging`, ve `Production`. Değerleri büyük küçük harfe duyarlı değildir. Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni. Kullanırken [Visual Studio](https://www.visualstudio.com/), ortam değişkenleri kümesinde *launchSettings.json* dosya. Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

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
**Kullanılarak ayarlanan**:`UseSetting`

Başlangıçta yüklemek için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış dize. Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.

Yapılandırma değeri için boş bir dize Varsayılanları rağmen barındırma başlangıç derlemeler her zaman uygulamanın derleme ekleyin. Barındırma başlangıç derlemeleri sağladığınızda, uygulama başlatma sırasında ortak hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Bu özellik ASP.NET Core kullanılamıyor 1.x.

---

### <a name="prefer-hosting-urls"></a>URL'leri barındırma tercih

Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.

**Anahtar**: preferHostingUrls  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: true  
**Kullanılarak ayarlanan**:`PreferHostingUrls`

Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Bu özellik ASP.NET Core kullanılamıyor 1.x.

---

### <a name="prevent-hosting-startup"></a>Başlangıç barındırma engelle

Başlangıç derlemelere, uygulamanın derleme dahil barındırma otomatik yüklenmesini engeller.

**Anahtar**: preventHostingStartup  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: yanlış  
**Kullanılarak ayarlanan**:`UseSetting`

Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Bu özellik ASP.NET Core kullanılamıyor 1.x.

---

### <a name="server-urls"></a>Sunucu URL'leri

IP adresi veya ana bilgisayar adresleriyle bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri gösterir.

**Anahtar**: URL'leri  
**Tür**: *dize*  
**Varsayılan**: http://localhost: 5000  
**Kullanılarak ayarlanan**:`UseUrls`

Bir noktalı virgülle ayrılmış ayarlayın (;) URL listesi önekleri sunucu yanıt. Örneğin, `http://localhost:123`. Kullan "\*" sunucu IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokolü kullanarak isteklerini dinleme yapması gerektiğini belirtmek için (örneğin, `http://*:5000`). Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir. Desteklenen biçimler sunucular arasında farklılık gösterir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel kendi uç nokta yapılandırması API vardır. Daha fazla bilgi için bkz: [Kestrel web server ASP.NET Core uygulamasında](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

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
**Kullanılarak ayarlanan**:`UseShutdownTimeout`

Anahtar kabul rağmen bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` genişletme yöntemi geçen bir `TimeSpan`. Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Bu özellik ASP.NET Core kullanılamıyor 1.x.

---

### <a name="startup-assembly"></a>Başlangıç derleme

Aranacak derleme belirler `Startup` sınıfı.

**Anahtar**: startupAssembly  
**Tür**: *dize*  
**Varsayılan**: uygulamanın derleme  
**Kullanılarak ayarlanan**:`UseStartup`

Ada göre derleme başvurusu yapabilir (`string`) veya türü (`TStartup`). Birden çok `UseStartup` yöntemleri çağrılmadan, son önceliklidir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

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
**Kullanılarak ayarlanan**:`UseWebRoot`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Yapılandırma geçersiz kılma

Kullanım [yapılandırma](xref:fundamentals/configuration/index) ana bilgisayarı yapılandırmak için. Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hosting.json* dosya. Herhangi bir yapılandırma aşağıdaki konumdan yüklendi *hosting.json* dosyası tarafından komut satırı bağımsız değişkenleri geçersiz. Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

*Hosting.JSON*:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

*Hosting.JSON*:

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
> `UseConfiguration` Genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`. `GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`). `UseConfiguration` Yöntemi bekliyor eşleşecek şekilde anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`). Bölüm adı anahtarlar varlığını ana bilgisayar yapılandırmalarını bölümün değerleri engeller. Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir. Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).

Ana bilgisayar üzerinde belirli bir URL çalıştırmak belirtmek için istediğiniz değeri bir komut isteminden yürütülürken geçirebilirdiniz `dotnet run`. Komut satırı bağımsız değişkeni geçersiz kılmaları `urls` değeri *hosting.json* dosya ve sunucunun dinlediği bağlantı noktası 8080 üzerinde:

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a>Önem derecesi sıralama

Bazı `WebHostBuilder` varsa ayarları ortam değişkenlerinden önce okunur ayarlayın. Bu ortam değişkenleri biçimini kullanın `ASPNETCORE_{configurationKey}`. Varsayılan olarak sunucunun dinlediği URL'leri ayarlamak için ayarladığınız `ASPNETCORE_URLS`.

Bu ortam değişken değerleri yapılandırma belirterek geçersiz (kullanarak `UseConfiguration`) veya değeri açıkça ayarlayarak (kullanarak `UseSetting` veya açık genişletme yöntemleri gibi `UseUrls`). Ana bilgisayar değeri en son hangi seçeneği ayarlar kullanır. Program aracılığıyla varsayılan bir değere ayarlayın ancak yapılandırması ile geçersiz kılınabilir izin vermek istiyorsanız, URL'yi ayarlandıktan sonra komut satırı yapılandırmayı kullanabilirsiniz. Bkz: [geçersiz kılınıyor yapılandırma](#overriding-configuration).

## <a name="starting-the-host"></a>Ana bilgisayar başlatma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

**Çalıştır**

`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatma kadar çağıran iş parçacığı engeller:

```csharp
host.Run();
```

**Start**

Engelleyici olmayan bir biçimde konak çağırarak çalıştırabilirsiniz kendi `Start` yöntemi:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

İçin URL'lerin bir listesini geçirirseniz `Start` yöntemi, belirtilen URL'leri dinler:

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

Başlatma ve önceden yapılandırılmış Varsayılanları birini kullanarak yeni bir ana bilgisayar Başlat `CreateDefaultBuilder` statik kolaylık yöntemini kullanarak. Bu yöntemler konsol çıktısı olmadan ve sunucuyu Başlat [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için bir sonu bekleyin:

**Başlangıç (RequestDelegate uygulama)**

İle başlayan bir `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için `WaitForShutdown`break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller. Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.

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

`WaitForShutdown`break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller. Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.

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

Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için `WaitForShutdown`break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller. Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

**Çalıştır**

`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatma kadar çağıran iş parçacığı engeller:

```csharp
host.Run();
```

**Start**

Engelleyici olmayan bir biçimde konak çağırarak çalıştırabilirsiniz kendi `Start` yöntemi:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

İçin URL'lerin bir listesini geçirirseniz `Start` yöntemi, belirtilen URL'leri dinler:


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

[IHostingEnvironment arabirimi](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar. Kullanabileceğiniz [Oluşturucu ekleme](xref:fundamentals/dependency-injection) almak için `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:

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

Kullanabileceğiniz bir [kurala dayalı yaklaşım](xref:fundamentals/environments#startup-conventions) ortamına bağlı başlangıçta Uygulamanızı yapılandırmak için. Alternatif olarak, Ekle `IHostingEnvironment` içine `Startup` kullanılmak üzere Oluşturucusu `ConfigureServices`:

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
> Ek olarak `IsDevelopment` genişletme yöntemi, `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri. Bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) Ayrıntılar için.

`IHostingEnvironment` Hizmet aynı zamanda eklenen doğrudan `Configure` yöntemi işlem hattınızı ayarlamak için:

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

Ekleme `IHostingEnvironment` içine `Invoke` özel oluştururken yöntemi [Ara](xref:fundamentals/middleware#writing-middleware):

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

[IApplicationLifetime arabirimi](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) etkinlikleri sonrası başlatma ve kapatma işlemleri yapmanıza olanak tanır. Üç arabirimde özelliklerdir ile kaydedebilirsiniz iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlamak için yöntemleri. Ayrıca bir `StopApplication` yöntemi.

| İptal belirteci    | &#8230;tetiklenen; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | Ana bilgisayar tam olarak başlatıldı. |
| `ApplicationStopping` | Konak bir kapama gerçekleştiriyor. İstekleri hala işliyor olabilir. Bu olay tamamlanana kadar kapatma engeller. |
| `ApplicationStopped`  | Konak bir kapama üzeredir. Tüm istekleri tamamen işlenmesi. Bu olay tamamlanana kadar kapatma engeller. |

| Yöntem            | Eylem                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Geçerli uygulamanın sonlandırılması istekleri. |

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

## <a name="troubleshooting-systemargumentexception"></a>Sorun giderme System.ArgumentException

**ASP.NET çekirdeği 2.0 yalnızca uygular**

Konak ekleyerek yapı `IStartup` doğrudan çağırmak yerine bağımlılık ekleme kapsayıcısını `UseStartup` veya `Configure`, aşağıdaki hatalardan biriyle karşılaşabilirsiniz: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.

Bu kaynaklanır [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (geçerli derleme) taramak için gerekli `HostingStartupAttributes`. El ile ekleme, `IStartup` bağımlılık ekleme kapsayıcısına aşağıdaki çağrısı ekleyin, `WebHostBuilder` ile belirtilen derleme adı:

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

**Not**: yok çağırdığınızda bu yalnızca gerekli ASP.NET Core 2.0 sürümüyle ve yalnızca `UseStartup` veya `Configure`.

Daha fazla bilgi için bkz: [duyuruları: Microsoft.Extensions.PlatformAbstractions süredir (Açıklama) kaldırıldı](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) ve [StartupInjection örnek](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS kullanan Windows için yayımlama](../publishing/iis.md)
* [Nginx kullanarak Linux yayımlama](../publishing/linuxproduction.md)
* [Apache kullanarak Linux yayımlama](../publishing/apache-proxy.md)
* [Bir Windows hizmeti ana bilgisayar](xref:hosting/windows-service)
