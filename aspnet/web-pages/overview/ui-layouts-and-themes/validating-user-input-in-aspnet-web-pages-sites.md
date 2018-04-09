---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: ASP.NET Web uygulamasında kullanıcı girdisi doğrulama sayfaları (Razor) siteleri | Microsoft Docs
author: tfitzmac
description: Bu makalede kullanıcılardan alma bilgileri doğrulamak nasıl ele &mdash; diğer bir deyişle, geçerli kullanıcıların girmesini emin olmak için bir AS bilgi HTML formları...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="5a9f3-103">ASP.NET Web sayfaları (Razor) sitelerdeki kullanıcı girişini doğrulama</span><span class="sxs-lookup"><span data-stu-id="5a9f3-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="5a9f3-104">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5a9f3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5a9f3-105">Bu makalede kullanıcılardan alma bilgileri doğrulamak nasıl ele &mdash; diğer bir deyişle, geçerli kullanıcıların girmesini emin olmak için bir ASP.NET Web sayfaları (Razor) sitede HTML bilgileri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="5a9f3-106">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="5a9f3-107">Bir kullanıcı girişi denetlemek nasıl tanımladığınız doğrulama ölçütleriyle eşleşiyor.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="5a9f3-108">Tüm doğrulama testlerini geçtiğini belirlemek nasıl.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="5a9f3-109">Doğrulama hataları görüntülemek nasıl (ve bunları biçimlendirme).</span><span class="sxs-lookup"><span data-stu-id="5a9f3-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="5a9f3-110">Doğrudan kullanıcılardan gelmeyen veri doğrulama yapma.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="5a9f3-111">Programlama Kavramları makalesinde sunulan ASP.NET şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="5a9f3-112">`Validation` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="5a9f3-113">`Html.ValidationSummary` Ve `Html.ValidationMessage` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5a9f3-114">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="5a9f3-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5a9f3-115">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5a9f3-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5a9f3-116">Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="5a9f3-117">Bu makalede aşağıdaki bölümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="5a9f3-118">Kullanıcı girdisi doğrulama genel bakış</span><span class="sxs-lookup"><span data-stu-id="5a9f3-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="5a9f3-119">Kullanıcı girişini doğrulama</span><span class="sxs-lookup"><span data-stu-id="5a9f3-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="5a9f3-120">İstemci tarafı doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="5a9f3-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="5a9f3-121">Doğrulama hataları biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="5a9f3-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="5a9f3-122">Doğrudan kullanıcılardan olmadıktan verileri doğrulama</span><span class="sxs-lookup"><span data-stu-id="5a9f3-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="5a9f3-123">Kullanıcı girdisi doğrulama genel bakış</span><span class="sxs-lookup"><span data-stu-id="5a9f3-123">Overview of User Input Validation</span></span>

<span data-ttu-id="5a9f3-124">Bir sayfa bilgilerini girmesini ister varsa — örneğin, bir forma — girmeleri değerlerinin geçerli olduğundan emin olmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="5a9f3-125">Örneğin, kritik bilgiler eksik bir formu işlemek istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="5a9f3-126">Kullanıcılar bir HTML formuna değerler girdiğinde girmeleri değerleri dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="5a9f3-127">Çoğu durumda, gereken bazı diğer veri türleri, tamsayı veya tarih gibi değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="5a9f3-128">Bu nedenle, ayrıca kullanıcıların giriş değerleri için uygun veri türleri doğru dönüştürülebilir emin olmak vardır.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="5a9f3-129">Ayrıca bazı kısıtlamalar değerlerine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="5a9f3-130">Örneğin, kullanıcıların doğru bir tamsayı girin olsa bile değeri belirli bir aralığa sınırları emin olmak gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![CSS stil sınıflarını kullanmak doğrulama hataları](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="5a9f3-132">**Önemli** kullanıcı girişini doğrulama ayrıca güvenlik için önemli.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="5a9f3-133">Kullanıcıların formlarında girebileceği değerleri kısıtladığınızda birisi, sitenizin güvenliğini tehlikeye atabilir bir değer girebilirsiniz olasılığını azaltır.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="5a9f3-134">Kullanıcı girişini doğrulama</span><span class="sxs-lookup"><span data-stu-id="5a9f3-134">Validating User Input</span></span>

<span data-ttu-id="5a9f3-135">ASP.NET Web Pages 2'de, kullandığınız `Validator` Yardımcısı kullanıcı girişini test etmek için.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="5a9f3-136">Aşağıdakileri yapmak için temel yaklaşım şöyledir:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="5a9f3-137">Hangi doğrulamak istediğiniz öğeleri (alanları) giriş belirler.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="5a9f3-138">Genellikle değerleri doğrulamak `<input>` öğeleri bir biçimde.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="5a9f3-139">Ancak, bu gibi kısıtlı bir öğeden gelen tüm giriş doğrulamak için hatta giriş iyi bir uygulamadır bir `<select>` listesi.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="5a9f3-140">Bu, kullanıcıların bir sayfadaki denetimlerin atlama yoksa ve bir form gönderme emin olmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="5a9f3-141">Yöntemlerini kullanarak her öğe girişi için sayfa kodunda, tek tek doğrulama denetimlerini ekleme `Validation` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="5a9f3-142">Gerekli alanlar için denetlemek için kullanın `Validation.RequireField(field, [error message])` (için tek bir alanı) veya `Validation.RequireFields(field1, field2, ...))` (için bir alanlar listesi).</span><span class="sxs-lookup"><span data-stu-id="5a9f3-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="5a9f3-143">Doğrulama diğer türleri için kullanmak `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="5a9f3-144">İçin `ValidationType`, bu seçenekleri kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="5a9f3-145">Sayfa gönderildiğinde, doğrulama denetleyerek başarılı olup olmadığını denetleyin `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="5a9f3-146">Herhangi bir doğrulama hatası varsa, normal sayfa işleme atlayın.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="5a9f3-147">Sayfa amacı bir veritabanını güncelleştirmek için ise, tüm doğrulama hataları sabit kadar Örneğin, bunu yok.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="5a9f3-148">Doğrulama hatası varsa, hata iletileri sayfanın biçimlendirmede kullanarak görüntüleme `Html.ValidationSummary` veya `Html.ValidationMessage`, veya her ikisini de.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="5a9f3-149">Aşağıdaki örnek, şu adımları gösterilmektedir bir sayfa gösterir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="5a9f3-150">Doğrulama nasıl çalıştığını görmek için bu sayfayı çalıştırın ve kasıtlı olarak hataları olun.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="5a9f3-151">Örneğin, işte sayfa göründüğünü girerseniz gibi indirmelere adı girmek unutursanız ve geçersiz bir tarih girin:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![İşlenen sayfanın doğrulama hataları](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="5a9f3-153">İstemci tarafı doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="5a9f3-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="5a9f3-154">Kullanıcıların sayfayı gönderdikten sonra varsayılan olarak, kullanıcı girişi doğrulanır — diğer bir deyişle, doğrulama sunucu kodunda da yapılır.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="5a9f3-155">Bu yaklaşımın bir dezavantajı, kullanıcıların gönderme sayfası hata kadar bunlar sonra yapmış olduğunuz bilmiyorsanız ' dir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="5a9f3-156">Bir form uzun veya karmaşık ise, yalnızca sayfa gönderildikten sonra hatalarını raporlama kullanıcıya bilinmelidir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="5a9f3-157">İstemci komut dosyası doğrulama gerçekleştirmek için destek ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="5a9f3-158">Bu durumda, kullanıcılar tarayıcı içinde çalışırken doğrulama gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="5a9f3-159">Örneğin, bir değeri bir tamsayı olmalıdır belirttiğiniz varsayalım.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="5a9f3-160">Bir kullanıcı bir tamsayı olmayan değerler girerse, kullanıcı girişi alanı terk hemen hata bildirilir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="5a9f3-161">Kullanıcılar için uygun olduğunu anında geri bildirim alır.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="5a9f3-162">İstemci tabanlı doğrulama da birden çok hataları düzeltmek için formu göndermek için kullanıcının sahip sayısını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="5a9f3-163">İstemci tarafı doğrulama kullansanız bile, doğrulama her zaman aynı zamanda sunucu kodunda da yapılır.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="5a9f3-164">Kullanıcılar istemci tabanlı Doğrulamayı atla durumunda sunucu kodunda doğrulama gerçekleştirmeden güvenlik önlemi olur.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="5a9f3-165">Şu JavaScript kitaplıklarını sayfasında kaydedin:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="5a9f3-166">Mutlaka bilgisayara veya sunucuya sahip zorunda kalmamak için iki kitaplıkların bir içerik teslim ağı (CDN) yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="5a9f3-167">Bununla birlikte, yerel bir kopyasını olmalıdır *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="5a9f3-168">Zaten bir WebMatrix şablonu ile çalışıyorsanız değil (gibi **başlangıç sitesi** ) kitaplığı içeren, temel alan bir Web sayfaları site oluşturma **başlangıç sitesi**.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="5a9f3-169">Ardından kopyalama *.js* geçerli sitenize dosya.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="5a9f3-170">Biçimlendirmede doğrulama her öğe için bir çağrı ekleyin `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="5a9f3-171">Bu yöntem, istemci tarafı doğrulama tarafından kullanılan öznitelikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="5a9f3-172">(Gerçek JavaScript kod yayma yerine yöntemi gibi özniteliklere yayar `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="5a9f3-173">Bu öznitelikler yapması için jQuery kullanan örtük istemci doğrulama destekler.)</span><span class="sxs-lookup"><span data-stu-id="5a9f3-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="5a9f3-174">Aşağıdaki sayfayı istemci doğrulama özelliklere daha önce gösterilen örnek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="5a9f3-175">İstemci üzerinde çalışan tüm doğrulama olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="5a9f3-176">Özellikle, veri türü doğrulama (tamsayı, tarih vb.) istemci üzerinde çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="5a9f3-177">Aşağıdaki denetimleri istemci ve sunucu üzerinde çalışır:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="5a9f3-178">Bu örnekte, test için geçerli bir tarih istemci kodu çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="5a9f3-179">Ancak, test sunucu kodunda gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="5a9f3-180">Doğrulama hataları biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="5a9f3-180">Formatting Validation Errors</span></span>

<span data-ttu-id="5a9f3-181">Aşağıdaki ayrılmış adları olan CSS sınıfı tanımlayarak doğrulama hataları nasıl görüntüleneceğini denetleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="5a9f3-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-182">`field-validation-error`.</span></span> <span data-ttu-id="5a9f3-183">Çıktısını tanımlar `Html.ValidationMessage` hata görüntülenirken yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="5a9f3-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-184">`field-validation-valid`.</span></span> <span data-ttu-id="5a9f3-185">Çıktısını tanımlar `Html.ValidationMessage` herhangi bir hata olduğunda yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="5a9f3-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-186">`input-validation-error`.</span></span> <span data-ttu-id="5a9f3-187">Tanımlar nasıl `<input>` öğeleri hata olduğunda işlenir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="5a9f3-188">(Örneğin, arka plan rengini ayarlamak için bu sınıf kullanabilirsiniz bir &lt;giriş&gt; değeri geçersiz, farklı bir renk öğesine.) Bu CSS sınıfı (ASP.NET Web Pages 2)'deki istemci doğrulama sırasında yalnızca kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="5a9f3-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-189">`input-validation-valid`.</span></span> <span data-ttu-id="5a9f3-190">Görünümünü tanımlayan `<input>` herhangi bir hata olduğunda öğeleri.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="5a9f3-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-191">`validation-summary-errors`.</span></span> <span data-ttu-id="5a9f3-192">Çıktısını tanımlar `Html.ValidationSummary` hataların listesini görüntülemeden yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="5a9f3-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-193">`validation-summary-valid`.</span></span> <span data-ttu-id="5a9f3-194">Çıktısını tanımlar `Html.ValidationSummary` herhangi bir hata olduğunda yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="5a9f3-195">Aşağıdaki `<style>` blok hata koşulları için kuralları gösterir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="5a9f3-196">Makalenin örnek sayfalarından bu stil bloğu dahil ederseniz, hata görünen aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![CSS stil sınıflarını kullanmak doğrulama hataları](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="5a9f3-198">ASP.NET Web Pages 2'de istemci doğrulaması kullanmıyorsanız için CSS sınıfları `<input>` öğeleri (`input-validation-error` ve `input-validation-valid` herhangi bir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="5a9f3-199">Statik ve dinamik hata görüntüleme</span><span class="sxs-lookup"><span data-stu-id="5a9f3-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="5a9f3-200">CSS kuralları gibi çiftler halinde gelen `validation-summary-errors` ve `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="5a9f3-201">Bu çiftlerine, her iki koşul için kuralları tanımlamanıza olanak sağlar: bir hata koşulu ve "normal" (hata olmayan) koşulu.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="5a9f3-202">Hiçbir hata olsa bile biçimlendirme hata görüntülemek için her zaman işlenir anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="5a9f3-203">Örneğin, bir sayfa varsa bir `Html.ValidationSummary` işaretleme yönteminde, sayfa kaynağı içerecek aşağıdaki biçimlendirme bile sayfa ilk kez istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="5a9f3-204">Diğer bir deyişle, `Html.ValidationSummary` yöntemi her zaman işler bir `<div>` öğesi ve hata listesinin boş olsa bile bir listesi.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="5a9f3-205">Benzer şekilde, `Html.ValidationMessage` yöntemi her zaman işler bir `<span>` öğesi hiçbir hata olsa bile bir tek tek alan hatası için bir yer tutucu olarak.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="5a9f3-206">Bazı durumlarda, hata iletisi görüntüleyen sayfanın yeniden akışı neden olabilir ve öğeleri hareket etmek için sayfasında neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="5a9f3-207">İçinde sona CSS kuralları `-valid` , bu sorunu önlemek bir düzen tanımlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="5a9f3-208">Örneğin, tanımlayabilirsiniz `field-validation-error` ve `field-validation-valid` her ikisi de aynı boyutu düzeltildi.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="5a9f3-209">Böylece, alan görüntü alanını statiktir ve bir hata iletisi görüntülenirse sayfa akışı değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="5a9f3-210">Doğrudan kullanıcılardan olmadıktan verileri doğrulama</span><span class="sxs-lookup"><span data-stu-id="5a9f3-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="5a9f3-211">Bazen doğrudan bir HTML form olmadıktan bilgileri doğrulamak sahip.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="5a9f3-212">Burada bir değer aşağıdaki örnekteki gibi bir sorgu dizesi geçirilen bir sayfa buna tipik bir örnektir:</span><span class="sxs-lookup"><span data-stu-id="5a9f3-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="5a9f3-213">Bu durumda, emin olmak istediğiniz sayfaya geçirilen değer (burada, 1022 değerini `classid`) geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="5a9f3-214">Doğrudan kullanamazsınız `Validation` bu doğrulamayı gerçekleştirmek için yardımcı.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="5a9f3-215">Ancak, diğer hata iletilerini görüntüleme yeteneği gibi doğrulama sistemi özelliklerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5a9f3-216">**Önemli** her zaman aldığınız değerleri doğrulaması *herhangi* form alanı değerleri, sorgu dizesi değerleri ve tanımlama bilgisi değerleri dahil olmak üzere kaynak.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="5a9f3-217">(Belki de kötü amaçlı olarak) bu değerleri değiştirmek üzere kişiler için kolay bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="5a9f3-218">Bu nedenle, uygulamanızın korumak için bu değerleri işaretlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="5a9f3-219">Aşağıdaki örnek, nasıl bir sorgu dizesinde geçirilen bir değeri doğrulamak gösterir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="5a9f3-220">Kodu test değeri boş olmayan ve bir tamsayı değil.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="5a9f3-221">İstek bir form gönderme olmadığında test gerçekleştirilir dikkat edin (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="5a9f3-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="5a9f3-222">Bu test sayfası istenen ilk kez geçip geçmeyeceğini, ancak ne zaman isteği form gönderme değil.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="5a9f3-223">Bu hatayı görüntülemek için hata doğrulama hataları listesine çağırarak ekleyebileceğiniz `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="5a9f3-224">Sayfa için bir çağrı içeriyorsa `Html.ValidationSummary` yöntemi, hata, yalnızca bir kullanıcı girişini doğrulama hatası gibi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5a9f3-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="5a9f3-225">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5a9f3-225">Additional Resources</span></span>

[<span data-ttu-id="5a9f3-226">ASP.NET Web Pages sitelerinde HTML formları ile çalışma</span><span class="sxs-lookup"><span data-stu-id="5a9f3-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
