---
uid: whitepapers/ms03-32-issue
title: IE güvenlik güncelleştirmesi uyguladıktan sonra 'Sunucu uygulaması kullanılamıyor' hata düzeltme | Microsoft Docs
author: rick-anderson
description: Bu yazı Wi üzerinde çalışan ASP.NET 1.0 uygulamaları etkiler Internet Explorer için MS03-32 güvenlik güncelleştirmesiyle bir sorunu giderir düzeltme eki açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: dd1a564cd347364abc3ca5ac0a9ffda448bcede8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>IE güvenlik güncelleştirmesi uygulandıktan sonra 'Sunucu uygulaması kullanılamıyor' hata düzeltme
====================
> Bu yazı, Windows XP Professional üzerinde çalışan ASP.NET 1.0 uygulamaları etkiler Internet Explorer için MS03-32 güvenlik güncelleştirmesiyle bir sorunu giderir düzeltme eki açıklar.
> 
> ASP.NET 1.0 ve Windows XP Professional için geçerlidir.


Microsoft Internet Explorer Güvenlik Düzeltme Eki MS03-32 güvenlik güncelleştirmesi ve Windows XP'de çalışan ASP.NET 1.0 ile ilgili bir sorun tanımlamıştır. Bu düzeltme eki el ile veya son kritik güncelleştirmeler Windows Update sitesinden alma yüklenebilir.

Bu sorunu düzeltme eki Windows XP makineye yükledikten sonra yerel IIS 5.1 web sunucusunda çalışan ASP.NET uygulamaları için tüm istekleri "Sunucu uygulaması kullanılamıyor" bildiren bir hata iletisi neden belirtisidir. Uzak web sunucularının isteklerine etkilenmez.

Bu sorun yalnızca ASP.NET 1.0 Windows XP çalıştıran yüklemeleri etkiler. Windows 2000 veya Windows Server 2003 çalıştıran makineleri etkilemez. Ayrıca ASP.NET 1.1 yüklenen Windows XP çalıştıran makineleri etkilemez.

Lütfen unutmayın bu sorunu **değil** güvenlik hata ASP.NET ile. Bu **yok** açık veya herhangi bir ASP.NET uygulaması veya sunucu kötü amaçlı saldırıları izin verin. Bunun yerine, bu düzeltme ekiyle neden zamanıyla ilgili bir işlev hatasıdır.

Sabit Bu sorun için kalıcı bir çözüm üzerinde çalışıyoruz. Bu arada, sorun için geçici çözüm olarak aşağıdaki toplu iş dosyasını çalıştırabilirsiniz. Toplu iş dosyası şunları yapar:

1. IIS ve ASP.NET durum hizmetleri durdurur
2. Siler ve bilinen bir geçici parola ASPNET hesabıyla yeniden oluşturur
3. Windows kullanan `runas` bir ASP.NET kullanıcı profili oluşturan bir yürütülebilir dosya başlatmak için komut
4. ASP.NET yeniden kaydeder. Bu hesap için yeni rastgele bir parola oluşturur ve bunu için varsayılan ASP.NET erişim denetimi ayarlarını uygular
5. IIS hizmetini yeniden başlatır

Toplu iş dosyası, sabit kodlanmış geçici bir parola içeren "<strong>1pass@word</strong>", olacağı toplu iş dosyasını çalıştırdığınızda runas komutunu için girmesi istenir. Runas komutunu tamamlandıktan sonra ASPNET hesabı parolası güçlü rastgele bir değeri ile yeniden oluşturulur. Sabit kodlanmış parola ortamınızdaki parola karmaşıklık gereksinimlerini karşılamıyorsa toplu iş dosyasını başlatılamayabilir unutmayın. Bu durumda, ortamınız için uygun olan başka bir değer değiştirebilirsiniz.

*> [!IMPORTANT]* Özel erişim denetimi ayarlarını veya ASP.NET hesabından veritabanı hesap izinlerini eklediyseniz, bu toplu iş dosyası tamamlandıktan sonra yeniden oluşturulması gerekir. Hesabı yeniden oluşturulduğunda, yeni bir güvenlik tanımlayıcısı (SID) alırsınız olmasıdır.

*> [!IMPORTANT]* Ardından ASP.NET hesabından farklı özel bir hesap ile ASP.NET çalışan işlemi çalıştırıyorsanız, bu toplu iş dosyası çalışmamalıdır. Bunun yerine, etkileşimli olarak oturum açın veya bu hesap için bir kullanıcı profili oluşturacak olan bu hesapla runas komutunu kullanın.

Toplu iş dosyasını aşağıdaki kendiliğinden açılan arşive eklenmiştir. Kullanmak için:

1. Yönetici ayrıcalıklarına sahip bir hesap gibi çalıştırmalıdır
2. [İndirin ve kendiliğinden açılan yürütülebilir dosyasını açın](ms03-32-issue/_static/fixup1.exe)
3. C:\ içeriği Ayıkla
4. Select... Başlat menüsünden çalıştırın ve girin `cmd.exe`
5. Aç komutu Windows'da yazın `c:\fixup.cmd`.
6. İstendiğinde, girin <strong>1pass@word</strong> ve parola olarak.
7. Daha önce özel erişim denetimi ayarlarını veya veritabanı hesap izinlerini ASPNET hesabı varsa, bu ayarları şimdi yeniden uygulanması gerekir.

Birçok bu neden bu sorundan dolayı özür. Kullanılabilir olduğunda size ek bilgi gönderin.

Matris platformları ve bu sorundan etkilenen sürümleri ayrıntıları.

| .NET Framework | Platform | Etkilenen |
| --- | --- | --- |
| Sürüm 1.0 | Windows 2000 Professional | Hayır |
| Sürüm 1.0 | Windows 2000 Server | Hayır |
| Sürüm 1.0 | Windows XP Professional | Evet |
| Sürüm 1.0 | Windows Server 2003 | Hayır |
| Sürüm 1.0 | Windows XP Home Cassini ile | Hayır |
| Sürüm 1.1 | Windows 2000 Professional | Hayır |
| Sürüm 1.1 | Windows 2000 Server | Hayır |
| Sürüm 1.1 | Windows XP Professional | Hayır |
| Sürüm 1.1 | Windows Server 2003 | Hayır |
| Sürüm 1.1 | Windows XP Home Cassini ile | Hayır |

teşekkürler   
 ASP.NET ekibi
