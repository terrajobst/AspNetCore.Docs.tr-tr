---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: "Özel bir AJAX oluşturma Denetim Araç Seti denetim genişletici (VB) | Microsoft Docs"
author: microsoft
description: "Özel Extender'larının özelleştirebilir ve ASP.NET denetimleri yeni sınıflar oluşturmak zorunda kalmadan yeteneklerini olanak sağlar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3e8fceb3c7570aa1bf085c8e1037736254e74ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a><span data-ttu-id="3dbf8-103">Özel AJAX Denetim Araç Seti denetim genişletici (VB) oluşturma</span><span class="sxs-lookup"><span data-stu-id="3dbf8-103">Creating a Custom AJAX Control Toolkit Control Extender (VB)</span></span>
====================
<span data-ttu-id="3dbf8-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3dbf8-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="3dbf8-105">Özel Extender'larının özelleştirebilir ve ASP.NET denetimleri yeni sınıflar oluşturmak zorunda kalmadan yeteneklerini olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="3dbf8-106">Bu öğreticide, bir özel AJAX Denetim Araç Seti denetim genişletici oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="3dbf8-107">Metni metin kutusuna yazdığınızda etkin için devre dışı bir düğme durumu değişiklikleri yararlı, yeni genişletici ancak basit bir oluşturuyoruz.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="3dbf8-108">Bu öğretici okuduktan sonra ASP.NET AJAX araç seti ile kendi denetim Extender'larının süresini uzatabilir olacaktır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="3dbf8-109">Visual Studio veya Visual Web Developer kullanarak özel denetim Extender'larının oluşturma (Visual Web Developer en son sürümüne sahip olduğunuzdan emin olun).</span><span class="sxs-lookup"><span data-stu-id="3dbf8-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="3dbf8-110">DisabledButton genişletici genel bakış</span><span class="sxs-lookup"><span data-stu-id="3dbf8-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="3dbf8-111">Bizim yeni denetim genişletici DisabledButton genişletici olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="3dbf8-112">Bu genişletici üç özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-112">This extender will have three properties:</span></span>

- <span data-ttu-id="3dbf8-113">TargetControlID - denetimi metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="3dbf8-114">TargetButtonIID - etkin veya devre dışı düğmesi.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="3dbf8-115">DisabledText - düğmesini başlangıçta görüntülenen metin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="3dbf8-116">Yazmaya başladığınızda, düğmenin düğme metni özelliğinin değeri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="3dbf8-117">Bir metin kutusu ve düğme denetimine DisabledButton genişletici bağlayın.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="3dbf8-118">Herhangi bir metin yazın önce düğmesi devre dışı bırakılır ve metin kutusu ve düğmesi şöyle:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

<span data-ttu-id="3dbf8-119">([Tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3dbf8-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span></span>


<span data-ttu-id="3dbf8-120">Metin yazmaya başlayın sonra düğmesi etkinleştirilir ve metin kutusu ve düğmesi şöyle:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

<span data-ttu-id="3dbf8-121">([Tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="3dbf8-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="3dbf8-122">Bizim denetim genişletici oluşturmak için aşağıdaki üç dosyaları oluşturmak ihtiyacımız var:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="3dbf8-123">DisabledButtonExtender.vb - bu dosyanın extender oluşturma yönetmek ve tasarım zamanında özelliklerini ayarlamanıza olanak sağlar sunucu tarafı denetimi sınıftır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-123">DisabledButtonExtender.vb - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="3dbf8-124">Ayrıca, extender'ayarlanabilir özellikleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="3dbf8-125">Bu özellikler kodu aracılığıyla ve tasarım zamanında erişilebilir ve DisableButtonBehavior.js dosyasında tanımlanan özellikler eşleşmesi.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="3dbf8-126">DisabledButtonBehavior.js--, Tüm istemci komut dosyası mantığınızı burada ekleyeceksiniz bu dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="3dbf8-127">Bu sınıf DisabledButtonDesigner.vb - tasarım zamanı işlevselliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-127">DisabledButtonDesigner.vb - This class enables design-time functionality.</span></span> <span data-ttu-id="3dbf8-128">Visual Studio/Visual Web Developer Tasarımcısı ile düzgün çalışması için Denetim genişletici istiyorsanız bu sınıf gerekir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="3dbf8-129">Bu nedenle denetim genişletici bir sunucu tarafı denetimi, bir istemci-tarafı davranışı ve sunucu tarafı Tasarımcı sınıfını oluşur.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="3dbf8-130">Aşağıdaki bölümlerde bu dosyaları üç oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="3dbf8-131">Proje ve özel genişletici Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="3dbf8-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="3dbf8-132">İlk adım bir sınıf kitaplığı proje ve Web sitesi Visual Studio/Visual Web Developer oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="3dbf8-133">Biz üm özel genişletici sınıf kitaplığı projesi oluşturun ve Web sitesi özel genişletici test.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="3dbf8-134">Web sitesiyle başlama s olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-134">Let�s start with the website.</span></span> <span data-ttu-id="3dbf8-135">Web sitesi oluşturmak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="3dbf8-136">Menü seçeneğini **dosya, yeni Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="3dbf8-137">Seçin **ASP.NET Web sitesi** şablonu.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="3dbf8-138">Yeni Web sitesi adı *websitesi1*.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="3dbf8-139">Tıklatın **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-139">Click the **OK** button.</span></span>

<span data-ttu-id="3dbf8-140">Ardından, Denetim genişletici kodunu içeren sınıf kitaplığı projesi oluşturmak ihtiyacımız var:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="3dbf8-141">Menü seçeneğini **dosya, Ekle, yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="3dbf8-142">Seçin **sınıf kitaplığı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="3dbf8-143">Yeni sınıf kitaplığı adıyla **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="3dbf8-144">Tıklatın **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-144">Click the **OK** button.</span></span>

<span data-ttu-id="3dbf8-145">Bu adımları tamamladıktan sonra Çözüm Gezgini penceresi Şekil 1 gibi görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="3dbf8-146">[![Web sitesi ve sınıf kitaplığı proje Çözümle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="3dbf8-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="3dbf8-147">**Şekil 01**: Web sitesi ve sınıf kitaplığı proje çözümüyle ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="3dbf8-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span></span>


<span data-ttu-id="3dbf8-148">Ardından, tüm gerekli derleme başvurularını sınıf kitaplığı projesine eklemeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="3dbf8-149">CustomExtenders projeye sağ tıklayın ve menü seçeneğini **Başvuru Ekle**.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="3dbf8-150">.NET sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="3dbf8-151">Aşağıdaki derlemelere başvurular ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="3dbf8-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="3dbf8-152">System.Web.dll</span></span>
    2. <span data-ttu-id="3dbf8-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="3dbf8-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="3dbf8-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="3dbf8-154">System.Design.dll</span></span>
    4. <span data-ttu-id="3dbf8-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="3dbf8-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="3dbf8-156">Gözat sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="3dbf8-157">AjaxControlToolkit.dll derlemesine başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="3dbf8-158">Bu derleme AJAX Denetim Araç Seti indirdiğiniz klasörde bulunur.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="3dbf8-159">Tüm sağ başvuruları projenize sağ tıklatıp Özellikler'i seçerek ve başvurular sekmesini tıklatarak eklediğiniz emin olun (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="3dbf8-159">You can verify that you have added all of the right references by right-clicking your project, selecting Properties, and clicking the References tab (see Figure 2).</span></span>


<span data-ttu-id="3dbf8-160">[![Başvurular klasörünü gerekli başvuruları](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="3dbf8-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span></span>

<span data-ttu-id="3dbf8-161">**Şekil 02**: gerekli başvurularıyla başvurular klasörünü ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="3dbf8-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="3dbf8-162">Özel denetim Genişletici Oluşturma</span><span class="sxs-lookup"><span data-stu-id="3dbf8-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="3dbf8-163">Bizim sınıf kitaplığı sahibiz, biz bizim genişletici denetim oluşturmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="3dbf8-164">Bir özel genişletici denetim sınıfın (1 listeleme bakın) ile bare kemikler Başlat s olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="3dbf8-165">**1 - MyCustomExtender.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="3dbf8-165">**Listing 1 - MyCustomExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

<span data-ttu-id="3dbf8-166">Listeleme 1'deki denetim genişletici sınıfı hakkında fark birkaç şey vardır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="3dbf8-167">İlk olarak, taban ExtenderControlBase sınıftan sınıfı dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="3dbf8-168">Tüm AJAX Denetim Araç Seti genişletici denetimleri bu temel sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="3dbf8-169">Örneğin, temel sınıfın her denetim genişletici gerekli bir özelliktir TargetID özelliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="3dbf8-170">Ardından, sınıf istemci komut dosyası için ilgili aşağıdaki iki öznitelikleri içerdiğine dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="3dbf8-171">Web kaynağı - derlemedeki katıştırılmış bir kaynağı olarak eklenmek üzere bir dosya neden olur.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="3dbf8-172">ClientScriptResource - bir derlemeye ait alınması bir komut dosyası kaynağı neden olur.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="3dbf8-173">Web kaynağı öznitelik özel genişletici derlendiğinde MyControlBehavior.js JavaScript dosyası derlemeye eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="3dbf8-174">ClientScriptResource öznitelik, bir web sayfasında özel genişletici kullanıldığında MyControlBehavior.js komut dosyası derlemeden almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="3dbf8-175">Çalışmak Web kaynağı ve ClientScriptResource öznitelikleri sırayla katıştırılmış bir kaynağı JavaScript dosyası derleme gerekir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="3dbf8-176">Çözüm Gezgini penceresinde dosyayı seçin, özellik sayfasını açın ve değer atamak *katıştırılmış kaynak* için **yapı eylemi** özelliği.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="3dbf8-177">Denetim genişletici TargetControlType özniteliği içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="3dbf8-178">Bu öznitelik tarafından denetim genişletici genişletilmiş denetim türünü belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="3dbf8-179">Listeleme 1 söz konusu olduğunda, Denetim genişletici TextBox genişletmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="3dbf8-180">Son olarak, özel genişletici MyProperty adlı bir özellik içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="3dbf8-181">Özellik ExtenderControlProperty özniteliği ile işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="3dbf8-182">GetPropertyValue() ve SetPropertyValue() yöntemleri, istemci-tarafı davranışı sunucu taraflı denetim genişletici özellik değeri geçirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="3dbf8-183">Bir tane bizim DisabledButton genişletici kodunu uygulamak s olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="3dbf8-184">Bu genişletici kodunu listeleme 2'de bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="3dbf8-185">**2 - DisabledButtonExtender.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="3dbf8-185">**Listing 2 - DisabledButtonExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

<span data-ttu-id="3dbf8-186">2. listeleme DisabledButton genişletici TargetButtonID ve DisabledText adlı iki özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="3dbf8-187">TargetButtonID özelliğine uygulanan IDReferenceProperty düğme denetiminin Kimliğini dışında her şey bu özelliğe atanmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="3dbf8-188">Web kaynağı ve ClientScriptResource öznitelikleri bu extender ile DisabledButtonBehavior.js adlı bir dosyada bulunan bir istemci-tarafı davranışı ilişkilendirin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="3dbf8-189">Bu JavaScript dosyası sonraki bölümde aşağıdakiler ele.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="3dbf8-190">Özel genişletici davranışı oluşturma</span><span class="sxs-lookup"><span data-stu-id="3dbf8-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="3dbf8-191">Bir denetim genişletici istemci-tarafı bileşeninin bir davranış adı verilir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="3dbf8-192">Devre dışı bırakma ve etkinleştirme düğmesi fiili mantığı DisabledButton davranışı yer alır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="3dbf8-193">JavaScript kodu davranışı için listeleme 3'te dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="3dbf8-194">**3 - DisabledButton.js listeleme**</span><span class="sxs-lookup"><span data-stu-id="3dbf8-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

<span data-ttu-id="3dbf8-195">JavaScript dosyası listeleme 3'te DisabledButtonBehavior adlı bir istemci-tarafı sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="3dbf8-196">Bu sınıfı, sunucu tarafı twin gibi TargetButtonID adlı iki özellik içerir ve kullanarak erişebileceğiniz DisabledText almak\_TargetButtonID/set\_TargetButtonID ve\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="3dbf8-197">Önce Initialize() yöntemini keyup olay işleyicisi davranışı için hedef öğe ile ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="3dbf8-198">Bu davranışı ile ilişkili metin bir harf yazdığınız her zaman keyup işleyici yürütür.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="3dbf8-199">Keyup işleyici etkinleştirir veya davranışı ile ilişkili metin kutusu herhangi bir metin içerip içermediğini bağlı olarak düğmesini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="3dbf8-200">Katıştırılmış bir kaynağı olarak listeleniyor 3'te JavaScript dosyası derleme unutmayın.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="3dbf8-201">Çözüm Gezgini penceresinde dosyayı seçin, özellik sayfasını açın ve değer atamak *katıştırılmış kaynak* için **yapı eylemi** özelliği (bkz: Şekil 3).</span><span class="sxs-lookup"><span data-stu-id="3dbf8-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="3dbf8-202">Bu seçenek, Visual Studio ve Visual Web Developer içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="3dbf8-203">[![Katıştırılmış bir kaynağı bir JavaScript dosyası ekleme](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="3dbf8-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span></span>

<span data-ttu-id="3dbf8-204">**Şekil 03**: katıştırılmış bir kaynağı bir JavaScript dosyası ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="3dbf8-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="3dbf8-205">Özel genişletici Tasarımcısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="3dbf8-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="3dbf8-206">Bizim genişletici tamamlamak için oluşturmanız gereken bir son sınıfı yok.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="3dbf8-207">Tasarımcı sınıfını listeleme 4'te oluşturmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="3dbf8-208">Bu sınıf, Visual Studio/Visual Web Developer tasarımcı ile birlikte düzgün çalışmasını genişletici yapmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="3dbf8-209">**4 - DisabledButtonDesigner.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="3dbf8-209">**Listing 4 - DisabledButtonDesigner.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

<span data-ttu-id="3dbf8-210">Listeleme 4 tasarımcıda Tasarımcısı özniteliğiyle DisabledButton genişletici ile ilişkilendirin. Bu gibi DisabledButtonExtender sınıfına Tasarımcısı özniteliği uygulamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="3dbf8-211">Özel genişletici kullanma</span><span class="sxs-lookup"><span data-stu-id="3dbf8-211">Using the Custom Extender</span></span>

<span data-ttu-id="3dbf8-212">Biz DisabledButton denetim genişletici oluşturmayı tamamladıktan, bizim ASP.NET Web sitesi kullanmak için zaman yapılır.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="3dbf8-213">İlk olarak, biz araç kutusuna özel genişletici eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="3dbf8-214">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-214">Follow these steps:</span></span>

1. <span data-ttu-id="3dbf8-215">Bir ASP.NET sayfasının Solution Explorer penceresi sayfasında çift tıklatarak açın.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="3dbf8-216">Araç kutusunu sağ tıklatın ve menü seçeneğini **öğeleri Seç**.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="3dbf8-217">Araç kutusu öğelerini Seç iletişim kutusunda, CustomExtenders.dll derlemeye göz atın.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="3dbf8-218">Tıklatın **Tamam** düğmesi iletişim kutusunu kapatın.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="3dbf8-219">Bu adımları tamamladıktan sonra DisabledButton denetim genişletici araç kutusunda görüntülenmesi gerekir (bkz: Şekil 4).</span><span class="sxs-lookup"><span data-stu-id="3dbf8-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="3dbf8-220">[![Araç çubuğundaki DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="3dbf8-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span></span>

<span data-ttu-id="3dbf8-221">**Şekil 04**: araç çubuğundaki DisabledButton ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="3dbf8-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span></span>


<span data-ttu-id="3dbf8-222">Ardından, size yeni bir ASP.NET sayfası oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="3dbf8-223">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-223">Follow these steps:</span></span>

1. <span data-ttu-id="3dbf8-224">ShowDisabledButton.aspx adlı yeni bir ASP.NET sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="3dbf8-225">Bir ScriptManager sayfaya sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="3dbf8-226">TextBox denetimi sayfaya sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="3dbf8-227">Düğme denetimi sayfaya sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="3dbf8-228">Özellikler penceresinde düğmesi ID özelliği değere değiştirin *btnSave* ve değeri metin özelliğini *kaydetmek\**.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-228">In the Properties window, change the Button ID property to the value *btnSave* and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="3dbf8-229">Standart bir düğmeyi ve ASP.NET TextBox denetimi ile bir sayfa oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="3dbf8-230">Ardından, TextBox denetimi DisabledButton genişletici ile genişletmek ihtiyacımız var:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="3dbf8-231">Seçin **eklemek genişletici** görev genişletici Sihirbazı iletişim kutusunu açmak için seçeneği (bkz. Şekil 5).</span><span class="sxs-lookup"><span data-stu-id="3dbf8-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="3dbf8-232">İletişim kutusu özel bizim DisabledButton genişletici içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="3dbf8-233">DisabledButton genişletici tıklatıp **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="3dbf8-234">[![Genişletici Sihirbazı iletişim kutusu](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="3dbf8-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span></span>

<span data-ttu-id="3dbf8-235">**Şekil 05**: genişletici sihirbaz iletişim ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="3dbf8-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span></span>


<span data-ttu-id="3dbf8-236">Son olarak, biz DisabledButton genişletici özellikleri ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="3dbf8-237">TextBox denetimi özelliklerini değiştirerek DisabledButton genişletici özelliklerini değiştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="3dbf8-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="3dbf8-238">TextBox Tasarımcısı'nda seçin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="3dbf8-239">Özellikleri penceresinde Extender'larının düğümünü genişletin (bkz. Şekil 6).</span><span class="sxs-lookup"><span data-stu-id="3dbf8-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="3dbf8-240">Değer atamak *kaydetmek* DisabledText özelliği ve değerini *btnSave* TargetButtonID özelliğine.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="3dbf8-241">[![Genişletici özelliklerini ayarlama](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="3dbf8-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span></span>

<span data-ttu-id="3dbf8-242">**Şekil 06**: genişletici özelliklerini ayarlama ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="3dbf8-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span></span>


<span data-ttu-id="3dbf8-243">Sayfa (F5 tuşuna tarafından) çalıştırdığınızda, düğme denetim başlangıçta devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="3dbf8-244">Metin kutusuna bir metin girerek başlar başlamaz denetimi düğmesi (bkz. Şekil 7) etkin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="3dbf8-245">[![Eylem DisabledButton genişletici](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="3dbf8-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span></span>

<span data-ttu-id="3dbf8-246">**Şekil 07**: DisabledButton genişletici eylem ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="3dbf8-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="3dbf8-247">Özet</span><span class="sxs-lookup"><span data-stu-id="3dbf8-247">Summary</span></span>

<span data-ttu-id="3dbf8-248">AJAX Denetim Araç Seti özel genişletici denetimleri ile nasıl genişletebileceğini açıklamak için bu öğreticinin amacı oluştu.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="3dbf8-249">Bu öğreticide, bir basit DisabledButton denetim genişletici oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="3dbf8-250">Biz bu genişletici DisabledButtonExtender sınıfı, bir DisabledButtonBehavior JavaScript davranışını ve DisabledButtonDesigner sınıfı oluşturarak uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="3dbf8-251">Her bir özel denetim extender oluşturduğunuzda benzer birtakım adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="3dbf8-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="3dbf8-252">Önceki</span><span class="sxs-lookup"><span data-stu-id="3dbf8-252">Previous</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
