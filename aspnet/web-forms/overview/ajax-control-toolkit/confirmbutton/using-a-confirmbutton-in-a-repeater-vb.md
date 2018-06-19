---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Yineleyici (VB) içinde bir ConfirmButton kullanarak | Microsoft Docs
author: wenz
description: AJAX Denetim araç setindeki ConfirmButton genişletici Evet oluşturur/Kullanıcı bir düğmesine tıkladığında herhangi bir açılır pencere (LinkButton denetim dahil). Yalnızca Evet ise...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 89f412c242a3a5bcd10b72b7f0cbfb23705edb51
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869458"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a><span data-ttu-id="219e4-104">Yineleyici (VB) içinde bir ConfirmButton kullanma</span><span class="sxs-lookup"><span data-stu-id="219e4-104">Using a ConfirmButton In a Repeater (VB)</span></span>
====================
<span data-ttu-id="219e4-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="219e4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="219e4-106">[Kodu indirme](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="219e4-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span></span>

> <span data-ttu-id="219e4-107">AJAX Denetim araç setindeki ConfirmButton genişletici Evet oluşturur/Kullanıcı bir düğmesine tıkladığında herhangi bir açılır pencere (LinkButton denetim dahil).</span><span class="sxs-lookup"><span data-stu-id="219e4-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="219e4-108">Yalnızca Evet tıklandığında, düğmenin eylemini, aksi takdirde iptal yürütülür.</span><span class="sxs-lookup"><span data-stu-id="219e4-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="219e4-109">Bu aynı zamanda bir yineleyici mümkündür.</span><span class="sxs-lookup"><span data-stu-id="219e4-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="219e4-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="219e4-110">Overview</span></span>

<span data-ttu-id="219e4-111">AJAX Denetim araç setindeki ConfirmButton genişletici Evet oluşturur/Kullanıcı bir düğmesine tıkladığında herhangi bir açılır pencere (LinkButton denetim dahil).</span><span class="sxs-lookup"><span data-stu-id="219e4-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="219e4-112">Yalnızca Evet tıklandığında, düğmenin eylemini, aksi takdirde iptal yürütülür.</span><span class="sxs-lookup"><span data-stu-id="219e4-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="219e4-113">Bu aynı zamanda bir yineleyici mümkündür.</span><span class="sxs-lookup"><span data-stu-id="219e4-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="219e4-114">Adımlar</span><span class="sxs-lookup"><span data-stu-id="219e4-114">Steps</span></span>

<span data-ttu-id="219e4-115">Öncelikle, bir veri kaynağı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="219e4-115">First of all, a data source is required.</span></span> <span data-ttu-id="219e4-116">Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır.</span><span class="sxs-lookup"><span data-stu-id="219e4-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="219e4-117">Veritabanı (express edition dahil) bir Visual Studio yükleme isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir yükleme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="219e4-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="219e4-118">AdventureWorks veritabanını SQL Server 2005 örnekler ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="219e4-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="219e4-119">En kolay yolu veritabanını için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve attach `AdventureWorks.mdf` veritabanı dosyası.</span><span class="sxs-lookup"><span data-stu-id="219e4-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="219e4-120">Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur.</span><span class="sxs-lookup"><span data-stu-id="219e4-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="219e4-121">Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.</span><span class="sxs-lookup"><span data-stu-id="219e4-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="219e4-122">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="219e4-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

<span data-ttu-id="219e4-123">Ardından, bir veri kaynağı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="219e4-123">Then, a data source is required.</span></span> <span data-ttu-id="219e4-124">Basitleştirmek amacıyla, yalnızca ilk beş girişlerini AdventureWorks satıcılar tablosundaki alınır.</span><span class="sxs-lookup"><span data-stu-id="219e4-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="219e4-125">Veri kaynağı, tablo adı oluşturmak için Visual Studio Sihirbazı'nı kullanırken dikkat edin (`Vendors`) şu anda doğru ile eklenmedi `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="219e4-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="219e4-126">Aşağıdaki biçimlendirmede doğru olur:</span><span class="sxs-lookup"><span data-stu-id="219e4-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

<span data-ttu-id="219e4-127">Bu veri kaynağı yineleyici içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="219e4-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="219e4-128">Her zamanki gibi `DataBinder.Eval()` yöntemi veri kaynağından veri alır.</span><span class="sxs-lookup"><span data-stu-id="219e4-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="219e4-129">`ConfirmButtonExtender` Denetim sonra yerleştirilmelidir içinde `<ItemTemplate>` olan her veri kaynağı girişi için görünmesi yineleyici bölümü.</span><span class="sxs-lookup"><span data-stu-id="219e4-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


<span data-ttu-id="219e4-130">[![Her giriş veri kaynağından yanındaki Onayla düğmesi görünür](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="219e4-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span></span>

<span data-ttu-id="219e4-131">Her giriş veri kaynağından yanındaki Onayla düğmesi görünür ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="219e4-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="219e4-132">Önceki</span><span class="sxs-lookup"><span data-stu-id="219e4-132">Previous</span></span>](using-a-confirmbutton-in-a-repeater-cs.md)
