---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: ColorPicker denetim genişletici (C#) kullanarak | Microsoft Docs
author: microsoft
description: ColorPicker kullanıcı Arabirimi ile bir açılan denetiminde istemci-tarafı renk çekme işlevselliği sağlayan ASP.NET AJAX genişletici ' dir. Tüm ASP.NET eklenebilecek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 4d44fc81305e668b545246cf044dce275563d81a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="6059b-104">ColorPicker denetim genişletici (C#) kullanarak</span><span class="sxs-lookup"><span data-stu-id="6059b-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="6059b-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6059b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6059b-106">ColorPicker kullanıcı Arabirimi ile bir açılan denetiminde istemci-tarafı renk çekme işlevselliği sağlayan ASP.NET AJAX genişletici ' dir.</span><span class="sxs-lookup"><span data-stu-id="6059b-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="6059b-107">Herhangi bir ASP.NET TextBox denetimi eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="6059b-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="6059b-108">.</span><span class="sxs-lookup"><span data-stu-id="6059b-108">It.</span></span>


<span data-ttu-id="6059b-109">Bu öğreticinin amacı AJAX Denetim Araç Seti ColorPicker denetim genişletici nasıl kullanabileceğinizi açıklar belirlemektir.</span><span class="sxs-lookup"><span data-stu-id="6059b-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="6059b-110">ColorPicker denetim genişletici bir renk seçmenizi sağlayan bir açılır iletişim kutusu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6059b-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="6059b-111">Bir renk seçmek bir kullanıcı için bir sezgisel bir kullanıcı arabirimi sağlamak istediğinizde ColorPicker yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="6059b-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="6059b-112">TextBox denetimi ColorPicker denetim genişletici ile genişletme</span><span class="sxs-lookup"><span data-stu-id="6059b-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="6059b-113">Örneğin, özelleştirilmiş kartvizitler oluşturmak ziyaretçileri sağlayan bir Web sitesi oluşturmak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="6059b-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="6059b-114">Ziyaretçi için bir iş kart metni girin ve rengi seçin.</span><span class="sxs-lookup"><span data-stu-id="6059b-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="6059b-115">ASP.NET sayfası listeleme 1 txtCardText ve txtCardColor adlı iki TextBox denetimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="6059b-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="6059b-116">Formu gönderdiğinde, seçili değerler görüntülenir (bkz: Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="6059b-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="6059b-117">[![Bir iş kartı oluşturmak için basit form](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6059b-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="6059b-118">**Şekil 01**: bir iş kartı oluşturmak için basit form ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6059b-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="6059b-119">**Listing 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="6059b-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="6059b-120">Formun listeleme 1 çalışır, ancak bir iyi kullanıcı deneyimi sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="6059b-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="6059b-121">Bir renk metin kutusuna yazmak kullanıcı vardır.</span><span class="sxs-lookup"><span data-stu-id="6059b-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="6059b-122">Özel renk - kullanıcı istiyorsa, örneğin, yalnızca pea yeşil - sonra kullanıcı sağ gölgesini HTML renk kodu yardıma olmadan tahmin gerekir.</span><span class="sxs-lookup"><span data-stu-id="6059b-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="6059b-123">ColorPicker denetim genişletici daha iyi bir kullanıcı deneyimi oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6059b-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="6059b-124">TextBox denetimi odağı taşıdığınızda ColorPicker bir renk iletişim kutusu görüntüler (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="6059b-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="6059b-125">[![ColorPicker denetim genişletici](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="6059b-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="6059b-126">**Şekil 02**: ColorPicker denetim genişletici ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="6059b-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="6059b-127">ColorPicker denetim genişletici listeleme 1 formunda ile kullanmak için iki adımları tamamlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="6059b-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="6059b-128">Sayfaya bir ScriptManager denetimi ekleyin</span><span class="sxs-lookup"><span data-stu-id="6059b-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="6059b-129">ColorPicker denetim genişletici sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="6059b-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="6059b-130">ColorPicker kullanabilmeniz için önce bir ScriptManager sayfanıza eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6059b-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="6059b-131">ScriptManager eklemek için sağ açılış sunucu tarafı uygundur &lt;form&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="6059b-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="6059b-132">(ScriptManager AJAX uzantılar sekmesi altında bulunur) araç sayfaya ScriptManager sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="6059b-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="6059b-133">Alternatif olarak, aşağıdaki etiketi açma sunucu tarafı form etiketi altında kaynak görünüme yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6059b-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="6059b-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="6059b-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="6059b-135">Tasarım görünümünde ColorPicker denetim genişletici eklemek için en kolay yoludur.</span><span class="sxs-lookup"><span data-stu-id="6059b-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="6059b-136">Farenizi TextBox txtCardColor gelin, bir akıllı görev seçeneği etkinleştirir görünür genişletici eklemenizi (bkz: Şekil 3).</span><span class="sxs-lookup"><span data-stu-id="6059b-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="6059b-137">Bu seçeneği seçerseniz, (bkz: Şekil 4) genişletici Sihirbazı görünür.</span><span class="sxs-lookup"><span data-stu-id="6059b-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="6059b-138">[![Genişletici ekleme](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="6059b-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="6059b-139">**Şekil 03**: genişletici ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6059b-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="6059b-140">[![Genişletici Sihirbazı'nı kullanarak bir denetim extender seçme](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="6059b-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="6059b-141">**Şekil 04**: denetim genişletici genişletici Sihirbazı'nı seçerek ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="6059b-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="6059b-142">ColorPicker genişletici ile TextBox txtCardColor genişletmek için ColorPicker genişletici seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6059b-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="6059b-143">İletişim kutusunu kapatmak için Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="6059b-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="6059b-144">Bu değişiklikleri yaptıktan sonra sayfa kaynağı listeleme 2 gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="6059b-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="6059b-145">2 - CreateCard.aspx (ile ColorPicker) listeleme</span><span class="sxs-lookup"><span data-stu-id="6059b-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="6059b-146">Sayfa artık doğrudan txtCardColor TextBox denetimi görünen ColorPickerExtender denetim içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6059b-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="6059b-147">Böylece bir Renk Seçici iletişim kutusu görüntüler ColorPickerExtender denetim txtCardColor denetim genişletir.</span><span class="sxs-lookup"><span data-stu-id="6059b-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="6059b-148">Renk Seçici iletişim düğme kullanarak</span><span class="sxs-lookup"><span data-stu-id="6059b-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="6059b-149">ColorPicker genişletici aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="6059b-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="6059b-150">PopupButtonId - görünmesi Renk Seçici iletişim neden sayfasındaki bir düğmeyi Kimliği'ni tıklatın.</span><span class="sxs-lookup"><span data-stu-id="6059b-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="6059b-151">PopupPosition - hedef denetimin Renk Seçici iletişim göre konumu.</span><span class="sxs-lookup"><span data-stu-id="6059b-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="6059b-152">Olası değerler şunlardır: mutlak, merkezi, sol alt, BottomRight, sol üst, sağ üst, sağ ve sol (sol alt varsayılandır).</span><span class="sxs-lookup"><span data-stu-id="6059b-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="6059b-153">SampleControlId - seçilen rengin görüntüleyen bir denetim kimliği'ni tıklatın.</span><span class="sxs-lookup"><span data-stu-id="6059b-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="6059b-154">SelectedColor - ColorPicker tarafından seçilen ilk renk.</span><span class="sxs-lookup"><span data-stu-id="6059b-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="6059b-155">Renk Seçici iletişim nasıl görüntülenir ve seçilen rengin nasıl görüntüleneceğini özelleştirmek için bu özellikleri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6059b-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="6059b-156">Sayfanın listeleme 3'te birkaç bu özelliklerin nasıl kullanabileceğiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6059b-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="6059b-157">**Listing 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="6059b-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="6059b-158">Renk Seç sayfasında listeleme 3'te içerir (bkz. Şekil 5) düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6059b-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="6059b-159">Bu düğmeye tıkladığınızda, TextBox Renk Seçici iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6059b-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="6059b-160">İletişim kutusundan bir renk seçerseniz seçili renk etiket denetimi lblSample arka plan rengi olarak görünür.</span><span class="sxs-lookup"><span data-stu-id="6059b-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="6059b-161">ColorPicker PopupButtonID özelliği, Renk Seç düğmesini ColorPicker extender ile ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6059b-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="6059b-162">PopupButtonID özelliği için bir değer sağladığında, hedef denetimi odağa sahip olduğunda Renk Seçici iletişim kutusu artık görünür.</span><span class="sxs-lookup"><span data-stu-id="6059b-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="6059b-163">İletişim kutusunu görüntülemek için düğmesini gerekir.</span><span class="sxs-lookup"><span data-stu-id="6059b-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="6059b-164">SampleControlID özelliği ColorPicker seçili renkle görüntüleyen bir denetim ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6059b-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="6059b-165">ColorPicker bu denetim arka plan rengini şu anda seçili renk değiştirir.</span><span class="sxs-lookup"><span data-stu-id="6059b-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="6059b-166">[![Düğme için Renk Seçici iletişim kutusunu görüntüleme](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="6059b-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="6059b-167">**Şekil 05**: düğme için Renk Seçici iletişim kutusunu görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="6059b-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="6059b-168">Özet</span><span class="sxs-lookup"><span data-stu-id="6059b-168">Summary</span></span>

<span data-ttu-id="6059b-169">Bu öğreticide, ColorPicker denetim genişletici açılan Renk Seçici iletişim kutusunu görüntülemek için nasıl kullanılacağı hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="6059b-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="6059b-170">İlk olarak, odak TextBox denetimine taşındığında nasıl iletişim kutusunu görüntüleyebilir incelendi.</span><span class="sxs-lookup"><span data-stu-id="6059b-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="6059b-171">Ardından, Renk Seçici iletişim düğmesine tıklandığında görüntüleyen bir düğmeye oluşturma hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="6059b-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6059b-172">Next</span><span class="sxs-lookup"><span data-stu-id="6059b-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
