---
title: Blazor barındırma modelleri
author: guardrex
description: İstemci tarafı Blazor ve sunucu tarafı ASP.NET Core Razor modelleri barındırma bileşenlerini anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/hosting-models
ms.openlocfilehash: 0e7598c9f27201a989d1088764e09461a37beae4
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614848"
---
# <a name="blazor-hosting-models"></a>Blazor barındırma modelleri

Tarafından [Daniel Roth](https://github.com/danroth27)

Blazor istemci-tarafı çalışmak üzere tasarlanmış bir web çerçevesi olan tarayıcıda bir [WebAssembly](http://webassembly.org/)-.NET çalışma zamanı tabanlı (*Blazor*) veya sunucu tarafı ASP.NET Core (*ASP.NET Core Razor bileşenleri*). Barındırma modeli, uygulama ve bileşen modelleri bakılmaksızın *aynı kalması*. Bu makalede, kullanılabilen barındırma modelleri açıklanmaktadır:

* [İstemci tarafı Blazor](#client-side-hosting-model)
* [Sunucu tarafı ASP.NET Core Razor bileşenleri](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a>İstemci tarafı barındırma modeli

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Asıl barındırma için Blazor WebAssembly tarayıcıda çalışan istemci-tarafı modelidir. Tarayıcıya .NET çalışma zamanı Blazor uygulamayı ve bağımlılıkları indirilir. Uygulamayı doğrudan tarayıcıda kullanıcı Arabirimi iş parçacığında yürütülür. Kullanıcı Arabirimi güncelleştirmeleri ve olay işleme, aynı işlem içinde oluşur. Statik dosya olarak bir web sunucusu veya hizmeti statik içeriği istemcilere hizmet uygulamasının varlıklarını dağıtılır.

![Blazor istemci-tarafı: Tarayıcı içinde bir kullanıcı Arabirimi iş parçacığında Blazor uygulama çalışır.](hosting-models/_static/client-side.png)

İstemci tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için aşağıdaki şablonlardan birini kullanın:

* **Blazor** ([dotnet yeni blazor](/dotnet/core/tools/dotnet-new)) &ndash; statik dosyalar bir dizi dağıtılabilir.
* **Blazor (ASP.NET Core barındırılan)** ([dotnet yeni blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; bir ASP.NET Core sunucusu tarafından barındırılan. ASP.NET Core uygulaması Blazor uygulama istemcilere hizmet. Web API çağrıları kullanarak ağ üzerinden istemci-tarafı Blazor uygulama sunucusu ile etkileşim kurabilir veya [SignalR](xref:signalr/introduction).

Şablonları içerir *components.webassembly.js* işleyen betik:

* .NET çalışma zamanı, uygulama ve uygulamanın bağımlılıklarını karşıdan yükleniyor.
* Uygulamayı çalıştırmak için çalışma zamanı başlatma.

İstemci tarafı barındırma modeli, çeşitli avantajlar sunar. İstemci tarafı Blazor:

* .NET sunucu tarafı bağımlılığı yoktur.
* İstemci kaynakları ve özellikleri tam olarak yararlanır.
* Yük boşaltma, sunucudan istemciye çalışır.
* Çevrimdışı senaryolarını destekler.

İstemci tarafı barındırma downsides vardır. İstemci tarafı Blazor:

* Uygulama, tarayıcının yeteneklerini kısıtlar.
* Özellikli istemci donanım ve yazılım (örneğin, WebAssembly desteği) gerektirir.
* İndirme boyutu daha büyük ve uzun uygulama yükleme süresi vardır.
* .NET çalışma zamanı ve araç desteği yetişkin daha az olan (örneğin, kısıtlamaları [.NET Standard](/dotnet/standard/net-standard) desteği ve hata ayıklama).

## <a name="server-side-hosting-model"></a>Sunucu tarafı barındırma modeli

Sunucu tarafı barındırma modeli ile uygulama sunucusunda bir ASP.NET Core uygulaması içinde yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.

![Tarayıcı uygulaması (bir ASP.NET Core uygulaması içinde barındırılan) ile sunucu üzerinde bir SignalR bağlantısı üzerinden etkileşim kurar.](hosting-models/_static/server-side.png)

Sunucu tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core kullanan **Razor bileşenleri** şablonu ([dotnet yeni razorcomponents](/dotnet/core/tools/dotnet-new)). ASP.NET Core uygulaması Razor bileşenleri sunucu-tarafı uygulamasını barındıran ve istemcilerin eriştikleri SignalR uç noktasını ayarlar. ASP.NET Core uygulaması uygulamanın başvuran `Startup` sınıfı eklemek için:

* Sunucu tarafı Razor bileşenleri Hizmetleri.
* Ardışık Düzen işleme isteği için uygulama.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

*Components.server.js* betik&dagger; istemci bağlantı kurar. Bunu kalıcı hale getirmek ve gerektiğinde (örneğin, durumunda kayıp ağ bağlantısı) uygulama durumunu geri yüklemek için uygulamanın sorumluluğudur.

Sunucu tarafı barındırma modeli, çeşitli avantajlar sunar. Sunucu tarafı Razor bileşenler:

* Bir istemci-tarafı Blazor uygulaması daha önemli ölçüde daha küçük bir uygulama boyuta sahiptir ve çok daha hızlı yükleyin.
* Sunucu özellikleri, herhangi bir .NET Core uyumlu API'sini kullanma dahil olmak üzere tüm avantajlarından yararlanabilirsiniz.
* Araç, hata ayıklama gibi mevcut .NET beklendiği gibi çalışır. Bu nedenle .NET Core üzerine sunucusunda çalıştırın.
* Çalışan ince istemciler (örneğin, WebAssembly ve kaynak desteklemeyen tarayıcılar cihazları kısıtlı).
* .NET /C# kod tabanına uygulama bileşeni kod dahil olmak üzere, istemcilere hizmet değil.

Sunucu tarafı barındırma downsides vardır. Sunucu tarafı Razor bileşenler:

* Daha yüksek gecikme süresi vardır: Her bir kullanıcı etkileşimi bir ağ atlama içerir.
* Çevrimdışı desteği sunar: İstemci bağlantı başarısız olursa, Uygulama çalışmayı durduruyor.
* Ölçeklenebilirlik tüketildikleri: Sunucu, birden çok istemci bağlantılarını yönetme ve istemci durumu işleme.
* Uygulama hizmet vermek için bir ASP.NET Core sunucusu gerekir. Dağıtım bir sunucudan (örneğin, bir CDN) olmadan mümkün değildir.

&dagger;*Components.server.js* betik aşağıdaki yola yayımlanan: *bin / {hata ayıklama | Yayın} / {hedef çerçeve} /publish/ {uygulama-adı}. Uygulama/dağıtım/_framework*.
