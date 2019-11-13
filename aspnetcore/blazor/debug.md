---
title: Hata ayıklama ASP.NET Core Blazor
author: guardrex
description: Blazor uygulamalarda hata ayıklamayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/debug
ms.openlocfilehash: 3096ad9b3a6904804f239d61f374adcd4d851978
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963141"
---
# <a name="debug-aspnet-core-opno-locblazor"></a>Hata ayıklama ASP.NET Core Blazor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Chrome 'da WebAssembly üzerinde çalışan Blazor WebAssembly uygulamalarında hata ayıklama için *erken* destek bulunur.

Hata ayıklayıcı özellikleri sınırlıdır. Kullanılabilir senaryolar şunlardır:

* Kesme noktaları ayarlayın ve kaldırın.
* Kod veya özgeçmişden (`F8`) kod yürütme aracılığıyla tek adımlı (`F10`).
* *Yereller* görünümü ' nde, `int`, `string` ve `bool` türündeki tüm yerel değişkenlerin değerlerini gözlemleyin.
* JavaScript 'ten .NET 'e ve .NET 'ten JavaScript 'e gidecek çağrı zincirleri dahil olmak üzere çağrı yığınına bakın.

Şunları *yapamazsınız*:

* `int`, `string`veya `bool`olmayan herhangi bir yerelin değerlerini gözlemleyin.
* Herhangi bir sınıf özelliklerinin veya alanının değerlerini gözlemleyin.
* Değerlerini görmek için değişkenlerin üzerine gelin.
* Konsolunda ifadeleri değerlendirin.
* Zaman uyumsuz çağrılar arasında adımla.
* Diğer birçok sıradan hata ayıklama senaryosunu gerçekleştirin.

Daha fazla hata ayıklama senaryosu geliştirmesi, mühendislik ekibinin bir odadır.

## <a name="prerequisites"></a>Prerequisites

Hata ayıklama aşağıdaki tarayıcılardan birini gerektirir:

* Google Chrome (sürüm 70 veya üzeri)
* Microsoft Edge Önizlemesi ([Edge geliştirme kanalı](https://www.microsoftedgeinsider.com))

## <a name="procedure"></a>Yordam

1. `Debug` yapılandırmasında bir Blazor WebAssembly uygulaması çalıştırın. `--configuration Debug` seçeneğini [DotNet Run](/dotnet/core/tools/dotnet-run) komutuna geçirin: `dotnet run --configuration Debug`.
1. Uygulamaya tarayıcıda erişin.
1. Klavye odağını geliştirici araçları paneline değil uygulamaya yerleştirin. Hata ayıklama başlatıldığında geliştirici araçları paneli kapatılabilir.
1. Aşağıdaki Blazorözgü klavye kısayolunu seçin:
   * Windows/Linux üzerinde `Shift+Alt+D`
   * macOS üzerinde `Shift+Cmd+D`
1. Uzaktan hata ayıklama etkinken tarayıcıyı yeniden başlatmak için ekranda listelenen adımları izleyin.
1. Hata ayıklama oturumu başlatmak için aşağıdaki Blazorözgü klavye kısayolunu bir kez daha seçin:
   * Windows/Linux üzerinde `Shift+Alt+D`
   * macOS üzerinde `Shift+Cmd+D`

## <a name="enable-remote-debugging"></a>Uzaktan hata ayıklamayı etkinleştir

Uzaktan hata ayıklama devre dışıysa, **hata ayıklanabilir Browser sekmesi** hata sayfası Chrome tarafından oluşturulur. Hata sayfası, hata ayıklama proxy 'sinin uygulamaya bağlanabilmesi için hata ayıklama bağlantı noktası Blazor açıkken Chrome çalıştırmaya yönelik yönergeler içerir. *Tüm Chrome örneklerini kapatın* ve belirtildiği şekilde Chrome 'u yeniden başlatın.

## <a name="debug-the-app"></a>Uygulamada hata ayıklama

Uzaktan hata ayıklama etkinken Chrome çalışmaya başladıktan sonra hata ayıklama klavye kısayolu yeni bir hata ayıklayıcı sekmesi açar. Bir süre sonra, **kaynaklar** sekmesi uygulamadaki .net derlemelerinin listesini gösterir. Her bir derlemeyi genişletin ve hata ayıklama için kullanılabilen *. cs*/ *. Razor* kaynak dosyalarını bulun. Kesme noktaları ayarlayın, uygulamanın sekmesine geri dönün ve kod yürütüldüğünde kesme noktaları isabet edilir. Kesme noktası isabet ettikten sonra, kod veya özgeçmişden (`F8`) kod yürütme işlemi normal şekilde tek adımlı (`F10`).

Blazor, [Chrome DevTools protokolünü](https://chromedevtools.github.io/devtools-protocol/) uygulayan ve protokolünü ile genişletgetiren bir hata ayıklama proxy 'si sağlar. NET 'e özgü bilgiler. Klavye kısayolunun hata ayıklaması basıldığında Blazor, ara sunucu üzerindeki Chrome DevTools 'u gösterir. Proxy, hata ayıklama işlemini Aradığınız tarayıcı penceresine bağlanır (Bu nedenle, uzaktan hata ayıklamayı etkinleştirmeniz gerekir).

## <a name="browser-source-maps"></a>Tarayıcı kaynağı eşlemeleri

Tarayıcı kaynak haritaları tarayıcının derlenmiş dosyaları özgün kaynak dosyalarına geri eşlemesine ve istemci tarafı hata ayıklama için yaygın olarak kullanılmasına izin verir. Ancak, Blazor Şu anda doğrudan C# JavaScript/, ile eşlenmiyor. Bunun yerine, Blazor, kaynak haritaları ilgili değil, tarayıcı içinde Il yorumu yapar.

## <a name="troubleshooting-tip"></a>Sorun giderme ipucu

Hatalar halinde çalıştırıyorsanız, aşağıdaki ipucu yardımcı olabilir:

**Hata ayıklayıcı** sekmesinde, tarayıcınızda Geliştirici Araçları ' nı açın. Konsolunda, tüm kesme noktalarını kaldırmak için `localStorage.clear()` yürütün.
