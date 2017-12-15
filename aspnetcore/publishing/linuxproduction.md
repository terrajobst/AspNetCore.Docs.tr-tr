---
title: ASP.NET Core Nginx ile Linux ana bilgisayar
description: "Ubuntu Kestrel üzerinde çalışan bir ASP.NET Core web uygulama HTTP trafiği iletmek için 16.04 üzerinde ters Ara sunucu Nginx Kurulum açıklar."
keywords: ASP.NET Core, Linux, nginx, Ubuntu, Ters Proxy
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 2c7401657486a8e5dbc8213d79dcfd5e0ec76585
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a>ASP.NET Core Nginx ile Linux için bir barındırma ortamını ayarlama ve dağıtma

Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Bu kılavuz bir Ubuntu 16.04 sunucusunda üretime hazır ASP.NET Core ortamını ayarlama açıklar.

**Not:** Kestrel işlem izleme için bir çözüm olarak supervisord Ubuntu 14.04 için önerilir. systemd Ubuntu 14.04 üzerinde kullanılabilir değil. [Bu belgenin önceki sürümünü bakın](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

Bu Kılavuzu:

* Var olan bir ASP.NET Core uygulamasını bir ters Ara sunucu arkasındaki yerleştirir
* Ters proxy sunucuyu isteklerini Kestrel web sunucusuna iletmek için ayarlar
* Web uygulaması bir arka plan programı Başlangıç çalışır sağlar
* Web uygulaması yeniden yardımcı olmak için bir işlem yönetim aracı yapılandırır

## <a name="prerequisites"></a>Önkoşullar

1. Sudo ayrıcalığa sahip standart kullanıcı hesabı ile bir Ubuntu 16.04 sunucuya erişimi
2. Mevcut bir ASP.NET Core uygulama

## <a name="copy-over-your-app"></a>Uygulamanızı kopyalama

Çalıştırma `dotnet publish` bir uygulama sunucusunda çalışabilecek kendi içinde bulunan bir dizine paketlemek için geliştirme ortamı'ndan.

ASP.NET Core uygulama ne olursa olsun Aracı (SCP, FTP, vb.) iş akışınıza tümleştirir kullanarak sunucuya kopyalayın. Uygulama, örneğin test edin:
 - Komut satırından çalıştırma`dotnet yourapp.dll`
 - Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulama Linux üzerinde çalıştığını doğrulayın. 
 
## <a name="configure-a-reverse-proxy-server"></a>Ters proxy sunucusunu yapılandırın

Ters proxy dinamik web uygulamaları için ortak bir kurulur. Ters proxy HTTP isteği sonlandırır ve ASP.NET Core uygulamaya iletir.

### <a name="why-use-a-reverse-proxy-server"></a>Ters proxy sunucusu neden kullanılır?

Kestrel, dinamik içerik ASP.NET çekirdek hizmet vermek için harikadır; Bununla birlikte, IIS, Apache veya Nginx gibi sunucuları olarak özellik zengin olarak web hizmet bölümleri değil. Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTP sunucusundan SSL sonlandırma sıkıştırma gibi iş boşaltabilir. Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılabilir.

Bu kılavuzun amaçları Nginx tek bir örneğini kullanılır. HTTP sunucusu yanında aynı sunucu üzerinde çalışır. Gereksinimlerinize bağlı olarak, farklı kurulum seçebilirsiniz.

İstekleri tarafından ters proxy iletilir olduğundan `ForwardedHeaders` Ara `Microsoft.AspNetCore.HttpOverrides` paket. Bu ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.

Ters proxy sunucusu kurma, kimlik doğrulaması ara yazılımı gereken `UseForwardedHeaders` ilk önce çalışmalıdır. Bu sıralama, kimlik doğrulaması ara yazılımı etkilenen değerleri kullanabilir ve doğru yeniden yönlendirme URI oluşturmak sağlar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

Çağırma `UseForwardedHeaders` yöntemi (içinde `Configure` yöntemi *haline*) çağırmadan önce `UseAuthentication` veya benzer kimlik doğrulama düzeni ara:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Çağırma `UseForwardedHeaders` yöntemi (içinde `Configure` yöntemi *haline*) çağırmadan önce `UseIdentity` ve `UseFacebookAuthentication` veya benzer kimlik doğrulama düzeni ara:

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

### <a name="install-nginx"></a>Nginx yükleyin

```bash
sudo apt-get install nginx
```

> [!NOTE]
> İsteğe bağlı Nginx modülleri yüklemeyi planlıyorsanız, Nginx kaynağından oluşturmak için gerekli.

Kullanım `apt-get` Nginx yüklemek için. Yükleyici Nginx arka plan programı gibi sistem başlangıcında çalışan bir sistem V init betiği oluşturur. Nginx ilk kez yüklendiğinden bu yana, açıkça başlatılsın çalıştırarak:

```bash
sudo service nginx start
```

Giriş sayfasında Nginx için varsayılan bir tarayıcı görüntüler doğrulayın.

### <a name="configure-nginx"></a>Nginx yapılandırın

ASP.NET Core uygulamamız için istekleri iletmek için ters Ara sunucu Nginx yapılandırmak için değiştirin `/etc/nginx/sites-available/default`. Bir metin düzenleyicisinde açın ve içeriği aşağıdakiyle değiştirin:

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Bu Nginx yapılandırma dosyası gelen ortak bağlantı noktasından trafiğini `80` bağlantı noktasına `5000`.

Tamamladıktan sonra değişiklik yapma Nginx yapılandırmanızı, çalıştırabilirsiniz `sudo nginx -t` yapılandırma dosyalarınızın söz dizimini doğrulayın. Yapılandırma dosyası sınaması başarılı olursa, çalıştırarak değişiklikleri almak için Nginx sorabileceğiniz `sudo nginx -s reload`.

## <a name="monitoring-our-application"></a>Uygulama izleme

Nginx olan şimdi yapılan isteklerini iletmek için kurulumu `http://yourhost:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`. Ancak, Nginx Kestrel işlemini yönetmek üzere ayarlanmış değil. Kullanabileceğiniz *systemd* ve Başlat ve temel web uygulaması izlemek için bir hizmet dosyası oluşturun. *systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir. 

### <a name="create-the-service-file"></a>Hizmet dosyası oluşturma

Hizmet tanımı dosyası oluşturun:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Uygulamamız için örnek bir hizmet dosyası verilmiştir:

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

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

**Not:** , kullanıcı *www veri* kullanılmaz yapılandırmanıza göre burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen.

Dosyayı kaydedin ve hizmeti etkinleştirin.

```bash
systemctl enable kestrel-hellomvc.service
```

Hizmeti başlatın ve çalıştığından emin olun.

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Web uygulaması yapılandırılmış ters proxy ve systemd yönetilen Kestrel ile tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`. Ayrıca, engelliyor olabilir herhangi bir güvenlik duvarını engelleme uzak bir makineden de erişilebilir. Yanıt Üstbilgileri incelemek `Server` üstbilgisi tarafından Kestrel sunulmasını ASP.NET Core uygulama gösterir.

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

## <a name="securing-our-application"></a>Uygulamamız güvenliğini sağlama

### <a name="enable-apparmor"></a>AppArmor etkinleştir

Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdek parçası olan bir çerçevedir. LSM güvenlik modüllerin farklı uygulamaları destekler. [AppArmor](https://wiki.ubuntu.com/AppArmor) kaynakları sınırlı sayıda programına sınırlandırma sağlayan bir zorunlu erişim denetimi sisteminde uygulayan bir LSM değil. AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.

### <a name="configuring-our-firewall"></a>Bizim güvenlik duvarını yapılandırma

Kullanılmayan tüm dış bağlantı noktalarını kapatmak kapatın. Eylemlerin Güvenlik Duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarı yapılandırması için bir komut satırı arabirimi sağlayarak. Doğrulayın `ufw` gereksinim duyduğunuz herhangi bir bağlantı trafiğine izin verecek şekilde yapılandırılmış.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Nginx güvenliğini sağlama

Nginx varsayılan dağıtımını SSL sağlamaz. Ek güvenlik özellikleri etkinleştirmek için kaynak sunucudan oluşturun.

#### <a name="download-the-source-and-install-the-build-dependencies"></a>Kaynak yükleyip derleme bağımlılıkları

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Nginx yanıt adını değiştirin

Düzen *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Seçenekleri yapılandırmak ve derleme

Normal ifadeler için PCRE kitaplığı gereklidir. Normal ifadeler konumu yönergesinde ngx_http_rewrite_module için kullanılır. Http_ssl_module HTTPS protokolü desteği ekler.

Web uygulaması güvenlik duvarı gibi kullanmayı *ModSecurity* uygulamanız sağlamlaştırmak için.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>SSL yapılandırma

* Bağlantı noktasında HTTPS trafiğini dinlemek için sunucu yapılandırma `443` bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika belirterek.

* Aşağıda gösterilen uygulamaları kullanan güvenlik sağlamlaştırmak */etc/nginx/nginx.conf* dosya. Örnek daha güçlü şifreleme seçme ve HTTPS için HTTP üzerinden tüm trafiği yönlendirerek verilebilir.

* Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üstbilgisi sağlar, HTTPS üzerinden yalnızca istemci tarafından yapılan tüm sonraki istekleri.

* Strict Aktarım güvenlik üstbilgisi eklemeyin veya uygun bir seçtiğiniz `max-age` SSL gelecekte devre dışı planlıyorsanız.

Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:

[!code-nginx[Main](linuxproduction/proxy.conf)]

Düzen */etc/nginx/nginx.conf* yapılandırma dosyası. Örnek içeren `http` ve `server` bölümler bir yapılandırma dosyası.

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Clickjacking öğesini gelen güvenli Nginx
Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir. Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları. Kullanım X-FRAME-sitenizin güvenliğini sağlamak için OPTIONS.

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
