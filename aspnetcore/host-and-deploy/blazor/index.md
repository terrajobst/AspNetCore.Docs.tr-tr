---
title: ASP.NET Core Blazor barındırma ve dağıtma
author: guardrex
description: Blazor uygulamalarının nasıl barındırılacağını ve dağıtılacağını öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 238e7fc8f8d64c7847dc8847fb66e22442a3c8e0
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160268"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor"></a>ASP.NET Core Blazor barındırma ve dağıtma

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

Blazor WebAssembly uygulaması */BIN/Release/{Target Framework}/publish/{ASSEMBLY Name}/Dist* klasörüne yayımlanır. Bir Blazor sunucusu uygulaması */BIN/Release/{Target Framework}/Publish* klasörüne yayımlanır.

Klasördeki varlıklar Web sunucusuna dağıtılır. Dağıtım, kullanımdaki geliştirme araçlarına bağlı olarak el ile veya otomatik bir süreç olabilir.

## <a name="app-base-path"></a>Uygulama temel yolu

*Uygulama temel yolu* , UYGULAMANıN kök URL yoludur. Aşağıdaki ASP.NET Core uygulamayı ve Blazor alt uygulamayı göz önünde bulundurun:

* ASP.NET Core uygulamasının adı `MyApp`:
  * Uygulama fiziksel olarak *d:/MyApp*konumunda bulunur.
  * İstekler `https://www.contoso.com/{MYAPP RESOURCE}`alındı.
* `CoolApp` adlı bir Blazor uygulaması, `MyApp`alt uygulamasıdır:
  * Alt uygulama fiziksel olarak *d:/MyApp/CoolApp*konumunda bulunur.
  * İstekler `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`alındı.

`CoolApp`için ek yapılandırma belirtmeden, Bu senaryodaki alt uygulama, sunucuda nerede bulunduğu konusunda bilgi sahibi değildir. Örneğin, uygulama, `/CoolApp/`göreli URL yolunda bulunduğunu bilmeden kaynaklarına doğru göreli URL 'Ler oluşturamıyoruz.

Blazor uygulamasının temel `https://www.contoso.com/CoolApp/`yolu için yapılandırma sağlamak üzere `<base>` etiketinin `href` özniteliği *Pages/_Host. cshtml* dosyasında (Blazor Server) veya *wwwroot/index.html* dosyasında (Blazor webassembly) göreli kök yolu olarak ayarlanır:

```html
<base href="/CoolApp/">
```

Blazor sunucu uygulamaları, uygulamanın `Startup.Configure`istek ardışık düzeninde <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> çağırarak sunucu tarafı taban yolunu da ayarlar:

```csharp
app.UsePathBase("/CoolApp");
```

Göreli URL yolunu sağlayarak, kök dizinde olmayan bir bileşen, uygulamanın kök yoluna göre URL 'Ler oluşturabilir. Farklı dizin yapısı düzeylerindeki bileşenler, uygulama genelinde konumlardaki diğer kaynakların bağlantılarını oluşturabilir. Uygulama temel yolu Ayrıca, bağlantının `href` hedefinin uygulama temel yolu URI alanı içinde olduğu seçili köprüleri ele almak için de kullanılır. Blazor yönlendirici iç gezintiyi işler.

Birçok barındırma senaryosunda, uygulamanın göreli URL yolu uygulamanın köküdür. Bu durumlarda, uygulamanın göreli URL taban yolu bir Blazor uygulamasının varsayılan yapılandırması olan eğik çizgi (`<base href="/" />`) olur. GitHub sayfaları ve IIS alt uygulamaları gibi diğer barındırma senaryolarında, uygulama temel yolu, sunucunun uygulamanın göreli URL 'SI yolu olarak ayarlanmalıdır.

Uygulamanın temel yolunu ayarlamak için, *Pages/_Host. cshtml* dosyasının (Blazor Server) veya *wwwroot/index.html* dosyasının (Blazor webassembly) `<head>` etiketi öğeleri içindeki `<base>` etiketini güncelleştirin. `href` öznitelik değerini `/{RELATIVE URL PATH}/` olarak ayarlayın (sondaki eğik çizgi gereklidir), burada `{RELATIVE URL PATH}` uygulamanın tam göreli URL yoludur.

Kök olmayan göreli URL yolu (örneğin, `<base href="/CoolApp/">`) olan bir Blazor WebAssembly uygulaması için, uygulama *yerel olarak çalıştırıldığında*kaynaklarını bulamaz. Yerel geliştirme ve test sırasında bu sorunu aşmak için, çalışma zamanında `<base>` etiketinin `href` değeriyle eşleşen bir *yol temel* bağımsız değişkeni sağlayabilirsiniz. Sondaki eğik çizgi eklemeyin. Uygulamayı yerel olarak çalıştırırken yol temel bağımsız değişkenini geçirmek için, `--pathbase` seçeneğiyle uygulamanın dizininden `dotnet run` komutunu yürütün:

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

Göreli URL yolu `/CoolApp/` (`<base href="/CoolApp/">`) olan bir Blazor WebAssembly uygulaması için, komut şu şekilde olur:

```dotnetcli
dotnet run --pathbase=/CoolApp
```

Blazor WebAssembly uygulaması, `http://localhost:port/CoolApp`yerel olarak yanıt verir.

## <a name="deployment"></a>Dağıtım

Dağıtım Kılavuzu için aşağıdaki konulara bakın:

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
