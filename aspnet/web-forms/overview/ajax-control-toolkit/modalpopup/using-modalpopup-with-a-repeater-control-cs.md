---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: (C#) Repeater denetimiyle ModalPopup kullanma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ayrıca, bu Sözl kullanmak da mümkündür...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7e6136bac48b7c1b0931cada66501d303be8e927
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371083"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a><span data-ttu-id="b846e-104">(C#) Repeater denetimiyle ModalPopup kullanma</span><span class="sxs-lookup"><span data-stu-id="b846e-104">Using ModalPopup with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="b846e-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b846e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b846e-106">[Kodu indir](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b846e-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span></span>

> <span data-ttu-id="b846e-107">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="b846e-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b846e-108">Repeater'da içindeki bu denetimi kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="b846e-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="b846e-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="b846e-109">Overview</span></span>

<span data-ttu-id="b846e-110">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="b846e-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b846e-111">Repeater'da içindeki bu denetimi kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="b846e-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="b846e-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="b846e-112">Steps</span></span>

<span data-ttu-id="b846e-113">İlk olarak bir veri kaynağı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b846e-113">First of all, a data source is required.</span></span> <span data-ttu-id="b846e-114">Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır.</span><span class="sxs-lookup"><span data-stu-id="b846e-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="b846e-115">Veritabanı (express sürüm dahil) bir Visual Studio yüklemesi isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="b846e-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="b846e-116">AdventureWorks veritabanı SQL Server 2005 örnekleri ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="b846e-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="b846e-117">En kolay yolu, veritabanını ayarlamak için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve ekleme `AdventureWorks.mdf` veritabanı dosyası.</span><span class="sxs-lookup"><span data-stu-id="b846e-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="b846e-118">Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur.</span><span class="sxs-lookup"><span data-stu-id="b846e-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="b846e-119">Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.</span><span class="sxs-lookup"><span data-stu-id="b846e-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="b846e-120">ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="b846e-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="b846e-121">Ardından, bir veri kaynağı sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b846e-121">Then, add a data source to the page.</span></span> <span data-ttu-id="b846e-122">Sınırlı miktarda veri kullanmak için yalnızca ilk beş girişleri AdventureWorks veritabanını Satıcı tablosunda seçiyoruz.</span><span class="sxs-lookup"><span data-stu-id="b846e-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="b846e-123">Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, güncel sürümdeki bir hatayı tablo adı öneki değil gerektiğini unutmayın (`Vendor`) ile `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="b846e-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="b846e-124">Aşağıdaki biçimlendirmede doğru sözdizimini gösterir:</span><span class="sxs-lookup"><span data-stu-id="b846e-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="b846e-125">Ardından, kalıcı açılan hizmet veren bir panel ekleme.</span><span class="sxs-lookup"><span data-stu-id="b846e-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="b846e-126">İçerdiği bir `Button` denetim açılan kapatın:</span><span class="sxs-lookup"><span data-stu-id="b846e-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="b846e-127">Yineleyici içinde iş açılan yapmak için `ModalPopupExtender` denetim yerleştirmek, içinde `<ItemTemplate>` yineleyicinin bölümü.</span><span class="sxs-lookup"><span data-stu-id="b846e-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="b846e-128">Bu nedenle paneli repeater dışında ancak genişletici içinde.</span><span class="sxs-lookup"><span data-stu-id="b846e-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="b846e-129">Yineleyici için biçimlendirme şöyledir:</span><span class="sxs-lookup"><span data-stu-id="b846e-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="b846e-130">Ardından, her bir veri kaynağı öğe kalıcı açılan tetikleyen bir düğmeyle yanında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b846e-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="b846e-131">[![Her veri kaynağı girişi için kalıcı açılan tetiklenebilir](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b846e-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="b846e-132">Her veri kaynağı girişi için kalıcı açılan tetiklenebilir ([tam boyutlu görüntüyü görmek için tıklatın](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b846e-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b846e-133">[Önceki](launching-a-modal-popup-window-from-server-code-cs.md)
> [İleri](handling-postbacks-from-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b846e-133">[Previous](launching-a-modal-popup-window-from-server-code-cs.md)
[Next](handling-postbacks-from-a-modalpopup-cs.md)</span></span>
