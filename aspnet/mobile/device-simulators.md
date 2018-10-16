---
uid: mobile/device-simulators
title: Test için popüler Mobil cihazların benzetimini yapma | Microsoft Docs
author: rick-anderson
description: Bu bağlantıları takip ederek öykünücüleri popüler mobil cihazlar ve tarayıcılar için indirebilirsiniz.
ms.author: riande
ms.date: 10/11/2018
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 8299295154d23f8fc430676b2c8ad8efc98ad185
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348383"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>Test için popüler Mobil cihazların benzetimini yapma

> Bu bağlantıları takip ederek öykünücüleri popüler mobil cihazlar ve tarayıcılar için indirebilirsiniz.

| Cihaz veya tarayıcı | Öykünücü / simülatör |
| --- | --- |
| Tarayıcı sanallaştırma BrowserStack barındırılan ![Tarayıcı sanallaştırma BrowserStack barındırılan](device-simulators/_static/image1.png) | [BrowserStack barındırılan tarayıcı sanallaştırma](http://browserstack.com) herhangi bir platformda herhangi bir tarayıcıda, yerel veya üretim ortamı test edin. Makinenize ve BrowserStack ağı arasında bir tünel kendi barındırılan bir sanal makine içinde oluşturabilirsiniz. Aldığınızdan emin olun [BrowserStack Visual Studio Uzantısı](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack) daha sorunsuz bir deneyim. |
| iPhone / iPod / iPad | [Elektrik mobil Studio](http://www.electricplum.com/studio.aspx) için iPhone ve iPad Simülatörleri Windows yanı sıra bir duyarlı tasarım aracı. |
| Android | [Android Studio](https://developer.android.com/studio/) veya [Android için Visual Studio öykünücüsü](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Opera Mobile | [Opera Mobile Klasik öykünücüsü](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Windows Mobile 6.5.3 Geliştirici Araç Seti](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) telefon ağ erişimi vermek için aynı zamanda dahil VPC ağ bağdaştırıcısı gerektiğini unutmayın [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). IE bağlanmak için Visual Studio geliştirme sunucusu için bir telefonda görmek [Kiran Patil'ın blog gönderisi](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |

> [!NOTE]
> (Windows için doğru hiçbir öykünücü olduğundan, iPhone veya iPad, tam olarak test için tek seçenek olan) gerçek mobil bir cihazda uygulamanızı görüntülemek istiyorsanız, IIS veya IIS Express uygulamanızda barındırmak gerekir. Diğer makinelerden isteklerine yanıt vermesi gerekmez çünkü kolayca Visual Studio'nun yerleşik web sunucusu bunun için kullanamazsınız.