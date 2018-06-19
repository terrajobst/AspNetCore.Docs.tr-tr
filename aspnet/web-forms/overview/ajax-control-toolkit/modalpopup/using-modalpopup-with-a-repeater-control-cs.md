---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: ModalPopup yineleyici denetimiyle (C#) kullanarak | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Bu Sözl kullanmak da mümkündür...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7124ca3fc346d78c3b235d1756695b008afb47aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872952"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a><span data-ttu-id="ef1a0-104">Yineleyici denetimiyle birlikte (C#) ModalPopup kullanma</span><span class="sxs-lookup"><span data-stu-id="ef1a0-104">Using ModalPopup with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="ef1a0-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ef1a0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ef1a0-106">[Kodu indirme](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ef1a0-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span></span>

> <span data-ttu-id="ef1a0-107">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ef1a0-108">Bu denetim bir yineleyici dahilinde kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="ef1a0-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="ef1a0-109">Overview</span></span>

<span data-ttu-id="ef1a0-110">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ef1a0-111">Bu denetim bir yineleyici dahilinde kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="ef1a0-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="ef1a0-112">Steps</span></span>

<span data-ttu-id="ef1a0-113">Öncelikle, bir veri kaynağı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-113">First of all, a data source is required.</span></span> <span data-ttu-id="ef1a0-114">Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="ef1a0-115">Veritabanı (express edition dahil) bir Visual Studio yükleme isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir yükleme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="ef1a0-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="ef1a0-116">AdventureWorks veritabanını SQL Server 2005 örnekler ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="ef1a0-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="ef1a0-117">En kolay yolu veritabanını için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve attach `AdventureWorks.mdf` veritabanı dosyası.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="ef1a0-118">Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="ef1a0-119">Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="ef1a0-120">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="ef1a0-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="ef1a0-121">Ardından, bir veri kaynağı sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-121">Then, add a data source to the page.</span></span> <span data-ttu-id="ef1a0-122">Çok sınırlı miktarda veri kullanabilmeniz için biz yalnızca ilk beş girişleri AdventureWorks veritabanını Satıcı tablosunda seçin.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="ef1a0-123">Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, geçerli sürümde hata tablo adı öneki değil şunları aklınızda (`Vendor`) ile `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="ef1a0-124">Aşağıdaki biçimlendirmede doğru sözdizimi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ef1a0-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="ef1a0-125">Ardından, kalıcı açılan hizmet veren bir panel ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="ef1a0-126">İçerdiği bir `Button` açılan kapatmak için tekrar denetimi:</span><span class="sxs-lookup"><span data-stu-id="ef1a0-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="ef1a0-127">Yineleyici içinde iş açılan yapmak için `ModalPopupExtender` denetim yerleştirmek, içinde `<ItemTemplate>` yineleyici bölümü.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="ef1a0-128">Böylece dışında yineleyici panosudur ancak genişletici içindedir.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="ef1a0-129">Yineleyici için biçimlendirme şöyledir:</span><span class="sxs-lookup"><span data-stu-id="ef1a0-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="ef1a0-130">Ardından, her bir veri kaynağı öğe kalıcı açılan tetikleyen bir düğme yanında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ef1a0-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="ef1a0-131">[![Her veri kaynağı girişi için kalıcı açılan tetiklenebilir](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ef1a0-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="ef1a0-132">Her veri kaynağı girişi için kalıcı açılan tetiklenebilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ef1a0-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ef1a0-133">[Önceki](launching-a-modal-popup-window-from-server-code-cs.md)
> [sonraki](handling-postbacks-from-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ef1a0-133">[Previous](launching-a-modal-popup-window-from-server-code-cs.md)
[Next](handling-postbacks-from-a-modalpopup-cs.md)</span></span>
