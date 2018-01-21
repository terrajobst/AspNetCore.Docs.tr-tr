---
title: "HTTP.sys web server ASP.NET Core uygulamasında"
author: rick-anderson
description: "Bir web sunucusu ASP.NET Core Windows için HTTP.sys tanıtır. Http.Sys çekirdek modu sürücüsü üzerinde oluşturulmuş, HTTP.sys bir IIS olmadan Internet'e doğrudan bağlantı için kullanılan Kestrel alternatifidir."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 60301e1e3eb96f51e86ef9f8be61f5fd8a4c009c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>HTTP.sys web server ASP.NET Core uygulamasında

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Chris fillerin](https://github.com/Tratcher)

> [!NOTE]
> Bu konuda, yalnızca ASP.NET Core 2.0 ve daha sonra uygulanır. ASP.NET Core önceki sürümlerde HTTP.sys adlı [WebListener](xref:fundamentals/servers/weblistener).

HTTP.sys olan bir [web sunucusu için ASP.NET Core](index.md) yalnızca Windows üzerinde çalışır. Yerleşik olan [Http.Sys çekirdek modu sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys olan alternatif [Kestrel](kestrel.md) Kestel olmayan bazı özellikler sunar. **HTTP.sys IIS veya kullanılamaz IIS Express ile uyumlu olarak [ASP.NET Core Modülü](aspnet-core-module.md).**

HTTP.sys aşağıdaki özellikleri destekler:

- [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)
- Bağlantı noktası paylaşma
- SNI ile HTTPS
- TLS (Windows 10) üzerinden HTTP/2
- Doğrudan dosya aktarımı
- Yanıt önbelleğe alma
- WebSockets (Windows 8)

Desteklenen Windows sürümlerine:

- Windows 7 ve Windows Server 2008 R2 ve sonraki sürümler

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>HTTP.sys kullanma zamanı

HTTP.sys IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak gereken burada dağıtımları için yararlıdır.

![HTTP.sys doğrudan Internet ile iletişim kurar](httpsys/_static/httpsys-to-internet.png)

Http.Sys inşa edildiğinden, HTTP.sys saldırılara karşı koruma için ters proxy sunucusu olmasını gerektirmez. Http.Sys pek saldırılarına karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir. IIS'nin bir HTTP dinleyicisi Http.Sys üstünde çalışır. 

Windows kimlik doğrulaması gibi Kestrel içinde değil kullanılabilecek bir özellik gerektiğinde HTTP.sys Dahili dağıtımlar için iyi bir seçimdir.

![HTTP.sys, iç ağınız ile doğrudan iletişim kurar](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>HTTP.sys kullanma

Burada, konak işletim sistemi ve ASP.NET Core uygulamanız için Kurulum görevleri genel bir bakış verilmiştir.

### <a name="configure-windows-server"></a>Windows Server yapılandırın

* Uygulamanızın gerektirdiği, gibi .NET sürümünü yüklemek [.NET Core](https://www.microsoft.com/net/download/core) veya [.NET Framework](https://www.microsoft.com/net/download/framework).

* HTTP.sys dosyasına bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister

   URL öneklerini Windows preregister yok, uygulamanızın yönetici ayrıcalıklarıyla çalıştırmanız gerekir. 1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak Localhost'a bağlamak tek istisnası; Bu durumda, yönetici ayrıcalıkları gerekli değildir.

   Ayrıntılar için bkz [önekleri preregister ve SSL yapılandırma](#preregister-url-prefixes-and-configure-ssl) bu makalenin ilerisinde yer.

* HTTP.sys ulaşması trafiğine izin veren güvenlik duvarı bağlantı noktalarını açın.

   Kullanabileceğiniz *netsh.exe* veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).

Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>ASP.NET Core uygulamanızı HTTP.sys kullanacak şekilde yapılandırma

* Paket yükleme kullanırsanız, gerekli [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) paketinde metapackage.

* Çağrı `UseHttpSys` genişletme yöntemi `WebHostBuilder` içinde `Main` herhangi belirtme yöntemi [HTTP.sys seçenekleri](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) aşağıdaki örnekte gösterildiği gibi gereken:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>HTTP.sys seçeneklerini yapılandırma

HTTP.sys ayarları ve yapılandırabileceğiniz sınırlarını bazıları aşağıda verilmiştir.

**En fazla istemci bağlantıları**

Aşağıdaki kod ile tüm uygulama için en fazla eş zamanlı açık TCP bağlantı sayısını ayarlanabilir *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

En fazla bağlantı sayısı, varsayılan olarak sınırsız (null) olur.

**En büyük istek gövdesi boyutu**

Varsayılan en büyük istek gövdesi boyutu 30.000.000, hangi 28.6 MB'dir bayttır.

Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yöntem kullanmaktır [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) bir eylem yönteminin özniteliği:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Aşağıda, tüm uygulama, her istek için kısıtlamayı yapılandırmak nasıl oluşturulduğunu gösteren bir örnek verilmiştir:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Belirli bir istekte ayarı geçersiz kılabilirsiniz *haline*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
İstek okunurken uygulama başlatıldıktan sonra bir istekte sınırı yapılandırmayı denerseniz özel durum oluşur. Var. bir `IsReadOnly` belirten özelliği, `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.

Diğer HTTP.sys seçenekleri hakkında daha fazla bilgi için bkz: [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Dinlemenin yapılacağı URL ve bağlantı noktalarını yapılandırmak 

Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`. URL öneklerini ve bağlantı noktalarını yapılandırmak için kullanabileceğiniz `UseUrls` genişletme yöntemi, `urls` komut satırı bağımsız değişkeni, ASPNETCORE_URLS ortam değişkeni veya `UrlPrefixes` özelliği [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). Aşağıdaki kod örneğinde `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Bir avantajı `UrlPrefixes` yanlış biçimlendirilmiş bir önek eklemeyi denediğinizde hata iletisi hemen alırsınız. Bir avantajı `UseUrls` (ile paylaşılan `urls` ve ASPNETCORE_URLS), daha kolay Kestrel ve HTTP.sys arasında geçiş yapabilirsiniz emin olan.

Her ikisi de kullanırsanız, `UseUrls` (veya `urls` veya ASPNETCORE_URLS) ve `UrlPrefixes`, ayarlarında `UrlPrefixes` olanları geçersiz `UseUrls`. Daha fazla bilgi için bkz: [barındırma](xref:fundamentals/hosting).

HTTP.sys kullanan [HTTP Sunucusu API UrlPrefix dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Aynı önek dizelerde belirttiğinizden emin olun `UseUrls` veya `UrlPrefixes` , sunucu üzerinde preregister. 

### <a name="dont-use-iis"></a>IIS kullanmayın

Uygulamanızı IIS veya IIS Express çalışmak üzere yapılandırılmamış olduğundan emin olun.

Visual Studio'da varsayılan başlatma için IIS Express profilidir. Projeyi bir konsol uygulaması olarak çalıştırmak için el ile seçilen profil aşağıdaki ekran görüntüsünde gösterildiği gibi değiştirin.

![Konsol uygulama profili seçin](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>URL öneklerini preregister ve SSL yapılandırma

IIS ve HTTP.sys isteklerini dinlemek için temel alınan Http.Sys çekirdek modu sürücüsü kullanır ve işleme başlangıç. IIS, kullanıcı Arabirimi yönetim her şeyi yapılandırmak için görece olarak daha kolay bir yol sağlar. Ancak, Http.Sys kendiniz yapılandırmanız gerekir. Diğer bir deyişle yapmak için yerleşik aracı *netsh.exe*. 

İle *netsh.exe* yedek URL öneklerini ve SSL sertifikalarını atayın. Aracı yönetim ayrıcalıkları gerektirir.

Aşağıdaki örnek, 80 ve 443 numaralı bağlantı noktaları için URL öneklerini ayırmak için gereken en düşük gösterir:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Aşağıdaki örnek, bir SSL sertifikası atama gösterilmiştir:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Başvuru belgelerini işte *netsh.exe*:

* [Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix dizeleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Aşağıdaki kaynaklar, çeşitli senaryoları için ayrıntılı yönergeler sağlar. Her ikisi de Http.Sys üzerinde tabanlı olarak HttpListener için başvuran makaleleri HTTP.sys için eşit olarak uygulanır.

* [Nasıl Yapılır: SSL Sertifikası ile Bir Bağlantı Noktasını Yapılandırma](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS iletişimi - HttpListener tabanlı barındırma ve istemci sertifikası](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) bu üçüncü taraf blog ve oldukça eski ancak hala yararlı bilgiler.
* [Nasıl yapılır: Kod (C++) bir SSL basit sunucu olarak izlenecek kullanarak HttpListener veya Http sunucusu yönetilmeyen](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) çok yararlı bilgiler ile daha eski bir blog budur.

Daha kolay olabilir bazı üçüncü taraf araçları işte *netsh.exe* komut satırı. Bunlar değil tarafından sağlanan veya Microsoft tarafından onaylanır. Varsayılan olarak, yönetici olarak araçları beri çalıştırmak *netsh.exe* kendisi için yönetici ayrıcalıkları gerekiyor.

* [HTTP.sys Yöneticisi](http://httpsysmanager.codeplex.com/) listesi için kullanıcı Arabirimi sağlar ve SSL sertifikaları ve seçenekleri yapılandırmak, önek ayırmaları ve sertifika güven listelerini. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) listesinde ya da SSL sertifikaları ve URL öneklerini yapılandırma sağlar. Kullanıcı arabirimini Yöneticisi http.sys daha gelişmiş ve daha fazla birkaç yapılandırma seçeneği sunar, ancak Aksi halde benzer bir işlevsellik sağlar. Yeni bir sertifika güven listesi (CTL) oluşturamaz, ancak var olanları atayabilirsiniz.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Bu makale için örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [HTTP.sys kaynak kodu](https://github.com/aspnet/HttpSysServer/)
* [Barındırma](xref:fundamentals/hosting)
