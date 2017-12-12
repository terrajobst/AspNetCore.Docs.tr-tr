---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: "Sürükleme ve bırakma ReorderList (C#) | Microsoft Docs"
author: wenz
description: "AJAX Denetim Araç Seti, ReorderList denetimi sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar. Listeden geçerli sırasını gelecektir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6afecfc7330647e6f4944c507e308afec6d2401b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="afb48-104">Sürükle ve bırak aracılığıyla ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="afb48-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="afb48-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="afb48-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="afb48-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="afb48-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="afb48-107">AJAX Denetim Araç Seti, ReorderList denetimi sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="afb48-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="afb48-108">Listeden geçerli sırasını sunucuda kalıcı.</span><span class="sxs-lookup"><span data-stu-id="afb48-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="afb48-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="afb48-109">Overview</span></span>

<span data-ttu-id="afb48-110">`ReorderList` AJAX Denetim Araç Seti denetiminde sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="afb48-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="afb48-111">Listeden geçerli sırasını sunucuda kalıcı.</span><span class="sxs-lookup"><span data-stu-id="afb48-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="afb48-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="afb48-112">Steps</span></span>

<span data-ttu-id="afb48-113">`ReorderList` Denetim listesi veritabanından veri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="afb48-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="afb48-114">En önemlisi, liste öğesi veri deposu {değişiklikleri yazma da destekler.</span><span class="sxs-lookup"><span data-stu-id="afb48-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="afb48-115">Bu örnek Microsoft SQL Server 2005 Express Edition veri deposu olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="afb48-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="afb48-116">Veritabanı express edition dahil olmak üzere bir Visual Studio yükleme, isteğe bağlı (ve ücretsiz) parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="afb48-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="afb48-117">Altında ayrı bir yükleme olarak kullanılabilir [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="afb48-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="afb48-118">Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur.</span><span class="sxs-lookup"><span data-stu-id="afb48-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="afb48-119">Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.</span><span class="sxs-lookup"><span data-stu-id="afb48-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="afb48-120">En kolay yolu veritabanını ayarlamak için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="afb48-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="afb48-121">Sunucuya, çift `Databases` ve yeni bir veritabanı oluşturun (sağ tıklatın ve seçin `New Database`) olarak adlandırılan `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="afb48-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="afb48-122">Bu veritabanında adlı yeni bir tablo oluşturmak `AJAX` aşağıdaki dört sütun ile:</span><span class="sxs-lookup"><span data-stu-id="afb48-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="afb48-123">`id`(birincil anahtar, tamsayı, kimlik, NULL)</span><span class="sxs-lookup"><span data-stu-id="afb48-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="afb48-124">`char`(char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="afb48-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="afb48-125">`description`(varchar(50), NULL)</span><span class="sxs-lookup"><span data-stu-id="afb48-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="afb48-126">`position`(int, NULL)</span><span class="sxs-lookup"><span data-stu-id="afb48-126">`position` (int, NULL)</span></span>


<span data-ttu-id="afb48-127">[![AJAX Tablo düzeni](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="afb48-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="afb48-128">AJAX tablo düzenini ([tam boyutlu görüntüyü görüntülemek için tıklatın](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="afb48-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="afb48-129">Ardından, tablo değerleri birkaç ile doldurun.</span><span class="sxs-lookup"><span data-stu-id="afb48-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="afb48-130">Unutmayın `position` sütun öğeleri sıralama düzenini tutar.</span><span class="sxs-lookup"><span data-stu-id="afb48-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="afb48-131">[![AJAX tablosundaki ilk veri](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="afb48-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="afb48-132">İlk veri AJAX tablosundaki ([tam boyutlu görüntüyü görüntülemek için tıklatın](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="afb48-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="afb48-133">Oluşturmak için sonraki adım gerektiren bir `SqlDataSource` , tablo ve yeni veritabanı ile iletişim kurmak için denetim.</span><span class="sxs-lookup"><span data-stu-id="afb48-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="afb48-134">Veri kaynağı desteklemelidir `SELECT` ve `UPDATE` SQL komutları.</span><span class="sxs-lookup"><span data-stu-id="afb48-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="afb48-135">Liste öğelerini sırasını daha sonra değiştirildiğinde `ReorderList` denetimi veri kaynağı için iki değer otomatik olarak gönderir `Update` komutu: yeni konumu ve öğenin kimliği.</span><span class="sxs-lookup"><span data-stu-id="afb48-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="afb48-136">Bu nedenle, gereksinimlerini veri kaynağı bir `<UpdateParameters>` bölüm bu iki değer için:</span><span class="sxs-lookup"><span data-stu-id="afb48-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="afb48-137">`ReorderList` Denetim gereken aşağıdaki öznitelikleri ayarlamak:</span><span class="sxs-lookup"><span data-stu-id="afb48-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="afb48-138">`AllowReorder`: Liste öğeleri olup düzenlenebilir</span><span class="sxs-lookup"><span data-stu-id="afb48-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="afb48-139">`DataSourceID`: Veri kaynağı kimliği</span><span class="sxs-lookup"><span data-stu-id="afb48-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="afb48-140">`DataKeyField`: Birincil anahtar sütunu, veri kaynağı adı</span><span class="sxs-lookup"><span data-stu-id="afb48-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="afb48-141">`SortOrderField`: Liste öğeleri için sıralama düzeni sağlar veri kaynağı sütunu</span><span class="sxs-lookup"><span data-stu-id="afb48-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="afb48-142">İçinde `<DragHandleTemplate>` ve `<ItemTemplate>` bölümler, listesinin düzenini olabilir denetleyecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="afb48-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="afb48-143">Ayrıca, veri bağlama mümkündür kullanarak `Eval()` yöntemi, burada görüldüğü gibi:</span><span class="sxs-lookup"><span data-stu-id="afb48-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="afb48-144">Aşağıdaki CSS stil bilgileri (başvurulan `<DragHandleTemplate>` bölümünü `ReorderList` denetimi) ne zaman sürükleyin tanıtıcı gezinen fare işaretçisini uygun şekilde değiştirir emin olur:</span><span class="sxs-lookup"><span data-stu-id="afb48-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="afb48-145">Son olarak, bir `ScriptManager` denetim sayfa için ASP.NET AJAX başlatır:</span><span class="sxs-lookup"><span data-stu-id="afb48-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="afb48-146">Bu örneği tarayıcıda çalıştırmak ve liste öğeleri biraz yeniden düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afb48-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="afb48-147">Ardından, sayfayı yeniden yükleyin ve/veya veritabanı bakın.</span><span class="sxs-lookup"><span data-stu-id="afb48-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="afb48-148">Değiştirilen konumları tutulmuş ve değerler de yansıtılır `position` veritabanı ve sütun herhangi kodu olmadan tüm yeni markup kullanarak.</span><span class="sxs-lookup"><span data-stu-id="afb48-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="afb48-149">[![Veritabanı değişikliklerini yeni liste öğesi sırası göre verileri](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="afb48-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="afb48-150">Veritabanı değişiklikleri yeni listesine göre veri öğesi sipariş ([tam boyutlu görüntüyü görüntülemek için tıklatın](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="afb48-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="afb48-151">[Önceki](using-postbacks-with-reorderlist-cs.md)
[sonraki](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="afb48-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
