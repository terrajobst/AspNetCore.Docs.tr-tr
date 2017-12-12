---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: "4 öğreticileri indirmeleri EF 5 MVC için bölüm oluşturma | Microsoft Docs"
author: Rick-Anderson
description: "Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 912a1383ed170b49782657372abc1801140df8dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Bölüm oluşturmak için EF 5 MVC 4 öğreticileri indirir
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Bölüm yüklemeler oluşturma

1. Karşıdan yükle ve projeyi örnek zip dosyası sıkıştırmasını açın. Sıkıştırması açılmış yükleme paketinde ek ZIP dosyaları, her bölüm tamamlanması için bir tane bulabilirsiniz.
2. İstenen zip dosyası üzerinde sağ tıklayın, **özellikleri**, tıklatıp **Engellemeyi Kaldır** düğmesi.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Dosyanın sıkıştırmasını açın.
4. Çift *CUx.sln* dosya Visual Studio'yu başlatın.
5. Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. Paket Yöneticisi Konsolu (PMC)'da, tıklatın **geri**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Visual Studio'dan çıkın.
8. Visual Studio'yu yeniden başlatın, çözüm dosya açılırken Yukarıdaki adımda kapalı.
9. Paket Yöneticisi Konsolu (PMC)'da, girin `Update-Database` komutu:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Şu hatayı alırsanız:  
    >   
    >  *'Update-Database' terimi, bir cmdlet, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor. Adının yazımını denetleyin veya bir yol dahilse, yolun doğru olduğundan emin olun ve yeniden deneyin.*  
    > Çıkmak ve Visual Studio'yu yeniden başlatın.

    Her geçiş çalıştırın, ardından seed yöntemi çalışır. Şimdi uygulamayı çalıştırabilirsiniz.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

>[!div class="step-by-step"]
[Önceki](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
