---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Yineleyici denetimi (VB) ile HoverMenu kullanarak | Microsoft Docs
author: wenz
description: 'AJAX Denetim Araç Seti HoverMenu denetiminde bir basit açılan efekti sağlar: fare işaretçisi bir öğenin üzerine geldiğinde popup specifi görünür...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: aa107a7483683d965f3a510e6a43f174a386da0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="779a5-103">Yineleyici denetimi (VB) ile HoverMenu kullanma</span><span class="sxs-lookup"><span data-stu-id="779a5-103">Using HoverMenu with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="779a5-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="779a5-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="779a5-105">[Kodu indirme](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="779a5-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="779a5-106">AJAX Denetim Araç Seti HoverMenu denetiminde bir basit açılan efekti sağlar: fare işaretçisi bir öğenin üzerine geldiğinde, belirtilen bir konumda açılan pencere görünür.</span><span class="sxs-lookup"><span data-stu-id="779a5-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="779a5-107">Bu denetim bir yineleyici dahilinde kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="779a5-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="779a5-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="779a5-108">Overview</span></span>

<span data-ttu-id="779a5-109">`HoverMenu` AJAX Denetim Araç Seti denetiminde bir basit açılan efekti sağlar: fare işaretçisi bir öğenin üzerine geldiğinde, belirtilen bir konumda açılan pencere görünür.</span><span class="sxs-lookup"><span data-stu-id="779a5-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="779a5-110">Bu denetim bir yineleyici dahilinde kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="779a5-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="779a5-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="779a5-111">Steps</span></span>

<span data-ttu-id="779a5-112">Öncelikle, bir veri kaynağı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="779a5-112">First of all, a data source is required.</span></span> <span data-ttu-id="779a5-113">Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır.</span><span class="sxs-lookup"><span data-stu-id="779a5-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="779a5-114">Veritabanı (express edition dahil) bir Visual Studio yükleme isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir yükleme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="779a5-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="779a5-115">AdventureWorks veritabanını SQL Server 2005 örnekler ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="779a5-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="779a5-116">En kolay yolu veritabanını için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve attach `AdventureWorks.mdf` veritabanı dosyası.</span><span class="sxs-lookup"><span data-stu-id="779a5-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="779a5-117">Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur.</span><span class="sxs-lookup"><span data-stu-id="779a5-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="779a5-118">Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.</span><span class="sxs-lookup"><span data-stu-id="779a5-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="779a5-119">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="779a5-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="779a5-120">Ardından, bir veri kaynağı sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="779a5-120">Then, add a data source to the page.</span></span> <span data-ttu-id="779a5-121">Çok sınırlı miktarda veri kullanabilmeniz için biz yalnızca ilk beş girişleri AdventureWorks veritabanını Satıcı tablosunda seçin.</span><span class="sxs-lookup"><span data-stu-id="779a5-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="779a5-122">Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, geçerli sürümde hata tablo adı öneki değil şunları aklınızda (`Vendor`) ile `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="779a5-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="779a5-123">Aşağıdaki biçimlendirmede doğru sözdizimi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="779a5-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="779a5-124">Ardından, kalıcı açılan hizmet veren bir panel ekleyin:</span><span class="sxs-lookup"><span data-stu-id="779a5-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="779a5-125">Şimdi, `HoverMenuExtender` oyuna gelir.</span><span class="sxs-lookup"><span data-stu-id="779a5-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="779a5-126">Veri kaynağında her öğe kendi açılan alır böylece genişletici yineleyici içinde 's götürüldüğü `<ItemTemplate>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="779a5-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="779a5-127">Biçimlendirme aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="779a5-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="779a5-128">Veri kaynağındaki her öğenin popup sağa görüntüler artık (`PopupPosition` özniteliği) 50 süresi (milisaniye) bir gecikmeden sonra (`PopDelay` özniteliği).</span><span class="sxs-lookup"><span data-stu-id="779a5-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="779a5-129">[![Yineleyicideki her öğesinin yanındaki vurgulu menüsü görüntülenir](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="779a5-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="779a5-130">Yineleyicideki her öğesinin yanındaki vurgulu menüsü görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="779a5-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="779a5-131">Önceki</span><span class="sxs-lookup"><span data-stu-id="779a5-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)
