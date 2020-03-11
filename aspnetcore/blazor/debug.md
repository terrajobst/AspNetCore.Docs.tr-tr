---
title: Hata ayıklama ASP.NET Core Blazor
author: guardrex
description: Blazor uygulamalarda hata ayıklamayı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661709"
---
# <a name="debug-aspnet-core-blazor"></a>Hata ayıklama ASP.NET Core Blazor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Kmıum tabanlı tarayıcılarda (Chrome/Edge) tarayıcı geliştirme araçlarını kullanarak Blazor WebAssembly hata ayıklaması için *erken* destek mevcuttur. İş şu şekilde devam ediyor:

* Visual Studio 'da hata ayıklamayı tamamen etkinleştirin.
* Visual Studio Code hata ayıklamayı etkinleştir.

Hata ayıklayıcı özellikleri sınırlıdır. Kullanılabilir senaryolar şunlardır:

* Kesme noktaları ayarlayın ve kaldırın.
* Kod yürütme (`F8`) kodu üzerinden tek adımlı (`F10`).
* *Yereller* görünümü ' nde, `int`, `string`ve `bool`türündeki tüm yerel değişkenlerin değerlerini gözlemleyin.
* JavaScript 'ten .NET 'e ve .NET 'ten JavaScript 'e gidecek çağrı zincirleri dahil olmak üzere çağrı yığınına bakın.

Şunları *yapamazsınız*:

* `int`, `string`veya `bool`olmayan herhangi bir yerelin değerlerini gözlemleyin.
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

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

> [!WARNING]
> Visual Studio 'da hata ayıklama desteği, daha erken bir geliştirme aşamasıdır. **F5** hata ayıklaması Şu anda desteklenmiyor.

1. Hata ayıklamadan `Debug` yapılandırmasında bir Blazor WebAssembly uygulaması**çalıştırın (** **f5**yerine **F5+F5** ).
1. Uygulamanın hata ayıklama özelliklerini açın ( **hata ayıklama** menüsünde son giriş) ve http **Uygulama URL**'sini kopyalayın. Bir Kmıum tabanlı tarayıcı (Edge beta veya Chrome) kullanarak uygulamanın HTTP adresine (HTTPS adresine değil) göz atabilirsiniz.
1. Klavye odağını geliştirici araçları paneline değil tarayıcı penceresinde uygulamaya yerleştirin. Geliştirici araçları bölmesinin Bu yordam için kapalı tutulması en iyisidir. Hata ayıklama başlatıldıktan sonra geliştirici araçları panelini yeniden açabilirsiniz.
1. Aşağıdaki Blazor özgü klavye kısayolunu seçin:

   * Windows üzerinde `Shift+Alt+D`
   * macOS üzerinde `Shift+Cmd+D`

   **Hata ayıklanabilir tarayıcısı bulunamıyor sekmesine**sahipseniz bkz. [Uzaktan hata ayıklamayı etkinleştir](#enable-remote-debugging).
   
   Uzaktan hata ayıklamayı etkinleştirdikten sonra:
   
   1 \. Yeni bir tarayıcı penceresi açılır. Önceki pencereyi kapatın.

   2 \. Klavye odağını uygulamayı tarayıcı penceresinde yerleştirin.

   3 \. Yeni tarayıcı penceresinde Blazor özgü klavye kısayolunu seçin: Windows üzerinde `Shift+Alt+D` veya macOS üzerinde `Shift+Cmd+D`.

   4 \. **Devtools** sekmesi tarayıcıda açılır. **Tarayıcı penceresinde uygulamanın sekmesini yeniden seçin.**

   Uygulamayı Visual Studio 'ya eklemek için, [Visual Studio 'da Işleme iliştirme](#attach-to-process-in-visual-studio) bölümüne bakın.

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. `--configuration Debug` seçeneğini [DotNet Run](/dotnet/core/tools/dotnet-run) komutuna geçirerek `Debug` yapılandırmada bir Blazor WebAssembly uygulaması çalıştırın: `dotnet run --configuration Debug`.
1. Kabuk penceresinde gösterilen HTTP URL 'sindeki uygulamaya gidin.
1. Klavye odağını geliştirici araçları paneline değil uygulamaya yerleştirin. Geliştirici araçları bölmesinin Bu yordam için kapalı tutulması en iyisidir. Hata ayıklama başlatıldıktan sonra geliştirici araçları panelini yeniden açabilirsiniz.
1. Aşağıdaki Blazor özgü klavye kısayolunu seçin:

   * Windows üzerinde `Shift+Alt+D`
   * macOS üzerinde `Shift+Cmd+D`

   **Hata ayıklanabilir tarayıcısı bulunamıyor sekmesine**sahipseniz bkz. [Uzaktan hata ayıklamayı etkinleştir](#enable-remote-debugging).
   
   Uzaktan hata ayıklamayı etkinleştirdikten sonra:
   
   1 \. Yeni bir tarayıcı penceresi açılır. Önceki pencereyi kapatın.

   2 \. Klavye odağını geliştirici araçları paneline değil tarayıcı penceresinde uygulamaya yerleştirin.

   3 \. Yeni tarayıcı penceresinde Blazor özgü klavye kısayolunu seçin: Windows üzerinde `Shift+Alt+D` veya macOS üzerinde `Shift+Cmd+D`.

---

## <a name="enable-remote-debugging"></a>Uzaktan hata ayıklamayı etkinleştir

Uzaktan hata ayıklama devre dışıysa, **hata ayıklanabilir Browser sekmesi** hata sayfası Chrome tarafından oluşturulur. Hata sayfası, hata ayıklama proxy 'sinin uygulamaya bağlanabilmesi için hata ayıklama bağlantı noktası Blazor açıkken Chrome çalıştırmaya yönelik yönergeler içerir. *Tüm Chrome örneklerini kapatın* ve belirtildiği şekilde Chrome 'u yeniden başlatın.

## <a name="debug-the-app"></a>Uygulamada hata ayıklama

Uzaktan hata ayıklama etkinken Chrome çalışmaya başladıktan sonra hata ayıklama klavye kısayolu yeni bir hata ayıklayıcı sekmesi açar. Bir süre sonra, **kaynaklar** sekmesi uygulamadaki .net derlemelerinin listesini gösterir. Her bir derlemeyi genişletin ve hata ayıklama için kullanılabilen *. cs*/ *. Razor* kaynak dosyalarını bulun. Kesme noktaları ayarlayın, uygulamanın sekmesine geri dönün ve kod yürütüldüğünde kesme noktaları isabet edilir. Kesme noktası isabet ettikten sonra kod veya özgeçmişi (`F8`) kod yürütme aracılığıyla tek adımlı (`F10`).

Blazor, [Chrome DevTools protokolünü](https://chromedevtools.github.io/devtools-protocol/) uygulayan ve protokolünü ile genişletgetiren bir hata ayıklama proxy 'si sağlar. NET 'e özgü bilgiler. Klavye kısayolunun hata ayıklaması basıldığında Blazor, ara sunucu üzerindeki Chrome DevTools 'u gösterir. Proxy, hata ayıklama işlemini Aradığınız tarayıcı penceresine bağlanır (Bu nedenle, uzaktan hata ayıklamayı etkinleştirmeniz gerekir).

## <a name="attach-to-process-in-visual-studio"></a>Visual Studio 'da işleme iliştirme

Visual Studio 'da uygulamanın sürecine iliştirme, **F5** hata ayıklaması geliştirilirken Blazor WebAssembly için *geçici* bir hata ayıklama senaryosudur.

Çalışan uygulamanın işlemini Visual Studio 'ya eklemek için:

1. Visual Studio 'da, **Işleme eklemek** > **Hata Ayıkla** ' yı seçin.
1. **Bağlantı türü**için **Chrome DevTools protokol WebSocket (kimlik doğrulaması yok)** öğesini seçin.
1. **Bağlantı hedefi**için, uygulamanın http ADRESINI (https adresini değil) yapıştırın.
1. **Kullanılabilir süreçler**altındaki girdileri yenilemek için **Yenile** ' yi seçin.
1. Hata ayıklama için tarayıcı işlemini seçin ve **Ekle**' yi seçin.
1. **Kod türünü seç** iletişim kutusunda, iliştirmekte olduğunuz belirli tarayıcıya ait kod türünü seçin (kenar veya Chrome) ve ardından **Tamam**' ı seçin.

## <a name="browser-source-maps"></a>Tarayıcı kaynağı eşlemeleri

Tarayıcı kaynak haritaları tarayıcının derlenmiş dosyaları özgün kaynak dosyalarına geri eşlemesine ve istemci tarafı hata ayıklama için yaygın olarak kullanılmasına izin verir. Ancak, Blazor Şu anda doğrudan C# JavaScript/, ile eşlenmiyor. Bunun yerine, Blazor, kaynak haritaları ilgili değil, tarayıcı içinde Il yorumu yapar.

## <a name="troubleshoot"></a>Sorun giderme

Hatalar halinde çalıştırıyorsanız, aşağıdaki ipucu yardımcı olabilir:

**Hata ayıklayıcı** sekmesinde, tarayıcınızda Geliştirici Araçları ' nı açın. Konsolunda, tüm kesme noktalarını kaldırmak için `localStorage.clear()` yürütün.
