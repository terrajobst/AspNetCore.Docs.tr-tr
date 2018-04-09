---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 sayfa modeli | Microsoft Docs
author: microsoft
description: ASP.NET 1.x, geliştiricilerin sahip bir satır içi kod modeli ve arka plan kodu kod modeli arasında bir seçim. Arka plan kodu Src attr kullanarak uygulanabilir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="1a0c3-104">ASP.NET 2.0 sayfa modeli</span><span class="sxs-lookup"><span data-stu-id="1a0c3-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="1a0c3-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1a0c3-106">ASP.NET 1.x, geliştiricilerin sahip bir satır içi kod modeli ve arka plan kodu kod modeli arasında bir seçim.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="1a0c3-107">Arka plan kodu Src özniteliği veya arkasındaki koda özniteliği kullanılarak gerçekleştirilen @Page yönergesi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="1a0c3-108">ASP.NET 2. 0'da, geliştiricilerin hala satır içi kod ve arka plan kodu arasında bir seçim var, ancak arka plan kod modeli önemli geliştirmeler olmuştur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="1a0c3-109">ASP.NET 1.x, geliştiricilerin sahip bir satır içi kod modeli ve arka plan kodu kod modeli arasında bir seçim.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="1a0c3-110">Arka plan kodu Src özniteliği veya arkasındaki koda özniteliği kullanılarak gerçekleştirilen @Page yönergesi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="1a0c3-111">ASP.NET 2. 0'da, geliştiricilerin hala satır içi kod ve arka plan kodu arasında bir seçim var, ancak arka plan kod modeli önemli geliştirmeler olmuştur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="1a0c3-112">Arka plan kod modeli yenilikleri</span><span class="sxs-lookup"><span data-stu-id="1a0c3-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="1a0c3-113">Hızlı şekilde modeli gözden geçirmek için en iyi, ASP.NET 2.0 arka plan kodu modelindeki değişikliklere tam olarak anlamak için ASP.NET var 1.x.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="1a0c3-114">ASP.NET arka plan kodu modelinde 1.x</span><span class="sxs-lookup"><span data-stu-id="1a0c3-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="1a0c3-115">ASP.NET 1.x, arka plan kod modeli oluşmuştur ASPX dosyası (Webform) ve programlama kodu içeren bir arka plan kod dosyası.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="1a0c3-116">İki dosya kullanarak bağlanmış @Page ASPX dosyasındaki yönergesi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="1a0c3-117">Her denetim ASPX sayfasında karşılık gelen bir bildirimi arka plan kod dosyasına bir örnek değişkeni vardı.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="1a0c3-118">Arka plan kod dosyası ayrıca olay bağlama kodunu bulunan ve oluşturulan kodda Visual Studio tasarımcısı için gerekli.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="1a0c3-119">Bu model oldukça iyi çalıştı, ancak her ASP.NET öğesi ASPX sayfasında arka plan kod dosyasına karşılık gelen kod gerektirdiğinden, kod ve içerik true hiçbir ayrılması vardı.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="1a0c3-120">Bir tasarımcı yeni bir sunucu denetimi Visual Studio IDE dışında ASPX dosyası eklediyseniz, örneğin, uygulama bu denetim için bir bildirim olmaması nedeniyle arka plan kod dosyasına çalışmamasına neden.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="1a0c3-121">ASP.NET 2.0 arka plan kod modeli</span><span class="sxs-lookup"><span data-stu-id="1a0c3-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="1a0c3-122">ASP.NET 2.0 Bu model önemli ölçüde artırır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="1a0c3-123">ASP.NET 2. 0'da, arka plan kodu kullanarak yeni uygulanan *kısmi sınıflar* ASP.NET 2.0 ile sağlanan.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="1a0c3-124">Arka plandaki kod sınıfı, ASP.NET 2.0 definied sınıf tanımını yalnızca bir parçasının içerdiği anlamına bir kısmi sınıftır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="1a0c3-125">Sınıf tanımı kalan kısmını çalışma zamanında veya Web sitesi önceden derlenmiş ASPX sayfasını kullanarak ASP.NET 2.0 tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="1a0c3-126">Arka plan kod dosyası ve ASPX Sayfası arasındaki bağlantı hala @page yönergesi kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="1a0c3-127">Ancak, bir arkasındaki koda veya Src özniteliği yerine ASP.NET 2.0 CodeFile özniteliği şimdi kullanır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="1a0c3-128">Inherits özniteliği de sayfa için sınıf adını belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="1a0c3-129">Tipik bir @page yönergesi şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="1a0c3-130">Bir ASP.NET 2.0 arka plan kod dosyasına tipik sınıf tanımında şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="1a0c3-131">C# ve Visual Basic şu anda kısmi sınıflar destekleyen yönetilen diller yalnızca var.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="1a0c3-132">Bu nedenle, geliştiricilerin J# kullanarak ASP.NET 2.0 ile arka plan kod modeli kullanmanız mümkün olmaz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="1a0c3-133">Geliştiriciler artık, oluşturmuş olduğunuz kod içeren kod dosyaları olacağı için yeni model arka plan kodu modelini geliştirir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="1a0c3-134">Hiçbir örnek değişken bildirimleri arka plan kod dosyasına olduğundan true ayrılması kodunuz ve içeriğinizle de sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="1a0c3-135">Olay bağlama gerçekleştiği parçalı sınıf ASPX Sayfası olduğu için Visual Basic geliştiricileri olayları bağlanacak kod arkasında tanıtıcıları anahtar sözcüğünü kullanarak hafif performans artışı sağlarsınız.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="1a0c3-136">C# eşdeğer anahtar sözcük vardır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="1a0c3-137">Yeni @ sayfa yönergesi öznitelikler</span><span class="sxs-lookup"><span data-stu-id="1a0c3-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="1a0c3-138">ASP.NET 2.0 @page yönergesi için birçok yeni öznitelikler ekler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="1a0c3-139">Aşağıdaki öznitelikler, ASP.NET 2.0 ile yenidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="1a0c3-140">Zaman Uyumsuz</span><span class="sxs-lookup"><span data-stu-id="1a0c3-140">Async</span></span>

<span data-ttu-id="1a0c3-141">Async özniteliği, zaman uyumsuz olarak yürütülecek sayfa yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="1a0c3-142">Zaman uyumsuz sayfalarında daha sonra bu modül de kapsar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="1a0c3-143">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="1a0c3-143">AsyncTimeout</span></span>

<span data-ttu-id="1a0c3-144">Zaman uyumsuz sayfalar için zaman aşımı belirtildi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="1a0c3-145">Varsayılan değer 45 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="1a0c3-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="1a0c3-146">CodeFile</span></span>

<span data-ttu-id="1a0c3-147">CodeFile özniteliği arkasındaki koda özniteliği, Visual Studio 2002/2003 yerini alır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="1a0c3-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="1a0c3-148">CodeFileBaseClass</span></span>

<span data-ttu-id="1a0c3-149">CodeFileBaseClass özniteliği tek bir taban sınıftan türetilen birden çok sayfa istediğiniz durumlarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="1a0c3-150">ASP.NET, kısmi sınıflar bu özniteliği olmadan uygulanması nedeniyle paylaşılan ortak alanları bir ASPX sayfasında bildirilen denetimleri başvurmak için kullandığı bir temel sınıf düzgün çünkü çalışmayan ASP. Ağ derleme altyapısı sayfasındaki denetimleri dayalı yeni üyeler otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="1a0c3-151">Bu nedenle, ASP.NET iki veya daha fazla sayfalar için ortak bir taban sınıf istiyorsanız tanımlamak gerekecektir CodeFileBaseClass özniteliğinde, taban sınıfı belirtin ve ardından her sayfa sınıfı, temel sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="1a0c3-152">Bu öznitelik kullanıldığında CodeFile özniteliği de gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="1a0c3-153">compilationMode</span><span class="sxs-lookup"><span data-stu-id="1a0c3-153">CompilationMode</span></span>

<span data-ttu-id="1a0c3-154">Bu öznitelik ASPX sayfasının CompilationMode özelliği ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="1a0c3-155">Değerleri içeren bir numaralandırma CompilationMode özelliktir **her zaman**, **otomatik**, ve **hiçbir zaman**.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="1a0c3-156">Varsayılan değer **her zaman**.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-156">The default is **Always**.</span></span> <span data-ttu-id="1a0c3-157">**Otomatik** ayarı, ASP.NET dinamik olarak sayfa mümkünse derlemesini engeller.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="1a0c3-158">Dinamik derlemeden sayfaları hariç performansı artırır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="1a0c3-159">Ancak, hariç tutulan bir sayfa derlenmelidir bu kodu içeriyorsa, bir hata sayfası tarandığında oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="1a0c3-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="1a0c3-160">EnableEventValidation</span></span>

<span data-ttu-id="1a0c3-161">Bu öznitelik geri gönderme ve geri çağırma olayları doğrulanmış olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="1a0c3-162">Bu etkin olduğunda, değişkenler geri gönderme veya geri çağırma olayları ilk olarak işlenen sunucu denetiminden kaynaklanan denetlenir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="1a0c3-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="1a0c3-163">EnableTheming</span></span>

<span data-ttu-id="1a0c3-164">Bu öznitelik, bir sayfa üzerinde kullanılan ASP.NET Temalar olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="1a0c3-165">Varsayılan değer **false**.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-165">The default is **false**.</span></span> <span data-ttu-id="1a0c3-166">ASP.NET temaları ele alınmıştır [modülü 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="1a0c3-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="1a0c3-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="1a0c3-167">LinePragmas</span></span>

<span data-ttu-id="1a0c3-168">Bu öznitelik, derleme sırasında satır pragmaları eklenmesi gerekip gerekmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="1a0c3-169">Satır pragmaları kodun belirli bölümlerine işaretlemek için hata ayıklayıcıları tarafından kullanılan seçeneklerdir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="1a0c3-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="1a0c3-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="1a0c3-171">Bu öznitelik Geri göndermeler arasında kaydırma konumunu korumak için sayfaya JavaScript eklenmiş olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="1a0c3-172">Bu öznitelik **false** varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="1a0c3-173">Bu öznitelik olduğunda **true**, ASP.NET ekleyecek bir &lt;betik&gt; şöyle geri gönderme blok:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="1a0c3-174">Bu betik bloğu src WebResource.axd olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="1a0c3-175">Bu kaynak fiziksel bir yol değil.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-175">This resource is not a physical path.</span></span> <span data-ttu-id="1a0c3-176">Bu komut dosyası istendiğinde, ASP.NET betik dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="1a0c3-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="1a0c3-177">MasterPageFile</span></span>

<span data-ttu-id="1a0c3-178">Bu öznitelik geçerli sayfa için ana sayfa dosyası belirtir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="1a0c3-179">Yol, göreli veya mutlak olabilir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-179">The path can be relative or absolute.</span></span> <span data-ttu-id="1a0c3-180">Ana sayfalar ele alınmıştır [modülü 4](master-pages.md).</span><span class="sxs-lookup"><span data-stu-id="1a0c3-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="1a0c3-181">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="1a0c3-181">StyleSheetTheme</span></span>

<span data-ttu-id="1a0c3-182">Bu öznitelik, bir ASP.NET 2.0 tema tarafından tanımlanan kullanıcı arabirimi görünüm özellikleri geçersiz kılmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="1a0c3-183">Temalar ele alınmıştır [modülü 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="1a0c3-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="1a0c3-184">Tema</span><span class="sxs-lookup"><span data-stu-id="1a0c3-184">Theme</span></span>

<span data-ttu-id="1a0c3-185">Sayfa temasının belirtir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-185">Specifies the theme for the page.</span></span> <span data-ttu-id="1a0c3-186">StyleSheetTheme özniteliği için bir değer belirtilmezse, öznitelikten sayfadaki denetimleri uygulanan tüm stillerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="1a0c3-187">Başlık</span><span class="sxs-lookup"><span data-stu-id="1a0c3-187">Title</span></span>

<span data-ttu-id="1a0c3-188">Sayfa başlığını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-188">Sets the title for the page.</span></span> <span data-ttu-id="1a0c3-189">Burada belirtilen değer görüntülenir &lt;başlık&gt; işlenen sayfanın öğesi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="1a0c3-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="1a0c3-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="1a0c3-191">ViewStateEncryptionMode numaralandırması için değeri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="1a0c3-192">Kullanılabilir değerler **her zaman**, **otomatik**, ve **hiçbir zaman**.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="1a0c3-193">Varsayılan değer **otomatik**. Bu öznitelik için bir değer ayarlandığında **otomatik**, viewstate şifrelenir denetim istekleri, çağırarak olan **RegisterRequiresViewStateEncryption** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="1a0c3-194">Ortak özellik değerlerinin aracılığıyla @ sayfa yönergesi</span><span class="sxs-lookup"><span data-stu-id="1a0c3-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="1a0c3-195">Başka bir yeni özelliği ASP.NET 2.0 @page yönergesinde, ortak bir taban sınıf özelliklerini ilk değerini ayarlamak için yeteneğidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="1a0c3-196">Örneğin, bir genel özelliğe sahip adlı varsayalım **SomeText** temel sınıfınız ve buna benzer d için başlatılması için **Hello** ne zaman bir sayfa yüklenir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="1a0c3-197">Bu değer yalnızca @page yönergesinde ayarlayarak gerçekleştirmek sözlüğüdür:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="1a0c3-198">**SomeText** @ sayfa yönergesi özniteliği için temel sınıf SomeText özelliğinin ilk değeri ayarlar *Hello!*.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="1a0c3-199">Video @page yönergesi kullanarak bir taban sınıf içinde bir genel özelliğinin ilk değeri ayarlamanın bir kılavuz ' dir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="1a0c3-200">Açık Tam Ekran Video</span><span class="sxs-lookup"><span data-stu-id="1a0c3-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="1a0c3-201">Sayfa sınıfının yeni ortak özellikleri</span><span class="sxs-lookup"><span data-stu-id="1a0c3-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="1a0c3-202">Aşağıdaki genel özellikleri ASP.NET 2. 0 ' yenidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="1a0c3-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="1a0c3-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="1a0c3-204">Uygulama göreli yolu sayfasının veya denetiminin döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="1a0c3-205">Örneğin, konumunda bulunan bir sayfa için http://app/folder/page.aspx, özellik döndürür ~ / klasör /.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="1a0c3-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="1a0c3-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="1a0c3-207">Göreli sanal dizin yolu sayfasının veya denetiminin döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="1a0c3-208">Örneğin konumunda bulunan bir sayfa için http://app/folder/page.aspx, özellik döndürür ~ / folder/page.aspx.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="1a0c3-209">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="1a0c3-209">AsyncTimeout</span></span>

<span data-ttu-id="1a0c3-210">Alır veya zaman uyumsuz sayfa işleme için kullanılan zaman aşımı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="1a0c3-211">(Zaman uyumsuz sayfaları Bu modülün daha sonra ele alınacaktır.)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="1a0c3-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="1a0c3-212">ClientQueryString</span></span>

<span data-ttu-id="1a0c3-213">İstenen URL sorgu dizesi bölümünü döndürür salt okunur özellik.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="1a0c3-214">URL kodlanmış değerdir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-214">This value is URL encoded.</span></span> <span data-ttu-id="1a0c3-215">Bunu çözmek için HttpServerUtility sınıfının UrlDecode yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="1a0c3-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="1a0c3-216">ClientScript</span></span>

<span data-ttu-id="1a0c3-217">Bu özellik, istemci tarafı komut dosyası ASP.NETs yayımlanmasını yönetmek için kullanılan bir ClientScriptManager nesnesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="1a0c3-218">(ClientScriptManager sınıfı daha sonra bu modülde ele alınmıştır.)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="1a0c3-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="1a0c3-219">EnableEventValidation</span></span>

<span data-ttu-id="1a0c3-220">Bu özellik, olay doğrulama geri gönderme ve geri çağırma olaylar için etkin denetler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="1a0c3-221">Etkin olduğunda, değişkenler geri gönderme veya geri çağırma olayları ilk olarak işlenen sunucu denetiminden kaynaklanan emin olmak için doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="1a0c3-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="1a0c3-222">EnableTheming</span></span>

<span data-ttu-id="1a0c3-223">Bu özellik alır veya ASP.NET 2.0 tema sayfasına uygulanıp uygulanmayacağını belirten bir Boole değeri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="1a0c3-224">Form</span><span class="sxs-lookup"><span data-stu-id="1a0c3-224">Form</span></span>

<span data-ttu-id="1a0c3-225">Bu özellik HTML formu ASPX sayfasında HtmlForm nesne olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="1a0c3-226">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="1a0c3-226">Header</span></span>

<span data-ttu-id="1a0c3-227">Bu özellik, bir nesneye başvuru sayfa üstbilgisi içeren HtmlHead döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="1a0c3-228">Döndürülen HtmlHead nesne get/set için stil sayfaları, Meta etiketleri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="1a0c3-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="1a0c3-229">IdSeparator</span></span>

<span data-ttu-id="1a0c3-230">Bu özelliği salt okunur bir sayfadaki denetimleri için benzersiz bir kimlik ASP.NET oluştururken denetim tanımlayıcıları ayırmak için kullanılan karakteri alır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="1a0c3-231">Doğrudan kodunuzdan kullanılmaya yönelik değildir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="1a0c3-232">IsAsync</span><span class="sxs-lookup"><span data-stu-id="1a0c3-232">IsAsync</span></span>

<span data-ttu-id="1a0c3-233">Bu özellik için zaman uyumsuz sayfaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="1a0c3-234">Zaman uyumsuz sayfaları daha sonra bu modülde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="1a0c3-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="1a0c3-235">IsCallback</span></span>

<span data-ttu-id="1a0c3-236">Bu salt okunur özellik döndürür **true** sayfa geri arama sonucu ise.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="1a0c3-237">Geri daha sonra bu modülde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="1a0c3-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="1a0c3-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="1a0c3-239">Bu salt okunur özellik döndürür **true** sayfa Sayfalar arası geri gönderimin parçası olduğunda.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="1a0c3-240">Çapraz sayfa Geri göndermeler daha sonra bu modülde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="1a0c3-241">Öğeler</span><span class="sxs-lookup"><span data-stu-id="1a0c3-241">Items</span></span>

<span data-ttu-id="1a0c3-242">IDictionary örneğine sayfaları bağlamda depolanan tüm nesnelerini içeren bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="1a0c3-243">Bunlar için bağlam ömrü kullanılabilir ve bu IDictionary nesnesine öğeleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="1a0c3-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="1a0c3-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="1a0c3-245">Bu özellik, ASP.NET geri gönderimin gerçekleştikten sonra sayfaları tarayıcının konumda kaydırma tutar JavaScript destekleyip desteklemediğini yayar denetler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="1a0c3-246">(Bu özellik ayrıntılarını Bu modülün daha önce bahsedilen.)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="1a0c3-247">Ana</span><span class="sxs-lookup"><span data-stu-id="1a0c3-247">Master</span></span>

<span data-ttu-id="1a0c3-248">Bu salt okunur özellik AnaSayfa örneğine bir ana sayfa uygulanmış bir sayfa için bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="1a0c3-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="1a0c3-249">MasterPageFile</span></span>

<span data-ttu-id="1a0c3-250">Alır veya ayarlar sayfası için ana sayfa dosya adı.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="1a0c3-251">Bu özellik yalnızca PreInit yönteminde ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="1a0c3-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="1a0c3-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="1a0c3-253">Bu özellik alır veya en fazla sayfa durumu için bayt cinsinden ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="1a0c3-254">Özelliği pozitif bir sayı olduğunda, böylece belirtilen bayt sayısını aşmadığından sayfaları görünüm durumu birden çok gizli alanlarına ayrılmış olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="1a0c3-255">Özellik negatif bir sayı ise, Görünüm durumu parçalara bozuk değil.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="1a0c3-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="1a0c3-256">PageAdapter</span></span>

<span data-ttu-id="1a0c3-257">Sayfa için istekte bulunan tarayıcıyı değiştirir PageAdapter nesnesine bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="1a0c3-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="1a0c3-258">PreviousPage</span></span>

<span data-ttu-id="1a0c3-259">Önceki sayfaya başvuru bir Server.Transfer veya çapraz sayfa geri gönderimin durumlarda döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="1a0c3-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="1a0c3-260">SkinID</span></span>

<span data-ttu-id="1a0c3-261">Sayfaya uygulamak için ASP.NET 2.0 kaplama belirtir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="1a0c3-262">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="1a0c3-262">StyleSheetTheme</span></span>

<span data-ttu-id="1a0c3-263">Bu özellik alır veya bir sayfaya uygulanan stil sayfasının ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="1a0c3-264">TemplateControl</span><span class="sxs-lookup"><span data-stu-id="1a0c3-264">TemplateControl</span></span>

<span data-ttu-id="1a0c3-265">Sayfa için içeren denetlemek için bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="1a0c3-266">Tema</span><span class="sxs-lookup"><span data-stu-id="1a0c3-266">Theme</span></span>

<span data-ttu-id="1a0c3-267">Alır veya sayfaya uygulanan ASP.NET 2.0 tema adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="1a0c3-268">Bu değer PreInit yöntemi önce ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="1a0c3-269">Başlık</span><span class="sxs-lookup"><span data-stu-id="1a0c3-269">Title</span></span>

<span data-ttu-id="1a0c3-270">Bu özellik alır veya sayfanın başlığı sayfaları başlığından edinildiği şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="1a0c3-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="1a0c3-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="1a0c3-272">Alır veya sayfanın ViewStateEncryptionMode ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="1a0c3-273">Bu özellik bu modüldeki hakkında ayrıntılı bilgi bakın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="1a0c3-274">Sayfa sınıfının yeni korumalı Özellikler</span><span class="sxs-lookup"><span data-stu-id="1a0c3-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="1a0c3-275">ASP.NET 2.0 sayfa sınıfının yeni korumalı özellikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="1a0c3-276">Bağdaştırıcı</span><span class="sxs-lookup"><span data-stu-id="1a0c3-276">Adapter</span></span>

<span data-ttu-id="1a0c3-277">Cihaz sayfasında işleyen ControlAdapter başvuru istendiğinde döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="1a0c3-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="1a0c3-278">AsyncMode</span></span>

<span data-ttu-id="1a0c3-279">Bu özellik sayfası zaman uyumsuz olarak işlenir olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="1a0c3-280">Bu kodu doğrudan de, çalışma zamanı tarafından kullanılmaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="1a0c3-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="1a0c3-281">ClientIDSeparator</span></span>

<span data-ttu-id="1a0c3-282">Bu özellik benzersiz istemci denetimleri için kimlikleri oluştururken ayırıcı olarak kullanılan karakter döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="1a0c3-283">Bu kodu doğrudan de, çalışma zamanı tarafından kullanılmaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="1a0c3-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="1a0c3-284">PageStatePersister</span></span>

<span data-ttu-id="1a0c3-285">Bu özellik sayfası için PageStatePersister nesnesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="1a0c3-286">Bu özellik, öncelikli olarak ASP.NET denetim geliştiriciler tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="1a0c3-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="1a0c3-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="1a0c3-288">Bu özellik, tarayıcılar önbelleğe alma için dosya yolu eklenen benzersiz bir suffic döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="1a0c3-289">Varsayılan değer \_ \_ufps = ve 6 basamaklı bir sayı.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="1a0c3-290">Sayfa sınıfı için yeni genel yöntemler</span><span class="sxs-lookup"><span data-stu-id="1a0c3-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="1a0c3-291">Aşağıdaki genel yöntemler, ASP.NET 2.0 sayfa sınıfında yenidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="1a0c3-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="1a0c3-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="1a0c3-293">Bu yöntem, olay işleyici temsilcileri zaman uyumsuz sayfa yürütme için kaydeder.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="1a0c3-294">Zaman uyumsuz sayfaları daha sonra bu modülde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="1a0c3-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="1a0c3-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="1a0c3-296">Stili yaprak özelliklerinde sayfasına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="1a0c3-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="1a0c3-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="1a0c3-298">Bu yöntem sorularınızı zaman uyumsuz bir görevi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="1a0c3-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="1a0c3-299">GetValidators</span></span>

<span data-ttu-id="1a0c3-300">Hiçbiri belirtilmezse belirtilen doğrulama grubunun veya varsayılan doğrulama grubunun doğrulayıcıları koleksiyonunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="1a0c3-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="1a0c3-301">RegisterAsyncTask</span></span>

<span data-ttu-id="1a0c3-302">Bu yöntem yeni bir zaman uyumsuz görev kaydeder.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-302">This method registers a new async task.</span></span> <span data-ttu-id="1a0c3-303">Zaman uyumsuz sayfaları daha sonra bu modülde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="1a0c3-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="1a0c3-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="1a0c3-305">Bu yöntem ASP.NET sayfaları denetim durumu kalıcı olduğunu söyler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="1a0c3-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="1a0c3-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="1a0c3-307">Bu yöntem ASP.NET sayfaları viewstate şifreleme gerektiren söyler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="1a0c3-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="1a0c3-308">ResolveClientUrl</span></span>

<span data-ttu-id="1a0c3-309">Görüntüler, vb. için istemci istekleri için kullanılan göreli bir URL döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="1a0c3-310">SetFocus</span><span class="sxs-lookup"><span data-stu-id="1a0c3-310">SetFocus</span></span>

<span data-ttu-id="1a0c3-311">Bu yöntem odağı sayfa ilk kez yüklendiğinde, belirttiğiniz denetimi için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="1a0c3-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="1a0c3-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="1a0c3-313">Bu yöntem artık denetim durumu kalıcılığını gerektiren olarak geçirilen denetim kaldırır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="1a0c3-314">Sayfa yaşam döngüsü yapılan değişiklikler</span><span class="sxs-lookup"><span data-stu-id="1a0c3-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="1a0c3-315">ASP.NET 2.0 sayfa çevriminin önemli ölçüde değişmediğinden, ancak bilincinde olmanız gereken bazı yeni yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="1a0c3-316">ASP.NET 2.0 sayfa yaşam döngüsü, aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="1a0c3-317">PreInit (yeni, ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="1a0c3-318">Bir geliştirici erişebilirsiniz yaşam döngüsü erken aşamada PreInit olayıdır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="1a0c3-319">Bu olay eklenmesi programlı olarak ASP.NET 2.0 temalar, ana sayfalar değiştirmek, bir ASP.NET 2.0 profili, vb. için özelliklerine erişmek mümkün kılar. En önemli nokta Viewstate henüz yaşam döngüsü bu noktada denetimlerine uygulandığını değil hayata geçirmek bir geri gönderme durumda varsa.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="1a0c3-320">Bir geliştirici bu aşamada bir denetimin bir özelliğini değişirse, bu nedenle, büyük olasılıkla daha sonra sayfa çevriminin üzerine yazılacak.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="1a0c3-321">Init</span><span class="sxs-lookup"><span data-stu-id="1a0c3-321">Init</span></span>

<span data-ttu-id="1a0c3-322">Init olayı ASP.NET tarafından değiştirilmediği 1.x.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="1a0c3-323">Okuma veya sayfanızda denetimlerin özelliklerini başlatmak istediğiniz budur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="1a0c3-324">Bu aşama, ana sayfalar, temalar vb. zaten sayfasına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="1a0c3-325">InitComplete (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="1a0c3-326">InitComplete olay sayfaları başlatma aşaması sonunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="1a0c3-327">Bu noktada çevriminin sayfadaki denetimleri erişebilirsiniz, ancak durumlarına değil henüz doldurulan.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="1a0c3-328">Önyükleme (2. 0'yeni)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="1a0c3-329">Bu olay, tüm geri gönderme verileri uygulandıktan sonra ve sayfa önceki çağrılır\_yük.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="1a0c3-330">Yükleme</span><span class="sxs-lookup"><span data-stu-id="1a0c3-330">Load</span></span>

<span data-ttu-id="1a0c3-331">Load olayı ASP.NET tarafından değiştirilmediği 1.x.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="1a0c3-332">LoadComplete (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="1a0c3-333">LoadComplete son olay sayfaları yük aşamasında olaydır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="1a0c3-334">Bu aşamada, tüm geri gönderme ve viewstate veri uygulandı sayfasına.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="1a0c3-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="1a0c3-335">PreRender</span></span>

<span data-ttu-id="1a0c3-336">Sayfaya dinamik olarak eklenen denetimler için düzgün bir şekilde korunmasını viewstate isterseniz PreRender olayını bunları eklemek için son şansınızdır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="1a0c3-337">PreRenderComplete (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="1a0c3-338">PreRenderComplete aşamada tüm denetimler sayfaya eklendiğinden ve sayfa işlenmek üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="1a0c3-339">Son olay sayfaları viewstate kaydedilmeden önce gerçekleşti PreRenderComplete etkinliğidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="1a0c3-340">SaveStateComplete (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="1a0c3-341">Tüm Sayfa görünüm durumu ve denetim durumu hemen kaydedildikten sonra SaveStateComplete olay çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="1a0c3-342">Bu sayfa gerçekten tarayıcıya işlenmeden önce son olaydır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="1a0c3-343">İşleme</span><span class="sxs-lookup"><span data-stu-id="1a0c3-343">Render</span></span>

<span data-ttu-id="1a0c3-344">İşleme yöntemi ASP.NET bu yana değişmemiştir 1.x.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="1a0c3-345">Burada HtmlTextWriter başlatılır ve sayfa tarayıcıda görüntülenen budur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="1a0c3-346">ASP.NET 2.0 Sayfalar arası geri gönderme</span><span class="sxs-lookup"><span data-stu-id="1a0c3-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="1a0c3-347">ASP.NET 1.x, Geri göndermeler aynı sayfaya göndermek için gerekli.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="1a0c3-348">Çapraz sayfa Geri göndermeler izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="1a0c3-349">ASP.NET 2.0 IButtonControl arabirimi aracılığıyla başka bir sayfaya geri gönderilecek yeteneği ekler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="1a0c3-350">(Düğme, LinkButton ve üçüncü taraf özel denetimler yanı sıra ImageButton) yeni IButtonControl arabirimini uygulayan herhangi bir denetimi bu yeni işlevsellik PostBackUrl özniteliğinin kullanımı aracılığıyla yararlanabilir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="1a0c3-351">Aşağıdaki kod, ikinci bir sayfaya yazılarını bir düğme denetimi gösterir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="1a0c3-352">Sayfa geri gönderildiğinde, geri gönderme başlatır sayfa ikinci sayfasında PreviousPage özelliği aracılığıyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="1a0c3-353">Bu işlev yeni WebForm uygulanır\_denetim başka bir sayfaya gönderdiğinde sayfasına ASP.NET 2.0 işleyen DoPostBackWithOptions istemci-tarafı işlevi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="1a0c3-354">Bu JavaScript işlevi, istemci betiği yayar yeni WebResource.axd işleyici tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="1a0c3-355">Video Sayfalar arası geri gönderimin bir kılavuz ' dir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="1a0c3-356">Açık Tam Ekran Video</span><span class="sxs-lookup"><span data-stu-id="1a0c3-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="1a0c3-357">Çapraz sayfa Geri göndermeler hakkında daha fazla bilgi</span><span class="sxs-lookup"><span data-stu-id="1a0c3-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="1a0c3-358">Görünüm durumu</span><span class="sxs-lookup"><span data-stu-id="1a0c3-358">Viewstate</span></span>

<span data-ttu-id="1a0c3-359">Kendiniz zaten viewstate Sayfalar arası geri gönderme senaryosunda ilk sayfadan olabilecekler sorulan.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="1a0c3-360">Tüm IPostBackDataHandler uygulamıyor herhangi bir denetimi viewstate, aracılığıyla durumu nedenle Sihirbazın ikinci sayfasında bir çapraz sayfa geri gönderme denetleyen özelliklerini erişimi için korunur, sayfanın görünüm durumu için erişimi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="1a0c3-361">ASP.NET 2.0 adlı ikinci sayfasında yeni bir gizli alan kullanarak bu senaryoyu mvc'deki \_ \_PREVIOUSPAGE.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="1a0c3-362">\_ \_PREVIOUSPAGE form alanı, tüm denetimler özelliklerine erişimi ikinci sayfasında böylece ilk sayfa için Görünüm durumu içerir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="1a0c3-363">Circumventing FindControl</span><span class="sxs-lookup"><span data-stu-id="1a0c3-363">Circumventing FindControl</span></span>

<span data-ttu-id="1a0c3-364">Çapraz sayfa geri gönderimin video kılavuzda ı FindControl TextBox denetimi ilk sayfasında bir başvuru almak için kullanılan yöntem.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="1a0c3-365">Bu yöntem, bu amaçla iyi çalışır, ancak FindControl pahalıdır ve ek kod yazma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="1a0c3-366">Neyse ki, ASP.NET 2.0, birçok senaryoda çalışır, bu amaç için alternatif FindControl sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="1a0c3-367">PreviousPageType yönergesi TypeName veya VirtualPath özniteliği kullanılarak kesin türü belirtilmiş bir başvuru önceki sayfaya sahip olmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="1a0c3-368">TypeName özniteliği VirtualPath özniteliği sanal yolu kullanarak önceki sayfaya bakın sağlar, ancak önceki sayfaya türünü belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="1a0c3-369">PreviousPageType yönergesi ayarladıktan sonra daha sonra ortaya gerekir denetimleri vb. ortak özellikleri kullanılarak erişimine izin vermek istiyor.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="1a0c3-370">Laboratuvar 1 Sayfalar arası geri gönderme</span><span class="sxs-lookup"><span data-stu-id="1a0c3-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="1a0c3-371">Bu laboratuvarda, ASP.NET 2.0 yeni sayfalar arası geri gönderme işlevlerini kullanan bir uygulama oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="1a0c3-372">Visual Studio 2005 açın ve yeni bir ASP.NET Web sitesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="1a0c3-373">Page2.aspx adlı yeni bir Webform ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="1a0c3-374">Default.aspx Tasarım görünümünde açın ve bir düğmeyi ve TextBox denetimi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="1a0c3-375">Düğme denetimi kimliği vermek **SubmitButton** ve metin kutusu denetim kimliği **kullanıcıadı**.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="1a0c3-376">Düğmenin PostBackUrl özelliği için page2.aspx ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="1a0c3-377">Page2.aspx kaynağı görünümünü açın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="1a0c3-378">@ PreviousPageType yönergesi aşağıda gösterildiği gibi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="1a0c3-379">Aşağıdaki kod sayfasına ekleme\_page2.aspx'ın arka plan kod yükünü:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="1a0c3-380">Build menüsünden üzerinde yapı tıklayarak projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="1a0c3-381">Arka plan kod Default.aspx için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="1a0c3-382">Sayfa değiştirme\_page2.aspx aşağıdaki yükleme:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="1a0c3-383">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-383">Build the project.</span></span>
11. <span data-ttu-id="1a0c3-384">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-384">Run the project.</span></span>
12. <span data-ttu-id="1a0c3-385">Metin kutusuna adınızı girin ve düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="1a0c3-386">Sonuç nedir?</span><span class="sxs-lookup"><span data-stu-id="1a0c3-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="1a0c3-387">ASP.NET 2.0 zaman uyumsuz sayfaları</span><span class="sxs-lookup"><span data-stu-id="1a0c3-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="1a0c3-388">ASP.NET birçok Çekişme sorunları (örneğin, Web hizmeti veya veritabanı çağrıları) dış çağrıları, dosya g/ç gecikmesi gecikmesine neden olur. Bir ASP.NET uygulaması karşı bir istek yapıldığında, ASP.NET birini kendi iş parçacıklarını Bu isteğe hizmet vermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="1a0c3-389">Bu istek, istek tamamlandıktan ve gönderilen yanıtı kadar o iş parçacığı sahip olur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="1a0c3-390">Özellik sayfaları zaman uyumsuz olarak yürütülecek ekleyerek bu tür sorunları gecikmesi sorunları çözümlemek ASP.NET 2.0 arar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="1a0c3-391">Bir çalışan iş parçacığı isteği başlatmak ve böylece kullanılabilir iş parçacığı havuzuna hızla döndürme başka bir iş parçacığı için ek yürütme kapalı el anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="1a0c3-392">Dosya g/ç, veritabanı çağrısı, vb. tamamlandı, yeni bir iş parçacığı isteği tamamlamak için iş parçacığı havuzundan elde edilir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="1a0c3-393">Zaman uyumsuz yürütme bir sayfa yapmadan ilk adımı ayarlamaktır **zaman uyumsuz** sayfa yönergesi özniteliği şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="1a0c3-394">Bu öznitelik sayfa IHttpAsyncHandler uygulamak için ASP.NET söyler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="1a0c3-395">Sonraki adım PreRender önce sayfasının ömrü içindeki bir noktada AddOnPreRenderCompleteAsync yöntemi çağırmaktır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="1a0c3-396">(Bu yöntem genellikle sayfasında çağrılır\_yük.) AddOnPreRenderCompleteAsync yöntemi iki parametre alır; bir BeginEventHandler ve bir EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="1a0c3-397">BeginEventHandler parametre olarak EndEventHandler sonra geçen bir IAsyncResult döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="1a0c3-398">Aşağıdaki video bir zaman uyumsuz sayfa isteği bir kılavuz ' dir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="1a0c3-399">Açık Tam Ekran Video</span><span class="sxs-lookup"><span data-stu-id="1a0c3-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="1a0c3-400">EndEventHandler tamamlanana kadar bir zaman uyumsuz sayfası tarayıcıya işlemez.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="1a0c3-401">Hiçbir şüpheli ancak bazı geliştiriciler için zaman uyumsuz geri aramalar benzer olarak zaman uyumsuz isteği düşünün.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="1a0c3-402">Olmadıklarını hayata geçirmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-402">It's important to realize that they are not.</span></span> <span data-ttu-id="1a0c3-403">Zaman uyumsuz isteklerine hizmet yeni istekler, böylece azaltma GÇ bağlı olması nedeniyle Çekişme, vb. için iş parçacığı havuzuna ilk çalışan iş parçacığı döndürülebilecek avantajdır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="1a0c3-404">ASP.NET 2.0 betik geri aramalar</span><span class="sxs-lookup"><span data-stu-id="1a0c3-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="1a0c3-405">Web geliştiricileri yolları geri arama ile ilişkili titremeyi önlemek için her zaman attıktan.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="1a0c3-406">ASP.NET 1.x, SmartNavigation titremeyi önlemenin en yaygın yöntem olsa da, istemci uygulaması karmaşıklığı nedeniyle bazı geliştiriciler için SmartNavigation sorunlarına neden.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="1a0c3-407">ASP.NET 2.0 betik geri aramalar ile bu sorunu giderir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="1a0c3-408">Betik geri aramalar JavaScript aracılığıyla Web sunucusuna karşı istekler yapmasını XMLHttp kullanın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="1a0c3-409">XMLHttp isteği sonra yönetilebilir XML verileri tarayıcının DOM döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="1a0c3-410">XMLHttp kod kullanıcıdan yeni WebResource.axd işleyici tarafından gizlenir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="1a0c3-411">ASP.NET 2.0 ile bir betik geri yapılandırmak için gerekli olan birkaç adım vardır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="1a0c3-412">1. adım: ICallbackEventHandler arabirimini uygulama</span><span class="sxs-lookup"><span data-stu-id="1a0c3-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="1a0c3-413">Bir komut dosyası geri katılan olarak sayfanızı tanımak ASP.NET sırayla ICallbackEventHandler arabirimini uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="1a0c3-414">Arka plan kodu dosyanızda bunu şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="1a0c3-415">Bu, @ Implements yönergesi benzer bunu kullanarak da yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="1a0c3-416">Satır içi ASP.NET kodunun kullanırken genellikle @ Implements yönergesi kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="1a0c3-417">2. adım: Çağrı GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="1a0c3-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="1a0c3-418">Daha önce belirtildiği gibi XMLHttp çağrısı WebResource.axd işleyicisinde kapsüllenir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="1a0c3-419">Sayfanız işlendiğinde, ASP.NET WebForm yapılan bir çağrı ekleyin\_DoCallback, WebResource.axd tarafından sağlanan bir istemci komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="1a0c3-420">WebForm\_DoCallback işlevi değiştirir \_ \_doPostBack işlevi için bir geri çağırma.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="1a0c3-421">Unutmayın \_ \_doPostBack sayfasındaki formu programlı olarak gönderir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="1a0c3-422">Bir geri çağırma senaryosunda, geri gönderimin, bu nedenle engellemek istediğiniz \_ \_doPostBack değil yeterli.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="1a0c3-423">\_\_doPostBack hala bir istemci komut dosyası geri çağırma senaryosunda sayfasına işlenir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="1a0c3-424">Ancak, geri çağırma için kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="1a0c3-425">WebForm bağımsız değişkenleri\_DoCallback istemci-tarafı işlevi, sunucu tarafı işlevi normalde sayfasında denilen GetCallbackEventReference aracılığıyla sağlanan\_yük.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="1a0c3-426">GetCallbackEventReference tipik bir çağrı şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="1a0c3-427">Bu durumda, cm ClientScriptManager örneğidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="1a0c3-428">ClientScriptManager sınıfı Bu modülün daha sonra ele alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="1a0c3-429">GetCallbackEventReference birkaç aşırı yüklü sürümü vardır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="1a0c3-430">Bu durumda, bağımsız değişkenleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="1a0c3-431">Burada GetCallbackEventReference çağrılan denetim referansı.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="1a0c3-432">Bu durumda, sayfa olur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="1a0c3-433">İstemci tarafı kodu sunucu tarafı olaya geçirilen dize bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="1a0c3-434">Bu durumda, anlık ileti bir açılır liste değeri geçirme ddlCompany çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="1a0c3-435">Dönüş değeri (dize) sunucu tarafı geri çağırma etkinlikten kabul eder istemci-tarafı işlevinin adı.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="1a0c3-436">Sunucu tarafı geri arama başarılı olduğunda bu işlev yalnızca çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="1a0c3-437">Bu nedenle, sağlamlık amacıyla, genellikle bir hata durumunda yürütmek için bir istemci-tarafı işlevin adını belirterek bir ek dize bağımsız değişken GetCallbackEventReference aşırı yüklü sürümünü kullanmak için önerilir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="1a0c3-438">Sunucuya geri çağırma önce başlatılan bir istemci-tarafı işlevi temsil eden dize.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="1a0c3-439">Bu durumda olmadığından bu tür bir betik yok, bağımsız değişkeni null.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="1a0c3-440">Geri çağırma zaman uyumsuz olarak gerçekleştir gerekip gerekmediğini belirten bir Boole değeri.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="1a0c3-441">WebForm çağrısı\_DoCallback istemcide, bu bağımsız değişkenler geçecek.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="1a0c3-442">Bu nedenle, bu sayfayı istemcide işlendiğinde, bu kodu görüneceğini sözlüğüdür:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="1a0c3-443">İstemci üzerinde işlevinin imzası biraz farklı olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="1a0c3-444">İstemci tarafı işlevi 5 dizeler ve bir Boole değeri geçirir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="1a0c3-445">Sunucu tarafı geri aramadan hataları işleyecek istemci-tarafı işlevi (yukarıdaki örnekte null ise) ek dizesini içerir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="1a0c3-446">3. adım: istemci-tarafı denetim olayı bağlayın</span><span class="sxs-lookup"><span data-stu-id="1a0c3-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="1a0c3-447">Yukarıdaki GetCallbackEventReference dönüş değeri bir dize değişkenine atanan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="1a0c3-448">Bu dize, geri çağırma başlatır denetimi için istemci tarafı bir olay kanca için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="1a0c3-449">Kanca istediğiniz şekilde bu örnekte, geri çağırma sayfasında açılır başlatılır *değiştiğinde* olay.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="1a0c3-450">İstemci tarafında olay kanca için yalnızca bir işleyicinin istemci-tarafı biçimlendirme aşağıdaki şekilde ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="1a0c3-451">Sözcüğünün *cbRef* GetCallbackEventReference çağrısı dönüş değeri.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="1a0c3-452">WebForm çağrısı içerdiği\_yukarıda gösterilen DoCallback.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="1a0c3-453">4. adım: istemci tarafı komut dosyası kaydetme</span><span class="sxs-lookup"><span data-stu-id="1a0c3-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="1a0c3-454">İstemci tarafı komut dosyası denen GetCallbackEventReference çağrısı belirtilen geri çağırma **ShowCompanyName** sunucu tarafı geri arama başarılı olduğunda yürütülmesi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="1a0c3-455">Bu komut dosyası ClientScriptManager örneği kullanarak sayfaya eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="1a0c3-456">(ClientScriptManager sınıfı daha sonra bu modüldeki dicussed olacaktır.) Bu benzer şekilde yapın:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="1a0c3-457">5. adım: ICallbackEventHandler arabiriminin yöntemlerini çağırın</span><span class="sxs-lookup"><span data-stu-id="1a0c3-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="1a0c3-458">ICallbackEventHandler kodunuzda uygulamanız gereken iki yöntem içerir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="1a0c3-459">Bunlar **RaiseCallbackEvent** ve **GetCallbackEvent**.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="1a0c3-460">**RaiseCallbackEvent** bağımsız değişken olarak bir dize alır ve hiçbir şey döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="1a0c3-461">Dize bağımsız değişkeni WebForm için istemci tarafı çağrısından geçirilen\_DoCallback.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="1a0c3-462">Bu durumda, bu değeri olan *değeri* ddlCompany adlı açılır özniteliğidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="1a0c3-463">Sunucu tarafı kodunuzu RaiseCallbackEvent yönteminde yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="1a0c3-464">Örneğin, geri WebRequest dış kaynak karşı değiştirirken, bu kod içinde RaiseCallbackEvent yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="1a0c3-465">**GetCallbackEvent** geri dönüş istemciye işlemekten sorumludur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="1a0c3-466">Bağımsız değişken almayan ve bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="1a0c3-467">Döndürdüğü dize bağımsız değişken olarak istemci tarafı işlevi için bu durumda geçirilir *ShowCompanyName*.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="1a0c3-468">Yukarıdaki adımları tamamladıktan sonra ASP.NET 2.0 ile bir betik geri gerçekleştirmek hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="1a0c3-469">Açık Tam Ekran Video</span><span class="sxs-lookup"><span data-stu-id="1a0c3-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="1a0c3-470">ASP komut dosyasını geri aramalar yapmayı XMLHttp çağrıları destekleyen herhangi bir tarayıcısında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="1a0c3-471">Tüm modern tarayıcılar kullanımda bugün içerir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="1a0c3-472">Modern tarayıcılar (yaklaşan IE 7 dahil) bir iç XMLHttp nesne kullanırken Internet Explorer XMLHttp ActiveX nesnesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="1a0c3-473">Program aracılığıyla bir tarayıcı geri aramalar destekliyorsa, kullanabileceğiniz belirlemek için **Request.Browser.SupportCallback** özelliği.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="1a0c3-474">Bu özellik döndürülecek **true** isteyen istemci komut dosyası geri aramalar destekliyorsa.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="1a0c3-475">ASP.NET 2.0 istemci komut dosyası ile çalışma</span><span class="sxs-lookup"><span data-stu-id="1a0c3-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="1a0c3-476">İstemci komut dosyalarını, ASP.NET 2.0 ClientScriptManager sınıfı kullanımı aracılığıyla yönetilir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="1a0c3-477">ClientScriptManager sınıfı, bir türü ve adı'nı kullanarak istemci betikleri izler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="1a0c3-478">Bu, aynı komut dosyasını program aracılığıyla bir sayfa üzerinde birden çok kez eklenmekte gelen önler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="1a0c3-479">Bir komut dosyası bir sayfa üzerinde başarıyla kaydedildikten sonra aynı komut dosyasını kaydetme sonraki girişimleri yalnızca ikinci kez kaydedilmemiş komut dosyasında neden olur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="1a0c3-480">Yinelenen komut dosyası eklenir ve hiçbir özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="1a0c3-481">Gereksiz hesaplama önlemek için böylece birden çok kez kaydettirmeye çalışırsanız olmayan bir komut dosyası zaten kayıtlı olup olmadığını belirlemek için kullanabileceğiniz yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="1a0c3-482">ClientScriptManager yöntemlerinin tüm geçerli ASP.NET geliştiricilerinin aşina olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="1a0c3-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="1a0c3-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="1a0c3-484">Bu yöntem, bir komut dosyası işlenen sayfanın üst kısmına ekler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="1a0c3-485">Bu, istemci üzerinde açıkça çağrılacağı işlevler eklemek için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="1a0c3-486">Bu yöntem aşırı yüklenmiş iki sürümü vardır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="1a0c3-487">Üç dört bağımsız bunlar arasında ortak olan.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="1a0c3-488">Bunlar:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-488">They are:</span></span>

`type (string)`

<span data-ttu-id="1a0c3-489">***Türü*** bağımsız değişken, komut dosyası için bir tür tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="1a0c3-490">Bu genellikle sayfa türü (Bu. kullanmak için iyi bir fikirdir GetType()) türü için.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="1a0c3-491">***Anahtar*** bağımsız değişkeni olan komut dosyası için bir kullanıcı tanımlı anahtar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="1a0c3-492">Bu, her komut dosyası için benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-492">This should be unique for each script.</span></span> <span data-ttu-id="1a0c3-493">Bir komut dosyası aynı anahtar ve zaten eklenmiş bir komut dosyası türünü eklemeye çalışırsanız, eklenmeyecek.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="1a0c3-494">***Betik*** bağımsız değişkeni eklemek için gerçek komut dosyasını içeren bir dize değil.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="1a0c3-495">Komut dosyası oluşturabilir ve ardından atamak için StringBuilder üzerinde ToString() yöntemini kullanmak için bir StringBuilder kullanmanız önerilir ***betik*** bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="1a0c3-496">Yalnızca üç bağımsız değişken almayan aşırı yüklenmiş RegisterClientScriptBlock kullanırsanız, komut dosyası öğeleri içermelidir (&lt;betik&gt; ve &lt;/script&gt;) komut.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="1a0c3-497">Dördüncü bir bağımsız değişken RegisterClientScriptBlock yüklemesini kullanmayı seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="1a0c3-498">Dördüncü değişken ASP.NET komut dosyası öğeleri için eklemelisiniz olup olmadığını belirten bir Boole değeri.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="1a0c3-499">Bu bağımsız değişken ise **doğru**, kodunuzu komut dosyası öğeleri açıkça içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="1a0c3-500">Bir komut dosyası zaten kayıtlı olup olmadığını belirlemek için IsClientScriptBlockRegistered yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="1a0c3-501">Bu, zaten kayıtlı bir komut dosyası yeniden kaydetmek için girişiminde önlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="1a0c3-502">RegisterClientScriptInclude (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="1a0c3-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="1a0c3-503">RegisterClientScriptInclude etiket bir dış komut dosyası bağlanan bir betik bloğu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="1a0c3-504">İki aşırı yüklemeye sahip.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-504">It has two overloads.</span></span> <span data-ttu-id="1a0c3-505">Bir anahtarı ve bir URL alır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-505">One takes a key and a URL.</span></span> <span data-ttu-id="1a0c3-506">İkinci, üçüncü bağımsız değişken türünü belirleme ekler.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="1a0c3-507">Örneğin, aşağıdaki kod betikler klasörüne uygulamanın kök dizininde jsfunctions.js bağlanan bir betik bloğu oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="1a0c3-508">Bu kod işlenen sayfa aşağıdaki kodda üretir:</span><span class="sxs-lookup"><span data-stu-id="1a0c3-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="1a0c3-509">Betik bloğu sayfasının en altında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="1a0c3-510">Bir komut dosyası zaten kayıtlı olup olmadığını belirlemek için IsClientScriptIncludeRegistered yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="1a0c3-511">Bu, bir komut dosyası yeniden kaydetmek için girişiminde önlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="1a0c3-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="1a0c3-512">RegisterStartupScript</span></span>

<span data-ttu-id="1a0c3-513">RegisterStartupScript yöntemi aynı bağımsız değişkenlere RegisterClientScriptBlock yöntemi alır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="1a0c3-514">RegisterStartupScript ile kayıtlı bir komut dosyası, Sayfa yüklendikten sonra ancak yüklendiğinde istemci-tarafı olayı önce yürütür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="1a0c3-515">1.X içinde RegisterStartupScript ile kayıtlı komut dosyaları yalnızca kapatmadan önce yerleştirildi &lt;/form&gt; RegisterClientScriptBlock ile kayıtlı betikleri açtıktan hemen sonra yerleştirildi sırada etiketi &lt;form&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="1a0c3-516">ASP.NET 2. 0'da, her ikisi de hemen kapatmadan önce yerleştirilir &lt;/form&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="1a0c3-517">Bir işlev ile RegisterStartupScript kaydolursanız, açıkça istemci-tarafı kodda çağrısı tamamlanana kadar bu işlev çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="1a0c3-518">Bir komut dosyası zaten kayıtlı olup olmadığını belirlemek ve komut dosyası yeniden kaydetmek için girişiminde önlemek için IsStartupScriptRegistered yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="1a0c3-519">Diğer ClientScriptManager yöntemleri</span><span class="sxs-lookup"><span data-stu-id="1a0c3-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="1a0c3-520">Diğer kullanışlı yöntemler ClientScriptManager sınıfının bazıları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>


|  <span data-ttu-id="1a0c3-521"><strong>GetCallbackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="1a0c3-521"><strong>GetCallbackEventReference</strong></span></span>   |                                                 <span data-ttu-id="1a0c3-522">Bu modüldeki betik geri aramalar bakın.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-522">See script callbacks earlier in this module.</span></span>                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <span data-ttu-id="1a0c3-523"><strong>GetPostBackClientHyperlink</strong></span><span class="sxs-lookup"><span data-stu-id="1a0c3-523"><strong>GetPostBackClientHyperlink</strong></span></span>  |                <span data-ttu-id="1a0c3-524">JavaScript başvurusu alır (javascript:&lt;çağrısı&gt;) bir istemci-tarafı olayından göndermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span>                 |
|  <span data-ttu-id="1a0c3-525"><strong>GetPostBackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="1a0c3-525"><strong>GetPostBackEventReference</strong></span></span>   |                                   <span data-ttu-id="1a0c3-526">Bir posta istemcisinden geri başlatmak için kullanılan bir dize alır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-526">Gets a string that can be used to initiate a post back from the client.</span></span>                                    |
|      <span data-ttu-id="1a0c3-527"><strong>GetWebResourceUrl</strong></span><span class="sxs-lookup"><span data-stu-id="1a0c3-527"><strong>GetWebResourceUrl</strong></span></span>       | <span data-ttu-id="1a0c3-528">Bir derlemede katıştırılmış bir kaynağı bir URL döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="1a0c3-529">İle birlikte kullanılmalıdır <strong>RegisterClientScriptResource</strong>.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-529">Must be used in conjunction with <strong>RegisterClientScriptResource</strong>.</span></span> |
| <span data-ttu-id="1a0c3-530"><strong>RegisterClientScriptResource</strong></span><span class="sxs-lookup"><span data-stu-id="1a0c3-530"><strong>RegisterClientScriptResource</strong></span></span> |     <span data-ttu-id="1a0c3-531">Bir Web kaynağını sayfasıyla kaydeder.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="1a0c3-532">Bu derlemede katıştırılmış ve yeni WebResource.axd işleyicisi tarafından işlenen kaynaklardır.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span>      |
|     <span data-ttu-id="1a0c3-533"><strong>RegisterHiddenField</strong></span><span class="sxs-lookup"><span data-stu-id="1a0c3-533"><strong>RegisterHiddenField</strong></span></span>      |                                                 <span data-ttu-id="1a0c3-534">Gizli bir form alanı sayfasıyla kaydeder.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-534">Registers a hidden form field with the page.</span></span>                                                 |
|  <span data-ttu-id="1a0c3-535"><strong>RegisterOnSubmitStatement</strong></span><span class="sxs-lookup"><span data-stu-id="1a0c3-535"><strong>RegisterOnSubmitStatement</strong></span></span>   |                                  <span data-ttu-id="1a0c3-536">HTML form gönderildiğinde yürütülen istemci-tarafı kodunun kaydeder.</span><span class="sxs-lookup"><span data-stu-id="1a0c3-536">Registers client-side code that executes when the HTML form is submitted.</span></span>                                   |

