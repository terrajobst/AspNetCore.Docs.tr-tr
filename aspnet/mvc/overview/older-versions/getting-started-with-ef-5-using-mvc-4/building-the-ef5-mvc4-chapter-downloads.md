---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: 4 öğreticileri indirmeleri EF 5 MVC için bölüm oluşturma | Microsoft Docs
author: Rick-Anderson
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: bda7effabd715e4658d2472e1f0a66d7bba18cab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="d2d82-103">Bölüm oluşturmak için EF 5 MVC 4 öğreticileri indirir</span><span class="sxs-lookup"><span data-stu-id="d2d82-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="d2d82-104">tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d2d82-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[<span data-ttu-id="d2d82-105">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="d2d82-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="d2d82-106">Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d2d82-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="d2d82-107">Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="d2d82-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="d2d82-108">Bölüm yüklemeler oluşturma</span><span class="sxs-lookup"><span data-stu-id="d2d82-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="d2d82-109">Karşıdan yükle ve projeyi örnek zip dosyası sıkıştırmasını açın.</span><span class="sxs-lookup"><span data-stu-id="d2d82-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="d2d82-110">Sıkıştırması açılmış yükleme paketinde ek ZIP dosyaları, her bölüm tamamlanması için bir tane bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d2d82-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="d2d82-111">İstenen zip dosyası üzerinde sağ tıklayın, **özellikleri**, tıklatıp **Engellemeyi Kaldır** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d2d82-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="d2d82-112">Dosyanın sıkıştırmasını açın.</span><span class="sxs-lookup"><span data-stu-id="d2d82-112">Unzip the file.</span></span>
4. <span data-ttu-id="d2d82-113">Çift *CUx.sln* dosya Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="d2d82-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="d2d82-114">Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="d2d82-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="d2d82-115">Paket Yöneticisi Konsolu (PMC)'da, tıklatın **geri**.</span><span class="sxs-lookup"><span data-stu-id="d2d82-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="d2d82-116">Visual Studio'dan çıkın.</span><span class="sxs-lookup"><span data-stu-id="d2d82-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="d2d82-117">Visual Studio'yu yeniden başlatın, çözüm dosya açılırken Yukarıdaki adımda kapalı.</span><span class="sxs-lookup"><span data-stu-id="d2d82-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="d2d82-118">Paket Yöneticisi Konsolu (PMC)'da, girin `Update-Database` komutu:</span><span class="sxs-lookup"><span data-stu-id="d2d82-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="d2d82-119">Şu hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="d2d82-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="d2d82-120">*'Update-Database' terimi, bir cmdlet, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor. Adının yazımını denetleyin veya bir yol dahilse, yolun doğru olduğundan emin olun ve yeniden deneyin.*</span><span class="sxs-lookup"><span data-stu-id="d2d82-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="d2d82-121">Çıkmak ve Visual Studio'yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="d2d82-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="d2d82-122">Her geçiş çalıştırın, ardından seed yöntemi çalışır.</span><span class="sxs-lookup"><span data-stu-id="d2d82-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="d2d82-123">Şimdi uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d2d82-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="d2d82-124">Önceki</span><span class="sxs-lookup"><span data-stu-id="d2d82-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
