---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET IIS dizinlerine erişim reddedildi | Microsoft Docs
author: rick-anderson
description: Bu teknik incelemede ASP.NET uygulamanız için bir istek "DirectoryName dizinine erişim engellendi. hata verirse ne yapmalısınız açıklar. S başarısız oldu...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: c3a14f51df7aaf5c5935cf60ee4e687c10048e91
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753191"
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET IIS dizinlerine erişim reddedildi
====================
> Bu teknik incelemede, ASP.NET uygulamanız için bir istek hata verirse ne yapmalısınız açıklar "erişim izni verilmeyen *DirectoryName* dizin. Dizin değişikliklerini izleme başlatılamadı."
> 
> ASP.NET 1.0 ve 1.1 ASP.NET için geçerlidir.


ASP.NET V1 RTM artık çalışan bir less kullanarak ayrıcalıklı windows hesabı - yerel bir makinede "ASPNET" hesabı olarak kayıtlı.

Bazı üzerinde sistemleri kilitli, bu hesap varsayılan olarak güvenlik erişimi bir Web sitesinin içerik dizinleri, uygulamanın kök dizinine veya web sitesi kök dizini için okuma izniniz olmayabilir. Bu durumda sayfaları belirli bir web uygulamasından isteğinde bulunurken aşağıdaki hatayı alırsınız:

![](denied-access-to-iis-directories/_static/image1.jpg)

Bu sorunu gidermek için uygun dizinleri güvenlik izinlerini değiştirmeniz gerekir.

Özellikle, ASP okuma, yürütün ve listeleme erişimi ASPNET hesabı web sitesinin kök için (örneğin: c:\inetpub\wwwroot veya IIS içinde yapılandırılmış herhangi bir alternatif site dizini), içerik dizinini ve uygulama kök dizini yapılandırma dosya değişiklikleri izlemek için. Uygulama kökü uygulama sanal dizini IIS Yönetim Aracı (inetmgr) içinde ilişkili klasör yoluna karşılık gelir.

Örneğin, aşağıdaki uygulama hiyerarşisi wwwroot klasörü altında göz önünde bulundurun.

`C:\inetpub\wwwroot\myapp\default.aspx`

Bu örnekte, myapp hem de wwwroot dizinini içerik için yukarıda tanımlanan Okuma izinleri ASPNET hesabı gerekir. İç içe Kök klasörde tek bir devralınan ACL isteğe bağlı olarak her iki dizini için de kullanılabilir.

Bir dizin için izinler eklemek için aşağıdaki adımları gerçekleştirin:

- Windows explorer'ı kullanarak, dizine gidin
- Dizin klasörü sağ tıklatın ve "Özellikler" öğesini seçin
- Özellik iletişim kutusundaki "Güvenlik" sekmesine gidin
- "Ekle" düğmesine tıklayın ve ardından ASP.NET hesap adı makine adı girin. Örneğin, "webdev" adlı bir makinede webdev\ASPNET girer ve "Tamam" düğmesine basın.
- ASP.NET hesabından olduğundan emin olun "okuma &amp; Çalıştır", "Klasör içeriğini listele" ve "Okuma" işaretli.
- İletişim kutusunu kapatmak ve değişiklikleri kaydetmek için Tamam'ı tıklatın.

![](denied-access-to-iis-directories/_static/image2.jpg)

İsterseniz, bu değişiklikler ile Windows betikleri veya birlikte verilen "cacls.exe" aracı kullanılarak otomatikleştirilebilir. ASP.NET hesabından hakkında daha fazla bilgi için lütfen bkz [SSS belge](https://go.microsoft.com/fwlink/?LinkId=5828).

Belirli bir web uygulaması yazma olması kullanır veya belirli bir klasör veya dosyanın izinlerini değiştirmek, bu yordamın aynısını uygulayarak ve "Yazma" ve/veya "Değiştirme" onay kutuları denetleyerek verilebilir.

Herkesin veya kullanıcılar grubu okuma erişimini (Bu varsayılan yapılandırma) bu dizinler izin makinelerde sorun ile karşılaşılan ve yukarıdaki adımları gerekmeyecektir.
