---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: ColorPicker denetim genişletici (C#) kullanarak | Microsoft Docs
author: microsoft
description: ColorPicker kullanıcı Arabirimi ile bir açılan denetimden içinde istemci tarafı renk çekme işlevselliği sağlayan bir ASP.NET AJAX genişletici ' dir. Tüm ASP.NET eklenebilecek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: f20928099e2b4db477705cd1634fd28745a328ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383910"
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="c06cb-104">ColorPicker denetim genişletici (C#) kullanma</span><span class="sxs-lookup"><span data-stu-id="c06cb-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="c06cb-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c06cb-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c06cb-106">ColorPicker kullanıcı Arabirimi ile bir açılan denetimden içinde istemci tarafı renk çekme işlevselliği sağlayan bir ASP.NET AJAX genişletici ' dir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="c06cb-107">Herhangi bir ASP.NET TextBox denetimi eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="c06cb-108">.</span><span class="sxs-lookup"><span data-stu-id="c06cb-108">It.</span></span>


<span data-ttu-id="c06cb-109">Bu öğreticinin amacı, AJAX Denetim Araç Seti ColorPicker denetim genişletici nasıl kullanabileceğinizi açıklar sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="c06cb-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="c06cb-110">ColorPicker denetim genişletici rengi seçmenize olanak sağlayan bir açılır iletişim kutusu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="c06cb-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="c06cb-111">ColorPicker bir renk seçmek bir kullanıcı için bir sezgisel kullanıcı arabirimiyle sağlamak istediğinizde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="c06cb-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="c06cb-112">ColorPicker denetim genişletici ile bir metin kutusu denetimini genişletme</span><span class="sxs-lookup"><span data-stu-id="c06cb-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="c06cb-113">Örneğin, özelleştirilmiş iş kartları oluşturmak ziyaretçilerinin sağlayan bir Web sitesi oluşturmak istediğinizi düşünün.</span><span class="sxs-lookup"><span data-stu-id="c06cb-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="c06cb-114">Ziyaretçi için bir iş kartı metin girebilir ve renk seçin.</span><span class="sxs-lookup"><span data-stu-id="c06cb-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="c06cb-115">ASP.NET sayfası listeleme 1 txtCardText ve txtCardColor adlı iki TextBox denetimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="c06cb-116">Formu gönderdiğinde, seçili değerler görüntülenir (bkz. Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="c06cb-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="c06cb-117">[![Basit bir iş kartı oluşturma formu](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c06cb-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="c06cb-118">**Şekil 01**: bir iş kartı oluşturmak için basit bir form ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c06cb-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="c06cb-119">**1 - CreateCard.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="c06cb-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="c06cb-120">1 listeleme çalışır, ancak biçiminde harika kullanıcı deneyimini sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="c06cb-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="c06cb-121">Kullanıcı bir renk TextBox'a türüne sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="c06cb-122">Örneğin, kullanıcı özel bir renk - isterse yoğun saatlerin yeşil - ardından kullanıcı yalnızca doğru gölge HTML rengi kodunu yardıma gerek kalmadan şekil gerekir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="c06cb-123">ColorPicker denetim genişletici daha iyi bir kullanıcı deneyimi oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c06cb-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="c06cb-124">TextBox denetimi için odak taşıdığınızda ColorPicker bir renk iletişim kutusu görüntüler (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="c06cb-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="c06cb-125">[![ColorPicker denetim genişletici](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c06cb-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="c06cb-126">**Şekil 02**: ColorPicker denetim genişletici ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="c06cb-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="c06cb-127">ColorPicker denetim genişletici listeleme 1 biçiminde kullanmak için iki adımı tamamlamamız gerekir:</span><span class="sxs-lookup"><span data-stu-id="c06cb-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="c06cb-128">Bir ScriptManager denetimi sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="c06cb-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="c06cb-129">ColorPicker denetim genişletici sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="c06cb-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="c06cb-130">ColorPicker kullanabilmeniz için önce bir ScriptManager sayfanıza eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="c06cb-131">ScriptManager eklemek iyi bir sağ açılış sunucu tarafı yerdir &lt;form&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="c06cb-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="c06cb-132">ScriptManager, sayfaya (ScriptManager AJAX uzantılar sekmesi altında bulunur) araç kutusundan sürükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c06cb-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="c06cb-133">Alternatif olarak, aşağıdaki etiketi altında sunucu tarafı formu etiketiyle kaynak görünüme yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c06cb-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="c06cb-134">&lt;ASP: ScriptManager kimliği = "ScriptManager1" runat = "server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="c06cb-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="c06cb-135">Tasarım görünümünde ColorPicker denetim genişletici sayfasına eklemek için en kolay yoludur.</span><span class="sxs-lookup"><span data-stu-id="c06cb-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="c06cb-136">Farenizi metin txtCardColor gelin, akıllı görev seçeneği sağlar görünür bir genişletici eklemeyi (bkz: Şekil 3).</span><span class="sxs-lookup"><span data-stu-id="c06cb-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="c06cb-137">Bu seçeneği seçerseniz, genişletici Sihirbazı'nı (bkz: Şekil 4) görünür.</span><span class="sxs-lookup"><span data-stu-id="c06cb-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="c06cb-138">[![Bir Genişletici ekleme](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c06cb-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="c06cb-139">**Şekil 03**: Bir Genişletici ekleme ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c06cb-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="c06cb-140">[![Genişletici Sihirbazı ile bir denetim genişletici seçme](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c06cb-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="c06cb-141">**Şekil 04**: bir denetim genişletici genişletici Sihirbazı'nı seçerek ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="c06cb-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="c06cb-142">' % S'txtCardColor TextBox ColorPicker genişletici ile genişletmek için ColorPicker genişletici seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c06cb-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="c06cb-143">İletişim kutusunu kapatmak için Tamam'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c06cb-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="c06cb-144">Bu değişiklikleri yaptıktan sonra sayfa için kaynak listeleme 2 gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="c06cb-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="c06cb-145">2 - CreateCard.aspx (ColorPicker ile) listeleme</span><span class="sxs-lookup"><span data-stu-id="c06cb-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="c06cb-146">Sayfa artık doğrudan txtCardColor TextBox denetiminde görünen ColorPickerExtender denetim içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="c06cb-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="c06cb-147">Böylece bir Renk Seçici iletişim kutusu görüntüler ColorPickerExtender denetimi txtCardColor denetim genişletir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="c06cb-148">Renk Seçici iletişim kutusunu başlatmak için bir düğme kullanma</span><span class="sxs-lookup"><span data-stu-id="c06cb-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="c06cb-149">ColorPicker genişletici aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="c06cb-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="c06cb-150">PopupButtonId - kimliği bir düğmeye sayfasında görüntülenecek Renk Seçici iletişim kutusunu neden olur.</span><span class="sxs-lookup"><span data-stu-id="c06cb-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="c06cb-151">PopupPosition - Renk Seçici iletişim kutusu, hedef denetime göre konumu.</span><span class="sxs-lookup"><span data-stu-id="c06cb-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="c06cb-152">Olası değerler şunlardır: mutlak, merkezi, sol alt, BottomRight, sol üst, sağ üst, sağ ve sol (sol alt varsayılandır).</span><span class="sxs-lookup"><span data-stu-id="c06cb-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="c06cb-153">SampleControlId - seçilen rengin görüntüleyen bir denetimi Kimliği'ni tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c06cb-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="c06cb-154">SelectedColor - ColorPicker tarafından seçilen ilk renk.</span><span class="sxs-lookup"><span data-stu-id="c06cb-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="c06cb-155">Bu özellikler, bir Renk Seçici iletişim kutusunu nasıl görüntüleneceğini ve seçilen rengin nasıl görüntüleneceğini özelleştirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c06cb-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="c06cb-156">Sayfa listesi 3'te birkaç bu özelliklerin nasıl kullanabileceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="c06cb-157">**3 - CreateCardButton.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="c06cb-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="c06cb-158">Renk Seç sayfasında listeleme 3 içerir (bkz: Şekil 5) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c06cb-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="c06cb-159">Bu düğmeye tıkladığınızda, metin kutusunun Renk Seçici iletişim kutusu görünür.</span><span class="sxs-lookup"><span data-stu-id="c06cb-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="c06cb-160">İletişim kutusundan bir renk seçin, seçilen rengin lblSample etiket denetiminin arka plan rengi görünür.</span><span class="sxs-lookup"><span data-stu-id="c06cb-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="c06cb-161">ColorPicker PopupButtonID özelliği, çekme rengi düğmesi ColorPicker genişletici ile ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c06cb-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="c06cb-162">PopupButtonID özelliği için bir değer sağladığında, hedef denetim odağa sahip olduğunda Renk Seçici iletişim bundan böyle görünür.</span><span class="sxs-lookup"><span data-stu-id="c06cb-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="c06cb-163">İletişim kutusunu görüntülemek için düğmesine tıklamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="c06cb-164">SampleControlID özellik ColorPicker seçilen rengi görüntüleyen bir denetimi ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c06cb-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="c06cb-165">ColorPicker bu denetimin arka plan rengini şu anda seçilen renge değişir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="c06cb-166">[![Renk Seçici iletişim kutusunu bir düğme ile görüntüleme](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="c06cb-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="c06cb-167">**Şekil 05**: Renk Seçici iletişim kutusunu bir düğme ile görüntüleme ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="c06cb-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="c06cb-168">Özet</span><span class="sxs-lookup"><span data-stu-id="c06cb-168">Summary</span></span>

<span data-ttu-id="c06cb-169">Bu öğreticide, bir açılan Renk Seçici iletişim kutusunu görüntülemek için ColorPicker denetim genişletici kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="c06cb-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="c06cb-170">İlk olarak, odağı bir TextBox denetimine taşındığında nasıl iletişim görüntüleyebilirsiniz incelenir.</span><span class="sxs-lookup"><span data-stu-id="c06cb-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="c06cb-171">Ardından, düğmeye tıklandığında, Renk Seçici iletişim kutusunu görüntüleyen bir düğmenin nasıl oluşturulacağını öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="c06cb-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c06cb-172">Next</span><span class="sxs-lookup"><span data-stu-id="c06cb-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
