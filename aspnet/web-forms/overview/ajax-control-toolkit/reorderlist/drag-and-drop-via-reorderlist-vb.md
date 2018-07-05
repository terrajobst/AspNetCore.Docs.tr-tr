---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Sürükleme ve bırakma (VB) ReorderList aracılığıyla | Microsoft Docs
author: wenz
description: /Data-Access/Tutorials/master-detail-Using-a-bulleted-List-of-master-records-with-a-details-DataList-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b10ecd4c061082aba5cefe9d92b8616e9f40e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389169"
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="af7ff-103">(VB) ReorderList sürükle ve bırak</span><span class="sxs-lookup"><span data-stu-id="af7ff-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="af7ff-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="af7ff-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="af7ff-105">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="af7ff-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="af7ff-106">AJAX Denetim Araç Seti ReorderList denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="af7ff-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="af7ff-107">Listenin geçerli sırasını sunucuda kalıcı hale.</span><span class="sxs-lookup"><span data-stu-id="af7ff-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="af7ff-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="af7ff-108">Overview</span></span>

<span data-ttu-id="af7ff-109">`ReorderList` AJAX Denetim Araç Seti denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="af7ff-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="af7ff-110">Listenin geçerli sırasını sunucuda kalıcı hale.</span><span class="sxs-lookup"><span data-stu-id="af7ff-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="af7ff-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="af7ff-111">Steps</span></span>

<span data-ttu-id="af7ff-112">`ReorderList` Denetim listesi veritabanından veri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="af7ff-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="af7ff-113">Hepsinden önemlisi, sıra liste öğesi için veri deposuna yazma değişiklikleri de destekler.</span><span class="sxs-lookup"><span data-stu-id="af7ff-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="af7ff-114">Bu örnek, Microsoft SQL Server 2005 Express Edition veri deposu olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="af7ff-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="af7ff-115">Veritabanı express sürüm dahil olmak üzere Visual Studio yüklemesi isteğe bağlı (ve boş) bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="af7ff-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="af7ff-116">Altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="af7ff-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="af7ff-117">Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur.</span><span class="sxs-lookup"><span data-stu-id="af7ff-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="af7ff-118">Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.</span><span class="sxs-lookup"><span data-stu-id="af7ff-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="af7ff-119">Microsoft SQL Server Management Studio Express kullanmak için veritabanı ayarlamak için en kolay yolu olan ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="af7ff-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="af7ff-120">Sunucuya, çift tıklayarak `Databases` ve yeni bir veritabanı oluşturun (sağ tıklatın ve seçin `New Database`) olarak adlandırılan `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="af7ff-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="af7ff-121">Bu veritabanında adlı yeni bir tablo oluşturma `AJAX` aşağıdaki dört sütun ile:</span><span class="sxs-lookup"><span data-stu-id="af7ff-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="af7ff-122">`id` (NULL olmayan birincil anahtar, tamsayı, kimlik)</span><span class="sxs-lookup"><span data-stu-id="af7ff-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="af7ff-123">`char` (char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="af7ff-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="af7ff-124">`description` (varchar(50), NULL)</span><span class="sxs-lookup"><span data-stu-id="af7ff-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="af7ff-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="af7ff-125">`position` (int, NULL)</span></span>


<span data-ttu-id="af7ff-126">[![AJAX Tablo düzeni](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="af7ff-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="af7ff-127">AJAX tablo düzenini ([tam boyutlu görüntüyü görmek için tıklatın](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="af7ff-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="af7ff-128">Ardından, tabloda değerlerin birkaç ile doldurun.</span><span class="sxs-lookup"><span data-stu-id="af7ff-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="af7ff-129">Unutmayın `position` sütun öğelerin sıralama düzenini tutar.</span><span class="sxs-lookup"><span data-stu-id="af7ff-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="af7ff-130">[![AJAX tablosundaki ilk veri](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="af7ff-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="af7ff-131">AJAX tablosundaki ilk veri ([tam boyutlu görüntüyü görmek için tıklatın](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="af7ff-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="af7ff-132">Oluşturulacak sonraki adım gerektirir bir `SqlDataSource` , tablo ve yeni veritabanı ile iletişim kurmak için denetim.</span><span class="sxs-lookup"><span data-stu-id="af7ff-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="af7ff-133">Veri kaynağı desteklemelidir `SELECT` ve `UPDATE` SQL komutları.</span><span class="sxs-lookup"><span data-stu-id="af7ff-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="af7ff-134">Daha sonra liste öğelerini sırası değiştirildiğinde, `ReorderList` denetimi otomatik olarak gönderir veri kaynağı için iki değer `Update` komut: yeni konumunu ve öğesinin kimliği.</span><span class="sxs-lookup"><span data-stu-id="af7ff-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="af7ff-135">Bu nedenle, gereksinimleri veri kaynağının bir `<UpdateParameters>` bölümü bu iki değer için:</span><span class="sxs-lookup"><span data-stu-id="af7ff-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="af7ff-136">`ReorderList` Aşağıdaki öznitelikleri ayarlamak denetim gerekir:</span><span class="sxs-lookup"><span data-stu-id="af7ff-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="af7ff-137">`AllowReorder`: Liste öğeleri düzenlenebilir olup olmadığı</span><span class="sxs-lookup"><span data-stu-id="af7ff-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="af7ff-138">`DataSourceID`: Veri kaynağı kimliği</span><span class="sxs-lookup"><span data-stu-id="af7ff-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="af7ff-139">`DataKeyField`: Birincil anahtar sütunu veri kaynağındaki adı</span><span class="sxs-lookup"><span data-stu-id="af7ff-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="af7ff-140">`SortOrderField`: Liste öğeleri için sıralama düzeni sağlar veri kaynak sütunu</span><span class="sxs-lookup"><span data-stu-id="af7ff-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="af7ff-141">İçinde `<DragHandleTemplate>` ve `<ItemTemplate>` bölümleri, liste düzeninin göre ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="af7ff-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="af7ff-142">Ayrıca, veri bağlama mümkündür kullanarak `Eval()` yöntemi, burada görüldüğü gibi:</span><span class="sxs-lookup"><span data-stu-id="af7ff-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="af7ff-143">Aşağıdaki CSS stil bilgileri (bulunulan `<DragHandleTemplate>` bölümünü `ReorderList` denetimi) sürükleme tutamacı geldiğinde fare işaretçisi uygun şekilde değiştirir emin olur:</span><span class="sxs-lookup"><span data-stu-id="af7ff-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="af7ff-144">Son olarak, bir `ScriptManager` denetim sayfa için ASP.NET AJAX başlatır:</span><span class="sxs-lookup"><span data-stu-id="af7ff-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="af7ff-145">Bu örnekte tarayıcıda çalıştırın ve liste öğeleri biraz yeniden düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="af7ff-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="af7ff-146">Daha sonra sayfayı yeniden yükleyin ve/veya veritabanı bir bakalım.</span><span class="sxs-lookup"><span data-stu-id="af7ff-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="af7ff-147">Değiştirilen konumları tutulmuş ve değerler tarafından yansıtılır `position` sütun veritabanı ve tüm herhangi bir kod yalnızca işaretleme kullanarak.</span><span class="sxs-lookup"><span data-stu-id="af7ff-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="af7ff-148">[![Veritabanı değişiklikleri yeni liste öğesi sırasına göre verileri](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="af7ff-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="af7ff-149">Veritabanı değişiklikleri yeni listesine göre verileri öğe sırasını ([tam boyutlu görüntüyü görmek için tıklatın](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="af7ff-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="af7ff-150">Önceki</span><span class="sxs-lookup"><span data-stu-id="af7ff-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
