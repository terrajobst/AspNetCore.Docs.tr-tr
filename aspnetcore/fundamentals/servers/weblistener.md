---
title: ASP.NET Core WebListener web sunucusu uygulaması
author: rick-anderson
description: WebListener, ASP.NET Core IIS olmadan Internet'e doğrudan bağlantı için kullanılan Windows için bir web sunucusu hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: cd2e477824d916afcf1a7901e935dd465a466922
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>ASP.NET Core WebListener web sunucusu uygulaması

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Chris fillerin](https://github.com/Tratcher)

> [!NOTE]
> Bu konu, yalnızca ASP.NET Core geçerlidir 1.x. ASP.NET Core 2. 0'WebListener adlı [HTTP.sys](httpsys.md).

WebListener olan bir [web sunucusu için ASP.NET Core](index.md) yalnızca Windows üzerinde çalışır. Yerleşik olan [Http.Sys çekirdek modu sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener olan alternatif [Kestrel](kestrel.md) kullanılabilecek doğrudan Internet bağlantısı için ters proxy sunucusu olarak IIS dayanmayan. Aslında, **WebListener IIS veya kullanılamaz IIS Express ile uyumlu değil olarak [ASP.NET Core Modülü](aspnet-core-module.md).**

WebListener ASP.NET Core için geliştirilen rağmen bunu doğrudan herhangi bir .NET Core veya .NET Framework uygulama kullanılabilir [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.

WebListener aşağıdaki özellikleri destekler:

- [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)
- Bağlantı noktası paylaşma
- SNI ile HTTPS
- TLS (Windows 10) üzerinden HTTP/2
- Doğrudan dosya aktarımı
- Yanıt önbelleğe alma
- WebSockets (Windows 8)

Desteklenen Windows sürümlerine:

- Windows 7 ve Windows Server 2008 R2 ve sonraki sürümler

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Ne zaman WebListener kullanılır

WebListener IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak gereken burada dağıtımları için yararlıdır.

![Weblistener doğrudan Internet ile iletişim kurar](weblistener/_static/weblistener-to-internet.png)

Http.Sys inşa edildiğinden, WebListener saldırılara karşı koruma için ters proxy sunucusu olmasını gerektirmez. Http.Sys pek saldırılarına karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir. IIS'nin bir HTTP dinleyicisi Http.Sys üstünde çalışır. 

Kestrel kullanarak alınamıyor sunduğu özelliklerden birini gerektiğinde WebListener de iyi bir dahili dağıtımlar için seçimdir.

![Weblistener, iç ağınız ile doğrudan iletişim kurar](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>WebListener kullanma

Burada, konak işletim sistemi ve ASP.NET Core uygulamanız için Kurulum görevleri genel bir bakış verilmiştir.

### <a name="configure-windows-server"></a>Windows Server yapılandırın

* Uygulamanızın gerektirdiği, gibi .NET sürümünü yüklemek [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) veya .NET Framework 4.5.1.

* WebListener için bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister

   URL öneklerini Windows preregister yok, uygulamanızın yönetici ayrıcalıklarıyla çalıştırmanız gerekir. 1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak Localhost'a bağlamak tek istisnası; Bu durumda yönetici ayrıcalıkları gerekli değildir.

   Ayrıntılar için bkz [önekleri preregister ve SSL yapılandırma](#preregister-url-prefixes-and-configure-ssl) bu makalenin ilerisinde yer.

* WebListener ulaşması trafiğine izin veren güvenlik duvarı bağlantı noktalarını açın.

   Netsh.exe kullanabilirsiniz veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).

Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>ASP.NET Core uygulamanızı yapılandırın

* NuGet paket yüklemesi [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Bu da yükler [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) bağımlılık olarak.

* Çağrı `UseWebListener` genişletme yöntemi [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) içinde `Main` yöntemi, hiçbir WebListener belirtme [seçenekleri](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) ve [ayarları](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) gereken , aşağıdaki örnekte gösterildiği gibi:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Dinlemenin yapılacağı URL ve bağlantı noktalarını yapılandırmak 

  Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`. URL öneklerini ve bağlantı noktalarını yapılandırmak için kullanabileceğiniz `UseURLs` genişletme yöntemi, `urls` komut satırı bağımsız değişkeni veya ASP.NET Core yapılandırma sistemi. Daha fazla bilgi için bkz: [barındırma](../../fundamentals/hosting.md).

  Dinleyici kullanan web [Http.Sys önek dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). WebListener için özel önek dizesi biçimi gereksinimi yoktur.

  > [!WARNING]
  > Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılabilir. Üst düzey joker bağlamaları uygulamanızı güvenlik açıkları için yedekleme açabilirsiniz. Bu, güçlü ve zayıf joker karakterler için geçerlidir. Joker karakterler yerine açık ana bilgisayar adları kullanın. Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanı denetlemek, bu güvenlik riskinin yok (tersine `*.com`, açık olduğu). Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.

  > [!NOTE]
  > Aynı önek dizelerde belirttiğinizden emin olun `UseUrls` , sunucu üzerinde preregister. 

* Uygulamanızı IIS veya IIS Express çalışmak üzere yapılandırılmamış olduğundan emin olun.

  Visual Studio'da varsayılan başlatma için IIS Express profilidir.  Projeyi bir konsol uygulaması olarak çalıştırmak için el ile seçilen profil değiştirmek için aşağıdaki ekran görüntüsünde gösterildiği gibi olması.

  ![Konsol uygulama profili seçin](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>ASP.NET Core dışında WebListener kullanma

* Yükleme [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.

* [WebListener için bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister](#preregister-url-prefixes-and-configure-ssl) ASP.NET Core kullanmak için olduğu gibi.

Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).


ASP.NET Core dışında WebListener kullanım gösteren bir kod örneği şöyledir:

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a>URL öneklerini preregister ve SSL yapılandırma

IIS ve WebListener isteklerini dinlemek için temel alınan Http.Sys çekirdek modu sürücüsü kullanır ve işleme başlangıç. IIS, kullanıcı Arabirimi yönetim her şeyi yapılandırmak için görece olarak daha kolay bir yol sağlar. Ancak, WebListener kullanıyorsanız, Http.Sys kendiniz yapılandırmanız gerekir. Netsh.exe olduğundan Bunu yapmak için yerleşik aracı. 

İçin netsh.exe kullanmak için gereken en yaygın görevler URL öneklerini ayırma ve SSL sertifikalarını atama.

NetSh.exe yeni başlayanlar için kullanmak için kolay bir aracı değildir. Aşağıdaki örnek, 80 ve 443 numaralı bağlantı noktaları için URL öneklerini ayırmak için gereken tam minimum gösterir:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Aşağıdaki örnek, bir SSL sertifikası atama gösterilmiştir:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

Resmi başvuru belgeleri aşağıda verilmiştir:

* [Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix dizeleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Aşağıdaki kaynaklar, çeşitli senaryoları için ayrıntılı yönergeler sağlar. Başvurmak makaleleri `HttpListener` için eşit oranda geçerli `WebListener`, her ikisi de Http.Sys üzerinde tabanlı olarak.

* [Nasıl Yapılır: SSL Sertifikası ile Bir Bağlantı Noktasını Yapılandırma](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS iletişimi - HttpListener tabanlı barındırma ve istemci sertifikası](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) bu üçüncü taraf blog ve oldukça eski ancak hala yararlı bilgiler.
* [Nasıl yapılır: Kod (C++) bir SSL basit sunucu olarak izlenecek kullanarak HttpListener veya Http sunucusu yönetilmeyen](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) çok yararlı bilgiler ile daha eski bir blog budur.
* [.NET Core WebListener SSL ile nasıl ayarlayabilirim?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Burada, kullanılacak netsh.exe komut satırı kolay olabilir bazı üçüncü taraf araçları bulunmaktadır. Bunlar değil tarafından sağlanan veya Microsoft tarafından onaylanır. Netsh.exe kendisine yönetici ayrıcalıkları gerektirdiğinden araçları varsayılan olarak, yönetici olarak çalıştırın.

* [HTTP.sys Yöneticisi](http://httpsysmanager.codeplex.com/) listesi için kullanıcı Arabirimi sağlar ve SSL sertifikaları ve seçenekleri yapılandırmak, önek ayırmaları ve sertifika güven listelerini. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) listesinde ya da SSL sertifikaları ve URL öneklerini yapılandırma sağlar. Kullanıcı arabirimini Yöneticisi http.sys daha gelişmiş ve daha fazla birkaç yapılandırma seçeneği sunar, ancak Aksi halde benzer bir işlevsellik sağlar. Yeni bir sertifika güven listesi (CTL) oluşturamaz, ancak var olanları atayabilirsiniz.

Otomatik imzalı SSL sertifikalarını oluşturmak için Microsoft komut satırı araçları sağlar: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) ve PowerShell cmdlet [yeni SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Otomatik olarak imzalanan SSL sertifikalarını oluşturmak kolaylaştırmak üçüncü taraf UI araçları vardır:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [MakeCert kullanıcı Arabirimi](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Bu makale için örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener kaynak kodu](https://github.com/aspnet/HttpSysServer/)
* [Barındırma](../hosting.md)
