---
title: ASP.NET Core Blazor barındırma ve dağıtma
author: guardrex
description: Blazor uygulamalarının nasıl barındırılacağını ve dağıtılacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 26c8fcf56ab8ca68aeca93560785fc6c1144ab86
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963685"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>ASP.NET Core Blazor barındırma ve dağıtma

, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Uygulamalar yayın yapılandırmasında dağıtım için yayımlanır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Gezinti çubuğundan **Build** > **Publish {APPLICATION}** öğesini seçin.
1. *Yayımla hedefini*seçin. Yerel olarak yayımlamak için **klasör**' ü seçin.
1. **Klasör seçin** alanında varsayılan konumu kabul edin veya farklı bir konum belirtin. **Yayımla** düğmesini seçin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uygulamayı bir sürüm yapılandırmasıyla yayımlamak için [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutunu kullanın:

```console
dotnet publish -c Release
```

---

Uygulamanın yayımlanması, projenin bağımlılıklarını [geri yüklemeyi](/dotnet/core/tools/dotnet-restore) tetikler ve dağıtım için varlıkları oluşturmadan önce projeyi [oluşturur](/dotnet/core/tools/dotnet-build) . Yapı işleminin bir parçası olarak, uygulama indirme boyutunu ve yükleme sürelerini azaltmak için kullanılmayan Yöntemler ve derlemeler kaldırılır.

Blazor WebAssembly uygulaması */BIN/Release/{Target Framework}/publish/{ASSEMBLY Name}/Dist* klasörüne yayımlanır. Blazor sunucu uygulaması */BIN/Release/{Target Framework}/Publish* klasörüne yayımlanır.

Klasördeki varlıklar Web sunucusuna dağıtılır. Dağıtım, kullanımdaki geliştirme araçlarına bağlı olarak el ile veya otomatik bir süreç olabilir.

## <a name="app-base-path"></a>Uygulama temel yolu

*Uygulama temel yolu* , UYGULAMANıN kök URL yoludur. Aşağıdaki ana uygulamayı ve Blazor uygulamasını göz önünde bulundurun:

* Ana uygulama şu şekilde adlandırılır `MyApp`:
  * Uygulama fiziksel olarak *\\d: MyApp*konumunda bulunur.
  * İstekleri tarihinde `https://www.contoso.com/{MYAPP RESOURCE}`alınır.
* Çağrılan `CoolApp` bir Blazor uygulaması, öğesinin `MyApp`bir alt uygulamasıdır:
  * Alt uygulama fiziksel olarak *d:\\MyApp\\CoolApp*konumunda bulunur.
  * İstekleri tarihinde `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`alınır.

İçin `CoolApp`ek yapılandırma belirtmeden, Bu senaryodaki alt uygulama, sunucuda nerede bulunduğu konusunda bilgi sahibi değildir. Örneğin, uygulama ilgili URL yolunda `/CoolApp/`bulunduğunu bilmeden kaynaklarına doğru göreli URL 'ler oluşturamıyoruz.

Blazor uygulamasının `https://www.contoso.com/CoolApp/`temel yolu `<base>` için yapılandırma sağlamak üzere etiketinin `href` özniteliği *Wwwroot/index.html* dosyasındaki göreli kök yoluna ayarlanır:

```html
<base href="/CoolApp/">
```

Göreli URL yolunu sağlayarak, kök dizinde olmayan bir bileşen, uygulamanın kök yoluna göre URL 'Ler oluşturabilir. Farklı dizin yapısı düzeylerindeki bileşenler, uygulama genelinde konumlardaki diğer kaynakların bağlantılarını oluşturabilir. Uygulama temel yolu Ayrıca, bağlantının `href` hedefinin uygulama temel yolu URI alanı&mdash;içinde olduğu yerde köprü tıklamalarını, Blazor yönlendiricisinin iç gezintiyi işlemesini sağlamak için de kullanılır.

Birçok barındırma senaryosunda, uygulamanın göreli URL yolu uygulamanın köküdür. Bu durumlarda, uygulamanın göreli URL taban yolu, bir Blazor uygulamasının varsayılan yapılandırması olan bir`<base href="/" />`eğik çizgi () olur. GitHub sayfaları ve IIS alt uygulamaları gibi diğer barındırma senaryolarında, uygulama temel yolu, sunucunun uygulamanın göreli URL 'SI yolu olarak ayarlanmalıdır.

Uygulamanın temel yolunu ayarlamak için `<base>` *Wwwroot/index.html* dosyasının `<head>` etiket öğeleri içindeki etiketi güncelleştirin. Öznitelik değerini olarak `/{RELATIVE URL PATH}/` ayarlayın (sondaki eğik çizgi gereklidir), burada `{RELATIVE URL PATH}` uygulamanın tam göreli URL yoludur. `href`

Kök olmayan göreli URL yoluna (örneğin, `<base href="/CoolApp/">`) sahip bir uygulama için, uygulama *yerel olarak çalıştırıldığında*kaynaklarını bulamaz. Yerel geliştirme ve test sırasında bu sorunu aşmak için, çalışma zamanında `<base>` etiketinin `href` değeriyle eşleşen bir *yol temel* bağımsız değişkeni sağlayabilirsiniz. Uygulamayı yerel olarak çalıştırırken yol temel bağımsız değişkenini geçirmek için, `dotnet run` komutu uygulamanın dizininden `--pathbase` çalıştırın, seçeneği:

```console
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

Göreli URL yolu `/CoolApp/` (`<base href="/CoolApp/">`) olan bir uygulama için, komut şu şekilde olur:

```console
dotnet run --pathbase=/CoolApp
```

Uygulama üzerinde `http://localhost:port/CoolApp`yerel olarak yanıt verir.

## <a name="deployment"></a>Dağıtım

Dağıtım Kılavuzu için aşağıdaki konulara bakın:

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
