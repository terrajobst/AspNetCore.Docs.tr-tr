---
title: ASP.NET Core Blazor barındırma ve dağıtma
author: guardrex
description: Blazor uygulamalarının nasıl barındırılacağını ve dağıtılacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 271135a0ebe67d31fd8e2bcf672e723814727147
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391335"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>ASP.NET Core Blazor barındırma ve dağıtma

, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Uygulamalar yayın yapılandırmasında dağıtım için yayımlanır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Gezinti çubuğundan **derleme** >  **{APPLICATION} Yayımla** ' yı seçin.
1. *Yayımla hedefini*seçin. Yerel olarak yayımlamak için **klasör**' ü seçin.
1. **Klasör seçin** alanında varsayılan konumu kabul edin veya farklı bir konum belirtin. **Yayımla** düğmesini seçin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uygulamayı bir sürüm yapılandırmasıyla yayımlamak için [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutunu kullanın:

```dotnetcli
dotnet publish -c Release
```

---

Uygulamanın yayımlanması, projenin bağımlılıklarını [geri yüklemeyi](/dotnet/core/tools/dotnet-restore) tetikler ve dağıtım için varlıkları oluşturmadan önce projeyi [oluşturur](/dotnet/core/tools/dotnet-build) . Yapı işleminin bir parçası olarak, uygulama indirme boyutunu ve yükleme sürelerini azaltmak için kullanılmayan Yöntemler ve derlemeler kaldırılır.

Blazor WebAssembly uygulaması */BIN/Release/{Target Framework}/publish/{ASSEMBLY Name}/Dist* klasörüne yayımlanır. Blazor sunucu uygulaması */BIN/Release/{Target Framework}/Publish* klasörüne yayımlanır.

Klasördeki varlıklar Web sunucusuna dağıtılır. Dağıtım, kullanımdaki geliştirme araçlarına bağlı olarak el ile veya otomatik bir süreç olabilir.

## <a name="app-base-path"></a>Uygulama temel yolu

*Uygulama temel yolu* , UYGULAMANıN kök URL yoludur. Aşağıdaki ana uygulamayı ve Blazor uygulamasını göz önünde bulundurun:

* Ana uygulama `MyApp`çağrılır:
  * Uygulama fiziksel olarak *d:\\MyApp*konumunda bulunur.
  * İstekler `https://www.contoso.com/{MYAPP RESOURCE}`alındı.
* `CoolApp` adlı bir Blazor uygulaması `MyApp`alt uygulamasıdır:
  * Alt uygulama fiziksel olarak *d:\\MyApp\\CoolApp*konumunda bulunur.
  * İstekler `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`alındı.

`CoolApp`için ek yapılandırma belirtmeden, Bu senaryodaki alt uygulama, sunucuda nerede bulunduğu konusunda bilgi sahibi değildir. Örneğin, uygulama, `/CoolApp/`göreli URL yolunda bulunduğunu bilmeden kaynaklarına doğru göreli URL 'Ler oluşturamıyoruz.

Blazor uygulamasının `https://www.contoso.com/CoolApp/`temel yolu için yapılandırma sağlamak üzere, `<base>` etiketinin `href` özniteliği *Wwwroot/index.html* dosyasındaki göreli kök yoluna ayarlanır:

```html
<base href="/CoolApp/">
```

Göreli URL yolunu sağlayarak, kök dizinde olmayan bir bileşen, uygulamanın kök yoluna göre URL 'Ler oluşturabilir. Farklı dizin yapısı düzeylerindeki bileşenler, uygulama genelinde konumlardaki diğer kaynakların bağlantılarını oluşturabilir. Uygulama temel yolu Ayrıca, bağlantının `href` hedefinin uygulama temel yolu URI alanı içinde olduğu yerde köprü tıklamasına yol açmak için kullanılır&mdash;Blazor yönlendiricisi iç gezintiyi işler.

Birçok barındırma senaryosunda, uygulamanın göreli URL yolu uygulamanın köküdür. Bu durumlarda, uygulamanın göreli URL taban yolu, bir Blazor uygulamasının varsayılan yapılandırması olan bir eğik çizgi (`<base href="/" />`) olur. GitHub sayfaları ve IIS alt uygulamaları gibi diğer barındırma senaryolarında, uygulama temel yolu, sunucunun uygulamanın göreli URL 'SI yolu olarak ayarlanmalıdır.

Uygulamanın temel yolunu ayarlamak için, *Wwwroot/index.html* dosyasının `<head>` etiketi öğeleri içindeki `<base>` etiketini güncelleştirin. `href` öznitelik değerini `/{RELATIVE URL PATH}/` olarak ayarlayın (sondaki eğik çizgi gereklidir), burada `{RELATIVE URL PATH}` uygulamanın tam göreli URL yoludur.

Kök olmayan göreli URL yoluna (örneğin, `<base href="/CoolApp/">`) sahip bir uygulama için, uygulama *yerel olarak çalıştırıldığında*kaynaklarını bulamaz. Yerel geliştirme ve test sırasında bu sorunu aşmak için, çalışma zamanında `<base>` etiketinin `href` değeriyle eşleşen bir *yol temel* bağımsız değişkeni sağlayabilirsiniz. Uygulamayı yerel olarak çalıştırırken yol temel bağımsız değişkenini geçirmek için, `--pathbase` seçeneğiyle uygulamanın dizininden `dotnet run` komutunu yürütün:

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

Göreli URL yolu `/CoolApp/` (`<base href="/CoolApp/">`) olan bir uygulama için, komut şu şekilde olur:

```dotnetcli
dotnet run --pathbase=/CoolApp
```

Uygulama, `http://localhost:port/CoolApp`yerel olarak yanıt verir.

## <a name="deployment"></a>Dağıtım

Dağıtım Kılavuzu için aşağıdaki konulara bakın:

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
