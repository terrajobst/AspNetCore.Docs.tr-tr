---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: "ASP.NET MVC 2 ASP.NET MVC 1,0 uygulamaya yükseltme | Microsoft Docs"
author: rick-anderson
description: "Her ikisi de bu belgede açıklanan el ile ve bir Sihirbazı'nı kullanarak bir ASP.NET MVC 1.0 uygulaması için ASP.NET MVC 2 yükseltme. Bu belge, ayrıca d kullanılabilir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>ASP.NET MVC 2 ASP.NET MVC 1,0 Uygulama yükseltme
====================
> Her ikisi de bu belgede açıklanan el ile ve bir Sihirbazı'nı kullanarak bir ASP.NET MVC 1.0 uygulaması için ASP.NET MVC 2 yükseltme. Bu belgede de kullanılabilir [indirin](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Giriş

ASP.NET MVC 2 aynı sunucuda ASP.NET MVC 1. 0'yan yana yüklenebilir. Bu, ASP.NET MVC 2 ASP.NET MVC 1.0 uygulamaya yükseltme zamanı seçme uygulama geliştiricilerin esneklik sağlar.

Visual Studio 2010 Visual Studio 2008 ASP.NET MVC 2 ile oluşturulmuş mevcut ASP.NET MVC 1.0 projeler, yükseltmeleri bir sihirbaz içerir. Visual Studio 2010'da bir ASP.NET MVC 1.0 projesi açarak Yükseltme Sihirbazı başlatılır.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>ASP.NET MVC 1.0 Visual Studio 2008 SP1 için Sihirbazı yükseltme

ASP.NET MVC 2 Visual Studio 2008 SP1'de bir ASP.NET MVC 1.0 uygulamaya yükseltmek için (desteklenmeyen) MvcAppConverter uygulamasını kullanın. Bu uygulama aşağıdaki URL'yi yükleyebilirsiniz:

[https://go.microsoft.com/fwlink/?LinkId=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>ASP.NET MVC 1,0 projesinde el ile yükseltme

El ile sürüm 2 varolan bir ASP.NET MVC 1.0 uygulamaya yükseltmek için aşağıdaki adımları izleyin:

1. Varolan projeyi yedeğini alın.
2. Bir metin düzenleyicisinde (.csproj veya .vbproj dosya uzantılı dosya) proje dosyasını açın ve ProjectTypeGuid öğesi bulunamıyor. Bu öğenin değeri olarak ' % s'GUID {603c0e0b-db56-11dc-be95-000d561079b0} {F85E285D-A4E0-4152-9332-AB1D724D3325} ile değiştirin. İşiniz bittiğinde, bu öğenin değerini şu şekilde olmalıdır: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Web uygulaması kök klasöründe Web.config dosyasını düzenleyin. Arama System.Web.Mvc, sürüm 1.0.0.0 = ve sürüm System.Web.Mvc ile tüm örnekleri değiştirmek 2.0.0.0 =.
4. Görünümler klasöründe yer alan Web.config dosyası için önceki adımı yineleyin.
5. Visual Studio kullanarak projeyi açın ve **Çözüm Gezgini**, genişletin **başvuruları** düğümü. Başvuru (hangi sürüm 1.0 derlemeye noktaları) System.Web.Mvc silin. System.Web.Mvc (v2.0.0.0) için bir başvuru ekleyin.
6. Yapılandırma bölümünde uygulama kök Web.config dosyasında aşağıdaki bindingRedirect öğesi ekleyin:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Yeni ve boş bir ASP.NET MVC 2 uygulaması oluşturun. Dosyaları betikler klasörüne yeni uygulamanın var olan uygulamanın betikleri klasöründen kopyalayın.
8. Güncelleştirme mevcut applicationâ€™ Site.css dosyasında CSS stil tanımları s CSS dosyası.
9. Uygulamayı derleyin ve çalıştırın. Herhangi bir hata oluşursa, son değişiklikler bölümüne bakın [ASP.NET MVC 2'deki yenilikler](https://go.microsoft.com/fwlink/?LinkID=185038) sayfası.
