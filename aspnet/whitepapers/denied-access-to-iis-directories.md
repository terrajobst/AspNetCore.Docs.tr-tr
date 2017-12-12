---
uid: whitepapers/denied-access-to-iis-directories
title: "ASP.NET IIS dizinlere erişimi reddedildi | Microsoft Docs"
author: rick-anderson
description: "Bu teknik ASP.NET uygulamanız için bir istek \"DirectoryName dizinine erişimi reddedildi. hata verirse yapmanız gerekir açıklar S başarısız..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 64118ac7a5f280775106d2dc7636923b08f28d89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET IIS dizinleri için erişim reddedildi
====================
> Bu teknik ASP.NET uygulamanız için bir istek hata verirse yapmanız gerekir açıklar "erişim reddedildi *DirectoryName* dizini. Dizin chaanges izleme başlatılamadı."
> 
> ASP.NET 1.0 ve 1.1 ASP.NET için geçerlidir.


ASP.NET V1 RTM şimdi çalışıyor daha az kullanılarak ayrıcalıklı windows hesabı - yerel makinede "ASPNET" hesabı olarak kayıtlı.

Bazı üzerinde sistemleri kilitli, bu hesap varsayılan olarak güvenlik erişimi bir Web sitesinin içeriği dizinlerini, uygulama kök dizini ya da web sitesinin kök dizini için okuma izniniz olmayabilir. Bu durumda verilen web uygulamasından sayfaları isteğinde bulunurken aşağıdaki hatayı alırsınız:

![](denied-access-to-iis-directories/_static/image1.jpg)

Bu sorunu gidermek için uygun dizinlerin güvenlik izinlerini değiştirmeniz gerekir.

Özellikle, ASP okuma, yürütme ve listesinde bir web sitesi kök ASPNET hesabı için erişim (örneğin: c:\inetpub\wwwroot veya IIS içinde yapılandırılmış herhangi bir alternatif site dizini), içerik dizinini ve uygulama kök dizini yapılandırma dosyası değişiklikleri izlemek için. Uygulama kök klasör yolu IIS Yönetim Aracı (inetmgr) uygulama sanal dizininde ilişkili karşılık gelir.

Örneğin, aşağıdaki uygulama hiyerarşi altındaki wwwroot klasörüne göz önünde bulundurun.

`C:\inetpub\wwwroot\myapp\default.aspx`

Bu örnekte, ASP.NET hesabından Uygulamam ve wwwroot dizinini içeriği için yukarıda tanımlanan Okuma izinleri olması gerekir. İç içe, kök klasöründe bir tek devralınan ACL isteğe bağlı olarak her iki dizini için kullanılabilir.

Bir dizine izinleri eklemek için aşağıdaki adımları gerçekleştirin:

- Windows Gezgini'ni kullanarak, dizine gidin
- Dizin klasörü sağ tıklatın ve "Özellikler"'i seçin
- Özellik iletişim kutusunda "Güvenlik" sekmesine gidin
- "Ekle" düğmesini tıklatın ve ardından ASPNET hesap adı makine adı girin. Örneğin, "webdev" adlı bir makinede webdev\ASPNET girer ve "Tamam" düğmesine basın.
- ASP.NET hesabından sahip olduğundan emin olun "okuma &amp; Execute", "Klasör içeriğini listele" ve "Okuma" onay kutusu işaretli.
- İletişim kutusunu kapatmak ve değişiklikleri kaydetmek için Tamam'ı tıklatın.

![](denied-access-to-iis-directories/_static/image2.jpg)

İsterseniz, bu değişiklikler ile Windows komut dosyaları veya gelir "cacls.exe" aracını kullanarak otomatik olarak yapılabilir. ASP.NET hesabından hakkında daha fazla bilgi için lütfen bkz [SSS belge](https://go.microsoft.com/fwlink/?LinkId=5828).

Belirli bir web uygulaması yazma sahip kullanır veya belirli klasör veya dosyanın izinlerini değiştirmek, bu "Yazma" ve/veya "Değiştirme" onay kutularını işaretleyerek ve aynı yordamı izleyerek verilebilir.

Herkes veya kullanıcılar grubunun Okuma erişimi (varsayılan yapılandırması olan) bu dizinleri izin makinelerde herhangi bir sorun karşılaştı ve yukarıdaki adımları gerekli değildir.
