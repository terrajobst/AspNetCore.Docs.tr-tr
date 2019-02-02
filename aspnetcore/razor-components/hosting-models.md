---
title: Razor bileşenleri modelleri barındırma
author: guardrex
description: İstemci tarafı Blazor ve sunucu tarafı ASP.NET Core Razor modelleri barındırma bileşenlerini anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/hosting-models
ms.openlocfilehash: dadae3de9ca3b67454865e2557d37c8b595f8f52
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668183"
---
# <a name="razor-components-hosting-models"></a>Razor bileşenleri modelleri barındırma

Tarafından [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Razor bileşenleri, istemci tarafı çalışmak üzere tasarlanmış bir web çerçevesi olan WebAssembly tabanlı .NET çalışma zamanı tarayıcıda (*Blazor*) veya sunucu tarafı ASP.NET Core (*ASP.NET Core Razor bileşenleri*). Barındırma modeli, uygulama ve bileşen modelleri bakılmaksızın *aynı kalması*. Bu makalede, kullanılabilen barındırma modelleri açıklanmaktadır.

## <a name="client-side-hosting-model"></a>İstemci tarafı barındırma modeli

Asıl barındırma Blazor için tarayıcıda çalışan istemci-tarafı modelidir. Bu modelde, tarayıcıya .NET çalışma zamanı Blazor uygulamayı ve bağımlılıkları indirilir. Uygulamayı doğrudan tarayıcıda kullanıcı Arabirimi iş parçacığında yürütülür. Tüm kullanıcı Arabirimi güncelleştirmeleri ve olay işleme, aynı işlem içinde gerçekleşir. Uygulama varlıkları ne olursa olsun web sunucusunu tercih edilen kullanarak statik dosya olarak dağıtılabilir (bkz [konak dağıtıp](xref:host-and-deploy/razor-components/index)).

![Blazor istemci-tarafı: Tarayıcı içinde bir kullanıcı Arabirimi iş parçacığında Blazor uygulama çalışır.](hosting-models/_static/client-side.png)

İstemci tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için kullanın **Blazor** veya **Blazor (ASP.NET Core barındırılan)** proje şablonları (`blazor` veya `blazorhosted` kullanırkenşablonu[yeni dotnet](/dotnet/core/tools/dotnet-new) komutu bir komut isteminde). Dahil edilen *blazor.webassembly.js* betik tanıtıcıları:

* .NET çalışma zamanı, uygulama ve onun bağımlılıklarını karşıdan yükleniyor.
* Uygulamayı çalıştırmak için çalışma zamanı başlatma.

İstemci tarafı barındırma modeli, çeşitli avantajlar sunar. İstemci tarafı Blazor:

* .NET sunucu tarafı bağımlılığı yoktur.
* Bir zengin etkileşimli kullanıcı Arabirimi var.
* İstemci kaynakları ve özellikleri tam olarak yararlanır.
* Yük boşaltma, sunucudan istemciye çalışır.
* Çevrimdışı senaryolarını destekler.

İstemci tarafı barındırma downsides vardır. İstemci tarafı Blazor:

* Uygulama, tarayıcının yeteneklerini kısıtlar.
* Özellikli istemci donanım ve yazılım (örneğin, WebAssembly desteği) gerektirir.
* İndirme boyutu daha büyük ve uzun uygulama yükleme süresi vardır.
* .NET çalışma zamanı ve araç desteği (örneğin, .NET Standard desteği ve hata ayıklama sınırlamaları) yetişkin daha az sahiptir.

Visual Studio içerir **Blazor (barındırılan ASP.NET Core)** WebAssembly üzerinde çalışır ve bir ASP.NET Core sunucusunda barındırılan bir Blazor uygulaması oluşturmaya yönelik proje şablonu. ASP.NET Core uygulaması Blazor uygulama istemcilere hizmet ancak Aksi durumda, ayrı bir işlemdir. İstemci tarafı Blazor uygulama sunucusu ile Web API çağrıları veya SignalR bağlantıları kullanarak ağ üzerinden etkileşim kurabilirsiniz.

> [!IMPORTANT]
> Bir istemci-tarafı Blazor uygulaması bir IIS alt uygulama barındırılan bir ASP.NET Core uygulaması tarafından sunulan devralınan ASP.NET Core modülü işleyici devre dışı bırakın. Blazor uygulaması'nın uygulama temel yolu ayarla *index.html* IIS diğer IIS alt uygulama yapılandırma sırasında kullanılan dosya.
>
> Daha fazla bilgi için [uygulama temel yolu](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="server-side-hosting-model"></a>Sunucu tarafı barındırma modeli

ASP.NET Core Razor bileşenleri sunucu tarafı barındırma modeli içinde uygulama sunucusunda bir ASP.NET Core uygulaması içinde yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını bir SignalR bağlantısı üzerinden işlenir.

![ASP.NET Core Razor bileşenleri sunucu-tarafı: Tarayıcı uygulaması (bir ASP.NET Core uygulaması içinde barındırılan) ile sunucu üzerinde bir SignalR bağlantısı üzerinden etkileşim kurar.](hosting-models/_static/client-side.png)

Sunucu tarafı barındırma modeli kullanarak bir Razor bileşenleri uygulaması oluşturmak için kullanın **Blazor (sunucu tarafı ASP.NET core'da)** şablonu (`blazorserver` kullanırken [yeni dotnet](/dotnet/core/tools/dotnet-new) bir komut isteminde). ASP.NET Core uygulaması Razor bileşenleri sunucu-tarafı uygulamasını barındıran ve istemcilerin eriştikleri SignalR uç noktasını ayarlar. ASP.NET Core uygulaması uygulamanın başvuran `Startup` sınıfı eklemek için:

* Sunucu tarafı Razor bileşenleri Hizmetleri.
* Ardışık Düzen işleme isteği için uygulama.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

*Blazor.server.js* betik&dagger; istemci bağlantı kurar. Bunu kalıcı hale getirmek ve gerektiğinde (örneğin, durumunda kayıp ağ bağlantısı) uygulama durumunu geri yüklemek için uygulamanın sorumluluğudur.

Sunucu tarafı barındırma modeli, çeşitli avantajlar sunar:

* Tüm .NET uygulamanızla yazmanıza olanak sağlar ve C# bileşen modeli kullanarak.
* Zengin, etkileşimli bir görünümünü sağlar ve gereksiz sayfa yenilenir önler.
* Bir istemci-tarafı Blazor uygulaması daha önemli ölçüde daha küçük bir uygulama boyutu vardır ve çok daha hızlı yükler.
* Bileşen mantığı server özellikleri, herhangi bir .NET Core uyumlu API'sini kullanma dahil olmak üzere tüm avantajlarından yararlanabilirsiniz.
* Araç, hata ayıklama gibi mevcut .NET beklendiği gibi çalışır. Bu nedenle .NET Core üzerinde sunucu üzerinde çalışır.
* Çalışan ince istemciler (örneğin, WebAssembly ve kaynak desteklemeyen tarayıcılar cihazları kısıtlı).

Sunucu tarafı barındırma downsides vardır:

* Daha yüksek gecikme süresi vardır: Her bir kullanıcı etkileşimi bir ağ atlama içerir.
* Çevrimdışı desteği sunar: İstemci bağlantı başarısız olursa, Uygulama çalışmayı durduruyor.
* Ölçeklenebilirlik azalttı: Sunucu, birden çok istemci bağlantılarını yönetme ve istemci durumu işleme.
* Uygulama hizmet vermek için bir ASP.NET Core sunucusu gerekir. Dağıtım bir sunucudan (örneğin, bir CDN) olmadan mümkün değildir.

&dagger;*Blazor.server.js* betik aşağıdaki yola yayımlanan: *bin / {hata ayıklama | Yayın} / {hedef çerçeve} /publish/ {uygulama-adı}. Uygulama/dağıtım/_framework*.
