---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Özel HTML Yardımcıları (VB) oluşturma | Microsoft Docs
author: microsoft
description: Bu öğreticide, MVC görünümlerinizde içinde kullanabileceğiniz özel HTML Yardımcıları nasıl oluşturacağınızı göstermek için hedefidir. HTML Yardımcısı yararlanarak...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: ed3f86d3f316d3e1630c6008c20c72c31d7864da
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821533"
---
<a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="75aba-104">Özel HTML Yardımcıları (VB) oluşturma</span><span class="sxs-lookup"><span data-stu-id="75aba-104">Creating Custom HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="75aba-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="75aba-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="75aba-106">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="75aba-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="75aba-107">Bu öğreticide, MVC görünümlerinizde içinde kullanabileceğiniz özel HTML Yardımcıları nasıl oluşturacağınızı göstermek için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="75aba-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="75aba-108">HTML Yardımcıları avantajlarından yararlanarak, standart bir HTML sayfası oluşturmak için gerçekleştirmeniz gereken HTML etiketleri tedious yazarak miktarını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75aba-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="75aba-109">Bu öğreticide, MVC görünümlerinizde içinde kullanabileceğiniz özel HTML Yardımcıları nasıl oluşturacağınızı göstermek için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="75aba-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="75aba-110">HTML Yardımcıları avantajlarından yararlanarak, standart bir HTML sayfası oluşturmak için gerçekleştirmeniz gereken HTML etiketleri tedious yazarak miktarını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75aba-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="75aba-111">Bu öğreticinin ilk bölümünde miyim mevcut HTML Yardımcıları ile ASP.NET MVC çerçevesi dahil bazılarını açıklar.</span><span class="sxs-lookup"><span data-stu-id="75aba-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="75aba-112">Ardından, ben özel HTML Yardımcıları oluşturmak için iki yöntem açıklar: Ben paylaşılan bir yöntem oluşturarak ve bir genişletme yöntemi oluşturarak özel HTML Yardımcıları oluşturma işlemleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="75aba-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="75aba-113">HTML yardımcılarını anlama</span><span class="sxs-lookup"><span data-stu-id="75aba-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="75aba-114">Bir HTML Yardımcısı yalnızca bir dize döndüren bir yöntem var.</span><span class="sxs-lookup"><span data-stu-id="75aba-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="75aba-115">Dize herhangi bir türde istediğiniz içerik temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="75aba-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="75aba-116">Örneğin, HTML gibi standart HTML etiketlerini işlemek için bir HTML Yardımcıları kullanabilirsiniz `<input>` ve `<img>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="75aba-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="75aba-117">HTML Yardımcıları, sekme şeridi veya veritabanı verilerinin bir HTML tablosu gibi daha karmaşık içeriğini işlemek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75aba-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="75aba-118">ASP.NET MVC çerçevesi aşağıdaki standart HTML Yardımcıları (Bu tam bir listesi değildir) içermektedir:</span><span class="sxs-lookup"><span data-stu-id="75aba-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="75aba-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="75aba-119">Html.ActionLink()</span></span>
- <span data-ttu-id="75aba-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="75aba-120">Html.BeginForm()</span></span>
- <span data-ttu-id="75aba-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="75aba-121">Html.CheckBox()</span></span>
- <span data-ttu-id="75aba-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="75aba-122">Html.DropDownList()</span></span>
- <span data-ttu-id="75aba-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="75aba-123">Html.EndForm()</span></span>
- <span data-ttu-id="75aba-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="75aba-124">Html.Hidden()</span></span>
- <span data-ttu-id="75aba-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="75aba-125">Html.ListBox()</span></span>
- <span data-ttu-id="75aba-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="75aba-126">Html.Password()</span></span>
- <span data-ttu-id="75aba-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="75aba-127">Html.RadioButton()</span></span>
- <span data-ttu-id="75aba-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="75aba-128">Html.TextArea()</span></span>
- <span data-ttu-id="75aba-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="75aba-129">Html.TextBox()</span></span>

<span data-ttu-id="75aba-130">Örneğin, 1 listeleme biçiminde göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="75aba-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="75aba-131">Bu formu Yardımı iki standart HTML Yardımcıları (bkz. Şekil 1) ile işlenir.</span><span class="sxs-lookup"><span data-stu-id="75aba-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="75aba-132">Bu form kullanır `Html.BeginForm()` ve `Html.TextBox()` yardımcı yöntemler.</span><span class="sxs-lookup"><span data-stu-id="75aba-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>


<span data-ttu-id="75aba-133">[![Sayfanın HTML Yardımcıları ile çizilir.](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="75aba-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="75aba-134">**Şekil 01**: sayfa İşlenmiş HTML Yardımcıları ile ([tam boyutlu görüntüyü görmek için tıklatın](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="75aba-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>


<span data-ttu-id="75aba-135">**Kod 1 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="75aba-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="75aba-136">`Html.BeginForm()` Açılış ve kapanış HTML oluşturmak için kullanılan yardımcı yöntem `<form>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="75aba-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="75aba-137">Dikkat `Html.BeginForm()` kullanarak içinde yöntemi çağrıldığında deyimi.</span><span class="sxs-lookup"><span data-stu-id="75aba-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="75aba-138">Using deyimi, sağlar, `<form>` etiketini kullanarak sonunda kapalı blok.</span><span class="sxs-lookup"><span data-stu-id="75aba-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="75aba-139">Bir kullanarak oluşturmak yerine tercih ederseniz, blok kapatmak için Html.EndForm() yardımcı yöntemini çağırabilirsiniz `<form>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="75aba-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="75aba-140">Oluşturma bir açılış ve kapanış yaklaşımı kullanmak `<form>` size en kolay görünen etiket.</span><span class="sxs-lookup"><span data-stu-id="75aba-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="75aba-141">`Html.TextBox()` Yardımcı yöntemler listeleme 1'de HTML oluşturmak için kullanılan `<input>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="75aba-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="75aba-142">Kaynağı Görüntüle tarayıcınıza seçerseniz, HTML kaynağını listeleme 2'de görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="75aba-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="75aba-143">Kaynak standart HTML etiketleri içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="75aba-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75aba-144">dikkat `Html.TextBox()`-HTML Yardımcısı ile işlenir `<%= %>` yerine etiketleri `<% %>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="75aba-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="75aba-145">Ardından, eşittir işareti eklemezseniz, hiçbir şey tarayıcıda görüntülenen.</span><span class="sxs-lookup"><span data-stu-id="75aba-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="75aba-146">ASP.NET MVC çerçevesi Yardımcıları küçük bir kümesini içerir.</span><span class="sxs-lookup"><span data-stu-id="75aba-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="75aba-147">Büyük olasılıkla özel HTML Yardımcıları ile MVC çerçevesi genişletmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="75aba-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="75aba-148">Bu öğreticinin geri kalanında içinde özel HTML Yardımcıları oluşturmak için iki yöntem öğrenin.</span><span class="sxs-lookup"><span data-stu-id="75aba-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="75aba-149">**Kod 2 – `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="75aba-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="75aba-150">Paylaşılan yöntemleriyle HTML Yardımcıları oluşturma</span><span class="sxs-lookup"><span data-stu-id="75aba-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="75aba-151">Yeni bir HTML Yardımcısı oluşturmanın en kolay yolu, bir dize döndürür paylaşılan bir yöntemine oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="75aba-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="75aba-152">Örneğin, bir HTML işleyen yeni bir HTML Yardımcısı oluşturmaya karar verdiğinizi düşünelim `<label>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="75aba-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="75aba-153">Sınıfı listeleme 2'de işlemek için kullanabileceğiniz bir `<label>`.</span><span class="sxs-lookup"><span data-stu-id="75aba-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="75aba-154">**Kod 2 – `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="75aba-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="75aba-155">Özel sınıf 2 listeleme hakkında bir şey yoktur.</span><span class="sxs-lookup"><span data-stu-id="75aba-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="75aba-156">`Label()` Yöntemi yalnızca bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="75aba-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="75aba-157">Listeleme 3'te değiştirilmiş dizin görünümünün kullanan `LabelHelper` HTML oluşturmak için `<label>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="75aba-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="75aba-158">Görünüm içeren bildirimi bir `<%@ imports %>` Application1.Helpers ad alanını içeri aktarır yönergesi.</span><span class="sxs-lookup"><span data-stu-id="75aba-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="75aba-159">**Kod 2 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="75aba-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="75aba-160">Genişletme yöntemleri ile HTML Yardımcıları oluşturma</span><span class="sxs-lookup"><span data-stu-id="75aba-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="75aba-161">Çalışmaya HTML Yardımcıları oluşturmak istiyorsanız, genişletme yöntemleri oluşturmanız gerekir ASP.NET MVC çerçevesi dahil standart HTML yardımcıları gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="75aba-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="75aba-162">Genişletme yöntemleri varolan bir sınıf için yeni yöntemler eklemenize imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="75aba-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="75aba-163">Bir HTML yardımcı yöntemi oluştururken, yeni yöntemler ekleyin `HtmlHelper` bir görünümün Html özelliği tarafından temsil edilen sınıf.</span><span class="sxs-lookup"><span data-stu-id="75aba-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="75aba-164">Visual Basic module'u listeleme 3'te adlı bir genişletme yöntemi ekler `Label()` için `HtmlHelper` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="75aba-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="75aba-165">Bu modül hakkında fark etmişsinizdir şey birkaç vardır.</span><span class="sxs-lookup"><span data-stu-id="75aba-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="75aba-166">İlk olarak, modül ile donatılmış fark `<Extension()>` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="75aba-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="75aba-167">Bu öznitelik kullanmak için aktarmanız gerekir `System.Runtime.CompilerServices` ad alanı</span><span class="sxs-lookup"><span data-stu-id="75aba-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="75aba-168">İkinci olarak, dikkat ilk parametresi `Label()` yöntemi temsil `HtmlHelper` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="75aba-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="75aba-169">Genişletme yönteminin ilk parametresi genişletme yönteminin genişlettiği sınıfın gösterir.</span><span class="sxs-lookup"><span data-stu-id="75aba-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="75aba-170">**Kod 3 – `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="75aba-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="75aba-171">Bir genişletme yöntemi oluşturma ve uygulamanızı başarıyla oluşturmak sonra Visual Studio IntelliSense gibi tüm diğer yöntemleri bir sınıfın içinde genişletme yöntemi görünür (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="75aba-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="75aba-172">Tek fark, bu uzantı yöntemleri özel bir simge (aşağı ok simgesi) yanında görünür.</span><span class="sxs-lookup"><span data-stu-id="75aba-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="75aba-173">[![Html.Label() genişletme yöntemini kullanma](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="75aba-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="75aba-174">**Şekil 02**: Html.Label() genişletme yöntemi kullanarak ([tam boyutlu görüntüyü görmek için tıklatın](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="75aba-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>


<span data-ttu-id="75aba-175">Listeleme 4'te değiştirilmiş dizin görünümü, tüm işlemek için Html.Label() genişletme yöntemini kullanır. kendi &lt;etiket&gt; etiketler.</span><span class="sxs-lookup"><span data-stu-id="75aba-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="75aba-176">**4 listeleme – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="75aba-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="75aba-177">Özet</span><span class="sxs-lookup"><span data-stu-id="75aba-177">Summary</span></span>

<span data-ttu-id="75aba-178">Bu öğreticide, özel HTML Yardımcıları oluşturmak için iki yöntem öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="75aba-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="75aba-179">İlk olarak, özel bir oluşturma işleminin nasıl yapılacağını öğrendiniz `Label()` HTML Yardımcısını, paylaşılan bir yöntem oluşturarak, bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="75aba-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="75aba-180">Ardından, özel bir oluşturma işleminin nasıl yapılacağını öğrendiniz `Label()` HTML yardımcı yöntem üzerinde bir genişletme yöntemi oluşturarak `HtmlHelper` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="75aba-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="75aba-181">Bu öğreticide, son derece basit bir HTML yardımcı yöntem geliştirmeye odaklanır.</span><span class="sxs-lookup"><span data-stu-id="75aba-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="75aba-182">Bir HTML Yardımcısı istediğiniz kadar karmaşık olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="75aba-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="75aba-183">Ağaç görünümleri, menüler ya da veritabanı verilerinin tablolar gibi zengin içerik oluşturan bir HTML Yardımcıları oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75aba-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="75aba-184">[Önceki](asp-net-mvc-views-overview-vb.md)
> [İleri](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="75aba-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
