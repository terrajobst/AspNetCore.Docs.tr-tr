---
title: ASP.NET Core Nginx ile Linux ana bilgisayar
author: rick-anderson
description: Ubuntu Kestrel üzerinde çalışan bir ASP.NET Core web uygulaması HTTP trafiği iletmek için 16.04 üzerinde ters Ara sunucu Nginx Kurulum öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 374b13e0851cd171a7d8500a4965851a3a0eb49c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277380"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>ASP.NET Core Nginx ile Linux ana bilgisayar

Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Bu kılavuz bir Ubuntu 16.04 sunucusunda üretime hazır ASP.NET Core ortamını ayarlama açıklar. Büyük olasılıkla bu yönergeleri Ubuntu daha yeni sürümleri ile çalışır ancak yönergeleri içeren daha yeni sürümlerle test edilmemiştir.

> [!NOTE]
> Ubuntu 14.04 için *supervisord* Kestrel işlem izleme için bir çözüm olarak önerilir. *systemd* Ubuntu 14.04 üzerinde kullanılamaz. Ubuntu 14.04 yönergeler için bkz: [bu konunun önceki sürümü](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

Bu Kılavuzu:

* Var olan bir ASP.NET Core uygulamaya bir ters Ara sunucu arkasındaki yerleştirir.
* Kestrel web sunucusu isteklerini iletmek için ters proxy sunucuyu ayarlar.
* Web uygulaması başlangıçta bir arka plan programı olarak gerçekleştirilmesini sağlar.
* Web uygulaması yeniden yardımcı olmak için bir işlem yönetimini yapılandırır.

## <a name="prerequisites"></a>Önkoşullar

1. Ubuntu 16.04 server sudo ayrıcalığa sahip standart kullanıcı hesabı ile erişim.
1. .NET çekirdeği çalışma zamanı sunucuya yükleyin.
   1. Ziyaret [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).
   1. En son Önizleme olmayan çalışma zamanı altındaki listeden seçin **çalışma zamanı**.
   1. Seçin ve Ubuntu için sunucu Ubuntu sürümüyle eşleşen yönergeleri izleyin.
1. Mevcut bir ASP.NET Core uygulama.

## <a name="publish-and-copy-over-the-app"></a>Yayımlama ve uygulama kopyalayın

Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulama paketi için geliştirme ortamı'ndan (örneğin, *bin/sürüm/&lt;target_framework_moniker&gt;/ yayımlama*), olabilir sunucusunda çalıştırın:

```console
dotnet publish --configuration Release
```

Uygulama aynı zamanda olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) sunucuda .NET çekirdeği çalışma zamanı sürekli olmayan tercih ederseniz.

ASP.NET Core uygulama (örneğin, SCP, SFTP) kuruluşunuzun akışına tümleşen bir aracı kullanarak sunucuya kopyalayın. Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *aspnetcore/var/hellomvc*).

> [!NOTE]
> Bir üretim dağıtım senaryosunda sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.

Uygulamayı test etme:

1. Komut satırından uygulamayı çalıştırın: `dotnet <app_assembly>.dll`.
1. Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulaması çalışır Linux üzerinde yerel olarak doğrulayın.

## <a name="configure-a-reverse-proxy-server"></a>Ters proxy sunucusunu yapılandırın

Ters proxy hizmet veren dinamik web uygulamaları için ortak bir kurulur. Ters proxy HTTP isteği sonlandırır ve ASP.NET Core uygulamaya iletir.

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> Her iki yapılandırma&mdash;ile veya bir ters Ara sunucu olmadan&mdash;geçerli ve desteklenen bir barındırma yapılandırması ASP.NET Core 2.0 veya sonraki uygulamalar içindir. Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a>Ters proxy sunucusu kullan

Kestrel, dinamik içerik ASP.NET çekirdek hizmet vermek için mükemmeldir. Bununla birlikte, IIS, Apache veya Nginx gibi sunucuları olarak özellik zengin olarak web hizmet yetenekleri değil. Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTP sunucusundan SSL sonlandırma sıkıştırma gibi iş boşaltabilir. Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılabilir.

Bu kılavuzun amaçları Nginx tek bir örneğini kullanılır. HTTP sunucusu yanında aynı sunucu üzerinde çalışır. Gereksinimlerine bağlı olarak, farklı kurulum seçmiş olabilirsiniz.

İstekleri tarafından ters proxy iletilir olduğundan [iletilen üstbilgileri Ara](xref:host-and-deploy/proxy-load-balancer) gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket. Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.

Kimlik doğrulama, bağlantı oluşturma, yeniden yönlendirir ve coğrafi konum, gibi şema bağımlı herhangi bir bileşeni iletilen üstbilgileri Ara başlatma sonrasında yerleştirilmelidir. Genel kural olarak, tanılama ve hata işleme ara yazılım dışındaki diğer ara yazılımdan önce iletilen üstbilgileri Ara çalıştırmanız gerekir. Bu sıralama iletilen üstbilgileri bilgi bağlı olan ara yazılım işleme üstbilgi değerleri tüketebileceği sağlar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) veya benzer kimlik doğrulama düzeni ara yazılım. İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) ve [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) veya benzer kimlik doğrulama şeması Ara yazılım. İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:

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

---

Öyle değilse [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen ara yazılımıyla iletmek için varsayılan üstbilgiler `None`.

Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir. Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-nginx"></a>Nginx yükleyin

Kullanım `apt-get` Nginx yüklemek için. Yükleyici oluşturur bir *systemd* Nginx arka plan programı gibi sistem başlangıcında çalışan init komut dosyası. 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

Ubuntu kişisel paket arşiv (PPA) gönüllüsü tarafından korunur ve tarafından dağıtılmadı [nginx.org](https://nginx.org/). Daha fazla bilgi için bkz: [Nginx: ikili sürümler: resmi Debian/Ubuntu paketleri](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).

> [!NOTE]
> İsteğe bağlı Nginx modülleri gerekirse, Nginx kaynağından derleme gerekli olabilir.

Nginx ilk kez yüklendiğinden bu yana, açıkça başlatılsın çalıştırarak:

```bash
sudo service nginx start
```

Giriş sayfasında Nginx için varsayılan bir tarayıcı görüntüler doğrulayın. Giriş sayfası erişilebildiğinden `http://<server_IP_address>/index.nginx-debian.html`.

### <a name="configure-nginx"></a>Nginx yapılandırın

ASP.NET Core uygulamanıza istekleri iletmek için ters Ara sunucu Nginx yapılandırmak için değiştirin */etc/nginx/sites-available/default*. Bir metin düzenleyicisinde açın ve içeriği aşağıdakiyle değiştirin:

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

Durumlarda `server_name` eşleşirse, Nginx varsayılan sunucu kullanır. Varsayılan sunucu tanımlanmışsa ilk yapılandırma dosyasındaki varsayılan sunucu sunucusudur. En iyi uygulama, yapılandırma dosyanızda 444 bir durum kodunu döndüren bir belirli varsayılan sunucu ekleyin. Varsayılan sunucu yapılandırma örneği verilmiştir:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

Önceki yapılandırma dosyası ve varsayılan sunucusuyla, Nginx ana bilgisayar üstbilgisi ile 80 numaralı bağlantı noktasında genel trafiği kabul `example.com` veya `*.example.com`. Bu konaklar eşleşmeyen istekleri için Kestrel iletilen olmaz. Nginx Kestrel eşleşen isteklerini iletir `http://localhost:5000`. Bkz: [nasıl nginx bir isteği işler](https://nginx.org/docs/http/request_processing.html) daha fazla bilgi için.

> [!WARNING]
> Uygun belirtmek için hata [sunucu_adı yönergesi](https://nginx.org/docs/http/server_names.html) güvenlik açıkları, uygulamanızın kullanıma sunar. Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanı denetlemek, bu güvenlik riski değil (tersine `*.com`, açık olduğu). Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.

Nginx yapılandırma kurulduktan sonra çalıştırmak `sudo nginx -t` yapılandırma dosyalarını söz dizimini doğrulayın. Yapılandırma dosyası sınaması başarılı olursa, çalıştırarak değişiklikleri almak için Nginx zorla `sudo nginx -s reload`.

Doğrudan uygulama sunucusunda çalıştırmak için:

1. Uygulamanın dizine gidin.
1. Uygulamanın yürütülebilir dosyayı çalıştırmak: `./<app_executable>`.

İzin hatası oluşursa, izinleri değiştirin:

```console
chmod u+x <app_executable>
```

Uygulama sunucusunda çalışır ancak Internet üzerinden yanıt başarısız olursa, sunucunun Güvenlik Duvarı'nı denetleyin ve 80 numaralı bağlantı noktası açık olduğundan emin olun. Bir Azure Ubuntu VM kullanıyorsanız, gelen bağlantı noktası 80 trafiğini etkinleştirir, bir ağ güvenlik grubu (NSG) kuralı ekleyin. Giden trafik gelen kuralı etkinleştirildiğinde otomatik olarak verilir gibi bir giden bağlantı noktası 80 Kuralı etkinleştirmek için gerek yoktur.

Uygulamayı test etme işiniz bittiğinde, uygulama ile kapatıldı `Ctrl+C` komut isteminde.

## <a name="monitoring-the-app"></a>Uygulama izleme

Yapılan isteklerini iletmek için Kurulum sunucusudur `http://<serveraddress>:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`. Ancak, Nginx Kestrel işlemini yönetmek için ayarlanmamış. *systemd* başlatmak ve temel web uygulaması izleme için bir hizmet dosyasını oluşturmak için kullanılabilir. *systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir. 

### <a name="create-the-service-file"></a>Hizmet dosyası oluşturma

Hizmet tanımı dosyası oluşturun:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Uygulama için bir örnek hizmet dosyası verilmiştir:

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Kullanıcı *www veri* kullanılmaz yapılandırma tarafından burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen.

Linux büyük küçük harfe duyarlı dosya sistemi vardır. Yapılandırma dosyası için arama sonuçlarındaki "Üretim" ASPNETCORE_ENVIRONMENT ayarlanması *appsettings. Production.JSON*değil *appsettings.production.json*.

> [!NOTE]
> Ortam değişkenleri okumak yapılandırma sağlayıcısı için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçış uygulanmalıdır. Yapılandırma dosyasında kullanmak için düzgün bir şekilde Atlanan değeri oluşturmak için aşağıdaki komutu kullanın:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Dosyayı kaydedin ve hizmeti etkinleştirin.

```bash
systemctl enable kestrel-hellomvc.service
```

Hizmeti başlatın ve çalıştığından emin olun.

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Ters proxy yapılandırılmış ve systemd yönetilen Kestrel ile web uygulaması tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`. Ayrıca, engelliyor olabilir herhangi bir güvenlik duvarını engelleme uzak bir makineden de erişilebilir. Yanıt Üstbilgileri incelemek `Server` üstbilgisi tarafından Kestrel sunulmasını ASP.NET Core uygulama gösterir.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Günlükleri görüntüleme

Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olaylar ve işlemleri merkezi bir günlüğe kaydedilir. Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`. Görüntülemek için `kestrel-hellomvc.service`-belirli öğeleri, aşağıdaki komutu kullanın:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir birleşimi döndürülen girişleri azaltmak.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Uygulama güvenliğini sağlama

### <a name="enable-apparmor"></a>AppArmor etkinleştir

Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdek parçası olan bir çerçevedir. LSM güvenlik modüllerin farklı uygulamaları destekler. [AppArmor](https://wiki.ubuntu.com/AppArmor) kaynakları sınırlı sayıda programına sınırlandırma sağlayan bir zorunlu erişim denetimi sisteminde uygulayan bir LSM değil. AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.

### <a name="configuring-the-firewall"></a>Güvenlik duvarını yapılandırma

Kullanılmayan tüm dış bağlantı noktalarını kapatmak kapatın. Eylemlerin Güvenlik Duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarı yapılandırması için bir komut satırı arabirimi sağlayarak. Doğrulayın `ufw` gereken herhangi bir bağlantı trafiğine izin verecek şekilde yapılandırılmış.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Nginx güvenliğini sağlama

#### <a name="change-the-nginx-response-name"></a>Nginx yanıt adını değiştirin

Edit *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a>Seçeneklerini yapılandırma

Sunucu ile ek gerekli modüllerini yapılandırın. Bir web uygulaması güvenlik duvarı gibi kullanmayı [ModSecurity](https://www.modsecurity.org/), uygulama sağlamlaştırmak için.

#### <a name="configure-ssl"></a>SSL yapılandırma

* Bağlantı noktasında HTTPS trafiğini dinlemek üzere yapılandırmak `443` bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika belirterek.

* Aşağıda gösterilen uygulamaları kullanan güvenliğini sağlamlaştırmak */etc/nginx/nginx.conf* dosya. Örnek daha güçlü şifreleme seçme ve HTTPS için HTTP üzerinden tüm trafiği yönlendirerek verilebilir.

* Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üstbilgisi sağlar, HTTPS üzerinden yalnızca istemci tarafından yapılan tüm sonraki istekleri.

* Strict Aktarım güvenlik üstbilgisi ekleme veya uygun bir seçtiğiniz `max-age` SSL gelecekte devre dışıysa.

Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:

[!code-nginx[](linux-nginx/proxy.conf)]

Düzen */etc/nginx/nginx.conf* yapılandırma dosyası. Örnek içeren `http` ve `server` bölümler bir yapılandırma dosyası.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Clickjacking öğesini gelen güvenli Nginx
Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir. Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları. Kullanım X-FRAME-site güvenli hale getirmek için OPTIONS.

Düzen *nginx.conf* dosyası:

```bash
sudo nano /etc/nginx/nginx.conf
```

Satırı ekleyin `add_header X-Frame-Options "SAMEORIGIN";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.

#### <a name="mime-type-sniffing"></a>MIME türü algılaması

Üstbilgi yanıt içerik türü geçersiz tarayıcıyı yönlendirir gibi bu başlığı MIME algılaması çoğu tarayıcılarından yanıt içerik türü, yöntemi bir kenara bırakarak engeller. İle `nosniff` seçeneği, sunucunun içeriktir "text/html" diyorsa, tarayıcı, "text/html" işler.

Düzen *nginx.conf* dosyası:

```bash
sudo nano /etc/nginx/nginx.conf
```

Satırı ekleyin `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Nginx: İkili sürümler: resmi Debian/Ubuntu paketleri](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [Proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırın](xref:host-and-deploy/proxy-load-balancer)
* [NGINX: iletilen üstbilgi kullanma](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
