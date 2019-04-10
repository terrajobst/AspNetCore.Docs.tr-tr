---
title: Barındırma ve Blazor dağıtma
author: guardrex
description: Ana bilgisayar ve ASP.NET Core, içerik teslim ağları (CDN), dosya sunucuları ve GitHub sayfaları kullanarak Blazor uygulamasını dağıtma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/blazor
ms.openlocfilehash: 1bf929ed37713f62511447524c8c47be6177f4c9
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468769"
---
# <a name="host-and-deploy-blazor"></a>Barındırma ve Blazor dağıtma

Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

Kullanan uygulamalar Blazor [istemci-tarafı barındırma modeli](xref:razor-components/hosting-models#client-side-hosting-model) geliştirme ortamında çalışma zamanında komut satırı bağımsız değişkenleri olarak aşağıdaki ana bilgisayar yapılandırma değerlerini kabul edebilir.

### <a name="content-root"></a>İçerik kök

`--contentroot` Bağımsız değişkeni, uygulamanın içerik dosyaları içeren dizine mutlak yolunu ayarlar. Aşağıdaki örneklerde, `/content-root-path` uygulamanın içerik kök yolu.

* Bağımsız değişken, uygulamayı bir komut isteminde yerel olarak çalıştırılırken geçirin. Uygulamanın dizinden yürütün:

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* Uygulamanın bir giriş ekleyin *launchSettings.json* dosyası **IIS Express** profili. Bu ayar ile bir komut isteminden Visual Studio hata ayıklayıcı ile uygulama çalıştırıldığında kullanılan `dotnet run`.

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* Visual Studio'da bağımsız değişkende belirtin **özellikleri** > **hata ayıklama** > **uygulamanın bağımsız değişkenleri**. Ekler için bağımsız değişken bağımsız değişken Visual Studio özellik sayfasında ayarlama *launchSettings.json* dosya.

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>Temel yol

`--pathbase` Bağımsız değişken ayarlar kök olmayan sanal yol ile yerel olarak çalıştırmak için bir uygulama için uygulama temel yolu ( `<base>` etiketi `href` yolu dışında ayarlı `/` hazırlama ve üretim için). Aşağıdaki örneklerde, `/virtual-path` uygulamanın temel yoludur. Daha fazla bilgi için [uygulama temel yolu](#app-base-path) bölümü.

> [!IMPORTANT]
> Sağlanan yol aksine `href` , `<base>` etiketinde, sonunda bir eğik çizgi eklemeyin (`/`) geçerken `--pathbase` bağımsız değişken değeri. Uygulama temel yolu sağlanmazsa `<base>` olarak etiketleyin `<base href="/CoolApp/">` (sonunda eğik çizgi içerir), komut satırı bağımsız değişkeni değer olarak geçirmeniz `--pathbase=/CoolApp` (Bitiş eğik).

* Bağımsız değişken, uygulamayı bir komut isteminde yerel olarak çalıştırılırken geçirin. Uygulamanın dizinden yürütün:

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* Uygulamanın bir giriş ekleyin *launchSettings.json* dosyası **IIS Express** profili. Bu ayar, uygulama ile bir komut isteminden Visual Studio hata ayıklayıcı ile çalıştırırken kullanılır `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* Visual Studio'da bağımsız değişkende belirtin **özellikleri** > **hata ayıklama** > **uygulamanın bağımsız değişkenleri**. Ekler için bağımsız değişken bağımsız değişken Visual Studio özellik sayfasında ayarlama *launchSettings.json* dosya.

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a>URL'ler

`--urls` IP adresi veya konak adresleriyle bağlantı noktaları ve protokoller üzerinde isteklerini dinlemek için bağımsız değişken ayarlar.

* Bağımsız değişken, uygulamayı bir komut isteminde yerel olarak çalıştırılırken geçirin. Uygulamanın dizinden yürütün:

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* Uygulamanın bir giriş ekleyin *launchSettings.json* dosyası **IIS Express** profili. Bu ayar, uygulama ile bir komut isteminden Visual Studio hata ayıklayıcı ile çalıştırırken kullanılır `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* Visual Studio'da bağımsız değişkende belirtin **özellikleri** > **hata ayıklama** > **uygulamanın bağımsız değişkenleri**. Ekler için bağımsız değişken bağımsız değişken Visual Studio özellik sayfasında ayarlama *launchSettings.json* dosya.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a>Dağıtım

İle [istemci-tarafı barındırma modeli](xref:razor-components/hosting-models#client-side-hosting-model):

* Tarayıcıya .NET çalışma zamanı Blazor uygulamayı ve bağımlılıkları indirilir.
* Uygulamayı doğrudan tarayıcıda kullanıcı Arabirimi iş parçacığında yürütülür. Aşağıdaki stratejilerin birini ya da desteklenir:
  * Blazor uygulama, ASP.NET Core uygulaması tarafından sunulur. Bu strateji ele alınmıştır [barındırılan ASP.NET Core ile dağıtım](#hosted-deployment-with-aspnet-core) bölümü.
  * Blazor uygulama bir statik barındırma web sunucusu veya hizmeti .NET Blazor uygulama sunmak için burada kullanılmayan yerleştirilir. Bu strateji ele alınmıştır [tek başına dağıtımda](#standalone-deployment) bölümü.

## <a name="configure-the-linker"></a>Bağlayıcıyı yapılandırma

Gereksiz IL çıkış derlemeleri kaldırmak için her derlemede Ara dil (IL) bağlama Blazor gerçekleştirir. Derleme bağlama üzerinde derleme denetlenebilir. Daha fazla bilgi için bkz. <xref:host-and-deploy/razor-components-blazor/configure-linker>.

## <a name="rewrite-urls-for-correct-routing"></a>Doğru yönlendirme URL yeniden yazma

İstekler sayfası bileşenler için bir istemci-tarafı uygulaması yönlendirme yönlendirme isteklerini bir sunucu tarafı, barındırılan uygulama kadar basit değildir. İki sayfa ile bir istemci-tarafı uygulaması göz önünde bulundurun:

* **_Main.cshtml_**  &ndash; uygulama köküne yükler ve hakkında sayfanın bağlantısını içeren (`href="About"`).
* **_About.cshtml_**  &ndash; hakkında sayfası.

Tarayıcı Adres çubuğunun kullanarak uygulamanın varsayılan belge istendiğinde (örneğin, `https://www.contoso.com/`):

1. Tarayıcı, bir istek gönderir.
1. Varsayılan sayfayı döndürülen, genellikle olduğu *index.html*.
1. *index.HTML* uygulama bootstraps.
1. Blazor'ın yönlendirici yükler ve Razor ana sayfa (*Main.cshtml*) görüntülenir.

Ana sayfada hakkında sayfasının bağlantısını seçme hakkında sayfası yükler. Hakkında sayfasının bağlantısını seçerek çalışan istemcide Internet üzerindeki bir isteği yapan tarayıcının Blazor yönlendirici durdurulur çünkü `www.contoso.com` için `About` ve hakkında sayfası görevi görür. Tüm istekler için iç sayfaları *istemci-tarafı uygulaması içinde* aynı şekilde çalışır: İstek sunucu tarafından barındırılan kaynaklara İnternet'e tarayıcı tabanlı istekleri tetikleyin yok. Yönlendirici isteklerini dahili olarak işler.

Bir istek için tarayıcının adres çubuğuna kullanarak denenirse `www.contoso.com/About`, istek başarısız olur. Böyle bir kaynak uygulamanın Internet konakta, bu nedenle mevcut bir *404 - Bulunamadı* yanıt döndürülür.

Tarayıcılar istemci-tarafı sayfaları için Internet tabanlı konakların isteklerde bulunmak için web sunucuları ve barındırma Hizmetleri sunucusuna fiziksel olarak şirket kaynakları için tüm istekleri yeniden yazmalısınız *index.html* sayfası. Zaman *index.html* döndürülür, uygulamanın istemci-tarafı yönlendirici alır ve doğru kaynak ile yanıt verir.

## <a name="app-base-path"></a>Uygulama temel yolu

*Uygulama temel yolu* sunucusundaki sanal uygulama kök yolu. Örneğin, Contoso sunucusunda bir sanal klasöründeki bulunan uygulama `/CoolApp/` en üst sınırına `https://www.contoso.com/CoolApp` ve sanal bir temel yolu `/CoolApp/`. Uygulama temel yolu ayarlayarak `CoolApp/`, uygulamayı kullanan sanal sunucu üzerinde bulunduğu yapılır. Uygulama, uygulama temel yolu kök dizininde değil bir bileşeni uygulama köküne URL'lerini oluşturmak için kullanabilirsiniz. Bu uygulamada konumlarda diğer kaynakların bağlantılarını oluşturmak için dizin yapısının farklı düzeylerde mevcut bileşenleri sağlar. Uygulama temel yolu da ele alınması için kullanılan köprüyü tıklattığında nerede `href` bağlantının hedefi olan uygulama temel yolu URI alanı içinde&mdash;iç Gezinti Blazor yönlendirici işler.

Birçok barındırma senaryolarında sunucu uygulamasının sanal yolu uygulamayı köküdür. Bu gibi durumlarda, uygulama temel yolu bir eğik çizgi olan (`<base href="/" />`), bir uygulama için varsayılan yapılandırma olduğu. GitHub sayfaları ve IIS sanal dizinler veya alt uygulamalar gibi diğer barındırma senaryolarında, uygulama temel yolu sunucu uygulamasının sanal yolu için ayarlanması gerekir. Uygulamanın temel yolunu ayarlamak için ekleme veya güncelleştirme `<base>` içindeki *index.html* içinde bulunan `<head>` öğeleri etiketleyin. Ayarlayın `href` öznitelik değerine `virtual-path/` (sonunda eğik çizgi gereklidir), burada `virtual-path/` uygulama için sunucuda tam sanal uygulama kök yolu. Önceki örnekte, sanal yol kümesi `CoolApp/`: `<base href="CoolApp/">`.

Yapılandırılmış bir kök olmayan sanal yol ile bir uygulama için (örneğin, `<base href="CoolApp/">`), kaynaklarını bulmak uygulamanın başarısız *yerel olarak çalıştırdığınızda*. Yerel geliştirme ve test sırasında bu sorunu çözmek için size sağlayabilir bir *yolu tabanı* eşleşen bağımsız değişken `href` değerini `<base>` zamanında etiketi.

Yol, kök yolu ile temel bağımsız değişken geçmek için (`/`) uygulamayı yerel olarak çalıştırırken, uygulamanın dizininden aşağıdaki komutu yürütün:

```console
dotnet run --pathbase=/CoolApp
```

Uygulama yanıt veren yerel olarak adresindeki `http://localhost:port/CoolApp`.

Daha fazla bilgi için üzerinde bölümüne bakın. [yolu temel konak yapılandırma değeri](#path-base).

Bir uygulama kullanıyorsa [istemci-tarafı barındırma modeli](xref:razor-components/hosting-models#client-side-hosting-model) (temel **Blazor** proje şablonu) ve barındırılan bir IIS alt uygulamada ASP.NET Core uygulaması, bu devralınan ASP.NET Core devre dışı bırakmanız önemlidir Modül işleyicisi veya kök (üst) uygulamanın emin `<handlers>` konusundaki *web.config* dosya alt uygulama tarafından devralınan değil.

Uygulama işleyicisinde yayımlanan Kaldır *web.config* dosyasını ekleyerek bir `<handlers>` dosya bölümüne:

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

Alternatif olarak, kök (üst) uygulamanın devralma devre dışı `<system.webServer>` kullanma bölümünde bir `<location>` öğeyle `inheritInChildApplications` kümesine `false`:

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

İşleyici kaldırma veya devralmayı devre dışı bırakarak, uygulamanın taban yolu yapılandırmaya ek olarak, bu bölümde açıklanan şekilde gerçekleştirilir. Uygulamanın uygulama temel yolu ayarla *index.html* IIS diğer IIS alt uygulama yapılandırma sırasında kullanılan dosya.

## <a name="hosted-deployment-with-aspnet-core"></a>ASP.NET Core ile barındırılan dağıtım

A *dağıtım barındırılan* tarayıcılardan istemci-tarafı Blazor uygulama yarar bir [ASP.NET Core uygulaması](xref:index) bir sunucuda çalışır.

Böylece iki uygulamanın birlikte dağıtılan Blazor uygulama yayımlanan çıktıda ASP.NET Core uygulaması ile birlikte gelir. ASP.NET Core uygulaması barındırma yeteneğine sahip bir web sunucusu gereklidir. Barındırılan dağıtım için Visual Studio içerir **Blazor (barındırılan ASP.NET Core)** proje şablonu (`blazorhosted` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).

ASP.NET Core uygulaması barındırma ve dağıtma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.

Azure App Service'e dağıtma hakkında daha fazla bilgi için bkz. <xref:tutorials/publish-to-azure-webapp-using-vs>.

## <a name="standalone-deployment"></a>Tek başına dağıtım

A *tek başına dağıtımda* doğrudan istemcileri tarafından istenen statik dosyalar bir dizi istemci-tarafı Blazor uygulama görür. Bir web sunucusu Blazor uygulama sunmak için kullanılmaz.

### <a name="iis"></a>IIS

IIS Blazor uygulamalar için bir statik özellikli dosya sunucusudur. Konağa Blazor IIS'yi yapılandırmak için bkz: [IIS üzerinde statik Web sitesi oluşturma](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Yayımlanmış varlık oluşturulan */bin/yayın / {hedef çerçeve} / publish* klasör. İçeriğini barındıran *yayımlama* web sunucusuna ya da barındırma hizmeti klasörü.

#### <a name="webconfig"></a>Web.config

Blazor proje yayımlandığında bir *web.config* dosyası, aşağıdaki IIS yapılandırma ile oluşturulur:

* MIME türleri, aşağıdaki dosya uzantıları için ayarlanır:
  * *.dll* &ndash; `application/octet-stream`
  * *.json* &ndash; `application/json`
  * *.wasm* &ndash; `application/wasm`
  * *.woff* &ndash; `application/font-woff`
  * *.woff2* &ndash; `application/font-woff`
* HTTP sıkıştırmasını aşağıdaki MIME türleri için etkinleştirilir:
  * `application/octet-stream`
  * `application/wasm`
* URL yeniden yazma modülü kuralları oluşturulur:
  * Uygulamanın statik varlıklar bulunduğu alt dizini hizmet (*{derleme adı} /dist/ {yolu İSTENEN}*).
  * Böylece uygulamanın varsayılan belge, statik varlıklar klasöründeki dosya olmayan varlıklar için istekleri yönlendirilirsiniz SPA geri dönüş yönlendirme oluşturma (*{derleme NAME}/dist/index.html*).

#### <a name="install-the-url-rewrite-module"></a>URL yeniden yazma modülünü yükleme

[URL yeniden yazma Modülü](https://www.iis.net/downloads/microsoft/url-rewrite) URL yeniden yazma için gereklidir. Modül varsayılan olarak yüklü değildir ve bir Web sunucusu (IIS) rolü hizmeti özellik olarak yüklenmesi için kullanılamaz. Modül IIS Web sitesinden yüklenmesi gerekir. Modülü yüklemek için Web Platformu Yükleyicisi'ni kullanın:

1. Yerel olarak gidin [URL yeniden yazma modülü indirmeler sayfasına](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). İngilizce sürümü için **Webpı** Webpı yükleyicisi indirilemedi. Diğer diller için uygun mimarinin yükleyiciyi indirmek sunucu (x86/x64) seçin.
1. Yükleyici, sunucuya kopyalayın. Yükleyiciyi çalıştırın. Seçin **yükleme** düğmesi ve lisans koşullarını kabul edin. Yükleme tamamlandıktan sonra sunucunun yeniden başlatılması gerekli değildir.

#### <a name="configure-the-website"></a>Web sitesi yapılandırma

Web sitesinin ayarlamak **fiziksel yolu** uygulamanın klasörüne. Klasör içerir:

* *Web.config* dosyasını gerekli bir yeniden yönlendirme kuralları dahil olmak üzere Web sitesi yapılandırma ve dosya içerik türleri için IIS kullanır.
* Uygulamanın statik varlık klasör.

#### <a name="troubleshooting"></a>Sorun giderme

Varsa bir *500 - İç sunucu hatası* alınır ve IIS Yöneticisi'ni çalışırken Web sitenizin yapılandırmasına erişim, URL yeniden yazma Modülü yüklü olduğundan emin olmak hataları oluşturur. Modül yüklenmediğinde *web.config* IIS tarafından dosya ayrıştırılamıyor. Bu IIS Yöneticisi'ni Web sitesinin yapılandırması ve Web sitesi Blazor'ın statik dosyalar hizmetinden yüklenmesini engeller.

IIS dağıtım sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/troubleshoot>.

### <a name="nginx"></a>Nginx

Aşağıdaki *nginx.conf* dosya göndermek için Ngınx yapılandırma gösterecek şekilde basitleştirilir *index.html* karşılık gelen bir dosya diskte bulunamıyor her dosya.

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

Üretim Ngınx web sunucusu yapılandırma hakkında daha fazla bilgi için bkz. [oluşturma NGINX Plus ve NGINX yapılandırma dosyalarını](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).

### <a name="nginx-in-docker"></a>Docker'da Nginx

Nginx kullanarak docker'da Blazor barındırmak için Dockerfile, Alpine tabanlı Nginx görüntüsü kullanmak için ayarlayın. Dockerfile kopyalayın güncelleştirme *nginx.config* kapsayıcı içine bir dosya.

Aşağıdaki örnekte gösterildiği gibi bir satırı Dockerfile içine ekleyin:

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a>GitHub sayfaları

URL yeniden yazma işlemleri işlemek için ekleme bir *404. html* yeniden yönlendirme isteği işleyen bir komut dosyasıyla *index.html* sayfası. Topluluk tarafından sağlanan bir örnek için bkz: [GitHub sayfaları için tek sayfa uygulamaları](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-sayfaları github'da](https://github.com/rafrex/spa-github-pages#readme)). Topluluk yaklaşımı kullanarak bir örnek, görülebilir [github'da blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([Canlı site](https://blazor-demo.github.io/)).

Proje sitesi yerine bir kuruluş site kullanırken, ekleme veya güncelleştirme `<base>` içindeki *index.html*. Ayarlama `href` sonunda eğik çizgi ile GitHub deposu adı için öznitelik değeri (örneğin, `my-repository/`.
