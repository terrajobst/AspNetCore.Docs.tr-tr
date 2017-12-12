---
title: ASP.NET Core Apache ile Linux ana bilgisayar
description: "Apache CentOS üzerinde ters proxy sunucu olarak Kestrel üzerinde çalışan bir ASP.NET Core web uygulaması için HTTP trafiği yönlendirmek için nasıl ayarlanacağını öğrenin."
keywords: "ASP.NET Core, Apache, CentOS, Ters Proxy, Linux, mod_proxy, httpd, barındırma"
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>ASP.NET Core Apache ile Linux için bir barındırma ortamını ayarlama ve dağıtma

Tarafından [Shayne Boyer](https://github.com/spboyer)

Apache çok yaygın bir HTTP sunucusudur ve HTTP trafiği için nginx benzer yönlendirmek için bir proxy olarak yapılandırılabilir. Bu kılavuzda, biz CentOS 7 üzerinde Apache ayarlama ve gelen bağlantıları Hoş Geldiniz ve Kestrel üzerinde çalışan ASP.NET Core uygulama yönlendirmek için ters proxy kullanmak üzere öğreneceksiniz. Bu amaçla kullanacağız *mod_proxy* uzantısı ve ilgili diğer Apache modüller.

## <a name="prerequisites"></a>Önkoşullar

1. Sudo ayrıcalığa sahip standart kullanıcı hesabı ile CentOS 7 çalıştıran bir sunucu.
2. Mevcut bir ASP.NET Core uygulama. 

## <a name="publish-your-application"></a>Uygulamanızı yayımlama

Çalıştırma `dotnet publish -c Release` sunucunuzda çalışan kendi içinde bulunan bir dizin uygulamanıza paketlemek için geliştirme ortamınızı gelen. Yayımlanan uygulama sonra SCP, FTP veya başka bir dosya aktarım yöntemi kullanarak sunucusuna kopyalanması gerekir. 

> [!NOTE]
> Bir üretim dağıtım senaryosunda sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar. 

## <a name="configure-a-proxy-server"></a>Bir proxy sunucusunu yapılandırın

Ters proxy dinamik web uygulamaları için ortak bir kurulur. Ters proxy HTTP isteği sonlandırır ve ASP.NET uygulamasına iletir.

Bir proxy sunucusu, istemci isteklerini yerine bunları kendisi yerine getiren başka bir sunucuya iletir biridir. Ters proxy genellikle rastgele istemcileri adına sabit bir hedef iletir. Bu kılavuzda, Apache Kestrel ASP.NET Core uygulama hizmet ettiğini aynı sunucu üzerinde çalışan ters proxy olarak yapılandırılmış. 

Her bir uygulamanın parçası ayrı fiziksel makineler, Docker kapsayıcıları veya yapılandırmaları mimari gereksinimleri veya sınırlamaları bağlı olarak birlikte bulunabilir.

### <a name="install-apache"></a>Apache yükleyin

Apache web sunucusu üzerinde CentOS yüklemektir ilk ancak tek bir komut şimdi paketlerimize güncelleştirin.

```bash
    sudo yum update -y
```

Bu, tüm yüklü paketleri kendi en son sürümüne güncelleştirilir sağlar. Apache kullanarak yükleme`yum`

```bash
    sudo yum -y install httpd mod_ssl
```

Çıktı aşağıdakine benzer bir şey yansıtmalıdır.

```bash
    Downloading packages:
    httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01     
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
    Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
    Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

    Installed:
    httpd.x86_64 0:2.4.6-40.el7.centos.4                                                                           

    Complete!
```

> [!NOTE]
> Bu örnekte, CentOS 7 sürümü 64-bit olduğundan çıktı httpd.86_64 yansıtır. Çıktı sunucunuz için farklı olabilir. Apache yüklendiği doğrulamak için çalıştırın `whereis httpd` bir komut isteminden. 

### <a name="configure-apache-for-reverse-proxy"></a>Apache için ters proxy ayarlarını yapılandır

İçinde Apache için yapılandırma dosyalarının bulunduğu `/etc/httpd/conf.d/` dizin. Herhangi dosya ile **.conf** uzantısı, modül yapılandırma dosyalarında yanı sıra alfabetik sırada işlenir `/etc/httpd/conf.modules.d/`, herhangi bir yapılandırma içeren modüllerini yüklemek gerekli dosyaları.

Bu arayacağız Bu örnek için uygulamanız için bir yapılandırma dosyası oluşturma`hellomvc.conf`

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

*VirtualHost* hangisinin olabilir birden çok düğüm, bir dosyada ya da çok sayıda dosya sunucusundaki 80 numaralı bağlantı noktasını kullanarak herhangi bir IP adresi dinleyecek şekilde ayarlanmıştır. Sonraki iki satır 5000 makine 127.0.0.1 noktasına ve geriye doğru kökündeki alınan tüm istekleri geçirmek için ayarlanır. Çift yönlü iletişim, her iki ayarın olmasını orada için *ProxyPass* ve *ProxyPassReverse* gereklidir.

Günlük VirtualHost yapılandırılabilir kullanarak *hata günlüğüne* ve *CustomLog* yönergeleri. *Hata günlüğü* sunucusu günlük nerede hataları konumu ve *CustomLog* filename ve günlük dosyası biçimini ayarlar. Örneğimizde isteği bilgilerini günlüğe nerede budur. Her istek için bir satır olacak.

Dosyayı kaydedin ve yapılandırmayı test etme. Her şeyi geçerse, yanıt olmalıdır `Syntax [OK]`.

```bash
    sudo service httpd configtest
```

Apache yeniden başlatın.

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>Uygulama izleme

Apache olan şimdi yapılan isteklerini iletmek için kurulumu `http://localhost:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`.  Ancak, Apache Kestrel işlemini yönetmek üzere ayarlanmış değil. Kullanacağız *systemd* ve Başlat ve temel web uygulaması izlemek için bir hizmet dosyası oluşturun. *systemd* , başlatma, durdurma ve işlemlerini yönetme pek çok güçlü özellikler sağlayan bir init sistemidir. 


### <a name="create-the-service-file"></a>Hizmet dosyası oluşturma

Hizmet tanımı dosyası oluşturma 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Uygulamamız bir örnek hizmet dosyası.

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

    [Service]
    WorkingDirectory=/var/aspnetcore/hellomvc
    ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
    Restart=always
    # Restart service after 10 seconds if dotnet service crashes
    RestartSec=10
    SyslogIdentifier=dotnet-example
    User=apache
    Environment=ASPNETCORE_ENVIRONMENT=Production 

    [Install]
    WantedBy=multi-user.target
```

> [!NOTE]
> **Kullanıcı** --If kullanıcı *apache* kullanılmaz yapılandırmanıza göre burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen

Dosyayı kaydedin ve hizmeti etkinleştirin.

```bash
    systemctl enable kestrel-hellomvc.service
```

Hizmeti başlatın ve çalıştığından emin olun.

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Web uygulaması yapılandırılmış ters proxy ve systemd yönetilen Kestrel ile tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`. Yanıt Üstbilgileri incelemek **Server** Kestrel tarafından sunulmasını ASP.NET Core uygulama görüntülenmeye devam eder.

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Günlükleri görüntüleme

Kestrel kullanarak web uygulamasına systemd kullanılarak yönetilir olduğundan, tüm olaylar ve işlemleri merkezi bir günlüğe kaydedilir. Ancak, bu günlük tüm hizmetleri ve işlemleri systemd tarafından yönetilen tüm girişleri içerir. Görüntülemek için `kestrel-hellomvc.service` belirli öğeleri, aşağıdaki komutu kullanın.

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir birleşimi döndürülen girişleri azaltmak.

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Uygulamamız güvenliğini sağlama

### <a name="configure-firewall"></a>Güvenlik duvarını yapılandırma

*Firewalld* bağlantı noktalarını ve paket filtreleme yönetmek için iptables kullanmaya devam edebilirsiniz ancak ağ bölgeleri için destek ile Güvenlik Duvarı'nı yönetmek için bir dinamik arka plan programı kullanılır. Firewalld, varsayılan olarak yüklü olması `yum` paketi yükleyin veya doğrulamak için kullanılır.

```bash
    sudo yum install firewalld -y
```

Kullanarak `firewalld` yalnızca uygulama için gerekli bağlantı noktalarını açın. Bu durumda, bağlantı noktası 80 ve 443 kullanılır. Aşağıdaki komutları kalıcı olarak ayarlar bu açın.

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

Güvenlik Duvarı ayarlarını yeniden yükleyin ve kullanılabilir hizmetleri ve bağlantı noktalarını varsayılan bölgede denetleyin. İnceleyerek seçenekleri kullanılabilir`firewall-cmd -h`

```bash 
    sudo firewall-cmd --reload
    sudo firewall-cmd --list-all
```

```bash
    public (default, active)
    interfaces: eth0
    sources: 
    services: dhcpv6-client
    ports: 443/tcp 80/tcp
    masquerade: no
    forward-ports: 
    icmp-blocks: 
    rich rules: 
```

### <a name="ssl-configuration"></a>SSL yapılandırması

SSL için Apache yapılandırmak için mod_ssl modülü kullanılır.  Biz yüklendiğinde bu başlangıçta yüklendi `httpd` modülü. Eksik veya yüklü değil, yum yapılandırmanıza eklemek için kullanın.

```bash
    sudo yum install mod_ssl
```
SSL zorlamak için yükleyin`mod_rewrite`

```bash
    sudo yum install mod_rewrite
```

`hellomvc.conf` Bu örnek yeniden yanı sıra yeni ekleme etkinleştirmek için değiştirilmesi gereken için oluşturulmuş olan dosyasını **VirtualHost** HTTPS için bölüm.

```text
    <VirtualHost *:80>
        RewriteEngine On
        RewriteCond %{HTTPS} !=on
        RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
    </VirtualHost>

    <VirtualHost *:443>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
        SSLEngine on
        SSLProtocol all -SSLv2
        SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
        SSLCertificateFile /etc/pki/tls/certs/localhost.crt
        SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    </VirtualHost>    
```

> [!NOTE]
> Bu örnek, yerel olarak oluşturulan bir sertifika kullanıyor. **SSLCertificateFile** birincil sertifikası dosyanız için etki alanı adı olmalıdır. **SSLCertificateKeyFile** CSR oluşturduğunuzda oluşturulan anahtar dosyası olmalıdır. **SSLCertificateChainFile** ara sertifika dosyası (varsa) olmalıdır, sertifika yetkilisi tarafından sağlandı

Dosyayı kaydedin ve yapılandırmayı test etme.

```bash
    sudo service httpd configtest
```

Apache yeniden başlatın.

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Ek Apache öneriler

### <a name="additional-headers"></a>Ek üstbilgileri 
Karşı güvenliğini sağlamak için kötü amaçlı saldırıları da değiştirildiğinde veya eklendiğinde birkaç üstbilgileri var. Emin `mod_headers` Modülü yüklüdür.

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>Clickjacking öğesini gelen güvenli Apache
Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir. Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları. Kullanım X-FRAME-sitenizin güvenliğini sağlamak için OPTIONS.

Httpd.conf dosyasını düzenleyin.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Satırı ekleyin `Header append X-FRAME-OPTIONS "SAMEORIGIN"` ve dosyayı kaydedin, sonra da Apache yeniden başlatın.

#### <a name="mime-type-sniffing"></a>MIME türü algılaması

Üstbilgi yanıt içerik türü geçersiz tarayıcıyı yönlendirir gibi bu başlığı MIME algılaması Internet Explorer'dan bildirilen content-type çıktığınızda yanıt engeller. Sunucu metin/html, içeriktir diyorsa nosniff seçeneğiyle tarayıcı, text/html olarak kabul eder.

Httpd.conf dosyasını düzenleyin.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Satırı ekleyin `Header set X-Content-Type-Options "nosniff"` ve dosyayı kaydedin, sonra da Apache yeniden başlatın.

### <a name="load-balancing"></a>YükDengeleme 

Bu örnekte, Kurulum ve Apache CentOS 7 ve Kestrel aynı örneği makinede yapılandırmak gösterilmektedir.  Ancak, tek bir hata; noktası olmaması için kullanarak *mod_proxy_balancer* ve VirtualHost değiştirmeye izin web uygulamaları Apache proxy sunucunun arkasında birden çok örneğini yönetmek için.

```bash
    sudo yum install mod_proxy_balancer
```

Yapılandırma dosyası, ek bir örneği `hellomvc` uygulama 5001 bağlantı noktası üzerinde çalıştırmak için Kurulum açıldı ve *Proxy* bölüm ayarlanmış iki üyeleriyle dengeleyici yapılandırmasına sahip yükünü dengelemek için *byrequests* .

```text
    <VirtualHost *:80>
        RewriteEngine On
        RewriteCond %{HTTPS} !=on
        RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
    </VirtualHost>

    <VirtualHost *:443>
            ProxyPass / balancer://mycluster/ 

            ProxyPassReverse / http://127.0.0.1:5000/
            ProxyPassReverse / http://127.0.0.1:5001/

            <Proxy balancer://mycluster>
                BalancerMember http://127.0.0.1:5000
                BalancerMember http://127.0.0.1:5001 
                ProxySet lbmethod=byrequests
            </Proxy>

            <Location />
                SetHandler balancer
            </Location>
            ErrorLog /var/log/httpd/hellomvc-error.log
            CustomLog /var/log/httpd/hellomvc-access.log common
            SSLEngine on
            SSLProtocol all -SSLv2
            SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
            SSLCertificateFile /etc/pki/tls/certs/localhost.crt
            SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    </VirtualHost>
```

### <a name="rate-limits"></a>Oran sınırları
Kullanarak `mod_ratelimit`, içinde bulunan `htttpd` istemciler, bant genişliği miktarını sınırla modülü. 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
Örnek dosyası kök konumu altında 600 KB/sn olarak bant genişliği sınırlar.

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
