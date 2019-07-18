---
title: Konak ve dağıtım ASP.NET Core Blazor istemci tarafı
author: guardrex
description: ASP.NET Core, Içerik teslim ağları (CDN), dosya sunucuları ve GitHub sayfalarını kullanarak bir Blazor uygulamasını nasıl barındırılacağını ve dağıtacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: be6b6c245440cb085a1a6b115f4f087306f7cc83
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308082"
---
# <a name="host-and-deploy-aspnet-core-blazor-client-side"></a>Konak ve dağıtım ASP.NET Core Blazor istemci tarafı

, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[İstemci tarafı barındırma modelini](xref:blazor/hosting-models#client-side) kullanan Blazor uygulamalar, geliştirme ortamındaki çalışma zamanında aşağıdaki ana bilgisayar yapılandırma değerlerini komut satırı bağımsız değişkenleri olarak kabul edebilir.

### <a name="content-root"></a>İçerik kökü

`--contentroot` Bağımsız değişkeni, uygulamanın içerik dosyalarını içeren dizinin mutlak yolunu ayarlar. Aşağıdaki örneklerde, `/content-root-path` uygulamanın içerik kök yolu bulunur.

* Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin. Uygulamanın dizininden şunu yürütün:

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* **IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin. Bu ayar, uygulama Visual Studio hata ayıklayıcısı ve ile `dotnet run`bir komut isteminden çalıştırıldığında kullanılır.

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* Visual Studio 'da, **Özellikler** > **hata ayıklama** > **uygulama bağımsız değişkenlerinde**bağımsız değişkenini belirtin. Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>Yol tabanı

Bağımsız değişkeni, kök olmayan bir sanal yol ile yerel olarak çalıştırılan bir uygulamanın uygulama temeli yolunu ayarlar `<base>` (etiket `href` , hazırlama ve üretim `/` için dışında bir yola ayarlanır). `--pathbase` Aşağıdaki örneklerde, `/virtual-path` uygulamanın yol tabanı bulunur. Daha fazla bilgi için [uygulama temel yolu](#app-base-path) bölümüne bakın.

> [!IMPORTANT]
> Etiketinde belirtilen `href` `/`yolun aksine, `--pathbase` bağımsız değişken değeri geçirilirken sondaki eğik çizgi () eklemeyin. `<base>` `<base>` Etikette uygulama temel yolu (sondaki eğik çizgi içeriyorsa) olarak `<base href="/CoolApp/">` sağlanmışsa, komut satırı bağımsız değişken değerini (sondaki eğik çizgi yok) olarak `--pathbase=/CoolApp` geçirin.

* Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin. Uygulamanın dizininden şunu yürütün:

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* **IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin. Bu ayar, uygulamayı Visual Studio hata ayıklayıcıyla ve ile `dotnet run`bir komut isteminden çalıştırırken kullanılır.

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* Visual Studio 'da, **Özellikler** > **hata ayıklama** > **uygulama bağımsız değişkenlerinde**bağımsız değişkenini belirtin. Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a>URL'ler

`--urls` Bağımsız değişkeni, istekler için dinlemek üzere bağlantı noktaları ve protokollerle IP adreslerini veya konak adreslerini ayarlar.

* Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin. Uygulamanın dizininden şunu yürütün:

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* **IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin. Bu ayar, uygulamayı Visual Studio hata ayıklayıcıyla ve ile `dotnet run`bir komut isteminden çalıştırırken kullanılır.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* Visual Studio 'da, **Özellikler** > **hata ayıklama** > **uygulama bağımsız değişkenlerinde**bağımsız değişkenini belirtin. Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a>Dağıtım

[İstemci tarafı barındırma modeliyle](xref:blazor/hosting-models#client-side):

* Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir.
* Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür. Aşağıdaki stratejilerden biri desteklenir:
  * Blazor uygulaması, bir ASP.NET Core uygulaması tarafından sunulur. Bu strateji [ASP.NET Core bölümünde barındırılan dağıtımda](#hosted-deployment-with-aspnet-core) ele alınmıştır.
  * Blazor uygulaması, .NET, Blazor uygulamasına hizmet vermek için kullanılmayan bir statik barındırma Web sunucusuna veya hizmetine yerleştirilir. Bu strateji, [tek başına dağıtım](#standalone-deployment) bölümünde ele alınmıştır.

## <a name="configure-the-linker"></a>Bağlayıcıyı yapılandırma

Blazor, çıkış derlemelerinden gereksiz Il 'yi kaldırmak için her bir derlemede ara dil (IL) bağlamayı gerçekleştirir. Derleme bağlama, derleme üzerinde denetlenebilir. Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/configure-linker>.

## <a name="rewrite-urls-for-correct-routing"></a>Doğru yönlendirme için URL 'Leri yeniden yazın

İstemci tarafı uygulamadaki sayfa bileşenlerine yönelik yönlendirme istekleri, sunucu tarafı barındırılan bir uygulamaya yönlendirme istekleri kadar basit değildir. İki bileşeni olan bir istemci tarafı uygulamayı göz önünde bulundurun:

* *Main. Razor* &ndash; , uygulamanın köküne yüklenir ve `About` bileşene (`href="About"`) bir bağlantı içerir.
* *.* &ndash; Razor`About` bileşeni hakkında.

Uygulamanın varsayılan belgesi, tarayıcının adres çubuğu kullanılarak istendiğinde (örneğin, `https://www.contoso.com/`):

1. Tarayıcı bir istek yapar.
1. Varsayılan sayfa döndürülür, bu genellikle *index. html*'dir.
1. uygulamanın *index. html* önyükleme.
1. Blazor 'in yönlendirici yükleri ve Razor `Main` bileşeni işlenir.

Ana sayfada `About` , Blazor yönlendiricisi tarayıcıyı `www.contoso.com` Internet `About` 'te için bir istek yapmadan ve işlenmiş `About` bileşenin kendisini görecek olduğundan, bileşen bağlantısını seçtiğinizde istemci üzerinde çalışır. *İstemci tarafı uygulama içindeki* iç uç noktalara yönelik tüm istekler aynı şekilde çalışır: İstekler tarayıcı tabanlı istekleri Internet 'teki sunucu tarafından barındırılan kaynaklara tetiklemez. Yönlendirici istekleri dahili olarak işler.

Tarayıcının adres çubuğu `www.contoso.com/About`kullanılarak bir istek yapılırsa, istek başarısız olur. Uygulamanın Internet ana bilgisayarında böyle bir kaynak yok, bu nedenle *404-bulunamayan* bir yanıt döndürülür.

Tarayıcılar, istemci tarafı sayfaları için Internet tabanlı konaklara istek yaptığından, Web sunucuları ve barındırma hizmetleri, fiziksel olarak sunucu üzerinde olmayan kaynakların tüm isteklerini *Dizin. html* sayfasına yeniden yazmalıdır. *İndex. html* döndürüldüğünde, uygulamanın istemci tarafı yönlendiricisi, doğru kaynakla sürer ve yanıt verir.

## <a name="app-base-path"></a>Uygulama temel yolu

*Uygulama temel yolu* , sunucusundaki sanal uygulama kök yoludur. Örneğin, ' de bir sanal klasördeki `/CoolApp/` `https://www.contoso.com/CoolApp` contoso sunucusunda bulunan bir uygulamaya, ve sanal temel yolu `/CoolApp/`vardır. Uygulama taban yolunu sanal yola (`<base href="/CoolApp/">`) ayarlayarak, uygulama sunucuda neredeyse bulunduğu yerden haberdar olur. Uygulama, kök dizinde olmayan bir bileşenden uygulama köküne göre URL 'Ler oluşturmak için uygulama temel yolunu kullanabilir. Bu, farklı düzeylerde dizin yapısında bulunan bileşenlerin, uygulama genelindeki konumlarda diğer kaynakların bağlantılarını oluşturmasına izin verir. Uygulama temel yolu Ayrıca, bağlantının `href` hedefinin uygulama temel yolu URI alanı&mdash;içinde olduğu yerde köprü tıklamalarını, Blazor yönlendiricisinin iç gezintiyi işlemesini sağlamak için de kullanılır.

Birçok barındırma senaryosunda, sunucunun uygulamanın sanal yolu uygulamanın köküdür. Bu durumlarda, uygulama temel yolu, bir uygulamanın varsayılan yapılandırması olan bir`<base href="/" />`eğik çizgi () olur. GitHub sayfaları ve IIS sanal dizinleri ya da alt uygulamalar gibi diğer barındırma senaryolarında, uygulama temel yolu sunucunun uygulamanın sanal yoluna ayarlanmalıdır. Uygulamanın temel yolunu ayarlamak için `<base>` *Wwwroot/index.html* dosyasının `<head>` etiket öğeleri içindeki etiketi güncelleştirin. Öznitelik değerini olarak `/virtual-path/` ayarlayın (sondaki eğik çizgi gereklidir); burada `/virtual-path/` , uygulamanın sunucusundaki tam sanal uygulama kök yoludur. `href` Yukarıdaki örnekte, sanal yol: `/CoolApp/` `<base href="/CoolApp/">`olarak ayarlanır.

Kök olmayan bir sanal yol (örneğin, `<base href="/CoolApp/">`) yapılandırılmış bir uygulama için, uygulama *yerel olarak çalıştırıldığında*kaynaklarını bulamaz. Yerel geliştirme ve test sırasında bu sorunu aşmak için, çalışma zamanında `<base>` etiketinin `href` değeriyle eşleşen bir *yol temel* bağımsız değişkeni sağlayabilirsiniz.

Uygulamayı yerel olarak çalıştırırken yol temel bağımsız değişkenini kök yol (`/`) ile geçirmek için, `dotnet run` komutunu `--pathbase` uygulama dizininden çalıştırın, seçeneği:

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

`/CoolApp/` (`<base href="/CoolApp/">`) Sanal taban yoluna sahip bir uygulama için, komut şu şekilde olur:

```console
dotnet run --pathbase=/CoolApp
```

Uygulama üzerinde `http://localhost:port/CoolApp`yerel olarak yanıt verir.

Daha fazla bilgi için, [yol temel ana bilgisayar yapılandırma değerindeki](#path-base)bölümüne bakın.

Bir uygulama, [istemci tarafı barındırma modelini](xref:blazor/hosting-models#client-side) ( **Blazor (istemci tarafı)** proje `blazor` şablonunu temel alan, [DotNet yeni](/dotnet/core/tools/dotnet-new) komut kullanılırken şablon) kullanıyorsa ve bir ASP.NET Core uygulamasında IIS alt uygulaması olarak barındırılıyorsa, devralınan ASP.NET Core modülü işleyicisini devre dışı bırakın veya *Web. config* dosyasındaki kök (üst) `<handlers>` uygulamanın bölümünün alt uygulama tarafından devralınamayacağını doğrulayın.

Dosyaya bir `<handlers>` bölüm ekleyerek uygulamanın yayınlanan *Web. config* dosyasındaki işleyiciyi kaldırın:

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

Alternatif olarak, `<system.webServer>` şu şekilde `<location>` `inheritInChildApplications` ayarlanmışbiröğekullanarakkök(üst)uygulamanındevralınmasınıdevredışıbırakın:`false`

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

İşleyicinin kaldırılması veya devralmayı devre dışı bırakmak, bu bölümde açıklandığı gibi uygulamanın temel yolunu yapılandırmaya ek olarak gerçekleştirilir. Uygulamanın *index. html* dosyasındaki uygulama temel yolunu, IIS 'de alt uygulamayı YAPıLANDıRıRKEN kullanılan IIS diğer adına ayarlayın.

## <a name="hosted-deployment-with-aspnet-core"></a>ASP.NET Core ile barındırılan dağıtım

*Barındırılan bir dağıtım* , Blazor istemci tarafı uygulamasını Web sunucusunda çalışan [ASP.NET Core bir uygulamadan](xref:index) tarayıcılara sunar.

Blazor uygulaması, yayımlanan çıktıda ASP.NET Core uygulamasına dahildir, böylece iki uygulama birlikte dağıtılır. ASP.NET Core uygulamasını barındırabilen bir Web sunucusu gereklidir. Barındırılan bir dağıtım için, Visual Studio **Blazor (ASP.NET Core hosted)** proje şablonunu (`blazorhosted` [DotNet New](/dotnet/core/tools/dotnet-new) komutu kullanılırken şablon) içerir.

Uygulama barındırma ve dağıtım ASP.NET Core hakkında daha fazla bilgi için bkz <xref:host-and-deploy/index>.

Azure App Service dağıtma hakkında daha fazla bilgi için bkz <xref:tutorials/publish-to-azure-webapp-using-vs>.

## <a name="standalone-deployment"></a>Tek başına dağıtım

*Tek başına dağıtım* , Blazor istemci tarafı uygulamasına doğrudan istemciler tarafından istenen statik dosyalar kümesi olarak hizmet verir. Herhangi bir statik dosya sunucusu Blazor uygulamasını sunabilir.

Tek başına dağıtım varlıkları *bin/Release/{Target Framework}/publish/{ASSEMBLY Name}/Dist* klasöründe yayımlanır.

### <a name="iis"></a>IIS

IIS, Blazor uygulamaları için özellikli bir statik dosya sunucusudur. IIS 'yi Blazor barındıracak şekilde yapılandırmak için bkz. [IIS 'de statik Web sitesi oluşturma](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Yayımlanan varlıklar */BIN/Release/{Target Framework}/Publish* klasöründe oluşturulur. Web sunucusunda veya barındırma hizmetinde *Yayımlama* klasörünün içeriğini barındırın.

#### <a name="webconfig"></a>Web. config

Bir Blazor projesi yayımlandığında, aşağıdaki IIS yapılandırmasıyla bir *Web. config* dosyası oluşturulur:

* MIME türleri aşağıdaki dosya uzantıları için ayarlanır:
  * *. dll* &ndash;`application/octet-stream`
  * *. JSON* &ndash;`application/json`
  * *.* &ndash;`application/wasm`
  * *. WOFF* &ndash;`application/font-woff`
  * *. woff2* &ndash;`application/font-woff`
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

#### <a name="troubleshooting"></a>Sorun giderme

*500-Iç sunucu hatası* ALıNMıŞSA ve IIS Yöneticisi Web sitesinin yapılandırmasına erişmeye çalışırken hatalar OLUŞTURURSA, URL yeniden yazma modülünün yüklü olduğunu doğrulayın. Modül yüklü olmadığında, *Web. config* dosyası IIS tarafından ayrıştırılamaz. Bu, IIS yöneticisinin Web sitesinin yapılandırmasını ve Web sitesinin Blazor 'in statik dosyalarına hizmet etmesini engeller.

IIS ile dağıtım sorunlarını giderme hakkında daha fazla bilgi için <xref:test/troubleshoot-azure-iis>bkz.

### <a name="azure-storage"></a>Azure Depolama

Azure depolama statik dosya barındırma, sunucusuz Blazor uygulamasının barındırılmasına olanak sağlar. Özel etki alanı adları, Azure Content Delivery Network (CDN) ve HTTPS desteklenir.

Blob hizmeti bir depolama hesabında barındırılan statik Web sitesi için etkinleştirildiğinde:

* **Dizin belgesi adını** olarak `index.html`ayarlayın.
* **Hata belge yolunu** olarak `index.html`ayarlayın. Razor bileşenleri ve diğer dosya olmayan uç noktaları, blob hizmeti tarafından depolanan statik içerikte fiziksel yollarda yer vermez. Blazor yönlendiricisinin işlemesi gereken bu kaynaklardan birine yönelik bir istek alındığında, blob hizmeti tarafından oluşturulan *404-bulunamayan* hata, isteği **hata belge yoluna**yönlendirir. *İndex. html* blobu döndürülür ve Blazor yönlendiricisi yolu yükler ve işler.

Daha fazla bilgi için bkz. [Azure Storage 'Da statik Web sitesi barındırma](/azure/storage/blobs/storage-blob-static-website).

### <a name="nginx"></a>NGINX

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

NGINX kullanarak Docker 'da Blazor barındırmak için Dockerfile 'ı alp tabanlı NGINX görüntüsünü kullanacak şekilde ayarlayın. Dockerfile dosyasını, *NGINX. config* dosyasını kapsayıcıya kopyalamak için güncelleştirin.

Aşağıdaki örnekte gösterildiği gibi Dockerfile dosyasına bir satır ekleyin:

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a>GitHub sayfaları

URL yeniden işlemesini işlemek için, isteği *index. html* sayfasına yönlendirmeyi işleyen bir betiği olan bir *404. html* dosyası ekleyin. Topluluk tarafından sunulan örnek bir uygulama için bkz. [GitHub sayfaları Için tek sayfalı uygulamalar](https://spa-github-pages.rafrex.com/) ([GitHub üzerinde rafrex/Spa-GitHub-Pages](https://github.com/rafrex/spa-github-pages#readme)). Topluluk yaklaşımını kullanan bir örnek, GitHub ([canlı site](https://blazor-demo.github.io/)) [üzerinde blazor-demo/blazor-demo. GitHub. IO](https://github.com/blazor-demo/blazor-demo.github.io) adresinde görülebilir.

Bir kuruluş sitesi yerine bir proje sitesi kullanırken `<base>` etiketi *index. html*dosyasına ekleyin veya güncelleştirin. Öznitelik değerini GitHub deposu adına sondaki eğik çizgiyle (örneğin, `my-repository/`) ayarlayın. `href`
