---
title: Hata ayıklama ASP.NET Core Blazor
author: guardrex
description: Blazor uygulamalarında hata ayıklamayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/22/2019
uid: blazor/debug
ms.openlocfilehash: e9477e504d32fd1dd5d6c87392386d1131f46e9f
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964004"
---
# <a name="debug-aspnet-core-blazor"></a>Hata ayıklama ASP.NET Core Blazor

[Daniel Roth](https://github.com/danroth27)

Chrome 'da WebAssembly üzerinde çalışan Blazor WebAssembly uygulamalarında hata ayıklama için *erken* destek mevcuttur.

Hata ayıklayıcı özellikleri sınırlıdır. Kullanılabilir senaryolar şunlardır:

* Kesme noktaları ayarlayın ve kaldırın.
* Code veya özgeçmişi (`F10``F8`) kod yürütme aracılığıyla tek adımlı ().
* *Yereller* görünümü ' nde, ve `int` `bool`türündeki `string`tüm yerel değişkenlerin değerlerini gözlemleyin.
* JavaScript 'ten .NET 'e ve .NET 'ten JavaScript 'e gidecek çağrı zincirleri dahil olmak üzere çağrı yığınına bakın.

Şunları *yapamazsınız*:

* `int` ,`string`Veya olmayan`bool`tüm Yereller için değerleri gözlemleyin.
* Herhangi bir sınıf özelliklerinin veya alanının değerlerini gözlemleyin.
* Değerlerini görmek için değişkenlerin üzerine gelin.
* Konsolunda ifadeleri değerlendirin.
* Zaman uyumsuz çağrılar arasında adımla.
* Diğer birçok sıradan hata ayıklama senaryosunu gerçekleştirin.

Daha fazla hata ayıklama senaryosu geliştirmesi, mühendislik ekibinin bir odadır.

## <a name="prerequisites"></a>Önkoşullar

Hata ayıklama aşağıdaki tarayıcılardan birini gerektirir:

* Google Chrome (sürüm 70 veya üzeri)
* Microsoft Edge Önizlemesi ([Edge geliştirme kanalı](https://www.microsoftedgeinsider.com))

## <a name="procedure"></a>Yordam

1. `Debug` Yapılandırmada bir Blazor webassembly uygulaması çalıştırın. Seçeneği DotNet [Run](/dotnet/core/tools/dotnet-run) komutuna geçirin: `dotnet run --configuration Debug`. `--configuration Debug`
1. Uygulamaya tarayıcıda erişin.
1. Klavye odağını geliştirici araçları paneline değil uygulamaya yerleştirin. Hata ayıklama başlatıldığında geliştirici araçları paneli kapatılabilir.
1. Aşağıdaki Blazor özgü klavye kısayolunu seçin:
   * `Shift+Alt+D`Windows/Linux 'ta
   * `Shift+Cmd+D`macOS 'ta
1. Uzaktan hata ayıklama etkinken tarayıcıyı yeniden başlatmak için ekranda listelenen adımları izleyin.
1. Hata ayıklama oturumu başlatmak için aşağıdaki Blazor özgü klavye kısayolunu bir kez daha seçin:
   * `Shift+Alt+D`Windows/Linux 'ta
   * `Shift+Cmd+D`macOS 'ta

## <a name="enable-remote-debugging"></a>Uzaktan hata ayıklamayı etkinleştir

Uzaktan hata ayıklama devre dışıysa, **hata ayıklanabilir Browser sekmesi** hata sayfası Chrome tarafından oluşturulur. Hata sayfası, Blazor hata ayıklama proxy 'sinin uygulamaya bağlanabilmesi için hata ayıklama bağlantı noktası açıkken Chrome çalıştırmaya yönelik yönergeler içerir. *Tüm Chrome örneklerini kapatın* ve belirtildiği şekilde Chrome 'u yeniden başlatın.

## <a name="debug-the-app"></a>Uygulamada hata ayıklama

Uzaktan hata ayıklama etkinken Chrome çalışmaya başladıktan sonra hata ayıklama klavye kısayolu yeni bir hata ayıklayıcı sekmesi açar. Bir süre sonra, **kaynaklar** sekmesi uygulamadaki .net derlemelerinin listesini gösterir. Her bir derlemeyi genişletin ve hata ayıklama için kullanılabilen *. cs*/ *. Razor* kaynak dosyalarını bulun. Kesme noktaları ayarlayın, uygulamanın sekmesine geri dönün ve kod yürütüldüğünde kesme noktaları isabet edilir. Kesme noktası isabet ettikten sonra, kod ya da özgeçmişi`F10`(`F8`) kodu üzerinden tek adımlı () kod yürütmeyi normal şekilde yapın.

Blazor, [Chrome DevTools protokolünü](https://chromedevtools.github.io/devtools-protocol/) uygulayan ve protokolünü ile genişlettiğini içeren bir hata ayıklama proxy 'si sağlar. NET 'e özgü bilgiler. Klavye kısayolunun hata ayıklaması basıldığında Blazor, ara sunucu üzerindeki Chrome DevTools 'u işaret eder. Proxy, hata ayıklama işlemini Aradığınız tarayıcı penceresine bağlanır (Bu nedenle, uzaktan hata ayıklamayı etkinleştirmeniz gerekir).

## <a name="browser-source-maps"></a>Tarayıcı kaynağı eşlemeleri

Tarayıcı kaynak haritaları tarayıcının derlenmiş dosyaları özgün kaynak dosyalarına geri eşlemesine ve istemci tarafı hata ayıklama için yaygın olarak kullanılmasına izin verir. Ancak, Blazor Şu anda doğrudan C# JavaScript/, ile eşlenmiyor. Bunun yerine Blazor, tarayıcı içinde Il yorumu yapar, bu nedenle kaynak haritaları ilgili değildir.

## <a name="troubleshooting-tip"></a>Sorun giderme ipucu

Hatalar halinde çalıştırıyorsanız, aşağıdaki ipucu yardımcı olabilir:

**Hata ayıklayıcı** sekmesinde, tarayıcınızda Geliştirici Araçları ' nı açın. Konsolunda, tüm kesme noktalarını `localStorage.clear()` kaldırmak için yürütün.
