---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: ComboBox denetimi nasıl kullanabilirim? (VB) | Microsoft Docs
author: microsoft
description: ComboBox kullanıcıların seçim yapabileceğiniz seçeneklerin bir listesini bir metin kutusu esnekliğini birleştiren bir ASP.NET AJAX denetimdir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e42844e326cb190502a51c5a85195b4752d7e827
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875415"
---
<a name="how-do-i-use-the-combobox-control-vb"></a><span data-ttu-id="f0e87-104">ComboBox denetimi nasıl kullanabilirim?</span><span class="sxs-lookup"><span data-stu-id="f0e87-104">How do I use the ComboBox Control?</span></span> <span data-ttu-id="f0e87-105">(VB)</span><span class="sxs-lookup"><span data-stu-id="f0e87-105">(VB)</span></span>
====================
<span data-ttu-id="f0e87-106">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f0e87-106">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f0e87-107">ComboBox kullanıcıların seçim yapabileceğiniz seçeneklerin bir listesini bir metin kutusu esnekliğini birleştiren bir ASP.NET AJAX denetimdir.</span><span class="sxs-lookup"><span data-stu-id="f0e87-107">ComboBox is an ASP.NET AJAX control that combines the flexibility of a TextBox with a list of options from which users can choose.</span></span>


<span data-ttu-id="f0e87-108">Bu öğretici AJAX Denetim Araç Seti ComboBox denetimi açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="f0e87-108">The goal of this tutorial is to explain the AJAX Control Toolkit ComboBox control.</span></span> <span data-ttu-id="f0e87-109">ComboBox bir standart ASP.NET DropDownList ve TextBox denetimi arasında birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-109">The ComboBox works like a combination between a standard ASP.NET DropDownList control and a TextBox control.</span></span> <span data-ttu-id="f0e87-110">Önceden var olan bir öğe listesinden seçin veya yeni bir öğe girin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-110">You can either select from a pre-existing list of items or enter a new item.</span></span>

<span data-ttu-id="f0e87-111">ComboBox otomatik tamamlama denetim extender benzer, ancak denetim farklı senaryolarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-111">The ComboBox is similar to the AutoComplete control extender, but the controls are used in different scenarios.</span></span> <span data-ttu-id="f0e87-112">Otomatik Tamamlama genişletici eşleşen girişlerini almak için bir web hizmetini sorgular.</span><span class="sxs-lookup"><span data-stu-id="f0e87-112">The AutoComplete extender queries a web service to get matching entries.</span></span> <span data-ttu-id="f0e87-113">ComboBox denetimi buna karşılık, öğeleri kümesi ile başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="f0e87-113">The ComboBox control, in contrast, is initialized with a set of items.</span></span> <span data-ttu-id="f0e87-114">Otomatik Tamamlama genişletici yapar kullanarak çok sayıda veri (car bölümleri milyonlarca) ComboBox denetimi kullanırken çalışırken algılama küçük bir veri kümesi ile çalışırken mantıklı (car bölümleri onlarca).</span><span class="sxs-lookup"><span data-stu-id="f0e87-114">Using the AutoComplete extender makes sense when you are working with a large set of data (millions of car parts) while using the ComboBox control makes sense when working with a small set of data (dozens of car parts).</span></span>

## <a name="selecting-from-a-static-list-of-items"></a><span data-ttu-id="f0e87-115">Statik öğeler listeden seçme</span><span class="sxs-lookup"><span data-stu-id="f0e87-115">Selecting from a Static List of Items</span></span>

<span data-ttu-id="f0e87-116">ComboBox denetimi kullanarak basit bir örnek Başlat s olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-116">Let�s start with a simple sample of using the ComboBox control.</span></span> <span data-ttu-id="f0e87-117">Statik öğelerin listesini aşağı açılan listesinde görüntülemek istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="f0e87-117">Imagine that you want to display a static list of items in a dropdown list.</span></span> <span data-ttu-id="f0e87-118">Ancak, liste eksiksiz değildir olasılığı açık ayrılmak istiyor.</span><span class="sxs-lookup"><span data-stu-id="f0e87-118">However, you want to leave open the possibility that the list is not complete.</span></span> <span data-ttu-id="f0e87-119">Listeye özel bir değer girmesini izin vermek istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-119">You want to allow a user to enter a custom value into the list.</span></span>

<span data-ttu-id="f0e87-120">Biz üm yeni bir ASP.NET Web Forms sayfası oluşturmak ve sayfanın ComboBox denetimi kullanın.</span><span class="sxs-lookup"><span data-stu-id="f0e87-120">We�ll create a new ASP.NET Web Forms page and use the ComboBox control in the page.</span></span> <span data-ttu-id="f0e87-121">Yeni ASP.NET sayfası projenize ekleyin ve Tasarım görünümüne geçin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-121">Add the new ASP.NET page to your project and switch to Design view.</span></span>

<span data-ttu-id="f0e87-122">ComboBox denetimi sayfasında kullanmak istiyorsanız sayfaya bir ScriptManager denetimi eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f0e87-122">If you want to use the ComboBox control in the page then you must add a ScriptManager control to the page.</span></span> <span data-ttu-id="f0e87-123">AJAX uzantılar sekmesi from beneath ScriptManager denetimi Tasarımcı yüzeyine sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-123">Drag the ScriptManager control from beneath the AJAX Extensions tab onto the Designer surface.</span></span> <span data-ttu-id="f0e87-124">Sayfanın en üstünde ScriptManager denetimi eklemeniz gerekir; Açılış sunucu-tarafı hemen ekleyebilirsiniz &lt;form&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="f0e87-124">You should add the ScriptManager control at the top of the page; you can add it immediately below the opening server-side &lt;form&gt; tag.</span></span>

<span data-ttu-id="f0e87-125">Ardından, ComboBox denetimi sayfaya sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-125">Next, drag the ComboBox control onto the page.</span></span> <span data-ttu-id="f0e87-126">ComboBox denetimi araç diğer AJAX Denetim Araç Seti denetimler ve denetim Extender'larının (bkz. Şekil 1) bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-126">You can find the ComboBox control in the Toolbox with the other AJAX Control Toolkit controls and control extenders (see figure1).</span></span>


<span data-ttu-id="f0e87-127">[![Bir iş kartı oluşturmak için basit form](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-127">[![Simple form for creating a business card](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="f0e87-128">**Şekil 01**: ComboBox Denetim Araç Kutusu'ndan seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-128">**Figure 01**: Selecting the ComboBox control from the toolbox ([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image2.png))</span></span>


<span data-ttu-id="f0e87-129">Biz üm ComboBox denetimi seçenekleri statik listesini görüntülemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="f0e87-129">We�ll use the ComboBox control to display a static list of choices.</span></span> <span data-ttu-id="f0e87-130">Kullanıcı kendi yemek spiciness belirli bir düzeyde üç seçenek listesinden seçebilirsiniz: hafif, Orta ve etkin (bkz. Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="f0e87-130">The user can select a particular level of spiciness for their food from a list of three choices: Mild, Medium, and Hot (see Figure 2).</span></span>


<span data-ttu-id="f0e87-131">[![Statik öğeler listeden seçme](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-131">[![Selecting from a static list of items](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span></span>

<span data-ttu-id="f0e87-132">**Şekil 02**: statik öğeler listeden seçerek ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-132">**Figure 02**: Selecting from a static list of items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image4.png))</span></span>


<span data-ttu-id="f0e87-133">Bu seçenekler ComboBox denetimine ekleyebilirsiniz iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-133">There are two ways that you can add these choices to the ComboBox control.</span></span> <span data-ttu-id="f0e87-134">İlk olarak, farenizi denetimini Tasarım görünümünde gezinirken seçeneklerini Düzenle görev seçeneğini belirleyin ve öğesi Düzenleyicisi'ni açın (bkz: Şekil 3).</span><span class="sxs-lookup"><span data-stu-id="f0e87-134">First, you select the Edit Options task option when hovering your mouse over the control in Design view and open the Item Editor (see Figure 3).</span></span>


<span data-ttu-id="f0e87-135">[![ComboBox öğelerini düzenleme](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-135">[![Editing ComboBox items](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span></span>

<span data-ttu-id="f0e87-136">**Şekil 03**: ComboBox düzenleme öğeleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-136">**Figure 03**: Editing ComboBox items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image6.png))</span></span>


<span data-ttu-id="f0e87-137">İkinci seçenek açma ve kapatma Between öğeleri listesi eklemektir &lt;asp: ComboBox&gt; kaynak görünümünde etiketler.</span><span class="sxs-lookup"><span data-stu-id="f0e87-137">The second option is to add the list of items in between the opening and closing &lt;asp:ComboBox&gt; tags in Source view.</span></span> <span data-ttu-id="f0e87-138">Sayfa listeleme 1 öğe listesinin sahip güncelleştirilmiş ComboBox içerir.</span><span class="sxs-lookup"><span data-stu-id="f0e87-138">The page in Listing 1 contains the updated ComboBox that has the list of items.</span></span>

<span data-ttu-id="f0e87-139">**1 - Static.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="f0e87-139">**Listing 1 - Static.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

<span data-ttu-id="f0e87-140">Listeleme 1'de sayfasını açtığınızda, ComboBox önceden mevcut seçeneklerden birini seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-140">When you open the page in Listing 1, you can select one of the pre-existing options from the ComboBox.</span></span> <span data-ttu-id="f0e87-141">Diğer bir deyişle, ComboBox yalnızca bir DropDownList denetimi gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-141">In other words, the ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="f0e87-142">Ancak, aynı zamanda mevcut listesinde olmayan yeni bir seçenek (örneğin, süper Spicy) girerek seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-142">However, you also have the option of entering a new choice (for example, Super Spicy) that is not in the existing list.</span></span> <span data-ttu-id="f0e87-143">Bu nedenle, ComboBox TextBox denetimi gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-143">So, the ComboBox also works like a TextBox control.</span></span>

<span data-ttu-id="f0e87-144">Etiket denetimi tercih ettiğiniz görünür formu gönderdiğinde olup, önceden var olan çekme bağımsız olarak öğesi veya özel bir öğesi girin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-144">Regardless of whether you pick a pre-existing item or you enter a custom item, when you submit the form, your choice appears in the label control.</span></span> <span data-ttu-id="f0e87-145">BtnSubmit form gönderme zaman\_işleyicisi yürütür ve etiket güncelleştirmeleri tıklatın (Şekil 4'e bakın).</span><span class="sxs-lookup"><span data-stu-id="f0e87-145">When you submit the form, the btnSubmit\_Click handler executes and updates the label (see Figure 4).</span></span>


<span data-ttu-id="f0e87-146">[![Seçili öğeyi görüntüleme](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-146">[![Displaying the selected item](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span></span>

<span data-ttu-id="f0e87-147">**Şekil 04**: seçilen öğe görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-147">**Figure 04**: Displaying the selected item([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image8.png))</span></span>


<span data-ttu-id="f0e87-148">ComboBox form gönderildikten sonra seçilen öğeyi almak için aynı DropDownList denetimi özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="f0e87-148">The ComboBox supports the same properties as the DropDownList control for retrieving the selected item after a form is submitted:</span></span>

- <span data-ttu-id="f0e87-149">SelectedItem.Text - seçilen öğenin metni özelliğinin değeri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f0e87-149">SelectedItem.Text - Displays the value of the Text property of the selected item.</span></span>
- <span data-ttu-id="f0e87-150">SelectedItem.Value - seçilen öğenin değeri özelliğinin değeri görüntüler veya ComboBox yazılan metin görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f0e87-150">SelectedItem.Value - Displays the value of the Value property of the selected item or displays the text typed into the ComboBox.</span></span>
- <span data-ttu-id="f0e87-151">SelectedValue - bu özellik, varsayılan (Başlangıç) seçili öğe belirtmenize olanak tanır ancak bu SelectedItem.Value aynıdır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-151">SelectedValue - Same as SelectedItem.Value except that this property enables you to specify the default (initial) selected item.</span></span>

<span data-ttu-id="f0e87-152">Yazarsanız, özel bir seçim ComboBox sonra özel seçeneği olarak hem SelectedItem.Text hem de SelectedItem.Value özelliklerine atanır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-152">If you type a custom choice into the ComboBox then the custom choice is assigned to both the SelectedItem.Text and SelectedItem.Value properties.</span></span>

## <a name="selecting-the-list-of-items-from-the-database"></a><span data-ttu-id="f0e87-153">Öğe listesinin veritabanından seçme</span><span class="sxs-lookup"><span data-stu-id="f0e87-153">Selecting the List of Items from the Database</span></span>

<span data-ttu-id="f0e87-154">ComboBox görüntüler öğe listesinin bir veritabanından alabilir.</span><span class="sxs-lookup"><span data-stu-id="f0e87-154">You can retrieve the list of items that the ComboBox displays from a database.</span></span> <span data-ttu-id="f0e87-155">Örneğin, bir SqlDataSource denetimi, bir ObjectDataSource Denetimi, bir LinqDataSource veya bir EntityDataSource ComboBox bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-155">For example, you can bind the ComboBox to a SqlDataSource control, an ObjectDataSource control, a LinqDataSource, or an EntityDataSource.</span></span>

<span data-ttu-id="f0e87-156">ComboBox içinde filmler listesini görüntülemek istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="f0e87-156">Imagine that you want to display a list of movies in a ComboBox.</span></span> <span data-ttu-id="f0e87-157">Film veritabanı tablosundan filmler listesini almak istiyor.</span><span class="sxs-lookup"><span data-stu-id="f0e87-157">You want to retrieve the list of movies from the Movies database table.</span></span> <span data-ttu-id="f0e87-158">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f0e87-158">Follow these steps:</span></span>

1. <span data-ttu-id="f0e87-159">Movies.aspx adlı bir sayfa oluşturma</span><span class="sxs-lookup"><span data-stu-id="f0e87-159">Create a page named Movies.aspx</span></span>
2. <span data-ttu-id="f0e87-160">Bir ScriptManager denetimi, araç çubuğundaki sayfaya AJAX uzantılar sekmesi altında ScriptManager sürükleyerek sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-160">Add a ScriptManager control to the page by dragging the ScriptManager from under the AJAX Extensions tab in the Toolbox onto the page.</span></span>
3. <span data-ttu-id="f0e87-161">ComboBox denetimi ComboBox sayfaya sürükleyerek sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-161">Add a ComboBox control to the page by dragging the ComboBox onto the page.</span></span>
4. <span data-ttu-id="f0e87-162">Tasarım görünümünde, farenizi üzerinde ComboBox denetim getirin ve seçin **veri kaynağı Seç** görev seçeneği (bkz. Şekil 5).</span><span class="sxs-lookup"><span data-stu-id="f0e87-162">In Design view, hover your mouse over the ComboBox control and select the **Choose Data Source** task option (see Figure 5).</span></span> <span data-ttu-id="f0e87-163">Veri Kaynağı Yapılandırma Sihirbazı başlatılır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-163">The Data Source Configuration Wizard is launched.</span></span>
5. <span data-ttu-id="f0e87-164">İçinde **veri kaynağı seçin** adım, select &lt;yeni veri kaynağı&gt; seçeneği.</span><span class="sxs-lookup"><span data-stu-id="f0e87-164">In the **Choose a Data Source** step, select the &lt;New data source�&gt; option.</span></span>
6. <span data-ttu-id="f0e87-165">İçinde **bir veri kaynağı türü seç** adım, veritabanını seçin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-165">In the **Choose a Data Source Type** step, select Database.</span></span>
7. <span data-ttu-id="f0e87-166">İçinde **veri bağlantınızı** adım, veritabanınızı (örneğin, MoviesDB.mdf) seçin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-166">In the **Choose Your Data Connection** step, select your database (for example, MoviesDB.mdf).</span></span>
8. <span data-ttu-id="f0e87-167">İçinde **bağlantı dizesini uygulama yapılandırma dosyasını Kaydet** adım, bağlantı dizenizi kaydetme seçeneğini seçin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-167">In the **Save the Connection String to the Application Configuration File** step, select the option to save your connection string.</span></span>
9. <span data-ttu-id="f0e87-168">İçinde **Select deyimi yapılandırma** adım, filmler veritabanı tablosu seçin ve tüm sütunları seçin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-168">In the **Configure the Select Statement** step, select the Movies database table and select all of the columns.</span></span>
10. <span data-ttu-id="f0e87-169">İçinde **Test sorgusu** adım, son düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f0e87-169">In the **Test Query** step, click the Finish button.</span></span>
11. <span data-ttu-id="f0e87-170">Geri **veri kaynağı Seç** görüntülenecek alan için başlık sütunu ve kimlik sütunu için veri alan (bkz. Şekil) adımı seçin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-170">Back in the **Choose Data Source** step, select the Title column for the field to display and the Id column for the data field (see Figure).</span></span>
12. <span data-ttu-id="f0e87-171">Sihirbazı kapatmak için Tamam düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f0e87-171">Click the OK button to close the wizard.</span></span>


<span data-ttu-id="f0e87-172">[![Veri kaynağı seçme](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-172">[![Choosing a data source](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span></span>

<span data-ttu-id="f0e87-173">**Şekil 05**: veri kaynağı seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-173">**Figure 05**: Choosing a data source([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image10.png))</span></span>


<span data-ttu-id="f0e87-174">[![Veri metni ve değeri alanlarını seçme](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-174">[![Choosing the data text and value fields](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span></span>

<span data-ttu-id="f0e87-175">**Şekil 06**: veri metin ve değer alanları seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-175">**Figure 06**: Choosing the data text and value fields([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image12.png))</span></span>


<span data-ttu-id="f0e87-176">Yukarıdaki adımları tamamladıktan sonra filmler filmler veritabanı tablosundan temsil eden bir SqlDataSource denetimi ComboBox bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-176">After you complete the steps above, the ComboBox is bound to a SqlDataSource control that represents the movies from the Movies database table.</span></span> <span data-ttu-id="f0e87-177">Sayfa için bir kaynak (t biraz biçimlendirme temizlendi) 2 listeleme gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="f0e87-177">The source for the page looks like Listing 2 (I cleaned up the formatting a little bit).</span></span>

<span data-ttu-id="f0e87-178">**Listing 2 - Movies.aspx**</span><span class="sxs-lookup"><span data-stu-id="f0e87-178">**Listing 2 - Movies.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

<span data-ttu-id="f0e87-179">ComboBox denetimi SqlDataSource denetim noktaları bir DataSourceID özelliği olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-179">Notice that the ComboBox control has a DataSourceID property that points to the SqlDataSource control.</span></span> <span data-ttu-id="f0e87-180">Bir tarayıcıda sayfasını açtığınızda, filmler veritabanından listesi görüntülenir (bkz. Şekil 7).</span><span class="sxs-lookup"><span data-stu-id="f0e87-180">When you open the page in a browser, the list of movies from the database is displayed (see Figure 7).</span></span> <span data-ttu-id="f0e87-181">Ya da bir çekme listesinden bir filmi olabilir veya film ComboBox yazarak yeni bir filmi girin.</span><span class="sxs-lookup"><span data-stu-id="f0e87-181">You can either a pick a movie from the list or enter a new movie by typing the movie into the ComboBox.</span></span>


<span data-ttu-id="f0e87-182">[![Film listesini görüntüleme](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-182">[![Displaying a list of movies](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span></span>

<span data-ttu-id="f0e87-183">**Şekil 07**: filmler listesini görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-183">**Figure 07**: Displaying a list of movies([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image14.png))</span></span>


## <a name="setting-the-dropdownstyle"></a><span data-ttu-id="f0e87-184">DropDownStyle ayarlama</span><span class="sxs-lookup"><span data-stu-id="f0e87-184">Setting the DropDownStyle</span></span>

<span data-ttu-id="f0e87-185">ComboBox davranışını değiştirmek için ComboBox DropDownStyle özelliğini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-185">You can use the ComboBox DropDownStyle property to change the behavior of the ComboBox.</span></span> <span data-ttu-id="f0e87-186">Bu özellik var kabul olası değerler:</span><span class="sxs-lookup"><span data-stu-id="f0e87-186">This property accepts there possible values:</span></span>

- <span data-ttu-id="f0e87-187">Aşağı açılan - bir açılır liste oku ve tıkladığınızda (varsayılan değer) ComboBox görüntüler özel bir değer girebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-187">DropDown - (default value) The ComboBox displays a dropdown list when you click the arrow and you can enter a custom value.</span></span>
- <span data-ttu-id="f0e87-188">Basit - ComboBox otomatik olarak açılır listesini görüntüler ve özel bir değer girebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-188">Simple - The ComboBox displays a dropdown list automatically and you can enter a custom value.</span></span>
- <span data-ttu-id="f0e87-189">DropDownList - ComboBox yalnızca bir DropDownList denetimi gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-189">DropDownList - The ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="f0e87-190">Öğe listesinin görüntülendiğinde açılır arasında basit farklıdır.</span><span class="sxs-lookup"><span data-stu-id="f0e87-190">The different between DropDown and Simple is when the list of items is displayed.</span></span> <span data-ttu-id="f0e87-191">ComboBox odak taşıdığınızda hemen basit söz konusu olduğunda, liste görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f0e87-191">In the case of Simple, the list is displayed immediately when you move focus to the ComboBox.</span></span> <span data-ttu-id="f0e87-192">Açılan listesinde olması durumunda, öğelerin listesini görmek için oka tıklayın gerekir.</span><span class="sxs-lookup"><span data-stu-id="f0e87-192">In the case of DropDown, you must click the arrow to see the list of items.</span></span>

<span data-ttu-id="f0e87-193">DropDownList değeri standart DropDownList denetimi gibi yalnızca bir iş için ComboBox denetimine neden olur.</span><span class="sxs-lookup"><span data-stu-id="f0e87-193">The DropDownList value causes the ComboBox control to work just like a standard DropDownList control.</span></span> <span data-ttu-id="f0e87-194">Ancak burada önemli bir fark yoktur.</span><span class="sxs-lookup"><span data-stu-id="f0e87-194">However, there is an important difference here.</span></span> <span data-ttu-id="f0e87-195">Denetimi, önündeki yerleştirilen herhangi bir denetim önünde görüntülenmesi için Internet Explorer'ın daha eski sürümleri sonsuz bir z-index DropDownList denetimiyle görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f0e87-195">Older versions of Internet Explorer display a DropDownList control with an infinite z-index so the control will appear in front of any control placed in front of it.</span></span> <span data-ttu-id="f0e87-196">ComboBox bir HTML işleyen çünkü &lt;div&gt; yerine bir HTML etiketi &lt;seçin&gt; etiketi ComboBox doğru uyar z sıralamasını.</span><span class="sxs-lookup"><span data-stu-id="f0e87-196">Because the ComboBox renders an HTML &lt;div&gt; tag instead of an HTML &lt;select&gt; tag, the ComboBox correctly respects z-ordering.</span></span>

## <a name="setting-the-autocompletemode"></a><span data-ttu-id="f0e87-197">AutoCompleteMode özelliği ayarlama</span><span class="sxs-lookup"><span data-stu-id="f0e87-197">Setting the AutoCompleteMode</span></span>

<span data-ttu-id="f0e87-198">ComboBox AutoCompleteMode özelliği özelliğini birisi metin ComboBox yazdığında ne olacağını belirlemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="f0e87-198">You use the ComboBox AutoCompleteMode property to specify what happens when someone types text into the ComboBox.</span></span> <span data-ttu-id="f0e87-199">Bu özellik aşağıdaki olası değerlerini kabul eder:</span><span class="sxs-lookup"><span data-stu-id="f0e87-199">This property accepts the following possible values:</span></span>

- <span data-ttu-id="f0e87-200">Hiçbiri - (varsayılan değer) ComboBox herhangi otomatik tamamlama davranışı sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-200">None - (default value) The ComboBox does not provide any auto-complete behavior.</span></span>
- <span data-ttu-id="f0e87-201">Önermek - ComboBox listesini görüntüler ve listedeki eşleşen öğe vurgular (bkz. Şekil 8).</span><span class="sxs-lookup"><span data-stu-id="f0e87-201">Suggest - The ComboBox displays the list and it highlights the matching item in the list (see Figure 8).</span></span>
- <span data-ttu-id="f0e87-202">Append - ComboBox listesi görüntülemez ve (bkz. Şekil 9) yazdığınızı üzerine listeden eşleşen öğe ekler.</span><span class="sxs-lookup"><span data-stu-id="f0e87-202">Append - The ComboBox does not display the list and it appends the matching item from the list onto what you have typed (see Figure 9).</span></span>
- <span data-ttu-id="f0e87-203">SuggestAppend - ComboBox görüntüler hem eşleşen öğe (bkz. Şekil 10) yazdığınızı üzerine listeden ekler.</span><span class="sxs-lookup"><span data-stu-id="f0e87-203">SuggestAppend - The ComboBox both displays the list and appends the matching item from the list onto what you have typed (see Figure 10).</span></span>


<span data-ttu-id="f0e87-204">[![ComboBox öneride yapar](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-204">[![The ComboBox makes a suggestion](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span></span>

<span data-ttu-id="f0e87-205">**Şekil 08**: ComboBox öneride yapar ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-205">**Figure 08**: The ComboBox makes a suggestion([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image16.png))</span></span>


<span data-ttu-id="f0e87-206">[![Eşleşen metin ComboBox ekler](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-206">[![ComboBox appends matching text](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span></span>

<span data-ttu-id="f0e87-207">**Şekil 09**: ComboBox eşleşen metin ekler ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-207">**Figure 09**: ComboBox appends matching text([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image18.png))</span></span>


<span data-ttu-id="f0e87-208">[![ComboBox önerir ve ekler](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="f0e87-208">[![The ComboBox suggests and appends](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span></span>

<span data-ttu-id="f0e87-209">**Şekil 10**: ComboBox önerir ve ekler ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="f0e87-209">**Figure 10**: The ComboBox suggests and appends([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image20.png))</span></span>


## <a name="summary"></a><span data-ttu-id="f0e87-210">Özet</span><span class="sxs-lookup"><span data-stu-id="f0e87-210">Summary</span></span>

<span data-ttu-id="f0e87-211">Bu öğreticide, ComboBox denetimi sabit bir öğe kümesini görüntülemek için nasıl kullanılacağı hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-211">In this tutorial, you learned how to use the ComboBox control to display a fixed set of items.</span></span> <span data-ttu-id="f0e87-212">ComboBox denetimi hem öğelerinin ayarlamak statik ve bir veritabanı tablosu biz bağlı.</span><span class="sxs-lookup"><span data-stu-id="f0e87-212">We bound the ComboBox control both to a static set of items and to a database table.</span></span> <span data-ttu-id="f0e87-213">Son olarak, DropDownStyle ve AutoCompleteMode özelliği özelliklerini ayarlayarak ComboBox davranışını değiştirmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="f0e87-213">Finally, you learned how to modify the behavior of the ComboBox by setting its DropDownStyle and AutoCompleteMode properties.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f0e87-214">Önceki</span><span class="sxs-lookup"><span data-stu-id="f0e87-214">Previous</span></span>](how-do-i-use-the-combobox-control-cs.md)
