---
title: Razor bileşenleri hata ayıklama
author: guardrex
description: Blazor ve Razor bileşenleri uygulamalarında hata ayıklama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/debug
ms.openlocfilehash: fb7ddcf3ae40ec28a372adf724a293b375be28a1
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668159"
---
# <a name="debug-razor-components"></a>Razor bileşenleri hata ayıklama

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Razor bileşenleri olan bazı *çok erken* WebAssembly chrome'da çalışan istemci-tarafı Blazor uygulamalarında hata ayıklamaya yönelik destek. Bu ilk hata ayıklama desteği çok sınırlı ve unpolished olsa da, temel hata ayıklama altyapısını bir araya geldiği gösterilmektedir.

Bir istemci-tarafı Blazor Chrome uygulamasında hata ayıklamak için:

* Blazor uygulama yapı içinde `Debug` yapılandırma (yayımlanmamış uygulamalar için varsayılan).
* (Sürüm 70 veya sonrası) Chrome'da Blazor uygulamayı çalıştırın.
* Uygulamasında (daha az karmaşık bir hata ayıklama deneyimi için büyük olasılıkla kapatmalısınız değil geliştirme araçları paneli,) klavye odağı ile aşağıdaki Blazor özgü klavye kısayolunu seçin:
  * `Shift+Alt+D` Windows/Linux üzerinde
  * `Shift+Cmd+D` MacOS üzerinde

Chrome uzak Blazor uygulamanın hatalarını ayıklamak için hata ayıklama etkin olan çalıştırın. Uzaktan hata ayıklama devre dışı bırakılırsa, bir hata sayfası Chrome tarafından oluşturulur. Hata sayfası Blazor hata ayıklama proxy'si uygulamasına bağlanabilmesi Chrome ile hata ayıklama bağlantı noktası açık çalıştırmak için yönergeler içerir. *Tüm Chrome örneklerini kapatın* ve Chrome belirtildiği gibi yeniden başlatın.

![Blazor hata ayıklama hata sayfası](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

Uzaktan hata ayıklama etkin olan Chrome çalıştırıldıktan sonra hata ayıklama klavye kısayolunu yeni bir hata ayıklayıcı sekmesi açılır. Kısa bir süre sonra *kaynakları* sekmesi, uygulamada .NET derlemelerin bir listesini gösterir. Her derleme genişletin ve bulma *.cs*/*.cshtml* kaynak dosyaları, hata ayıklama için kullanılabilir. Kesme noktaları belirleyin, uygulamanın sekmesine dönün ve kesme noktaları isabet. Sonra bir kesme noktası İsabeti, tek adımlı olduğu (`F10`) veya sürdürme (`F8`) normal şekilde.

![Blazor hata ayıklama](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

Blazor uygulayan bir hata ayıklama proxy'si sağlar [Chrome DevTools Protokolü](https://chromedevtools.github.io/devtools-protocol/) ve protokolü ile artırmaktadır. NET özgü bilgileri. Hata ayıklama klavye kısayolu basıldığında Blazor Chrome DevTools proxy işaret eder. Hata ayıklamak için aradığınız tarayıcı penceresine proxy bağlar (Bu nedenle uzaktan hata ayıklamayı etkinleştirmek için gereken).

Neden biz yalnızca tarayıcı kaynak eşlemeleri kullanmayın merak ediyor olabilirsiniz. Kaynak eşlemeleri tarayıcının derlenmiş dosyalar geri orijinal kaynak dosyalarına eşleme izin verin. Blazor değil ancak harita C# JS/WASM (en az henüz) için doğrudan. Bunun yerine, kaynak eşlemeleri uygun olmayan şekilde Blazor IL yorumlayıcısı içinde tarayıcı yapar.

Hata ayıklayıcı özellikleri Not **çok sınırlı**. Şu anda yalnızca yapabilecekleriniz:

* Ayarlayın ve kesme noktalarını kaldırın.
* Tek-sürdürme ve kodu adımlayın (`F8`).
* İçinde *Yereller* görüntülemek, tür yerel değişkenlerin değerlerini gözlemleyin `int`, `string`, ve `bool`.
* .NET içinde JavaScript ve .NET, JavaScript için Git çağrı zincirleri dahil olmak üzere çağrı yığını bakın.

*Olamaz*:

* Değerlerin değil herhangi bir yerel öğeler gözlemleyin bir `int`, `string`, veya `bool`.
* Herhangi bir sınıf özelliklerini veya alanlarını değerlerini gözlemleyin.
* Değerleri görmek için değişkenler üzerinde gezdirin
* Konsolunda ifadeleri değerlendirin.
* Zaman uyumsuz çağrıları boyunca ilerleyin.
* Çoğu diğer normal hata ayıklama senaryoları gerçekleştirin.

Daha fazla hata ayıklama senaryoları biri geliştirme bir devam eden mühendislik ekibinin biridir.

## <a name="troubleshooting-tip"></a>Sorun giderme İpucu

Hatalarla karşılaşırsanız çalıştırıyorsanız, aşağıdaki ipucu yardımcı olabilir:

İçinde **hata ayıklayıcı** sekmesinde, tarayıcınızda geliştirici araçlarını açın. Konsolda, yürütme `localStorage.clear()` tüm kesme noktalarını kaldırmak için.
