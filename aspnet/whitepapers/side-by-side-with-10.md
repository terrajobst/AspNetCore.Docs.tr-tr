---
uid: whitepapers/side-by-side-with-10
title: ".NET Framework 1.0 ve 1.1 ASP.NET yan yana yürütme | Microsoft Docs"
author: rick-anderson
description: "Bu teknik ya da çerçeve sürümünde çalıştırmak bir ASP.NET Web uygulamasına izin vererek, makinenizde .NET 1.0 ve .NET 1.1 yüklemeyi açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>.NET Framework 1.0 ve 1.1 ASP.NET yan yana yürütme
====================
> Bu teknik inceleme, .NET 1.0 ve 1.1 .NET framework'ün her iki sürümde de çalıştırmak bir ASP.NET Web uygulamasına izin verme makinenizde yüklemek açıklar.
> 
> ASP.NET 1.0 ve 1.1 ASP.NET için geçerlidir.


ASP.NET, uygulamaların aynı bilgisayarda yüklü olan ancak .NET Framework'ün farklı sürümlerini kullanan yan yana çalıştırılması söylenir. Aşağıdaki konu yan yana yürütme için ASP.NET uygulamalarının nasıl yapılandırılacağını açıklar ve ayrıntılı adımlar için sağlar:

- [Yükleme sırasında Web uygulamanızın eşleme .NET Framework sürüm 1.0 için koru](#1)
- [Bir Web uygulaması .NET Framework'ün belirli bir sürüme eşleme](#2)
- [Bir Web sitesini kullanarak .NET Framework sürümü bulunamıyor](#3)

Geleneksel olarak, bir bilgisayarda bir bileşeni ya da uygulama güncelleştirildiğinde, eski sürüm kaldırılır ve yeni sürüm ile değiştirilir. Yeni sürüm önceki sürümü ile uyumlu değilse, bu genellikle bileşen veya uygulamayı kullanan diğer uygulamalar keser. .NET Framework birden fazla sürümünü bir derlemeyi ya da uygulamanın aynı bilgisayarda aynı anda yüklenmesine izin veren yan yana yürütme için destek sağlar. Birden çok sürümü aynı anda yüklenebildiğinden, yönetilen uygulamaların hangi sürümün farklı bir sürümünü kullanan uygulamaları etkilemeden kullanılacağını seçebilirsiniz.

Varsayılan olarak, .NET Framework sürüm 1.1, yükleme sırasında tüm mevcut ASP.NET uygulamalarını otomatik olarak .NET Framework'ün en son sürümünü kullanacak şekilde yapılandırılır. .NET Framework 1.1 için varsayılan olarak, ASP.NET uygulamalarınızın istemiyorsanız tıklayın [burada](#1) yükleme sırasında bunu önlemek hakkında bilgi edinmek için.

Web sunucunuzun .NET Framework 1.1 güncelleştirmek ve .NET Framework 1.0 çalıştırmak için bir veya daha fazla Web uygulaması istiyorsanız Internet Information Services (IIS) betik eşlemesi güncelleştirmeniz gerekir. Komut dosyası eşlemeleri .NET Framework sürümü için belirli bir Web uygulaması için .aspx dosya uzantısı eşlemek için mekanizmadır. Tıklatın [burada](#2) belirli bir .NET Framework sürümünü bir Web uygulamasına eşlenen öğrenin.

Internet Bilgi Yöneticisi veya ASP.NET IIS Kayıt Aracı'nı kullanabilirsiniz (Aspnet\_regiis.exe) belirli bir Web uygulamasına hangi .NET Framework sürümü bulunamıyor. Tıklatın [burada](#3) bir Web sitesini kullanarak .NET Framework sürümünü bulmak nasıl öğrenin.

.NET Framework 1.1 geçirirken bir alma göz önünde bulundurarak her .NET Framework sürümü kendi Machine.config dosyasının kullanmasıdır. Sonuç olarak, Web Yöneticisi Machine.config dosyasının değişiklikler yaptı, bu değişiklikleri için .NET Framework 1.1 Machine.config dosyasının geçirilmesi gerekir.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Yükleme sırasında Web uygulamanızın eşleme .NET Framework 1.0 için koruma

Varsayılan olarak, tüm ASP.NET uygulamalarının .NET Framework'ün daha yeni sürümü kullanmak için yükleme sırasında otomatik olarak yapılandırılır. .NET Framework'in daha yeni sürümünü kullanan, uygulamaları tam geliştirmeleri ve yeni sürümde sunulan yeni özelliklerle yararlanabilir. Aynı zamanda, hangi uygulamalar üzerinde ayrıntılı denetim isteyebilirsiniz yönetici Web güncelleştirilir, otomatik varolan tüm ASP.NET uygulamalarına .NET Framework'ün bir yükleme sırasında yeniden eşleme engelleyebilirsiniz.

Tüm ASP.NET uygulama .NET Framework sürümü için otomatik yeniden önlemek için Web Yöneticisi Dotnetfx.exe Kurulum programını/noaspupgrade komut satırı seçeneğini kullanabilirsiniz.

**ASP.NET uygulamasının daha yeni sürüme toplam yeniden eşleme engellemek için**

1. Git **Başlat**.
2. Tıklayın **çalıştırmak**.
3. Tür **cmd**.
4. **Tamam**'ı tıklatın.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. .NET Framework'ün yüklemeyi başlatmak için aşağıdaki satırı komut isteminde aşağıdakileri yazın: **Dotnetfx.exe. / c: "/ noaspupgrade yükleme?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Tıklatın **Evet** Microsoft .NET Framework 1.1 kurulumunda. Bu, .NET Framework 1.1 Kurulum işlemini başlatır.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Bir Web uygulaması .NET Framework'ün belirli bir sürüme eşleme

ASP.NET IIS Kayıt Aracı sürümünü her .NET Framework sürümü içeriyor (Aspnet\_regiis.exe). Bu araç, yöneticilerin bir Web uygulaması belirli bir .NET Framework sürümü altında çalıştırılması gerektiğini belirtin olanak tanır. Bu için .NET Framework sürümü için bir Web uygulaması eşleme olarak adlandırılır. Yöneticiler, Aspnet seçmelisiniz\_Web uygulamasıyla ilişkilendirilmesi .NET Framework sürümünü karşılık gelen regiis.exe. Örneğin, bir Web sitesi .NET Framework 1.1 kullanacağını belirtin isteyen bir yönetici Aspnet kullanmalısınız\_.NET Framework 1.1 ile birlikte gelen regiis.exe.

Aspnet\_sürüm 1.0 için regiis.exe adresindedir:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis

Aspnet\_sürüm 1,1 regiis.exe adresindedir:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis

Aspnet\_regiis.exe bir Web uygulaması eşleme komut dosyası için iki seçenek sunar:

- **-s** ayarlar betik eşlemesi yolundaki ve alt dizinleri.
- **-sn** betik eşlemesi yalnızca yolunda ayarlar.

W3SVC/ROOT formunda tanımlanan Web uygulama IIS meta veri yolu yolunu tanımlar / {WebSiteNumber} / {uygulama\_adı}. Örneğin, varsayılan Web sitesi altında bulunan portalı adlı bir Web uygulaması için metatabanı W3SVC/1/kök/Portal yoludur.

![](side-by-side-with-10/_static/image4.gif)

Metatabanı yolu metatabanı düzenleyici olarak adlandırılan bir aracı kullanabilirsiniz unutmayın. Bu araç Microsoft Support sitesini indirebileceğiniz [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- ASPNET çalıştırmak\_regiis.exe -s W3SVC/1/kök/portal IIS güncelleştirmek için Portal harita ve kendi subapplication komut dosyası.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- ASPNET çalıştırmak\_regiis.exe -sn W3SVC/1/kök/portal IIS komut dosyası güncelleştirmek için Portal eşleme, portal uygulamaları etkilemeden? s alt dizinleri.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Bir Web uygulaması kullanarak .NET Framework sürümünü bulma

Bir yönetici, hangi .NET Framework sürümünü çalıştıran bir Web sitesi bulmak için Internet Hizmet Yöneticisi'ni kullanabilirsiniz. Farklı işletim sistemi sürümleri farklı Internet Hizmet Yöneticisi'ni başlatın. Hizmet Yöneticisi'ni başlatmak için aşağıda listelenen adımları izleyin.

**Internet Hizmet Yöneticisi'ni başlatmak için**

1. Git **Başlat**.
2. Tıklayın **çalıştırmak**.
3. Tür **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Internet Hizmet Yöneticisi'nden, .NET Framework sürümünü bilmek ister Web uygulaması seçin.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Web uygulamasına sağ tıklayın ve tıklayın **özellikleri.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Özellik penceresinden seçin **yapılandırma.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Uygulama eşleme tablosundan seçmek **.aspx**, tıklatıp **Düzenle**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Gelen **yürütülebilir** metin kutusuna, kaydırarak sürüm dizini bakın. Sürüm dizini v.1.1.4322 ise, uygulama için .NET Framework 1.1 eşleştirilir. Buna karşılık, sürüm dizini v1.0.3705 ise, uygulama için .NET Framework 1.0 eşleştirilir.  
  
    ![](side-by-side-with-10/_static/image12.gif)
