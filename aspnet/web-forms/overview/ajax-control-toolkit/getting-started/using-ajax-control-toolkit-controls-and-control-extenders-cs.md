---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: AJAX Denetim Araç Seti denetimlerini ve denetim Genişleticilerini (C#) kullanma | Microsoft Docs
author: microsoft
description: AJAX Denetim Araç Seti denetimlerini ve genişleticilerini ASP.NET sayfalarınıza eklemeyi öğrenin.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 18ee6dd71fe0e84ec7628eba63aabeee0690d0b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756230"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a><span data-ttu-id="4ab1c-103">AJAX Denetim Araç Seti denetimlerini ve denetim Genişleticilerini (C#) kullanma</span><span class="sxs-lookup"><span data-stu-id="4ab1c-103">Using AJAX Control Toolkit Controls and Control Extenders (C#)</span></span>
====================
<span data-ttu-id="4ab1c-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4ab1c-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="4ab1c-105">AJAX Denetim Araç Seti denetimlerini ve genişleticilerini ASP.NET sayfalarınıza eklemeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="4ab1c-106">AJAX Denetim Araç Seti denetimlerini ve denetim genişleticilerini kümesi içerir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="4ab1c-107">Bu kısa öğreticide bir ASP.NET sayfasına hem denetimlerini ve denetim genişleticilerini ekleme konusunda bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4ab1c-108">AJAX Denetim Araç Seti yükleme ve AJAX Denetim Araç Seti Visual Studio/Visual Web Developer araç kutusuna ekleme yönergeleri görmek için öğreticiyi [AJAX Denetim Araç Seti ile çalışmaya başlama](get-started-with-the-ajax-control-toolkit-cs.md).</span><span class="sxs-lookup"><span data-stu-id="4ab1c-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="4ab1c-109">AJAX Denetim Araç Seti denetimlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="4ab1c-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="4ab1c-110">AJAX Denetim Araç Seti denetim, yalnızca normal ASP.NET denetim gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="4ab1c-111">Bir ASP.NET sayfasının araç kutusundan denetimi sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="4ab1c-112">Tasarım görünümü veya kaynak görünümü sayfası denetimi ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="4ab1c-113">AJAX Denetim Araç Seti denetimlerini kullanarak özel bir gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="4ab1c-114">Sayfanın bir ScriptManager denetimi içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="4ab1c-115">ScriptManager denetimi için tüm gerekli JavaScript AJAX Denetim Araç Seti denetimlerini tarafından gerekli birlikte sorumludur.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="4ab1c-116">Örneğin, AJAX Denetim Araç Seti sekmesi düzenleyici denetimi adlı bir denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="4ab1c-117">Bu denetimi bir zengin HTML Düzenleyicisi'ni görüntüler.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="4ab1c-118">Düzenleyici denetimi bir sayfasına eklemek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="4ab1c-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="4ab1c-119">ShowEditor.aspx adlı yeni bir ASP.NET sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ab1c-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="4ab1c-120">Araç kutusunda AJAX uzantılar sekmesinde from beneath ScriptManager denetimini seçin ve denetim sayfaya sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="4ab1c-121">Araç kutusunda düzenleyici denetimi from beneath AJAX Denetim Araç Seti sekmesini seçin ve denetim sayfaya sürükleyin (bkz. Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="4ab1c-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="4ab1c-122">Tasarımcı, Şekil 2'gibi görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="4ab1c-123">Menü seçeneğini belirleyerek web sitesi çalıştırmak **hata ayıklama, hata ayıklamayı Başlat** veya F5 tuşuna basarak.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="4ab1c-124">Şekil 3'te sayfası görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="4ab1c-125">[![HTML düzenleyicisi denetimi seçme](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4ab1c-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span></span>

<span data-ttu-id="4ab1c-126">**Şekil 01**: HTML düzenleyicisi denetimi seçme ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4ab1c-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span></span>


<span data-ttu-id="4ab1c-127">[![ScriptManager ve düzen denetimi ile Visual Studio Tasarımcısı](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4ab1c-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span></span>

<span data-ttu-id="4ab1c-128">**Şekil 02**: ScriptManager ve düzen denetimi ile Visual Studio Tasarımcısı ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="4ab1c-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span></span>


<span data-ttu-id="4ab1c-129">[![DisplayEditor.aspx sayfası](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="4ab1c-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span></span>

<span data-ttu-id="4ab1c-130">**Şekil 03**: DisplayEditor.aspx sayfa ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4ab1c-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="4ab1c-131">AJAX Denetim Araç Seti denetim Genişleticilerini kullanma</span><span class="sxs-lookup"><span data-stu-id="4ab1c-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="4ab1c-132">AJAX Denetim Araç Seti denetim genişleticilerini de içerir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="4ab1c-133">Adından da anlaşılacağı gibi bir denetim genişletici varolan bir denetimi işlevselliğini genişletir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="4ab1c-134">Örneğin, standart ASP.NET düğme denetimini ConfirmButton denetim genişletici genişletir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="4ab1c-135">Düğmeyi tıklattığınızda düğme onay iletişim kutusu görüntülenir. böylece genişletici düğme denetimi s davranışını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="4ab1c-136">Bir AJAX Denetim Araç Seti denetimi gibi bir denetim genişletici bir ScriptManager denetimi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="4ab1c-137">Denetim genişleticilerini sayfasında kullanmaya başlamadan önce bir ScriptManager denetimi bir sayfasına eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="4ab1c-138">ConfirmButton denetim genişletici kullanmak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="4ab1c-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="4ab1c-139">ShowConfirmButton.aspx adlı yeni bir ASP.NET sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ab1c-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="4ab1c-140">Bir ScriptManager denetimi sayfasına AJAX uzantılar sekmesinde from beneath sayfaya, Denetim sürükleyerek ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="4ab1c-141">Standart bir düğme denetimi, Standart sekmesini from beneath düğme araç kutusunda Tasarımcı yüzeyine sürükleyerek sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="4ab1c-142">Tıklayın **ekleme genişletici** görev seçeneği (bkz: Şekil 4).</span><span class="sxs-lookup"><span data-stu-id="4ab1c-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="4ab1c-143">Genişletici seçin iletişim kutusunda ConfirmButtonExtender seçin (bkz: Şekil 5) ve Tamam düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="4ab1c-144">Tasarımcıda düğme denetimini seçin ve Genişleticileri Button1 genişletin\_ConfirmButtonExtender düğümü Özellikler penceresinde (bkz. Şekil 6).</span><span class="sxs-lookup"><span data-stu-id="4ab1c-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="4ab1c-145">Değer atamak *gerçekten?* ConfirmText özelliğine.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="4ab1c-146">Menü seçeneği seçerek çalıştırırsanız **hata ayıklama, hata ayıklamayı Başlat** veya F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="4ab1c-147">[![Genişletici ekleme görev seçeneği](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4ab1c-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span></span>

<span data-ttu-id="4ab1c-148">**Şekil 04**: ekleme genişletici görev seçeneği ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="4ab1c-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span></span>


<span data-ttu-id="4ab1c-149">[![ConfirmButton denetim genişletici seçme](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="4ab1c-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span></span>

<span data-ttu-id="4ab1c-150">**Şekil 05**: seçme ConfirmButton denetim genişletici ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="4ab1c-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span></span>


<span data-ttu-id="4ab1c-151">[![ConfirmButton özelliğini ayarlama](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="4ab1c-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span></span>

<span data-ttu-id="4ab1c-152">**Şekil 06**: ConfirmButton özelliği ayarı ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="4ab1c-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span></span>


<span data-ttu-id="4ab1c-153">Sayfa açıldığında bir düğme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="4ab1c-154">Düğmeye tıkladığınızda, Şekil 7'de onay iletişim kutusunda alın.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="4ab1c-155">[![Onay iletişim kutusunda görüntüleme](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="4ab1c-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span></span>

<span data-ttu-id="4ab1c-156">**Şekil 07**: onay iletişim kutusunda görüntüleme ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="4ab1c-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span></span>


<span data-ttu-id="4ab1c-157">Normalde bir denetim genişletici bir sayfaya sürükleyin değil, dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="4ab1c-158">Bunun yerine, kullandığınız **ekleme genişletici** görev bir sayfasına eklemiş bir denetim için bir uzatıcı ekleme seçeneği.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="4ab1c-159">Ayrıca, Denetim Genişletilmekte olan denetim için özellik sayfası açarak genişletici özelliklerini ayarlamak dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="4ab1c-160">Tek bir ASP.NET denetimi tarafından birden çok denetim genişleticilerini genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="4ab1c-161">Genişletilmekte olan denetim için özellik sayfası tüm denetimle ilişkili denetim genişleticilerini listeler.</span><span class="sxs-lookup"><span data-stu-id="4ab1c-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4ab1c-162">[Önceki](get-started-with-the-ajax-control-toolkit-cs.md)
> [İleri](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4ab1c-162">[Previous](get-started-with-the-ajax-control-toolkit-cs.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span></span>
