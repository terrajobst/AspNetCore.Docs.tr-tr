---
title: ASP.NET Core Blazor WebAssembly 'ı barındırma ve dağıtma
author: guardrex
description: ASP.NET Core, Içerik teslim ağları (CDN), dosya sunucuları ve GitHub sayfalarını kullanarak bir Blazor uygulamasını nasıl barındırılacağını ve dağıtacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: 748ac9969134f4c89cc8c1235958dcc7ac1d1080
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434284"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor-webassembly"></a>ASP.NET Core Blazor WebAssembly 'ı barındırma ve dağıtma

, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[Blazor WebAssembly barındırma modeliyle](xref:blazor/hosting-models#blazor-webassembly):

* Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir.
* Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür.

Aşağıdaki dağıtım stratejileri desteklenir:

* Blazor uygulama ASP.NET Core bir uygulama tarafından sunulur. Bu strateji [ASP.NET Core bölümünde barındırılan dağıtımda](#hosted-deployment-with-aspnet-core) ele alınmıştır.
* Blazor uygulaması, .NET Blazor uygulamasına hizmet vermek için kullanılmayan bir statik barındırma Web sunucusuna veya hizmetine yerleştirilir. Bu strateji [tek başına dağıtım](#standalone-deployment) bölümünde ele alınmıştır. Bu, bir Blazor WebAssembly UYGULAMASıNı bir IIS alt uygulaması olarak barındırma hakkında bilgiler içerir.

## <a name="rewrite-urls-for-correct-routing"></a>Doğru yönlendirme için URL 'Leri yeniden yazın

Blazor WebAssembly uygulamasındaki sayfa bileşenlerine yönelik yönlendirme istekleri, Blazor sunucusu, barındırılan bir uygulamada yönlendirme istekleri kadar basittir. İki bileşeni olan bir Blazor WebAssembly uygulaması düşünün:

* *Main. razor* &ndash;, uygulamanın köküne yüklenir ve `About` bileşenine (`href="About"`) bir bağlantı içerir.
* *. Razor* &ndash; `About` bileşeni hakkında.

Uygulamanın varsayılan belgesi, tarayıcının adres çubuğu kullanılarak istendiğinde (örneğin, `https://www.contoso.com/`):

1. Tarayıcı bir istek yapar.
1. Varsayılan sayfa döndürülür, bu genellikle *index. html*'dir.
1. uygulamanın *index. html* önyükleme.
1. Blazoryönlendirici yükleri ve Razor `Main` bileşeni işlenir.

Ana sayfada, Blazor yönlendirici tarayıcının `About` için `www.contoso.com` için bir istek yapmasını durdurduğundan ve işlenmiş `About` bileşeninin kendisini hizmet ettiğinden, `About` bileşene olan bağlantıyı seçmek istemcide çalışır. *Blazor WebAssembly uygulaması içindeki* iç uç noktalara yönelik tüm istekler aynı şekilde çalışır: istekler tarayıcı tabanlı istekleri Internet 'teki sunucu tarafından barındırılan kaynaklara tetiklemez. Yönlendirici istekleri dahili olarak işler.

`www.contoso.com/About`için tarayıcının adres çubuğu kullanılarak bir istek yapılırsa, istek başarısız olur. Uygulamanın Internet ana bilgisayarında böyle bir kaynak yok, bu nedenle *404-bulunamayan* bir yanıt döndürülür.

Tarayıcılar, istemci tarafı sayfaları için Internet tabanlı konaklara istek yaptığından, Web sunucuları ve barındırma hizmetleri, fiziksel olarak sunucu üzerinde olmayan kaynakların tüm isteklerini *Dizin. html* sayfasına yeniden yazmalıdır. *İndex. html* döndürüldüğünde, uygulamanın Blazor yönlendiricisi, doğru kaynakla yararlanır ve yanıt verir.

Bir IIS sunucusuna dağıtım yaparken, URL yeniden yazma modülünü uygulamanın yayınlanan *Web. config* dosyasıyla birlikte kullanabilirsiniz. Daha fazla bilgi için [IIS](#iis) bölümüne bakın.

## <a name="hosted-deployment-with-aspnet-core"></a>ASP.NET Core ile barındırılan dağıtım

*Barındırılan bir dağıtım* , bir Web sunucusu üzerinde çalışan bir [ASP.NET Core uygulamasındaki](xref:index) tarayıcılara Blazor webassembly uygulamasını sunar.

İstemci Blazor WebAssembly uygulaması sunucu uygulamasının */BIN/Release/{Target Framework}/Publish/Wwwroot* klasöründe, sunucu uygulamasının diğer statik Web varlıklarıyla birlikte yayımlanır. İki uygulama birlikte dağıtılır. ASP.NET Core uygulamasını barındırabilen bir Web sunucusu gereklidir. Barındırılan bir dağıtım için Visual Studio, **barındırılan** seçenek seçili olan (`dotnet new` komutu kullanılırken`-ho|--hosted`) **Webassembly uygulama** projesi şablonu ( [DotNet New](/dotnet/core/tools/dotnet-new) komutu kullanılırken`blazorwasm` şablonu)Blazor içerir.

Uygulama barındırma ve dağıtım ASP.NET Core hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.

Azure App Service dağıtma hakkında daha fazla bilgi için bkz. <xref:tutorials/publish-to-azure-webapp-using-vs>.

## <a name="standalone-deployment"></a>Tek başına dağıtım

*Tek başına dağıtım* , istemci tarafından doğrudan istenen statik dosyalar kümesi olarak Blazor WebAssembly uygulamasına hizmet verir. Herhangi bir statik dosya sunucusu Blazor uygulamasına sunabilir.

Tek başına dağıtım varlıkları */BIN/Release/{Target Framework}/Publish/Wwwroot* klasöründe yayımlanır.

### <a name="iis"></a>IIS

IIS, Blazor uygulamalar için özellikli bir statik dosya sunucusudur. IIS 'yi Blazorbarındıracak şekilde yapılandırmak için bkz. [IIS 'de statik Web sitesi oluşturma](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Yayımlanan varlıklar */BIN/Release/{Target Framework}/Publish* klasöründe oluşturulur. Web sunucusunda veya barındırma hizmetinde *Yayımlama* klasörünün içeriğini barındırın.

#### <a name="webconfig"></a>Web. config

Bir Blazor projesi yayımlandığında, aşağıdaki IIS yapılandırmasıyla bir *Web. config* dosyası oluşturulur:

* MIME türleri aşağıdaki dosya uzantıları için ayarlanır:
  * *. dll* &ndash; `application/octet-stream`
  * *. json* &ndash; `application/json`
  * *.* , &ndash; `application/wasm`
  * *. WOFF* &ndash; `application/font-woff`
  * *. woff2* &ndash; `application/font-woff`
* Aşağıdaki MIME türleri için HTTP sıkıştırması etkindir:
  * `application/octet-stream`
  * `application/wasm`
* URL yeniden yazma modülü kuralları oluşturuldu:
  * Uygulamanın statik varlıklarının bulunduğu alt dizini ( *{Assembly Name}/dist/{PATH istenen}* ) sunar.
  * Dosya olmayan varlıklar için isteklerin statik varlıklar klasöründe ( *{Assembly Name}/Dist/index.html*) uygulamanın varsayılan belgesine yeniden YÖNLENDIRILMESI için Spa geri dönüş yönlendirmesi oluşturun.

#### <a name="install-the-url-rewrite-module"></a>URL yeniden yazma modülünü yükler

[URL yeniden yazma modülü](https://www.iis.net/downloads/microsoft/url-rewrite) , URL 'leri yeniden yazmak için gereklidir. Modül varsayılan olarak yüklenmez ve bir Web sunucusu (IIS) rol hizmeti özelliği olarak yükleme için kullanılamaz. Modül IIS Web sitesinden indirilmelidir. Modülünü yüklemek için Web Platformu Yükleyicisi 'ni kullanın:

1. Yerel olarak, [URL yeniden yazma modülü İndirmeleri sayfasına](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)gidin. Ingilizce sürümünde, WebPI yükleyicisini indirmek için **WebPI** ' ı seçin. Diğer diller için, yükleyiciyi indirmek üzere sunucu için uygun mimariyi (x86/x64) seçin.
1. Yükleyiciyi sunucuya kopyalayın. Yükleyiciyi çalıştırın. , **Install** düğmesini seçin ve lisans koşullarını kabul edin. Yüklemesi tamamlandıktan sonra sunucu yeniden başlatması gerekli değildir.

#### <a name="configure-the-website"></a>Web sitesini yapılandırma

Web sitesinin **fiziksel yolunu** uygulamanın klasörüne ayarlayın. Klasör şunları içerir:

* Gerekli yeniden yönlendirme kuralları ve dosya içerik türleri dahil olmak üzere IIS 'nin Web sitesini yapılandırmak için kullandığı *Web. config* dosyası.
* Uygulamanın statik varlık klasörü.

#### <a name="host-as-an-iis-sub-app"></a>IIS alt uygulaması olarak barındırma

Tek başına bir uygulama bir IIS alt uygulaması olarak barındırılıyorsa, aşağıdakilerden birini yapın:

* Devralınan ASP.NET Core modülü işleyicisini devre dışı bırakın.

  Dosyaya bir `<handlers>` bölümü ekleyerek Blazor uygulamasının yayınlanan *Web. config* dosyasındaki işleyiciyi kaldırın:

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* `inheritInChildApplications` `false`olarak ayarlanan `<location>` bir öğesi kullanarak kök (üst) uygulamanın `<system.webServer>` bölümünü devralmayı devre dışı bırakın:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <handlers>
          <add name="aspNetCore" ... />
        </handlers>
        <aspNetCore ... />
      </system.webServer>
    </location>
  </configuration>
  ```

İşleyicinin kaldırılması veya devralma devre dışı bırakılması, [uygulamanın temel yolunun yapılandırılmasına](xref:host-and-deploy/blazor/index#app-base-path)ek olarak gerçekleştirilir. Uygulamanın *index. html* dosyasındaki uygulama temel yolunu, IIS 'de alt uygulamayı YAPıLANDıRıRKEN kullanılan IIS diğer adına ayarlayın.

#### <a name="troubleshooting"></a>Sorun giderme

*500-Iç sunucu hatası* ALıNMıŞSA ve IIS Yöneticisi Web sitesinin yapılandırmasına erişmeye çalışırken hatalar OLUŞTURURSA, URL yeniden yazma modülünün yüklü olduğunu doğrulayın. Modül yüklü olmadığında, *Web. config* dosyası IIS tarafından ayrıştırılamaz. Bu, IIS yöneticisinin Web sitesinin yapılandırmasını ve Web sitesinin Blazorstatik dosyalarına hizmet etmesini engeller.

IIS ile dağıtım sorunlarını giderme hakkında daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.

### <a name="azure-storage"></a>Azure Depolama

[Azure depolama](/azure/storage/) statik dosya barındırma, sunucusuz Blazor uygulama barındırılmasına olanak sağlar. Özel etki alanı adları, Azure Content Delivery Network (CDN) ve HTTPS desteklenir.

Blob hizmeti bir depolama hesabında barındırılan statik Web sitesi için etkinleştirildiğinde:

* **Dizin belgesi adını** `index.html`olarak ayarlayın.
* **Hata belge yolunu** `index.html`olarak ayarlayın. Razor bileşenleri ve diğer dosya olmayan uç noktaları, blob hizmeti tarafından depolanan statik içerikte fiziksel yollarda yer vermez. Blazor yönlendiricisinin işlemesi gereken bu kaynaklardan birine yönelik bir istek alındığında, blob hizmeti tarafından oluşturulan *404-bulunamayan* hata, isteği **hata belge yoluna**yönlendirir. *İndex. html* blobu döndürülür ve Blazor yönlendirici yolu yükler ve işler.

Daha fazla bilgi için bkz. [Azure Storage 'Da statik Web sitesi barındırma](/azure/storage/blobs/storage-blob-static-website).

### <a name="nginx"></a>Nginx

Aşağıdaki *NGINX. conf* dosyası, NGINX 'in, disk üzerinde karşılık gelen bir dosyayı bulamadığı her seferinde *index. html* dosyasını göndermek üzere nasıl yapılandırılacağını gösterecek şekilde basitleştirilmiştir.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

Üretim NGINX web sunucusu yapılandırması hakkında daha fazla bilgi için bkz. [NGINX Plus ve NGINX yapılandırma dosyaları oluşturma](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).

### <a name="nginx-in-docker"></a>Docker 'da NGINX

Docker 'da NGINX kullanarak Blazor barındırmak için Dockerfile 'ı alp tabanlı NGINX görüntüsünü kullanacak şekilde ayarlayın. Dockerfile dosyasını, *NGINX. config* dosyasını kapsayıcıya kopyalamak için güncelleştirin.

Aşağıdaki örnekte gösterildiği gibi Dockerfile dosyasına bir satır ekleyin:

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a>Apache

CentOS 7 veya üzeri bir Blazor WebAssembly uygulaması dağıtmak için:

1. Apache yapılandırma dosyasını oluşturun. Aşağıdaki örnek basitleştirilmiş bir yapılandırma dosyasıdır (*blazorapp. config*):

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType application/wasm .wasm
       AddType application/octet-stream .dll
   
       <Directory "/var/www/blazorapp">
           Options -Indexes
           AllowOverride None
       </Directory>

       <IfModule mod_deflate.c>
           AddOutputFilterByType DEFLATE text/css
           AddOutputFilterByType DEFLATE application/javascript
           AddOutputFilterByType DEFLATE text/html
           AddOutputFilterByType DEFLATE application/octet-stream
           AddOutputFilterByType DEFLATE application/wasm
           <IfModule mod_setenvif.c>
           BrowserMatch ^Mozilla/4 gzip-only-text/html
           BrowserMatch ^Mozilla/4.0[678] no-gzip
           BrowserMatch bMSIE !no-gzip !gzip-only-text/html
       </IfModule>
       </IfModule>

       ErrorLog /var/log/httpd/blazorapp-error.log
       CustomLog /var/log/httpd/blazorapp-access.log common
   </VirtualHost>
   ```

1. Apache yapılandırma dosyasını, CentOS 7 ' de varsayılan Apache yapılandırma dizini olan `/etc/httpd/conf.d/` dizinine yerleştirin.

1. Uygulamanın dosyalarını `/var/www/blazorapp` dizinine yerleştirin (yapılandırma dosyasına `DocumentRoot` için belirtilen konum).

1. Apache hizmetini yeniden başlatın.

Daha fazla bilgi için bkz. [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) ve [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).

### <a name="github-pages"></a>GitHub sayfaları

URL yeniden işlemesini işlemek için, isteği *index. html* sayfasına yönlendirmeyi işleyen bir betiği olan bir *404. html* dosyası ekleyin. Topluluk tarafından sunulan örnek bir uygulama için bkz. [GitHub sayfaları Için tek sayfalı uygulamalar](https://spa-github-pages.rafrex.com/) ([GitHub üzerinde rafrex/Spa-GitHub-Pages](https://github.com/rafrex/spa-github-pages#readme)). Topluluk yaklaşımını kullanan bir örnek, GitHub ([canlı site](https://blazor-demo.github.io/)) [üzerinde blazor-demo/blazor-demo. GitHub. IO](https://github.com/blazor-demo/blazor-demo.github.io) adresinde görülebilir.

Bir kuruluş sitesi yerine bir proje sitesi kullanırken, *index. html*' de `<base>` etiketini ekleyin veya güncelleştirin. `href` öznitelik değerini, sondaki eğik çizgiyle (örneğin, `my-repository/`) GitHub depo adına ayarlayın.

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[Blazor WebAssembly Apps](xref:blazor/hosting-models#blazor-webassembly) , geliştirme ortamındaki çalışma zamanında aşağıdaki ana bilgisayar yapılandırma değerlerini komut satırı bağımsız değişkenleri olarak kabul edebilir.

### <a name="content-root"></a>İçerik kökü

`--contentroot` bağımsız değişkeni, uygulamanın içerik dosyalarını ([içerik kökü](xref:fundamentals/index#content-root)) içeren dizinin mutlak yolunu ayarlar. Aşağıdaki örneklerde, uygulamanın içerik kök yolu `/content-root-path`.

* Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin. Uygulamanın dizininden şunu yürütün:

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* **IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin. Bu ayar, uygulama Visual Studio hata ayıklayıcıyla ve `dotnet run`ile bir komut isteminden çalıştırıldığında kullanılır.

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* Visual Studio 'da, **özellikler** > **hata ayıklama** > **uygulama bağımsız değişkenleri**' nde bağımsız değişkeni belirtin. Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>Yol tabanı

`--pathbase` bağımsız değişkeni, kök olmayan göreli URL yoluyla yerel olarak çalıştırılan bir uygulamanın uygulama temel yolunu ayarlar (`<base>` etiketi `href`, hazırlama ve üretim için `/` dışında bir yola ayarlanır). Aşağıdaki örneklerde, uygulamanın yol tabanı `/relative-URL-path`. Daha fazla bilgi için bkz. [uygulama temel yolu](xref:host-and-deploy/blazor/index#app-base-path).

> [!IMPORTANT]
> `<base>` etiketinin `href` olarak belirtilen yolun aksine, `--pathbase` bağımsız değişken değeri geçirilirken sondaki eğik çizgi (`/`) eklemeyin. Uygulama temel yolu, `<base>` etiketinde `<base href="/CoolApp/">` (sondaki eğik çizgi içeriyorsa) olarak sağlanmışsa, komut satırı bağımsız değişken değerini `--pathbase=/CoolApp` (sonunda eğik çizgi yok) olarak geçirin.

* Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin. Uygulamanın dizininden şunu yürütün:

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* **IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin. Bu ayar, uygulamayı Visual Studio hata ayıklayıcıyla çalıştırırken ve `dotnet run`ile bir komut isteminden kullanılır.

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* Visual Studio 'da, **özellikler** > **hata ayıklama** > **uygulama bağımsız değişkenleri**' nde bağımsız değişkeni belirtin. Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a>URL'ler

`--urls` bağımsız değişkeni, istekler için dinlemek üzere bağlantı noktaları ve protokollerle IP adreslerini veya konak adreslerini ayarlar.

* Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin. Uygulamanın dizininden şunu yürütün:

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* **IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin. Bu ayar, uygulamayı Visual Studio hata ayıklayıcıyla çalıştırırken ve `dotnet run`ile bir komut isteminden kullanılır.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* Visual Studio 'da, **özellikler** > **hata ayıklama** > **uygulama bağımsız değişkenleri**' nde bağımsız değişkeni belirtin. Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a>Bağlayıcıyı yapılandırma

Blazor, çıkış derlemelerinden gereksiz Il 'yi kaldırmak için her sürüm derlemesinde ara dil (IL) bağlamayı gerçekleştirir. Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/configure-linker>.
