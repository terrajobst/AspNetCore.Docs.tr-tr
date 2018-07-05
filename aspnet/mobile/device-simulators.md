---
uid: mobile/device-simulators
title: Test için popüler Mobil cihazların benzetimini yapma | Microsoft Docs
author: rick-anderson
description: Bu bağlantıları takip ederek öykünücüleri popüler mobil cihazlar ve tarayıcılar için indirebilirsiniz.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2011
ms.topic: article
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
ms.technology: ''
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: b3dbb4dc83b602cd1aa374d3aa05c1b09a5dfe64
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396090"
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Test için popüler Mobil cihazların benzetimini yapma
====================
> Bu bağlantıları takip ederek öykünücüleri popüler mobil cihazlar ve tarayıcılar için indirebilirsiniz.


| Cihaz veya tarayıcı | Öykünücü / simülatör |
| --- | --- |
| Tarayıcı sanallaştırma BrowserStack barındırılan ![Tarayıcı sanallaştırma BrowserStack barındırılan](device-simulators/_static/image1.png) | [BrowserStack barındırılan tarayıcı sanallaştırma](http://browserstack.com) herhangi bir platformda herhangi bir tarayıcıda, yerel veya üretim ortamı test edin. Makinenize ve BrowserStack ağı arasında bir tünel kendi barındırılan bir sanal makine içinde oluşturabilirsiniz. Aldığınızdan emin olun [BrowserStack Visual Studio Uzantısı](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) daha sorunsuz bir deneyim. |
| Windows Phone | [Windows Phone SDK indirmeleri](https://dev.windowsphone.com/downloadsdk) Windows Phone Yazılım Geliştirme Seti (SDK) için Windows Phone uygulamaları ve oyunları geliştirmek için gereken araçların tümünü içerir |
| iPhone / iPod / iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) için iPhone ve iPad Simülatörleri Windows yanı sıra bir duyarlı tasarım aracı. VS 2012 "İle Gözat..." seçeneği ile tümleştirebilirsiniz. |
| Android | [Android SDK Giriş sayfası](https://developer.android.com/sdk) |
| Opera Mobile / Opera Mini | En son sürümleri: [Opera geliştirici araçları giriş](http://www.opera.com/developer/tools/) Opera Mini 4.2: [çevrimiçi Java tabanlı simülatör](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Windows Mobile 6.5.3 Geliştirici Araç Seti](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) telefon ağ erişimi vermek için aynı zamanda dahil VPC ağ bağdaştırıcısı gerektiğini unutmayın [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). IE bağlanmak için Visual Studio geliştirme sunucusu için bir telefonda görmek [Kiran Patil'ın blog gönderisi](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 cihazları | [Öykünücü görüntüleri için Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

(Windows için doğru hiçbir öykünücü olduğundan, iPhone veya iPad, tam olarak test için tek seçenek olan) gerçek mobil bir cihazda uygulamanızı görüntülemek istiyorsanız, IIS veya IIS Express uygulamanızı barındırmak gerektiğini unutmayın. Diğer makinelerden isteklerine yanıt vermesi gerekmez çünkü kolayca Visual Studio'nun yerleşik web sunucusu bunun için kullanamazsınız.
