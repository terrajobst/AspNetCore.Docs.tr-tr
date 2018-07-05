---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 yükseltme | Microsoft Docs
author: rick-anderson
description: Bu belge hem açıklar el ile ve bir Sihirbazı ile bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 sürümüne yükseltme yapmayı. Bu belge, d için de kullanılabilir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 354dbab3057068eb13b16c9aa31f66c72371ce7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381922"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 yükseltme
====================
> Bu belge hem açıklar el ile ve bir Sihirbazı ile bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 sürümüne yükseltme yapmayı. Bu belge için de kullanılabilir olan [indirin](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Giriş

ASP.NET MVC 2 aynı sunucuda ASP.NET MVC 1.0 ile yan yana yüklenebilir. Bu ne zaman bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 yükseltme seçme içinde uygulama geliştiricilerin esnekliği sağlar.

Visual Studio 2010, Visual Studio 2008 ASP.NET MVC 2 ile oluşturulan mevcut bir ASP.NET MVC 1.0 projeleri, bu yükseltme bir sihirbaz içerir. Visual Studio 2010'da bir ASP.NET MVC 1.0 projesi açarak Yükseltme Sihirbazı başlatılır.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>ASP.NET MVC 1.0 Visual Studio 2008 SP1 için Sihirbazı'nı yükseltme

Bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2'de Visual Studio 2008 SP1 için yükseltme için (desteklenmeyen) MvcAppConverter uygulamasını kullanın. Bu uygulama aşağıdaki URL'den indirebilirsiniz:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Bir ASP.NET MVC 1.0 projesini el ile yükseltme

Sürüm 2 mevcut bir ASP.NET MVC 1.0 uygulamasına el ile yükseltmek için aşağıdaki adımları izleyin:

1. Varolan projeyi bir yedeğini alın.
2. Bir metin düzenleyicisinde proje dosyasını (.csproj veya .vbproj dosyası uzantılı dosya) açın ve ProjectTypeGuid öğesini bulun. Bu öğenin değeri ' % s'GUID {603c0e0b-db56-11dc-be95-000d561079b0} {F85E285D-A4E0-4152-9332-AB1D724D3325} ile değiştirin. İşiniz bittiğinde bu öğenin değeri şu şekilde olmalıdır: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Web uygulaması kök klasöründe, Web.config dosyasını düzenleyin. Arama System.Web.Mvc, sürüm 1.0.0.0 = ve System.Web.Mvc, sürüm tüm örneklerinin yerine 2.0.0.0 =.
4. Görünümleri klasöründe yer alan Web.config dosyası için önceki adımı yineleyin.
5. Visual Studio kullanarak projeyi açın ve **Çözüm Gezgini**, genişletme **başvuruları** düğümü. (Sürüm 1.0 derlemeye işaret eden) System.Web.Mvc başvuruyu silin. System.Web.Mvc (v2.0.0.0) bir başvuru ekleyin.
6. BindingRedirect öğesi configuraton bölümünde uygulama kök Web.config dosyasına ekleyin:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Yeni ve boş bir ASP.NET MVC 2 uygulaması oluşturun. Dosyaları, betikler klasörüne yeni uygulamaya mevcut uygulamanın betikleri klasörden kopyalayın.
8. Güncelleştirme mevcut applicationâ€™ s CSS dosyası Site.css dosyasında CSS stil tanımları ile.
9. Uygulamayı derleyin ve çalıştırın. Herhangi bir hata oluşursa, bozucu değişiklikleri bölümüne bakın. [ASP.NET MVC 2'deki yenilikler](https://go.microsoft.com/fwlink/?LinkID=185038) sayfası.
