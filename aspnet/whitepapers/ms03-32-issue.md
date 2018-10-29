---
uid: whitepapers/ms03-32-issue
title: IE için güvenlik güncelleştirmesi uygulandıktan sonra 'Sunucu uygulaması kullanılamıyor' hatası için düzeltme | Microsoft Docs
author: rick-anderson
description: Bu incelemede, bir sorunu gideren MS03 32 güvenlik güncelleştirmesiyle Wi üzerinde çalışan ASP.NET 1.0 uygulamaları etkiler Internet Explorer için düzeltme eki anlatılmaktadır...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 9041f8d15a449a517594f8051c3d9f0ceb18a8a3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207023"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>IE için güvenlik güncelleştirmesi uygulandıktan sonra 'Sunucu uygulaması kullanılamıyor' hatası için düzeltme
====================
> Bu yazıda, Internet Explorer, Windows XP Professional üzerinde çalışan ASP.NET 1.0 uygulamaları etkiler MS03 32 güvenlik güncelleştirmesiyle bir sorunu giderir düzeltme açıklar.
> 
> ASP.NET 1.0 ve Windows XP Professional için geçerlidir.


Microsoft Internet Explorer güvenlik düzeltme ekini MS03 32 güvenlik güncelleştirmesi ve Windows XP'de çalışan ASP.NET 1.0 ile bir sorun belirledik. Bu düzeltme ekini el ile veya Windows Update sitesinden yeni kritik güncelleştirmeler edinme yüklenebilir.

Bu sorunun belirtisi, Windows XP makine üzerinde düzeltme eki yükledikten sonra yerel IIS 5.1 web sunucusu üzerinde çalışan ASP.NET uygulamaları için tüm istekleri "Sunucu uygulaması kullanılamıyor" belirten bir hata iletisi neden olur. Uzak web sunucuları istekleri bundan etkilenmez.

Bu sorun, yalnızca ASP.NET 1.0 Windows XP'de çalışan yüklemeleri etkiler. Windows 2000 veya Windows Server 2003 çalıştıran makineleri etkilemez. Aynı zamanda ASP.NET 1.1 ile Windows XP çalıştıran makineleri etkilemez.

Unutmayın, bu sorunu **değil** ASP.NET bir güvenlik hatası. Bunu **yok** açmanız veya herhangi bir ASP.NET uygulamasını veya server kötü amaçlı saldırıları izin verin. Bunun yerine, bu düzeltme ekiyle neden yalnızca bir işlev hatadır.

Bu sorun için kalıcı bir çözüm üzerinde çalışıyoruz. Bu sırada, sorun için geçici bir çözüm olarak şu toplu iş dosyasını çalıştırabilirsiniz. Toplu iş dosyası şunları yapar:

1. IIS ve ASP.NET durum hizmetleri durdurur
2. Siler ve bilinen bir geçici parola ASPNET hesabıyla oluşturur
3. Windows kullanan `runas` bir ASP.NET kullanıcı profili oluşturan bir yürütülebilir dosyayı başlatmak için komut
4. ASP.NET yeniden kaydeder. Bu hesap için yeni rastgele bir parola oluşturur ve onu için varsayılan ASP.NET erişim denetimi ayarlarını uygular
5. IIS hizmetini yeniden başlatır

Toplu iş dosyası, sabit kodlanmış geçici bir parola içeren "<strong>1pass\@word</strong>", olacağı için toplu iş dosyasını çalıştırdığınızda runas komutunu girmesi istenir. Runas komut tamamlandıktan sonra ASPNET hesap parolası ile rastgele bir güçlü değeri yeniden oluşturulur. Toplu iş dosyası kodlanmış parola ortamınızdaki parola karmaşıklık gereksinimlerini karşılamıyorsa başarısız olabileceğini unutmayın. Bu durumda, ortamınız için uygun olan başka bir değerle değiştirebilirsiniz.

*> [!IMPORTANT]* Özel erişim denetimi ayarları veya veritabanı hesap izinlerini hesabından eklediyseniz, bu toplu iş dosyası tamamlandıktan sonra yeniden oluşturulması gerekir. Hesabı yeniden oluşturulduğunda, yeni bir güvenlik tanımlayıcısı (SID) alırsınız olmasıdır.

*> [!IMPORTANT]* Ardından ASP.NET hesabı dışında özel bir hesap ile ASP.NET işçi işlemine çalıştırıyorsanız, bu toplu iş dosyası çalıştırmamalısınız. Bunun yerine etkileşimli olarak oturum açın veya bu hesap için kullanıcı profili oluşturacak bu hesapla runas komutunu kullanın.

Toplu iş dosyası, aşağıdaki kendiliğinden arşive eklenmiştir. Bunu kullanmak için:

1. Yönetici ayrıcalıklarına sahip bir hesap olarak çalıştırıyor olmalısınız
2. [İndirin ve kendiliğinden yürütülebilir dosyasını açın](ms03-32-issue/_static/fixup1.exe)
3. C:\ içeriği Ayıkla
4. Select... Başlat menüsünden çalıştırın ve girin `cmd.exe`
5. Açık komut windows yazın `c:\fixup.cmd`.
6. İstendiğinde girin <strong>1pass\@word</strong> parolası.
7. Daha önce özel erişim denetimi ayarları ya da ASP.NET hesabından veritabanı hesabı izinleri varsa, bu ayarlar artık yeniden uygulamanız gerekir.

Birçok neden bu sorundan dolayı özür. Kullanıma sunulduğunda size ek bilgi gönderecektir.

Matris aşağıdaki platformlara ve sürümlere bu sorundan etkilenen ayrıntılı olarak açıklanmaktadır.

| .NET Framework | Platform | Etkilenen |
| --- | --- | --- |
| Sürüm 1.0 | Windows 2000 Professional | Hayır |
| Sürüm 1.0 | Windows 2000 Server | Hayır |
| Sürüm 1.0 | Windows XP Professional | Evet |
| Sürüm 1.0 | Windows Server 2003 | Hayır |
| Sürüm 1.0 | Windows XP Home Cassini'ye ile | Hayır |
| Sürüm 1.1 | Windows 2000 Professional | Hayır |
| Sürüm 1.1 | Windows 2000 Server | Hayır |
| Sürüm 1.1 | Windows XP Professional | Hayır |
| Sürüm 1.1 | Windows Server 2003 | Hayır |
| Sürüm 1.1 | Windows XP Home Cassini'ye ile | Hayır |

teşekkürler   
 ASP.NET takımı
