---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: TextBoxWatermark bir FormView (VB) kullanarak | Microsoft Docs
author: wenz
description: "Böylece bir metin kutusu içinde görüntülenir AJAX Denetim Araç Seti TextBoxWatermark denetiminde bir metin kutusu genişletir. Bir kullanıcı kutunun içine tıkladığında, ı..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: ad75d9729c068f7d512cf076b2ae156291fe76ec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-textboxwatermark-in-a-formview-vb"></a><span data-ttu-id="6ddcf-104">TextBoxWatermark bir FormView (VB) kullanma</span><span class="sxs-lookup"><span data-stu-id="6ddcf-104">Using TextBoxWatermark in a FormView (VB)</span></span>
====================
<span data-ttu-id="6ddcf-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6ddcf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6ddcf-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6ddcf-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)</span></span>

> <span data-ttu-id="6ddcf-107">Böylece bir metin kutusu içinde görüntülenir AJAX Denetim Araç Seti TextBoxWatermark denetiminde bir metin kutusu genişletir.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="6ddcf-108">Bir kullanıcı kutunun içine tıkladığında boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="6ddcf-109">Kullanıcı kutunun metin girmeden ayrılırsa, doldurulmuş metin görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="6ddcf-110">İçinde bir FormView denetimi de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-110">This is also possible within a FormView control.</span></span>


## <a name="overview"></a><span data-ttu-id="6ddcf-111">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="6ddcf-111">Overview</span></span>

<span data-ttu-id="6ddcf-112">`TextBoxWatermark` AJAX Denetim Araç Seti denetiminde genişleten bir metin kutusu böylece bir metin kutusu içinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="6ddcf-113">Bir kullanıcı kutunun içine tıkladığında boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="6ddcf-114">Kullanıcı kutunun metin girmeden ayrılırsa, doldurulmuş metin görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="6ddcf-115">Bu aynı zamanda içinde mümkündür bir `FormView` denetim.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-115">This is also possible within a `FormView` control.</span></span>

## <a name="steps"></a><span data-ttu-id="6ddcf-116">Adımlar</span><span class="sxs-lookup"><span data-stu-id="6ddcf-116">Steps</span></span>

<span data-ttu-id="6ddcf-117">Öncelikle, bir veri kaynağı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-117">First of all, a data source is required.</span></span> <span data-ttu-id="6ddcf-118">Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-118">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="6ddcf-119">Veritabanı (express edition dahil) bir Visual Studio yükleme isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir yükleme olarak kullanılabilir [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="6ddcf-119">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="6ddcf-120">AdventureWorks veritabanını SQL Server 2005 örnekler ve örnek veritabanları parçasıdır (adresinden [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="6ddcf-120">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="6ddcf-121">En kolay yolu veritabanını için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx? FamilyID c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796 =&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve attach `AdventureWorks.mdf` veritabanı dosyası.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-121">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="6ddcf-122">Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-122">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="6ddcf-123">Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-123">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="6ddcf-124">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="6ddcf-124">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

<span data-ttu-id="6ddcf-125">Ardından, bir veri kaynağı destekleyen sayfasına ekleme `DELETE`, `INSERT` ve `UPDATE` SQL deyimlerini.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-125">Then, add a data source to the page which supports the `DELETE`, `INSERT` and `UPDATE` SQL statements.</span></span> <span data-ttu-id="6ddcf-126">Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, geçerli sürümde hata tablo adı öneki değil şunları aklınızda (`Vendor`) ile `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-126">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="6ddcf-127">Aşağıdaki biçimlendirmede doğru sözdizimi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6ddcf-127">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

<span data-ttu-id="6ddcf-128">Adı unutmayın (`ID`) veri kaynağının içinde kullanılacağından `DataSourceID` özelliği `FormView` denetim.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-128">Remember the name (`ID`) of the data source, since it will be used in the `DataSourceID` property of the `FormView` control.</span></span> <span data-ttu-id="6ddcf-129">`<InsertItemTemplate>` Bölümünü `FormView` tarafından genişletilmiş bir metin kutusu içeren `TextBoxWatermarkExtender` denetim.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-129">The `<InsertItemTemplate>` section of the `FormView` contains a textbox which is extended by the `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="6ddcf-130">Genişletici şablon içinde ve dışında bulunduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-130">Make sure that the extender resides within the template and not outside of it.</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

<span data-ttu-id="6ddcf-131">Artık kullanıcı değiştiğinde INSERT moduna `FormView` denetlemek, metin alanı yeni satıcı teşekkürler doldurulmuş için `TextBoxWatermarkExtender` denetim.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-131">Now when the user changes into the insert mode of the `FormView` control, the text field for the new vendor is prefilled thanks to the `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="6ddcf-132">Metin kutusu içinde bir tıklatma kayboluyor dolgusu metin sağlar.</span><span class="sxs-lookup"><span data-stu-id="6ddcf-132">A click inside the textbox lets the filler text disappear.</span></span>


<span data-ttu-id="6ddcf-133">[![Filigran alanında genişletici gelir](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6ddcf-133">[![The watermark in the field comes from the extender](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)</span></span>

<span data-ttu-id="6ddcf-134">Filigran alanında genişletici gelir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-textboxwatermark-in-a-formview-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6ddcf-134">The watermark in the field comes from the extender ([Click to view full-size image](using-textboxwatermark-in-a-formview-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6ddcf-135">[Önceki](using-textboxwatermark-with-validation-controls-cs.md)
[sonraki](using-textboxwatermark-with-validation-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6ddcf-135">[Previous](using-textboxwatermark-with-validation-controls-cs.md)
[Next](using-textboxwatermark-with-validation-controls-vb.md)</span></span>
