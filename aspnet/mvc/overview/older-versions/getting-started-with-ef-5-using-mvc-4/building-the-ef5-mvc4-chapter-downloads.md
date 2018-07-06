---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Bölüm oluşturmak için EF 5 MVC 4 öğreticiler indirir | Microsoft Docs
author: Rick-Anderson
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 0433c07bc42d7d5f397772704a6cb7aa2e03f8e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810795"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Bölüm oluşturmak için EF 5 MVC 4 öğreticiler indirir
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Bölüm indirmeleri oluşturma

1. İndirin ve proje örnek zip dosyasını açın. Sıkıştırması açılmış indirme paketine ek zip dosyaları, her bölümün tamamlanması için bir tane bulabilirsiniz.
2. İstenen zip dosyasını sağ tıklatın, **özellikleri**, tıklatıp **Engellemeyi Kaldır** düğmesi.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Dosyanın sıkıştırmasını açın.
4. Çift *CUx.sln* dosyasını Visual Studio'yu başlatın.
5. Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. Paket Yöneticisi Konsolu (PMC'de), tıklayın **geri**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Visual Studio'dan çıkın.
8. Visual Studio'yu yeniden başlatın, çözüm dosyasını açıp Yukarıdaki adımda kapalı.
9. Paket Yöneticisi Konsolu (PMC'yi) içinde girmek `Update-Database` komutu:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Şu hatayı alırsanız:  
    >   
    >  *' % S'terim 'Veritabanını Güncelleştir' cmdlet'i, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor. Adının yazımını denetleyin veya bir yol varsa, yolun doğru olduğundan emin olun ve yeniden deneyin.*  
    > Çıkın ve Visual Studio'yu yeniden başlatın.

    Her geçişi çalışır ve ardından seed yöntemi çalışır. Artık uygulamayı çalıştırabilirsiniz.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Önceki](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
