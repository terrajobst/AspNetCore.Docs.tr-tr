---
title: ASP.NET Core Blazor sunucu-tarafı barındırma ve dağıtma
author: guardrex
description: ASP.NET Core kullanarak Blazor sunucu tarafı uygulamasını nasıl barındırılacağını ve dağıtacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: fc47dfa1344b74ec7110211e3698217e246ab86d
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878491"
---
# <a name="host-and-deploy-blazor-server-side"></a>Blazor sunucu tarafı barındırma ve dağıtma

, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[Sunucu tarafı barındırma modelini](xref:blazor/hosting-models#server-side) kullanan sunucu tarafı uygulamalar [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration)kabul edebilir.

## <a name="deployment"></a>Dağıtım

[Sunucu tarafı barındırma modeliyle](xref:blazor/hosting-models#server-side), Blazor bir ASP.NET Core uygulamasının içinden sunucuda yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.

ASP.NET Core uygulaması barındırabilen bir Web sunucusu gerekiyor. Visual Studio, **Blazor Server uygulaması** proje şablonunu (`blazorserverside` [DotNet New](/dotnet/core/tools/dotnet-new) komutu kullanılırken şablon) içerir.

## <a name="scalability"></a>Ölçeklenebilirlik

Bir Blazor sunucusu uygulaması için kullanılabilir altyapıyı en iyi şekilde kullanmasını sağlamak üzere bir dağıtım planlayın. Blazor Server uygulama ölçeklenebilirliğini ele almak için aşağıdaki kaynaklara bakın:

* [Blazor Server uygulamalarının temelleri](xref:blazor/hosting-models#server-side)
* <xref:security/blazor/server-side>

### <a name="deployment-server"></a>Dağıtım sunucusu

Tek bir sunucunun ölçeklenebilirliğini değerlendirirken (ölçeği büyütme), bir uygulama için kullanılabilir olan bellek büyük olasılıkla uygulamanın kullanıcı talebi arttıkça arttırabileceği ilk kaynaktır. Sunucudaki kullanılabilir bellek şunları etkiler:

* Bir sunucunun destekleyebileceği etkin devre sayısı.
* İstemcide UI gecikme süresi.

Güvenli ve ölçeklenebilir Blazor Server uygulamaları oluşturma hakkında yönergeler için bkz <xref:security/blazor/server-side>.

Her bağlantı hattı en düşük *Merhaba Dünya*stilinde bir uygulama için YAKLAŞıK 250 KB bellek kullanır. Bir devrenin boyutu, uygulamanın koduna ve her bileşenle ilişkili durum bakım gereksinimlerine bağlıdır. Uygulamanız ve altyapınız için geliştirme sırasında kaynak taleplerini ölçmenizi öneririz, ancak aşağıdaki taban çizgisi dağıtım hedefini planlarken bir başlangıç noktası olabilir: Uygulamanızı 5.000 eşzamanlı kullanıcı desteğine karşı bekleliyorsanız, en az 1,3 GB sunucu belleğini uygulamaya (veya Kullanıcı başına ~ 273 KB) göre bütçeleme yapmayı düşünün.

### <a name="signalr-configuration"></a>SignalR yapılandırması

Blazor Server Apps, tarayıcıyla iletişim kurmak için ASP.NET Core SignalR kullanır. [SignalR 'nin barındırma ve ölçeklendirme koşulları](xref:signalr/publish-to-azure-web-app) Blazor Server uygulamaları için geçerlidir.

Blazor, düşük gecikme süresi, güvenilirlik ve [güvenlik](xref:signalr/security)nedeniyle SignalR taşıması olarak WebSockets kullanırken en iyi şekilde geçerlidir. Uzun yoklama, WebSockets kullanılamadığında veya uygulama açıkça uzun yoklamayı kullanacak şekilde yapılandırıldığında, SignalR tarafından kullanılır. Azure App Service dağıtım sırasında, uygulamayı hizmetin Azure portal ayarları içinde kullanmak üzere yapılandırın. Azure App Service için uygulamayı yapılandırma hakkında ayrıntılı bilgi için bkz. [SignalR yayımlama yönergeleri](xref:signalr/publish-to-azure-web-app).

Blazor Server uygulamaları için [Azure SignalR hizmetini](/azure/azure-signalr) kullanmanızı öneririz. Hizmet, Blazor sunucu uygulamasını çok sayıda eşzamanlı SignalR bağlantısına ölçeklendirmeye olanak tanır. Ayrıca, SignalR hizmetinin küresel erişim ve yüksek performanslı veri merkezleri Coğrafya nedeniyle gecikme süresini azaltmaya önemli ölçüde yardımcı olur.

### <a name="measure-network-latency"></a>Ölçü ağı gecikmesi

Aşağıdaki örnekte gösterildiği gibi, [js birlikte çalışması](xref:blazor/javascript-interop) ağ gecikmesini ölçmek için kullanılabilir:

```cshtml
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
