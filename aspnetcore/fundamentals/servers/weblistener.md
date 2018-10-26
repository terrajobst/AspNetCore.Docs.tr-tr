---
title: ASP.NET core'da WebListener web sunucusu uygulaması
author: rick-anderson
description: WebListener, bir web sunucusu IIS olmadan İnternet'e doğrudan bağlantı için kullanılan Windows üzerinde ASP.NET Core hakkında bilgi edinin.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: e359d8d3ff443009128d7c76bf13f3c9a0a54730
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090582"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>ASP.NET core'da WebListener web sunucusu uygulaması

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Bu konu, yalnızca ASP.NET Core için geçerlidir. 1.x. ASP.NET Core 2.0 sürümünde WebListener adlı [HTTP.sys](httpsys.md).

WebListener olduğu bir [ASP.NET Core web sunucusu](index.md) yalnızca Windows üzerinde çalışır. Yerleşik [Http.Sys çekirdek modu sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener olan alternatif [Kestrel](kestrel.md) kullanılabilecek doğrudan Internet bağlantısı için ters proxy sunucusu olarak IIS üzerinde bağlı olmadan. Aslında, **WebListener kullanılamaz IIS veya IIS Express ile uyumlu değil olarak [ASP.NET Core Modülü](aspnet-core-module.md).**

WebListener ASP.NET Core için geliştirilmiş olsa da, bunu doğrudan herhangi bir .NET Core veya .NET Framework uygulama kullanılabilir [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.

WebListener aşağıdaki özellikleri destekler:

- [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)
- Bağlantı noktası paylaşma
- SNI ile HTTPS
- HTTP/2 üzerinden TLS (Windows 10)
- Doğrudan bir dosya aktarımı
- Yanıtları önbelleğe alma
- WebSockets (Windows 8)

Desteklenen Windows sürümleri:

- Windows 7 ve Windows Server 2008 R2 ve üzeri

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>WebListener kullanıldığı durumlar

WebListener, IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak gereken dağıtımları için kullanışlıdır.

![Weblistener doğrudan Internet ile iletişim kurar.](weblistener/_static/weblistener-to-internet.png)

Http.Sys üzerinde oluşturulduğundan, WebListener saldırılara karşı koruma için ters Ara sunucu gerektirmez. Http.Sys, pek çok saldırılara karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir. IIS'nin, Http.Sys üzerine bir HTTP dinleyicisi olarak çalışır.

Kestrel'i kullanarak alınamıyor sunduğu özelliklerden biri, gereksinim duyduğunuz WebListener de iç dağıtımları için iyi bir seçimdir andır.

![Weblistener doğrudan iç ağınıza ile iletişim kurar.](weblistener/_static/weblistener-to-internal.png)

## <a name="kernel-mode-authentication-with-kerberos"></a>Çekirdek modu kimlik doğrulamasını Kerberos ile

Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü WebListener temsil eder. Kullanıcı modu kimlik doğrulaması, Kerberos ve WebListener ile desteklenmez. Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir. Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.

## <a name="how-to-use-weblistener"></a>WebListener kullanma

Kurulum görevleri için ana işletim sistemi ve ASP.NET Core uygulamanızı genel bir bakış aşağıdadır.

### <a name="configure-windows-server"></a>Windows Server'ı yapılandırma

* Uygulamanızın gerektirdiği, gibi .NET sürümünü [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) veya .NET Framework 4.5.1.

* WebListener için bağlama ve SSL sertifikaları ayarlama URL ön ekleri preregister

   Windows URL ön ekleri preregister yoksa, uygulamanızın yönetici ayrıcalıklarıyla çalıştırmanız gerekir. 1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil)'ı kullanarak Localhost'a bağlama tek istisnası; Bu durumda yönetici ayrıcalıkları gerekli değildir.

   Ayrıntılar için bkz [önekleri preregister ve SSL yapılandırma](#preregister-url-prefixes-and-configure-ssl) bu makalenin ilerleyen bölümlerinde.

* Trafiğin WebListener ulaşmasına izin vermek için güvenlik duvarı bağlantı noktalarını açın.

   Netsh.exe kullanabilirsiniz veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).

Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>ASP.NET Core uygulamanızı yapılandırma

* NuGet paketini yüklemek [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Bu da yükler [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) bağımlılık olarak.

* Çağrı `UseWebListener` genişletme yöntemini [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) içinde `Main` yöntemi, tüm WebListener belirtme [seçenekleri](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) ve [ayarları](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) gereken , aşağıdaki örnekte gösterildiği gibi:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* URL ve bağlantı noktası dinleyecek şekilde yapılandırma 

  Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`. URL ön ekleri ve bağlantı noktalarını yapılandırmak için kullanabileceğiniz `UseURLs` genişletme yöntemi `urls` komut satırı bağımsız değişkeni veya ASP.NET Core yapılandırma sistemi. Daha fazla bilgi için bkz. [ASP.NET Core(xref:fundamentals/host/index) ana bilgisayar.

  Dinleyici kullanan web [Http.Sys önek dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). WebListener için özel ön eki dizesi biçimi gereksinimi yoktur.

  > [!WARNING]
  > Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır. Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz. Bu, güçlü ve zayıf joker karakterler için geçerlidir. Joker karakterler yerine açık bir ana bilgisayar adları kullanın. Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan). Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.

  > [!NOTE]
  > Aynı ön eki dizelerde belirttiğinizden emin olun `UseUrls` , sunucuda preregister. 

* Uygulamanızı IIS veya IIS Express çalıştırmak için yapılandırılmamış olduğundan emin olun.

  Visual Studio'da varsayılan başlatma için IIS Express profilidir.  Projeyi konsol uygulaması olarak çalıştırmak için el ile seçilen profil değiştirmek aşağıdaki ekran görüntüsünde gösterildiği gibi olması.

  ![Konsol uygulama profili seçin](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>ASP.NET Core dışında WebListener kullanma

* Yükleme [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.

* [WebListener için bağlama ve SSL sertifikaları ayarlama URL ön ekleri preregister](#preregister-url-prefixes-and-configure-ssl) kullanılmak üzere ASP.NET Core gibi.

Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).


ASP.NET Core dışında WebListener kullanım gösteren kod örneği aşağıda verilmiştir:

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>URL ön ekleri preregister ve SSL'yi yapılandırma

Hem IIS hem de WebListener isteklerini dinlemek için temel alınan Http.Sys çekirdek modu sürücüsü özelliğine dayanır ve işleme ilk. IIS, yönetimi kullanıcı Arabirimi, her şeyi yapılandırmak için daha kolay bir yol sunar. Ancak, WebListener kullanıyorsanız, Http.Sys kendiniz yapılandırmak gerekir. Netsh.exe, bunu yapmak için yerleşik aracı. 

En yaygın görevleri için netsh.exe kullanmanız gereken URL ön ekleri ayırma ve SSL sertifikaları atama.

NetSh.exe, yeni başlayanlar için kolay bir aracı değildir. Aşağıdaki örnek, 80 ve 443 bağlantı noktaları için URL ön ekleri ayırmada gereken en az gösterir:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Aşağıdaki örnek, bir SSL sertifikası atamak gösterilmektedir:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

Resmi başvuru belgeleri aşağıda verilmiştir:

* [Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix dizeleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Aşağıdaki kaynaklar, çeşitli senaryolar için ayrıntılı yönergeler sağlar. Başvuran makaleler `HttpListener` için eşit oranda geçerli `WebListener`gibi her ikisi de Http.Sys üzerinde temel alır.

* [Nasıl Yapılır: SSL Sertifikası ile Bir Bağlantı Noktasını Yapılandırma](/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS iletişimi - HttpListener tabanlı barındırma ve istemci sertifikası](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) bu üçüncü taraf Web günlüğü ve oldukça eskidir ancak yine de yararlı bilgiler vardır.
* [Nasıl yapılır: Kılavuz kullanarak HttpListener veya Http sunucusu SSL basit sunucu olarak kod (C++) yönetilmeyen](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) çok yararlı bilgiler içeren eski bir blog budur.
* [SSL ile bir .NET Core WebListener nasıl ayarlayabilirim?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Kullanılacak netsh.exe komut satırı kolay olabilir bazı üçüncü taraf araçları şunlardır. Bunlar değil tarafından sağlanan veya Microsoft tarafından desteklendiğini düşündürecek. Netsh.exe kendisine yönetici ayrıcalıkları gerektirdiğinden araçları varsayılan olarak, yönetici olarak çalıştırın.

* [HTTP.sys Manager](http://httpsysmanager.codeplex.com/) listesi için kullanıcı Arabirimi sağlar ve SSL sertifikaları ve seçenekleri yapılandırmak, ayırmaları ön ek ve sertifika güven listeleri. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) listesinde ya da SSL sertifikaları ve URL ön ekleri yapılandırma sağlar. Kullanıcı arabirimini Manager http.sys daha daraltılmış ve birkaç daha fazla yapılandırma seçeneği sunar, ancak Aksi takdirde, benzer bir işlevsellik sağlar. Yeni bir sertifika güven listesi (CTL) oluşturulamıyor, ancak var olanları atayabilirsiniz.

Otomatik olarak imzalanan SSL sertifikaları oluşturmak için Microsoft komut satırı araçları sağlar: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) ve PowerShell cmdlet'ini [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Ayrıca üçüncü taraf kullanıcı Arabirimi otomatik olarak imzalanan SSL sertifikalarını oluşturmak kolaylaştıran bir araç da vardır:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [MakeCert kullanıcı Arabirimi](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Bu makalede örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener kaynak kodu](https://github.com/aspnet/HttpSysServer/)
* [Barındırma](xref:fundamentals/host/index)
