---
uid: mobile/device-simulators
title: "Test etmek için popüler mobil cihazlar benzetimini | Microsoft Docs"
author: rick-anderson
description: "Bu bağlantıları takip ederek popüler mobil cihazlar ve tarayıcılar için Öykünücüler indirebilirsiniz"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2011
ms.topic: article
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 48145b15b4983d6a143a8c53c9e6e8b4639da91e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Test etmek için popüler mobil cihazlar benzetimi
====================
> Bu bağlantıları takip ederek popüler mobil cihazlar ve tarayıcılar için Öykünücüler indirebilirsiniz


| Cihaz veya tarayıcı | Öykünücü / Simulator |
| --- | --- |
| Tarayıcı sanallaştırma BrowserStack barındırılan ![Tarayıcı sanallaştırma BrowserStack barındırılan](device-simulators/_static/image1.png) | [BrowserStack barındırılan tarayıcı sanallaştırma](http://browserstack.com) herhangi bir platformda herhangi bir tarayıcıda yerel veya üretim ortamınıza sınayın. Bir tünel kendi barındırılan sanal makine, makine ve BrowserStack ağ arasında oluşturabilirsiniz. Aldığınızdan emin olun [BrowserStack Visual Studio Uzantısı](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) daha sorunsuz bir deneyim. |
| Windows Phone | [Windows Phone SDK indirmeleri](https://dev.windowsphone.com/en-us/downloadsdk) Windows Phone Yazılım Geliştirme Seti (SDK) tüm Windows Phone için uygulama ve oyun geliştirmek için gereken araçları içerir |
| iPhone / iPod / iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) bir Responsive yanı sıra, iPhone ve iPad benzeticileri Windows için tasarım aracı. VS 2012 "İle tara..." seçeneği ile tümleştirebilirsiniz. |
| Android | [Android SDK Giriş sayfası](https://developer.android.com/sdk) |
| Opera Mobile / Opera Mini | En son sürümleri: [Opera geliştirici araçları ev](http://www.opera.com/developer/tools/) Opera Mini 4.2: [çevrimiçi Java tabanlı simulator](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Windows Mobile 6.5.3 Geliştirici Araç Seti](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) telefon ağ erişimi vermek için aynı zamanda dahil VPC ağ bağdaştırıcısının gerektiğini unutmayın [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). IE bağlanmak için Visual Studio geliştirme sunucunuza telefonda bkz [Kiran Patil'ın blog gönderisi](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 | [Öykünücü görüntüleri için Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

(Windows için doğru hiçbir öykünücüsü olduğundan iPhone veya iPad, tam olarak test için tek seçenek olan) gerçek mobil aygıttaki uygulamanızı görüntülemek istiyorsanız, uygulamanızı IIS veya IIS Express barındırmak gerekeceğini unutmayın. Bu diğer makinelerden isteklerine yanıt vermiyor kolayca Visual Studio'nun yerleşik web sunucusu bu için kullanamazsınız, çünkü.
