---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: ASP.NET MVC 2 ASP.NET MVC 1,0 uygulamaya yükseltme | Microsoft Docs
author: rick-anderson
description: Her ikisi de bu belgede açıklanan el ile ve bir Sihirbazı'nı kullanarak bir ASP.NET MVC 1.0 uygulaması için ASP.NET MVC 2 yükseltme. Bu belge, ayrıca d kullanılabilir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573324"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="9d338-104">ASP.NET MVC 2 ASP.NET MVC 1,0 Uygulama yükseltme</span><span class="sxs-lookup"><span data-stu-id="9d338-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="9d338-105">Her ikisi de bu belgede açıklanan el ile ve bir Sihirbazı'nı kullanarak bir ASP.NET MVC 1.0 uygulaması için ASP.NET MVC 2 yükseltme.</span><span class="sxs-lookup"><span data-stu-id="9d338-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="9d338-106">Bu belgede de kullanılabilir [indirin](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="9d338-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="9d338-107">Giriş</span><span class="sxs-lookup"><span data-stu-id="9d338-107">Introduction</span></span>

<span data-ttu-id="9d338-108">ASP.NET MVC 2 aynı sunucuda ASP.NET MVC 1. 0'yan yana yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="9d338-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="9d338-109">Bu, ASP.NET MVC 2 ASP.NET MVC 1.0 uygulamaya yükseltme zamanı seçme uygulama geliştiricilerin esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d338-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="9d338-110">Visual Studio 2010 Visual Studio 2008 ASP.NET MVC 2 ile oluşturulmuş mevcut ASP.NET MVC 1.0 projeler, yükseltmeleri bir sihirbaz içerir.</span><span class="sxs-lookup"><span data-stu-id="9d338-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="9d338-111">Visual Studio 2010'da bir ASP.NET MVC 1.0 projesi açarak Yükseltme Sihirbazı başlatılır.</span><span class="sxs-lookup"><span data-stu-id="9d338-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="9d338-112">ASP.NET MVC 1.0 Visual Studio 2008 SP1 için Sihirbazı yükseltme</span><span class="sxs-lookup"><span data-stu-id="9d338-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="9d338-113">ASP.NET MVC 2 Visual Studio 2008 SP1'de bir ASP.NET MVC 1.0 uygulamaya yükseltmek için (desteklenmeyen) MvcAppConverter uygulamasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d338-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="9d338-114">Bu uygulama aşağıdaki URL'yi yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9d338-114">You can download this application from the following URL:</span></span>

[<span data-ttu-id="9d338-115">https://go.microsoft.com/fwlink/?LinkId=185351</span><span class="sxs-lookup"><span data-stu-id="9d338-115">https://go.microsoft.com/fwlink/?LinkID=185351</span></span>](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="9d338-116">ASP.NET MVC 1,0 projesinde el ile yükseltme</span><span class="sxs-lookup"><span data-stu-id="9d338-116">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="9d338-117">El ile sürüm 2 varolan bir ASP.NET MVC 1.0 uygulamaya yükseltmek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="9d338-117">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="9d338-118">Varolan projeyi yedeğini alın.</span><span class="sxs-lookup"><span data-stu-id="9d338-118">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="9d338-119">Bir metin düzenleyicisinde (.csproj veya .vbproj dosya uzantılı dosya) proje dosyasını açın ve ProjectTypeGuid öğesi bulunamıyor.</span><span class="sxs-lookup"><span data-stu-id="9d338-119">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="9d338-120">Bu öğenin değeri olarak ' % s'GUID {603c0e0b-db56-11dc-be95-000d561079b0} {F85E285D-A4E0-4152-9332-AB1D724D3325} ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9d338-120">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="9d338-121">İşiniz bittiğinde, bu öğenin değerini şu şekilde olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="9d338-121">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="9d338-122">Web uygulaması kök klasöründe Web.config dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="9d338-122">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="9d338-123">Arama System.Web.Mvc, sürüm 1.0.0.0 = ve sürüm System.Web.Mvc ile tüm örnekleri değiştirmek 2.0.0.0 =.</span><span class="sxs-lookup"><span data-stu-id="9d338-123">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="9d338-124">Görünümler klasöründe yer alan Web.config dosyası için önceki adımı yineleyin.</span><span class="sxs-lookup"><span data-stu-id="9d338-124">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="9d338-125">Visual Studio kullanarak projeyi açın ve **Çözüm Gezgini**, genişletin **başvuruları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="9d338-125">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="9d338-126">Başvuru (hangi sürüm 1.0 derlemeye noktaları) System.Web.Mvc silin.</span><span class="sxs-lookup"><span data-stu-id="9d338-126">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="9d338-127">System.Web.Mvc (v2.0.0.0) için bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9d338-127">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="9d338-128">Yapılandırma bölümünde uygulama kök Web.config dosyasında aşağıdaki bindingRedirect öğesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d338-128">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="9d338-129">Yeni ve boş bir ASP.NET MVC 2 uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9d338-129">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="9d338-130">Dosyaları betikler klasörüne yeni uygulamanın var olan uygulamanın betikleri klasöründen kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="9d338-130">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="9d338-131">Güncelleştirme mevcut applicationâ€™ Site.css dosyasında CSS stil tanımları s CSS dosyası.</span><span class="sxs-lookup"><span data-stu-id="9d338-131">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="9d338-132">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d338-132">Compile the application and run it.</span></span> <span data-ttu-id="9d338-133">Herhangi bir hata oluşursa, son değişiklikler bölümüne bakın [ASP.NET MVC 2'deki yenilikler](https://go.microsoft.com/fwlink/?LinkID=185038) sayfası.</span><span class="sxs-lookup"><span data-stu-id="9d338-133">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
