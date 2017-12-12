---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: "Veri bağlama için bir Accordion (C#) | Microsoft Docs"
author: wenz
description: "AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle w bildirilir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a8250f58655b8fe8638d8e7a7b084ee9c33fe986
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-c"></a><span data-ttu-id="5e882-104">Veri bağlama için bir Accordion (C#)</span><span class="sxs-lookup"><span data-stu-id="5e882-104">Databinding to an Accordion (C#)</span></span>
====================
<span data-ttu-id="5e882-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5e882-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5e882-106">[Kodu indirme](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5e882-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span></span>

> <span data-ttu-id="5e882-107">AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e882-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="5e882-108">Paneller genellikle sayfa içinde bildirilen, ancak veri kaynağına bağlama daha fazla esneklik sunar.</span><span class="sxs-lookup"><span data-stu-id="5e882-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="5e882-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="5e882-109">Overview</span></span>

<span data-ttu-id="5e882-110">AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e882-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="5e882-111">Paneller genellikle sayfa içinde bildirilen, ancak veri kaynağına bağlama daha fazla esneklik sunar.</span><span class="sxs-lookup"><span data-stu-id="5e882-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="5e882-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="5e882-112">Steps</span></span>

<span data-ttu-id="5e882-113">Öncelikle, bir veri kaynağı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5e882-113">First of all, a data source is required.</span></span> <span data-ttu-id="5e882-114">Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır.</span><span class="sxs-lookup"><span data-stu-id="5e882-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="5e882-115">Veritabanı (express edition dahil) bir Visual Studio yükleme isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir yükleme olarak kullanılabilir [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="5e882-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="5e882-116">AdventureWorks veritabanını SQL Server 2005 örnekler ve örnek veritabanları parçasıdır (adresinden [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="5e882-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="5e882-117">En kolay yolu veritabanını için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx? FamilyID c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796 =&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve attach `AdventureWorks.mdf` veritabanı dosyası.</span><span class="sxs-lookup"><span data-stu-id="5e882-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="5e882-118">Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur.</span><span class="sxs-lookup"><span data-stu-id="5e882-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="5e882-119">Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.</span><span class="sxs-lookup"><span data-stu-id="5e882-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="5e882-120">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="5e882-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

<span data-ttu-id="5e882-121">Ardından, bir veri kaynağı sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5e882-121">Then, add a data source to the page.</span></span> <span data-ttu-id="5e882-122">Çok sınırlı miktarda veri kullanabilmeniz için biz yalnızca ilk beş girişleri AdventureWorks veritabanını Satıcı tablosunda seçin.</span><span class="sxs-lookup"><span data-stu-id="5e882-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="5e882-123">Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, geçerli sürümde hata tablo adı öneki değil şunları aklınızda (`Vendor`) ile `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="5e882-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="5e882-124">Aşağıdaki biçimlendirmede doğru sözdizimi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5e882-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

<span data-ttu-id="5e882-125">Veri kaynağı adı (kimlik) unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5e882-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="5e882-126">Bu çok kimliği sonra kullanılmalıdır `DataSourceID` Accordion denetiminin özelliği:</span><span class="sxs-lookup"><span data-stu-id="5e882-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

<span data-ttu-id="5e882-127">Accordion denetim içinde denetimin üstbilgisi de dahil olmak üzere çeşitli bölümleri için şablonlar sağlayabilir (`<HeaderTemplate>`) ve içeriği (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="5e882-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="5e882-128">Bu öğeleri içinde yalnızca çıktı verileri veri kaynağı, kullanarak `DataBinder.Eval()` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5e882-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

<span data-ttu-id="5e882-129">Sayfa yüklendiğinde, bu sunucu tarafı kodu ile accordion için veri kaynağı bağlı olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="5e882-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

<span data-ttu-id="5e882-130">Bu örnek sonuçlandırmak için başvurulan iki CSS sınıfları Accordion denetiminde tanımlamanız gerekir (özelliklerinde `HeaderCssClass` ve `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="5e882-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="5e882-131">Aşağıdaki biçimlendirmede koyun `<head>` sayfasının bölümünde:</span><span class="sxs-lookup"><span data-stu-id="5e882-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


<span data-ttu-id="5e882-132">[![Veriler accordion, doğrudan veri kaynağından gelir](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5e882-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span></span>

<span data-ttu-id="5e882-133">Veriler accordion, doğrudan veri kaynağından gelir ([tam boyutlu görüntüyü görüntülemek için tıklatın](databinding-to-an-accordion-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5e882-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5e882-134">Sonraki</span><span class="sxs-lookup"><span data-stu-id="5e882-134">Next</span></span>](dynamically-adding-an-accordion-pane-cs.md)
