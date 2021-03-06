---
title: ASP.NET Core Blazor sunucusu barındırma ve dağıtma
author: guardrex
description: ASP.NET Core kullanarak Blazor sunucu uygulamasının nasıl barındırılacağını ve dağıtılacağını öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: 866bb348180c872d8ab20787283cfb7217183a8d
ms.sourcegitcommit: 3ca4a2235a8129def9e480d0a6ad54cc856920ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79025420"
---
# <a name="host-and-deploy-opno-locblazor-server"></a>Blazor sunucusu barındırma ve dağıtma

, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[Blazor Server uygulamaları](xref:blazor/hosting-models#blazor-server) [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration)kabul edebilir.

## <a name="deployment"></a>Dağıtım

[Blazor sunucusu barındırma modelini](xref:blazor/hosting-models#blazor-server)kullanarak, Blazor sunucuda bir ASP.NET Core uygulaması içinden yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları [SignalR](xref:signalr/introduction) bir bağlantı üzerinden işlenir.

ASP.NET Core uygulaması barındırabilen bir Web sunucusu gerekiyor. Visual Studio, [DotNet New](/dotnet/core/tools/dotnet-new) komutu kullanılırken **Blazor Server uygulaması** proje şablonunu (`blazorserverside` şablonu) içerir.

## <a name="scalability"></a>Ölçeklenebilirlik

Bir Blazor sunucusu uygulaması için kullanılabilir altyapıyı en iyi şekilde kullanmasını sağlamak üzere bir dağıtım planlayın. Blazor Server uygulama ölçeklenebilirliğini karşılamak için aşağıdaki kaynaklara bakın:

* [Blazor Server uygulamalarının temelleri](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>Dağıtım sunucusu

Tek bir sunucunun ölçeklenebilirliğini değerlendirirken (ölçeği büyütme), bir uygulama için kullanılabilir olan bellek büyük olasılıkla uygulamanın kullanıcı talebi arttıkça arttırabileceği ilk kaynaktır. Sunucudaki kullanılabilir bellek şunları etkiler:

* Bir sunucunun destekleyebileceği etkin devre sayısı.
* İstemcide UI gecikme süresi.

Güvenli ve ölçeklenebilir Blazor sunucu uygulamaları oluşturma hakkında yönergeler için bkz. <xref:security/blazor/server>.

Her bağlantı hattı en düşük *Merhaba Dünya*stilinde bir uygulama için YAKLAŞıK 250 KB bellek kullanır. Bir devrenin boyutu, uygulamanın koduna ve her bileşenle ilişkili durum bakım gereksinimlerine bağlıdır. Uygulama ve altyapınız için geliştirme sırasında kaynak taleplerini ölçmenizi öneririz, ancak aşağıdaki taban çizgisi, dağıtım hedefini planlamada bir başlangıç noktası olabilir: uygulamanızın 5.000 eşzamanlı kullanıcı desteklemesini istiyorsanız, şu adreste bütçeleme yapmayı düşünün: uygulamaya en az 1,3 GB sunucu belleği (veya Kullanıcı başına ~ 273 KB).

### <a name="opno-locsignalr-configuration"></a>SignalR yapılandırması

Blazor sunucu uygulamaları tarayıcıyla iletişim kurmak için ASP.NET Core SignalR kullanır. [SignalRbarındırma ve ölçeklendirme koşulları](xref:signalr/publish-to-azure-web-app) Blazor sunucu uygulamaları için geçerlidir.

daha düşük gecikme süresi, güvenilirlik ve [güvenlik](xref:signalr/security)nedeniyle SignalR taşıma olarak WebSockets kullanırken Blazor en iyi şekilde işe yarar. Uzun yoklama, WebSockets kullanılamadığında veya uygulama açıkça uzun yoklamayı kullanacak şekilde yapılandırıldığında SignalR tarafından kullanılır. Azure App Service dağıtım sırasında, uygulamayı hizmetin Azure portal ayarları içinde kullanmak üzere yapılandırın. Azure App Service için uygulamayı yapılandırma hakkında ayrıntılı bilgi için bkz. [SignalR yayımlama yönergeleri](xref:signalr/publish-to-azure-web-app).

#### <a name="azure-opno-locsignalr-service"></a>Azure SignalR hizmeti

Blazor Server uygulamaları için [Azure SignalR hizmetini](/azure/azure-signalr) kullanmanızı öneririz. Hizmet, Blazor sunucu uygulamasının ölçeğini çok sayıda eşzamanlı SignalR bağlantı ile ölçeklendirmeye olanak tanır. Ayrıca, SignalR hizmetin küresel erişim ve yüksek performanslı veri merkezleri Coğrafya nedeniyle gecikme süresini azaltmaya önemli ölçüde yardımcı olur. Azure SignalR hizmetini bir uygulamayı yapılandırmak (ve isteğe bağlı olarak sağlamak) için:

1. [Prerendering sırasında istemciler aynı sunucuya geri yönlendirildiği](xref:blazor/hosting-models#connection-to-the-server) *yapışkan oturumları*desteklemek için hizmeti etkinleştirin. `ServerStickyMode` seçeneğini veya yapılandırma değerini `Required`olarak ayarlayın. Genellikle, bir uygulama aşağıdaki yaklaşımlardan **birini** kullanarak yapılandırmayı oluşturur:

   * `Startup.ConfigureServices`:
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * Yapılandırma (aşağıdaki yaklaşımlardan **birini** kullanın):
  
     * *appSettings. JSON*:

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * App Service 'in **yapılandırma** Azure Portal **uygulama ayarları** > (**ad**: `Azure:SignalR:ServerStickyMode`, **değer**: `Required`).

1. Blazor Server uygulaması için Visual Studio 'da bir Azure Apps yayımlama profili oluşturun.
1. **Azure SignalR hizmeti** bağımlılığını profile ekleyin. Azure aboneliğinin uygulamaya atanacak önceden mevcut bir Azure SignalR hizmeti örneği yoksa, yeni bir hizmet örneği sağlamak için **Yeni bir azure SignalR hizmet örneği oluştur** ' u seçin.
1. Uygulamayı Azure’da yayımlama.

#### <a name="iis"></a>IIS

IIS kullanırken şunları etkinleştirin:

* [IIS üzerinde WebSockets](xref:fundamentals/websockets#enabling-websockets-on-iis).
* [Uygulama Isteği yönlendirme Ile yapışkan oturumlar](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

#### <a name="kubernetes"></a>Kubernetes

[Yapışkan oturumlar için aşağıdaki Kubernetes ek açıklamalarını](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/)içeren bir giriş tanımı oluşturun:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <ingress-name>
  annotations:
    nginx.ingress.kubernetes.io/affinity: "cookie"
    nginx.ingress.kubernetes.io/session-cookie-name: "affinity"
    nginx.ingress.kubernetes.io/session-cookie-expires: "14400"
    nginx.ingress.kubernetes.io/session-cookie-max-age: "14400"
```

#### <a name="linux-with-nginx"></a>Nginx ile Linux

SignalR WebSockets 'in düzgün çalışması için, proxy 'nin `Upgrade` ve `Connection` üstbilgilerinin aşağıdaki değerlere ayarlandığını ve `$connection_upgrade` şu değerlere eşlendiğinden emin olun:

* Varsayılan olarak yükseltme üst bilgisi değeri.
* Yükseltme üst bilgisi eksik veya boş olduğunda `close`.

```
http {
    map $http_upgrade $connection_upgrade {
        default Upgrade;
        ''      close;
    }

    server {
        listen      80;
        server_name example.com *.example.com
        location / {
            proxy_pass         http://localhost:5000;
            proxy_http_version 1.1;
            proxy_set_header   Upgrade $http_upgrade;
            proxy_set_header   Connection $connection_upgrade;
            proxy_set_header   Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Proto $scheme;
        }
    }
}
```

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [WebSocket proxy 'Si olarak NGıNX](https://www.nginx.com/blog/websocket-nginx/)
* [WebSocket proxy](http://nginx.org/docs/http/websocket.html)
* <xref:host-and-deploy/linux-nginx>

### <a name="measure-network-latency"></a>Ölçü ağı gecikmesi

Aşağıdaki örnekte gösterildiği gibi, [js birlikte çalışması](xref:blazor/call-javascript-from-dotnet) ağ gecikmesini ölçmek için kullanılabilir:

```razor
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

Makul bir kullanıcı arabirimi deneyimi için, 250ms veya daha az sayıda sürekli Kullanıcı arabirimi gecikme süresi önerilir.
