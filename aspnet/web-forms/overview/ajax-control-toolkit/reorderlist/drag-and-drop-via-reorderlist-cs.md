---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Sürükle ve bırak ReorderList aracılığıyla (C#) | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ReorderList denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar. Listenin geçerli sırasını gelecektir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 15ae6ae60381f3f656f667a97dac72dbb283c80e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752349"
---
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="64d5f-104">(C#) ReorderList sürükle ve bırak</span><span class="sxs-lookup"><span data-stu-id="64d5f-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="64d5f-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="64d5f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="64d5f-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="64d5f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="64d5f-107">AJAX Denetim Araç Seti ReorderList denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="64d5f-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="64d5f-108">Listenin geçerli sırasını sunucuda kalıcı hale.</span><span class="sxs-lookup"><span data-stu-id="64d5f-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="64d5f-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="64d5f-109">Overview</span></span>

<span data-ttu-id="64d5f-110">`ReorderList` AJAX Denetim Araç Seti denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="64d5f-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="64d5f-111">Listenin geçerli sırasını sunucuda kalıcı hale.</span><span class="sxs-lookup"><span data-stu-id="64d5f-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="64d5f-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="64d5f-112">Steps</span></span>

<span data-ttu-id="64d5f-113">`ReorderList` Denetim listesi veritabanından veri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="64d5f-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="64d5f-114">Hepsinden önemlisi, sıra liste öğesi için veri deposuna yazma değişiklikleri de destekler.</span><span class="sxs-lookup"><span data-stu-id="64d5f-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="64d5f-115">Bu örnek, Microsoft SQL Server 2005 Express Edition veri deposu olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="64d5f-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="64d5f-116">Veritabanı express sürüm dahil olmak üzere Visual Studio yüklemesi isteğe bağlı (ve boş) bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="64d5f-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="64d5f-117">Altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="64d5f-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="64d5f-118">Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur.</span><span class="sxs-lookup"><span data-stu-id="64d5f-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="64d5f-119">Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.</span><span class="sxs-lookup"><span data-stu-id="64d5f-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="64d5f-120">Microsoft SQL Server Management Studio Express kullanmak için veritabanı ayarlamak için en kolay yolu olan ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="64d5f-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="64d5f-121">Sunucuya, çift tıklayarak `Databases` ve yeni bir veritabanı oluşturun (sağ tıklatın ve seçin `New Database`) olarak adlandırılan `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="64d5f-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="64d5f-122">Bu veritabanında adlı yeni bir tablo oluşturma `AJAX` aşağıdaki dört sütun ile:</span><span class="sxs-lookup"><span data-stu-id="64d5f-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="64d5f-123">`id` (NULL olmayan birincil anahtar, tamsayı, kimlik)</span><span class="sxs-lookup"><span data-stu-id="64d5f-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="64d5f-124">`char` (char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="64d5f-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="64d5f-125">`description` (varchar(50), NULL)</span><span class="sxs-lookup"><span data-stu-id="64d5f-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="64d5f-126">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="64d5f-126">`position` (int, NULL)</span></span>


<span data-ttu-id="64d5f-127">[![AJAX Tablo düzeni](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="64d5f-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="64d5f-128">AJAX tablo düzenini ([tam boyutlu görüntüyü görmek için tıklatın](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="64d5f-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="64d5f-129">Ardından, tabloda değerlerin birkaç ile doldurun.</span><span class="sxs-lookup"><span data-stu-id="64d5f-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="64d5f-130">Unutmayın `position` sütun öğelerin sıralama düzenini tutar.</span><span class="sxs-lookup"><span data-stu-id="64d5f-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="64d5f-131">[![AJAX tablosundaki ilk veri](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="64d5f-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="64d5f-132">AJAX tablosundaki ilk veri ([tam boyutlu görüntüyü görmek için tıklatın](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="64d5f-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="64d5f-133">Oluşturulacak sonraki adım gerektirir bir `SqlDataSource` , tablo ve yeni veritabanı ile iletişim kurmak için denetim.</span><span class="sxs-lookup"><span data-stu-id="64d5f-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="64d5f-134">Veri kaynağı desteklemelidir `SELECT` ve `UPDATE` SQL komutları.</span><span class="sxs-lookup"><span data-stu-id="64d5f-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="64d5f-135">Daha sonra liste öğelerini sırası değiştirildiğinde, `ReorderList` denetimi otomatik olarak gönderir veri kaynağı için iki değer `Update` komut: yeni konumunu ve öğesinin kimliği.</span><span class="sxs-lookup"><span data-stu-id="64d5f-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="64d5f-136">Bu nedenle, gereksinimleri veri kaynağının bir `<UpdateParameters>` bölümü bu iki değer için:</span><span class="sxs-lookup"><span data-stu-id="64d5f-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="64d5f-137">`ReorderList` Aşağıdaki öznitelikleri ayarlamak denetim gerekir:</span><span class="sxs-lookup"><span data-stu-id="64d5f-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="64d5f-138">`AllowReorder`: Liste öğeleri düzenlenebilir olup olmadığı</span><span class="sxs-lookup"><span data-stu-id="64d5f-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="64d5f-139">`DataSourceID`: Veri kaynağı kimliği</span><span class="sxs-lookup"><span data-stu-id="64d5f-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="64d5f-140">`DataKeyField`: Birincil anahtar sütunu veri kaynağındaki adı</span><span class="sxs-lookup"><span data-stu-id="64d5f-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="64d5f-141">`SortOrderField`: Liste öğeleri için sıralama düzeni sağlar veri kaynak sütunu</span><span class="sxs-lookup"><span data-stu-id="64d5f-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="64d5f-142">İçinde `<DragHandleTemplate>` ve `<ItemTemplate>` bölümleri, liste düzeninin göre ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="64d5f-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="64d5f-143">Ayrıca, veri bağlama mümkündür kullanarak `Eval()` yöntemi, burada görüldüğü gibi:</span><span class="sxs-lookup"><span data-stu-id="64d5f-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="64d5f-144">Aşağıdaki CSS stil bilgileri (bulunulan `<DragHandleTemplate>` bölümünü `ReorderList` denetimi) sürükleme tutamacı geldiğinde fare işaretçisi uygun şekilde değiştirir emin olur:</span><span class="sxs-lookup"><span data-stu-id="64d5f-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="64d5f-145">Son olarak, bir `ScriptManager` denetim sayfa için ASP.NET AJAX başlatır:</span><span class="sxs-lookup"><span data-stu-id="64d5f-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="64d5f-146">Bu örnekte tarayıcıda çalıştırın ve liste öğeleri biraz yeniden düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="64d5f-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="64d5f-147">Daha sonra sayfayı yeniden yükleyin ve/veya veritabanı bir bakalım.</span><span class="sxs-lookup"><span data-stu-id="64d5f-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="64d5f-148">Değiştirilen konumları tutulmuş ve değerler tarafından yansıtılır `position` sütun veritabanı ve tüm herhangi bir kod yalnızca işaretleme kullanarak.</span><span class="sxs-lookup"><span data-stu-id="64d5f-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="64d5f-149">[![Veritabanı değişiklikleri yeni liste öğesi sırasına göre verileri](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="64d5f-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="64d5f-150">Veritabanı değişiklikleri yeni listesine göre verileri öğe sırasını ([tam boyutlu görüntüyü görmek için tıklatın](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="64d5f-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="64d5f-151">[Önceki](using-postbacks-with-reorderlist-cs.md)
> [İleri](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="64d5f-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
