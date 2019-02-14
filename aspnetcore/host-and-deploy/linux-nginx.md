---
title: ASP.NET Core Nginx ile Linux'ta barındırma
author: rick-anderson
description: Ngınx Kestrel üzerinde çalışan ASP.NET Core web uygulaması HTTP trafiği iletmek için Ubuntu 16.04 ters bir proxy olarak ayarlamayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: a04927ca0377b965f3b4574e55fb9ed450959a7f
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248322"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>ASP.NET Core Nginx ile Linux'ta barındırma

Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Bu kılavuzda bir Ubuntu 16.04 sunucusunda bir üretime hazır ASP.NET Core ortamını ayarlama açıklanmaktadır. Büyük olasılıkla bu yönergeleri Ubuntu daha yeni sürümleri ile çalışır, ancak yeni sürümleriyle yönergeleri test edilmemiştir.

ASP.NET Core tarafından desteklenen diğer Linux dağıtımları hakkında daha fazla bilgi için bkz: [Linux üzerinde .NET Core önkoşulları](/dotnet/core/linux-prerequisites).

> [!NOTE]
> Ubuntu 14.04 için *supervisord* Kestrel işlem izleme için bir çözüm olarak önerilir. *systemd* Ubuntu 14.04 üzerinde kullanılamaz. Ubuntu 14.04 yönergeler için bkz: [bu konunun önceki sürümü](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

Bu Kılavuzu:

* Mevcut bir ASP.NET Core uygulaması bir ters proxy sunucusunun arkasında yerleştirir.
* Ters proxy sunucuyu Kestrel web sunucusu istekleri iletmek için ayarlar.
* Web uygulaması başlangıç bir daemon olarak gerçekleştirilmesini sağlar.
* Web uygulaması yeniden yardımcı olmak için bir işlem yönetimini yapılandırır.

## <a name="prerequisites"></a>Önkoşullar

1. Bir Ubuntu 16.04 sunucusuyla sudo ayrıcalıklarıyla standart kullanıcı hesabı erişimi.
1. .NET Core çalışma zamanı sunucuya yükleyin.
   1. Ziyaret [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).
   1. En son Önizleme çalışma zamanı altında listeden seçin **çalışma zamanı**.
   1. Seçin ve Ubuntu için Ubuntu server sürümüyle eşleşen yönergeleri izleyin.
1. Mevcut bir ASP.NET Core uygulaması.

## <a name="publish-and-copy-over-the-app"></a>Yayımlama ve uygulamanın üzerine kopyalayın

Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulamayı paketlemek için geliştirme ortamından (örneğin, *bin/yayın/&lt;target_framework_moniker&gt;/ publish*), olabilir sunucusunda çalıştırın:

```console
dotnet publish --configuration Release
```

Uygulama ayrıca olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) .NET Core çalışma zamanı sunucuda değil sağlamak isterseniz.

ASP.NET Core uygulaması (örneğin, SCP, SFTP) kuruluşun iş akışınıza tümleştirir bir aracını kullanarak sunucuya kopyalayın. Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *www/var/helloapp*).

> [!NOTE]
> Üretim dağıtım senaryosunda, sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.

Uygulamayı test edin:

1. Uygulamayı komut satırından çalıştırın: `dotnet <app_assembly>.dll`.
1. Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulamayı Linux üzerinde yerel olarak çalıştığını doğrulayın.

## <a name="configure-a-reverse-proxy-server"></a>Ters proxy sunucusunu yapılandırma

Ters proxy hizmet dinamik web uygulamaları için ortak bir kurulum var. Ters proxy, HTTP isteği sonlandırır ve ASP.NET Core uygulamasına iletir.

### <a name="use-a-reverse-proxy-server"></a>Ters proxy sunucusu kullan

Kestrel'i, ASP.NET Core dinamik içerik hizmet vermek için idealdir. Ancak, zengin olarak IIS, Apache veya Ngınx gibi sunucuları olarak web hizmeti özellikleri değildir. Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTPS sonlandırma HTTP sunucusundan sıkıştırma gibi iş boşaltabilirsiniz. Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılır.

Bu kılavuzun amacı doğrultusunda, Ngınx tek bir örneği kullanılır. Aynı sunucuda, HTTP sunucusu ile birlikte çalışır. Gereksinimlerinize bağlı olarak, bu farklı kurulum seçmiş olabilirsiniz.

Ters proxy tarafından istekleri iletilir çünkü [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket. Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` bu yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri doğru çalışması için başlık.

Kimlik doğrulaması, bağlantı oluşturma, yeniden yönlendirir ve coğrafi konum, gibi bir düzen bağlı olduğu herhangi bir bileşeni çağrılırken iletilen üstbilgileri Ara sonra yerleştirilmelidir. Genel kural olarak, tanılama ve hata işleme ara yazılım dışındaki diğer ara yazılımdan önce iletilen üstbilgileri ara yazılım çalıştırmanız gerekir. Bu sıralama, iletilen üst bilgi bağlı olan ara yazılım işleme için üstbilgi değerlerini tüketebileceği sağlar.

::: moniker range=">= aspnetcore-2.0"

Çağırma <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yönteminde `Startup.Configure` çağırmadan önce <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> veya benzer kimlik doğrulaması düzeni ara yazılımı. İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Çağırma <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yönteminde `Startup.Configure` çağırmadan önce <xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*> ve <xref:Microsoft.AspNetCore.Builder.FacebookAppBuilderExtensions.UseFacebookAuthentication*> veya benzer kimlik doğrulaması düzeni ara yazılımı. İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

Hayır ise <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilen ara yazılımıyla iletmek için varsayılan başlıkları `None`.

Yalnızca komut localhost üzerinde çalışan proxy'leri (127.0.0.1, [:: 1]) varsayılan olarak güvenilir. Diğer güvenilen proxy'leri veya kuruluş tanıtıcı istekler Internet ile web sunucusu arasında ağ eklerseniz bunları listesine <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> veya <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> ile <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>. Aşağıdaki örnekte güvenilen Ara sunucu IP adresi 10.0.0.100 iletilen üstbilgileri ara yazılımı için ekler `KnownProxies` içinde `Startup.ConfigureServices`:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

### <a name="install-nginx"></a>Ngınx yükleme

Kullanım `apt-get` Ngınx yüklemek için. Yükleyici oluşturur bir *systemd* Ngınx arka plan programı sistem başlangıcında çalışan başlatma betiği. Ubuntu için yükleme yönergelerini izleyin [Nginx: Debian/Ubuntu paketleri resmi](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).

> [!NOTE]
> İsteğe bağlı Ngınx modülleri gerekiyorsa, Ngınx kaynaktan oluşturmak gerekebilir.

Ngınx ilk kez yüklemenizden açıkça başlatın çalıştırarak:

```bash
sudo service nginx start
```

Giriş sayfası için Nginx varsayılan bir tarayıcı görüntüler doğrulayın. Giriş sayfası ulaşılabildiğinden `http://<server_IP_address>/index.nginx-debian.html`.

### <a name="configure-nginx"></a>Ngınx yapılandırma

Ngınx ters Ara sunucu istekleri iletmek üzere ASP.NET Core Uygulamanızı yapılandırmak için değiştirin */etc/nginx/sites-available/default*. Bir metin düzenleyicisinde açın ve içeriğini aşağıdakilerle değiştirin:

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

Hiçbir `server_name` eşleşme, Nginx varsayılan sunucu kullanır. Varsayılan sunucu tanımladıysanız, ilk sunucu yapılandırma dosyasındaki varsayılan sunucusudur. En iyi uygulama, yapılandırma dosyanızdaki 444 durum kodu döndüren bir belirli varsayılan sunucu ekleyin. Varsayılan sunucu yapılandırma örneği verilmiştir:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

Önceki yapılandırma dosyası ve varsayılan sunucusuyla, Ngınx ana bilgisayar üst bilgisi ile 80 numaralı bağlantı noktasında ortak trafiğin kabul `example.com` veya `*.example.com`. Bu konaklar eşleşmeyen istekleri için Kestrel iletilen olmaz. Ngınx eşleşen istekleri sırasında Kestrel iletir `http://localhost:5000`. Bkz: [nasıl ngınx bir isteği işler](https://nginx.org/docs/http/request_processing.html) daha fazla bilgi için. Kestrel'i'nın IP/bağlantı noktasını değiştirmek için bkz [Kestrel: Uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).

> [!WARNING]
> Uygun belirtmek için hata [sunucu_adı yönergesi](https://nginx.org/docs/http/server_names.html) uygulamanıza güvenlik açıklarını kullanıma sunar. Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanını denetimi bu güvenlik riski yoktur (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan). Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.

Ngınx yapılandırmasında kurulduktan sonra Çalıştır `sudo nginx -t` yapılandırma dosyalarının söz dizimini doğrulayın. Yapılandırma dosyası test başarılı olursa, Ngınx çalıştırarak değişikliklerini seçmek için zorlama `sudo nginx -s reload`.

Doğrudan uygulama sunucusunda çalıştırmak için:

1. Uygulamanın dizine gidin.
1. Uygulamayı çalıştırın: `dotnet <app_assembly.dll>`burada `app_assembly.dll` uygulamanın derleme dosya adı.

Uygulama sunucu üzerinde çalışır, ancak Internet üzerinden yanıt verememesi durumunda sunucunun Güvenlik Duvarı'nı denetleyin ve bağlantı noktası 80 açık olduğundan emin olun. Azure Ubuntu sanal makinesi kullanıyorsanız, gelen bağlantı noktası 80 trafiğini etkinleştirir, bir ağ güvenlik grubu (NSG) kuralı ekleyin. Gelen kural etkinleştirildiğinde giden trafiği otomatik olarak verilir olarak bir giden bağlantı noktası 80 kuralını etkinleştirmek için gerek yoktur.

Uygulamayı test etme işiniz bittiğinde, uygulama ile kapatma `Ctrl+C` komut isteminde.

## <a name="monitor-the-app"></a>Uygulamayı izleme

Sunucu yapılan istekleri iletmek üzere kurulur `http://<serveraddress>:80` sırasında Kestrel üzerinde çalışan ASP.NET Core uygulaması açın `http://127.0.0.1:5000`. Ancak, Ngınx Kestrel işlemini yönetmek için ayarlanmamış. *systemd* başlatmak ve temel alınan web uygulamasını izleme için bir hizmet dosyası oluşturmak için kullanılabilir. *systemd* başlatılmasını, durdurmasını ve işlemlerini yönetme için çok sayıda güçlü özellikler sağlar init sistemidir. 

### <a name="create-the-service-file"></a>Hizmet dosyası oluşturma

Hizmet tanım dosyası oluşturun:

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

Örnek bir uygulama için hizmet dosyası aşağıda verilmiştir:

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Kullanıcı *www-veri* kullanılmaz yapılandırma tarafından burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için uygun sahipliği verilen.

Kullanım `TimeoutStopSec` uygulama ilk kesme sinyallerini aldığı sonra kapatmak beklenecek süre yapılandırmak için. Uygulama bu dönemde değil kapatırsanız SIGKILL uygulamayı sonlandırmak için verilir. Unitless saniye değer sağlayın (örneğin, `150`), bir zaman aralığı değeri (örneğin, `2min 30s`), veya `infinity` zaman aşımı devre dışı bırakmak için. `TimeoutStopSec` değerini varsayılan olarak `DefaultTimeoutStopSec` Yöneticisi yapılandırma dosyasında (*systemd system.conf*, *system.conf.d*, *systemd user.conf*,  *User.conf.d*). Çoğu dağıtımlar için varsayılan zaman aşımı değeri 90 saniyedir.

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

Linux, büyük küçük harfe duyarlı dosya sistemi vardır. Yapılandırma dosyası arama sonuçlarındaki "Üretim" ASPNETCORE_ENVIRONMENT ayarlanması *appsettings. Production.JSON*değil *appsettings.production.json*.

Ortam değişkenlerini okumak yapılandırma sağlayıcıları için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçınılmalıdır. Yapılandırma dosyasında kullanmak için düzgün bir şekilde atlanan bir değer oluşturmak için aşağıdaki komutu kullanın:

```console
systemd-escape "<value-to-escape>"
```

Dosyayı kaydedin ve hizmeti etkinleştirin.

```bash
sudo systemctl enable kestrel-helloapp.service
```

Hizmeti başlatın ve çalıştığından emin olun.

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

Web uygulaması yönetilen systemd Kestrel ve yapılandırılmış ters proxy, tam olarak yapılandırılır ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`. Ayrıca, bir uzak makineden engelleyebilecek bir güvenlik duvarı engelleme de erişilebilir. Yanıt üst bilgilerini inceleyerek `Server` üstbilgisi Kestrel tarafından sunulan ASP.NET Core uygulaması gösterir.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a>Günlükleri görüntüleme

Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olayları ve işlemler için merkezi bir günlüğe kaydedilir. Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`. Görüntülenecek `kestrel-helloapp.service`-belirli öğeler, aşağıdaki komutu kullanın:

```bash
sudo journalctl -fu kestrel-helloapp.service
```

Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir kombinasyonunu döndürülen girdileri miktarını azaltabilirsiniz.

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a>Veri koruma

[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index)kimlik doğrulaması ara yazılımı (örneğin, tanımlama bilgisi Ara) dahil olmak üzere ve siteler arası istek sahteciliği (CSRF) korumaları. Bir kalıcı oluşturmak için veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management). Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.

Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:

* Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.
* Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.
* Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir. Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).

Kalıcı hale getirmek ve anahtar halkası şifrelemek için veri korumayı yapılandırmak için bkz:

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a>Uzun bir isteği üst bilgi alanları

Uygulama isteği gerektiriyorsa, proxy sunucu tarafından izin verilenden daha uzun olan üstbilgi alanlarını varsayılan ayarlar (genellikle 4K ya da platforma bağlı olarak 8K), aşağıdaki yönergeleri ayarlaması gerekir. Uygulamak için değerleri senaryoya bağlıdır. Daha fazla bilgi için sunucunuzun belgelerine bakın.

* [proxy_buffer_size](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [proxy_buffers](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [proxy_busy_buffers_size](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [large_client_header_buffers](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> Proxy arabellekler için varsayılan değerleri sürece artırmaz gerekli. Bu değerleri artırma (taşma) arabellek taşması riskini artırır ve kötü amaçlı kullanıcılar tarafından hizmet reddi (DoS) saldırıları.

## <a name="secure-the-app"></a>Bir uygulamanın güvenliğini sağlama

### <a name="enable-apparmor"></a>AppArmor etkinleştir

Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdeğinin parçası olan bir çerçevedir. LSM güvenlik modülleri'nın farklı uygulamaları destekler. [AppArmor](https://wiki.ubuntu.com/AppArmor) sınırlı bir kaynak kümesini programa sınırlandırma izin veren bir zorunlu erişim denetimi sistemi uygulayan bir LSM olduğu. AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.

### <a name="configure-the-firewall"></a>Güvenlik duvarını yapılandırma

Kullanılmayan tüm dış bağlantı devre dışı kapatın. Karmaşık güvenlik duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarını yapılandırmak için bir komut satırı arabirimi sağlayarak.

> [!WARNING]
> Bir güvenlik duvarı tüm sisteme erişimini engelleyecek değilse doğru yapılandırılmamış. Bağlanmak için SSH kullanıyorsanız hata doğru SSH bağlantı noktası belirtmek için etkili bir şekilde, sistemin dışında kilitleyecek. Varsayılan bağlantı noktası 22'dir. Daha fazla bilgi için [ufw giriş](https://help.ubuntu.com/community/UFW) ve [el ile](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).

Yükleme `ufw` ve gerekli herhangi bir bağlantı noktası üzerinde trafiğe izin verecek şekilde yapılandırın.

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a>Güvenli Ngınx

#### <a name="change-the-nginx-response-name"></a>Ngınx yanıt adını değiştirin

Edit *src/http/ngx_http_header_filter_module.c*:

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a>Seçeneklerini yapılandırın

Ek gerekli modülleri ile yapılandırın. Bir web uygulaması güvenlik duvarı gibi kullanmayı göz önünde bulundurun [ModSecurity](https://www.modsecurity.org/)uygulama sağlamlaştırmak için.

#### <a name="https-configuration"></a>HTTPS yapılandırma

* Bağlantı HTTPS trafiği için dinlemek üzere yapılandırmak `443` belirterek bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika.

* Aşağıda gösterilen yöntemler kullanarak güvenliğini sağlamlaştırmak */etc/nginx/nginx.conf* dosya. Daha güçlü şifreleme seçme ve tüm trafiği, HTTP, HTTPS üzerinden yönlendirme verilebilir.

* Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üst bilgisi olan HTTPS üzerinden istemci tarafından yapılan tüm sonraki istekleri sağlar.

* HSTS üst bilgi eklemeyin veya uygun bir seçtiğiniz `max-age` varsa HTTPS gelecekte devre dışı bırakılır.

Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:

[!code-nginx[](linux-nginx/proxy.conf)]

Düzen */etc/nginx/nginx.conf* yapılandırma dosyası. Örnek içeren `http` ve `server` bölümlerde bir yapılandırma dosyası.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Güvenli Ngınx clickjacking gelen

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)olarak da bilinen bir *UI redress saldırı*, bir kötü amaçlı bir Web sitesi ziyaretçi yere sağladı daha şu anda ziyaret ettiğiniz bir bağlantı veya başka bir sayfaya düğmesine tıklamak saldırıdır. Kullanım `X-FRAME-OPTIONS` sitesini güvenli hale getirmek için.

Clickjacking saldırıları azaltmak için:

1. Düzen *nginx.conf* dosyası:

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   Satır Ekle `add_header X-Frame-Options "SAMEORIGIN";`.
1. Dosyayı kaydedin.
1. Ngınx yeniden başlatın.

#### <a name="mime-type-sniffing"></a>MIME türü algılaması

Tarayıcı yanıt içerik türü geçersiz üstbilgi bildirir gibi bu başlığı çoğu tarayıcılardan MIME algılaması bildirilen içerik türü, uzakta bir yanıt engeller. İle `nosniff` seçeneğinde, tarayıcı sunucu içeriktir "text/html" derse, "metin/html olarak" işleyen.

Düzen *nginx.conf* dosyası:

```bash
sudo nano /etc/nginx/nginx.conf
```

Satır Ekle `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin ve ardından Ngınx'i yeniden başlatın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Linux üzerinde .NET Core önkoşulları](/dotnet/core/linux-prerequisites)
* [Ngınx: İkili sürümler: Debian/Ubuntu paketleri resmi](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* [NGINX: İletilen üstbilgi kullanılıyor](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
