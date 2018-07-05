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
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="07c91-104">Bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 yükseltme</span><span class="sxs-lookup"><span data-stu-id="07c91-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="07c91-105">Bu belge hem açıklar el ile ve bir Sihirbazı ile bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 sürümüne yükseltme yapmayı.</span><span class="sxs-lookup"><span data-stu-id="07c91-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="07c91-106">Bu belge için de kullanılabilir olan [indirin](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="07c91-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="07c91-107">Giriş</span><span class="sxs-lookup"><span data-stu-id="07c91-107">Introduction</span></span>

<span data-ttu-id="07c91-108">ASP.NET MVC 2 aynı sunucuda ASP.NET MVC 1.0 ile yan yana yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="07c91-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="07c91-109">Bu ne zaman bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2 yükseltme seçme içinde uygulama geliştiricilerin esnekliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="07c91-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="07c91-110">Visual Studio 2010, Visual Studio 2008 ASP.NET MVC 2 ile oluşturulan mevcut bir ASP.NET MVC 1.0 projeleri, bu yükseltme bir sihirbaz içerir.</span><span class="sxs-lookup"><span data-stu-id="07c91-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="07c91-111">Visual Studio 2010'da bir ASP.NET MVC 1.0 projesi açarak Yükseltme Sihirbazı başlatılır.</span><span class="sxs-lookup"><span data-stu-id="07c91-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="07c91-112">ASP.NET MVC 1.0 Visual Studio 2008 SP1 için Sihirbazı'nı yükseltme</span><span class="sxs-lookup"><span data-stu-id="07c91-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="07c91-113">Bir ASP.NET MVC 1.0 uygulamasını ASP.NET MVC 2'de Visual Studio 2008 SP1 için yükseltme için (desteklenmeyen) MvcAppConverter uygulamasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="07c91-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="07c91-114">Bu uygulama aşağıdaki URL'den indirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="07c91-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="07c91-115">Bir ASP.NET MVC 1.0 projesini el ile yükseltme</span><span class="sxs-lookup"><span data-stu-id="07c91-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="07c91-116">Sürüm 2 mevcut bir ASP.NET MVC 1.0 uygulamasına el ile yükseltmek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="07c91-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="07c91-117">Varolan projeyi bir yedeğini alın.</span><span class="sxs-lookup"><span data-stu-id="07c91-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="07c91-118">Bir metin düzenleyicisinde proje dosyasını (.csproj veya .vbproj dosyası uzantılı dosya) açın ve ProjectTypeGuid öğesini bulun.</span><span class="sxs-lookup"><span data-stu-id="07c91-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="07c91-119">Bu öğenin değeri ' % s'GUID {603c0e0b-db56-11dc-be95-000d561079b0} {F85E285D-A4E0-4152-9332-AB1D724D3325} ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="07c91-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="07c91-120">İşiniz bittiğinde bu öğenin değeri şu şekilde olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="07c91-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="07c91-121">Web uygulaması kök klasöründe, Web.config dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="07c91-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="07c91-122">Arama System.Web.Mvc, sürüm 1.0.0.0 = ve System.Web.Mvc, sürüm tüm örneklerinin yerine 2.0.0.0 =.</span><span class="sxs-lookup"><span data-stu-id="07c91-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="07c91-123">Görünümleri klasöründe yer alan Web.config dosyası için önceki adımı yineleyin.</span><span class="sxs-lookup"><span data-stu-id="07c91-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="07c91-124">Visual Studio kullanarak projeyi açın ve **Çözüm Gezgini**, genişletme **başvuruları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="07c91-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="07c91-125">(Sürüm 1.0 derlemeye işaret eden) System.Web.Mvc başvuruyu silin.</span><span class="sxs-lookup"><span data-stu-id="07c91-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="07c91-126">System.Web.Mvc (v2.0.0.0) bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="07c91-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="07c91-127">BindingRedirect öğesi configuraton bölümünde uygulama kök Web.config dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="07c91-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="07c91-128">Yeni ve boş bir ASP.NET MVC 2 uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="07c91-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="07c91-129">Dosyaları, betikler klasörüne yeni uygulamaya mevcut uygulamanın betikleri klasörden kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="07c91-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="07c91-130">Güncelleştirme mevcut applicationâ€™ s CSS dosyası Site.css dosyasında CSS stil tanımları ile.</span><span class="sxs-lookup"><span data-stu-id="07c91-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="07c91-131">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="07c91-131">Compile the application and run it.</span></span> <span data-ttu-id="07c91-132">Herhangi bir hata oluşursa, bozucu değişiklikleri bölümüne bakın. [ASP.NET MVC 2'deki yenilikler](https://go.microsoft.com/fwlink/?LinkID=185038) sayfası.</span><span class="sxs-lookup"><span data-stu-id="07c91-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
