---
uid: whitepapers/side-by-side-with-10
title: .NET Framework 1.0 ve 1.1 ASP.NET yan yana yürütülmesi | Microsoft Docs
author: rick-anderson
description: Bu teknik incelemede, makinenizde ya da çerçeve sürümünde çalıştırmak bir ASP.NET Web uygulamasına izin verme ve .NET 1.0 ve 1.1 .NET yüklemek açıklar...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1018845e3d2c6603732bfbbde78f4a9183e49d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808401"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>.NET Framework 1.0 ve 1.1 ASP.NET yan yana yürütme
====================
> Bu teknik incelemeyi ve .NET 1.0 ve 1.1 .NET framework'ün her iki sürümde de çalıştırmak bir ASP.NET Web uygulamasına izin verme makinenize yükleyin açıklar.
> 
> ASP.NET 1.0 ve 1.1 ASP.NET için geçerlidir.


ASP.NET'te, uygulamaları aynı bilgisayarda yüklü ancak .NET Framework'ün farklı sürümlerini kullanan yan yana çalıştırılması söylenir. Aşağıdaki konuda yan yana yürütme için ASP.NET uygulamalarının nasıl yapılandırılacağını açıklar ve için ayrıntılı adımlar verilmektedir:

- [Yükleme sırasında .NET Framework sürüm 1.0, Web uygulamanızın eşlemesini koru](#1)
- [.NET Framework'ün belirli bir sürümünü bir Web uygulamasına eşleme](#2)
- [Bir Web sitesini kullanarak .NET Framework sürümünü bulun](#3)

Geleneksel olarak, bir bilgisayar üzerinde bir bileşen ya da uygulama güncelleştirildiğinde, eski sürüm kaldırılır ve yeni sürümle değiştirilir. Yeni sürümü önceki sürümüyle uyumlu değilse, bu genellikle bileşen veya uygulama kullanan diğer uygulamalar keser. .NET Framework sağlayan bir derleme veya aynı anda aynı bilgisayara yüklenecek uygulama birden çok sürümünü yan yana yürütme için destek sağlar. Birden çok sürümü aynı anda yüklenebildiğinden, yönetilen uygulamaların hangi sürümün farklı bir sürümünü kullanan uygulamaları etkilemeden kullanılacağını seçebilirsiniz.

Varsayılan olarak, .NET Framework sürüm 1.1, yükleme sırasında tüm ASP.NET uygulamalarının otomatik olarak .NET Framework'ün en son sürümünü kullanacak şekilde yapılandırılır. .NET Framework 1. 1'için varsayılan olarak, ASP.NET uygulamaları istemiyorsanız tıklayın [burada](#1) yükleme sırasında bunu önlemek öğrenin.

Web sunucunuzu güncelleştirmek için .NET Framework 1.1 ve istediğiniz .NET Framework 1.0 çalıştırılacak bir veya daha fazla Web uygulamaları, Internet Information Services (IIS) betik eşlemesi güncelleştirmeniz gerekiyor. Betik eşlemesi, .NET Framework sürümü için belirli bir Web uygulaması dosya uzantısı .aspx eşlemek için yönelik mekanizmadır. Tıklayın [burada](#2) .NET Framework'ün belirli bir sürüme bir Web uygulaması eşlemeyle ilgili bilgi edinmek için.

Internet Bilgi Yöneticisi'ni veya ASP.NET IIS Kayıt aracını kullanın (Aspnet\_regiis.exe) hangi .NET Framework sürümünü çalıştıran belirli bir Web uygulaması bulunamadı. Tıklayın [burada](#3) bir Web sitesini kullanarak .NET Framework sürümünü bulmak öğrenin.

.NET Framework 1.1 olarak geçiş sırasında bir içeri aktarma göz önünde bulundurarak her .NET Framework sürümü kendi Machine.config dosyasının kullanmasıdır. Sonuç olarak, bir Web Yöneticisi Machine.config dosyasına değişiklikler yaptı, bu değişiklikleri .NET Framework 1.1 Machine.config dosyasına geçirilmesi gerekir.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Yükleme sırasında .NET Framework 1.0 için Web uygulamanızın eşlemesini bakımını yapma

Varsayılan olarak, tüm mevcut ASP.NET uygulamaları, .NET Framework'ün daha yeni sürümü kullanmak için yükleme sırasında otomatik olarak yapılandırılır. .NET Framework'ün daha yeni sürümünü kullanarak, uygulamaları geliştirmeleri ve yeni sürümde sunulan yeni özelliklerle tam avantajlarından yararlanabilirsiniz. Aynı anda kimin hangi uygulamaları üzerinde ayrıntılı denetim isteyebilirsiniz Web Yöneticisi güncelleştirilir, otomatik tüm ASP.NET uygulamalarına .NET Framework'ün bir yükleme sırasında yeniden eşleme engelleyebilirsiniz.

Otomatik tüm ASP.NET uygulamasının .NET Framework'ün daha yeni sürüme yeniden eşleme önlemek için Web Yöneticisi Dotnetfx.exe Kurulum programını/noaspupgrade komut satırı seçeneğini kullanabilirsiniz.

**ASP.NET uygulamasının daha yeni sürüme toplam yeniden eşleme önlemek için**

1. Git **Başlat**.
2. Tıklayarak **çalıştırma**.
3. Tür **cmd**.
4. **Tamam**'ı tıklatın.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Komut İstemi'nden .NET Framework'ün yüklemesini başlatmak için şu satırı girin: **Dotnetfx.exe/c: "/ noaspupgrade yükleme?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Tıklayın **Evet** Microsoft .NET Framework 1.1 kurulumunda. Bu, .NET Framework 1.1 Kurulum işlemi başlatır.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>.NET Framework'ün belirli bir sürümünü bir Web uygulamasına eşleme

ASP.NET IIS Kayıt Aracı sürümü .NET Framework'ün her sürümü içerir (Aspnet\_regiis.exe). Bu araç, bir Web uygulaması belirli bir .NET Framework sürümünün altında çalıştırılması yöneticilerin sağlar. Bu .NET Framework sürümü için bir Web uygulaması eşleme olarak adlandırılır. Yöneticiler, ASP.NET seçmelisiniz\_Web uygulaması ile ilişkilendirilecek olan .NET Framework sürümüne karşılık gelen regiis.exe. Örneğin, bir Web sitesi .NET Framework 1.1 kullandığını belirtmek için isteyen bir yönetici Aspnet kullanmalısınız\_.NET Framework 1.1 ile birlikte gelen regiis.exe.

ASP.NET\_regiis.exe sürüm 1.0 için konumu:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis

ASP.NET\_regiis.exe sürümünün 1,1 yer:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis

ASP.NET\_regiis.exe betik eşlemesi bir Web uygulaması için iki seçenek sağlar:

- **-s** ayarlar betik eşlemesi yolundaki ve alt dizinleri.
- **-sn** yolda yalnızca betik eşlemesi ayarlar.

W3SVC/ROOT biçiminde tanımlanan Web uygulamasını IIS meta veri yolu yolunu tanımlar / {WebSiteNumber} / {uygulama\_adı}. Örneğin, bir Web uygulaması için varsayılan Web sitesi altında bulunan portalı adlı metataban yolu W3SVC/1/kök/Portal ' dir.

![](side-by-side-with-10/_static/image4.gif)

Metatabanı yolu metatabanı düzenleyici olarak adlandırılan bir araç kullanabilirsiniz unutmayın. Bu araç Microsoft Support sitesini yükleyebilir [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- ASP.NET çalıştırma\_Haritası ve kendi subapplication regiis.exe -s W3SVC/1/kök/IIS portalı güncelleştirmek için Portal komut dosyası.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- ASP.NET çalıştırma\_regiis.exe -sn W3SVC/1/kök/portal IIS betik güncelleştirmek için Portal harita, portal uygulamaları etkilemeden? s alt dizinleri.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Bir Web uygulaması kullanarak .NET Framework sürümünü bulun

Yönetici, hangi .NET Framework sürümünü çalıştıran bir Web sitesi bulmak için Internet Hizmet Yöneticisi'ni kullanabilirsiniz. Farklı işletim sistemi sürümleri, farklı Internet Hizmet Yöneticisi'ni başlatın. Hizmet Yöneticisi'ni başlatmak için aşağıda listelenen adımları izleyin.

**Internet Hizmet Yöneticisi'ni başlatmak için**

1. Git **Başlat**.
2. Tıklayarak **çalıştırma**.
3. Tür **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Internet Hizmet Yöneticisi'nden, .NET Framework sürümü bilmek istediğiniz Web uygulamasını seçin.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Web uygulamasına sağ tıklayın ve tıklayarak **özellikleri.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Özellik penceresinden seçmek **yapılandırma.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Uygulama eşlemesi tablosundan seçin **.aspx**, tıklatıp **Düzenle**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Gelen **yürütülebilir** metin kutusuna kaydırarak sürümü dizininden bakın. Sürüm dizini v.1.1.4322 ise, uygulama için .NET Framework 1.1 eşleştirilir. Buna karşılık, sürüm dizini v1.0.3705 ise, uygulama için .NET Framework 1.0 eşleştirilir.  
  
    ![](side-by-side-with-10/_static/image12.gif)
