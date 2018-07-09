---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 sayfa modeli | Microsoft Docs
author: microsoft
description: ASP.NET'te 1.x, geliştiricilerin sahip bir satır içi kod modeli ve gerideki kod modeli arasında seçim yapma. Arka plan kod Src attr kullanarak uygulanabileceğine...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 2c8a0624af30d93d2ba68dc4b0b0880d6e371616
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811707"
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="ae818-104">ASP.NET 2.0 sayfa modeli</span><span class="sxs-lookup"><span data-stu-id="ae818-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="ae818-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ae818-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ae818-106">ASP.NET'te 1.x, geliştiricilerin sahip bir satır içi kod modeli ve gerideki kod modeli arasında seçim yapma.</span><span class="sxs-lookup"><span data-stu-id="ae818-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="ae818-107">Arka plan kod Src özniteliğine veya CodeBehind özniteliğinin kullanarak uygulanmasını @Page yönergesi.</span><span class="sxs-lookup"><span data-stu-id="ae818-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="ae818-108">ASP.NET 2. 0'da, geliştiricilerin yine de satır içi kod ve arka plan kod arasında bir seçim var, ancak arka plan kod modeline önemli geliştirmeler yapıldı.</span><span class="sxs-lookup"><span data-stu-id="ae818-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="ae818-109">ASP.NET'te 1.x, geliştiricilerin sahip bir satır içi kod modeli ve gerideki kod modeli arasında seçim yapma.</span><span class="sxs-lookup"><span data-stu-id="ae818-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="ae818-110">Arka plan kod Src özniteliğine veya CodeBehind özniteliğinin kullanarak uygulanmasını @Page yönergesi.</span><span class="sxs-lookup"><span data-stu-id="ae818-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="ae818-111">ASP.NET 2. 0'da, geliştiricilerin yine de satır içi kod ve arka plan kod arasında bir seçim var, ancak arka plan kod modeline önemli geliştirmeler yapıldı.</span><span class="sxs-lookup"><span data-stu-id="ae818-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="ae818-112">Arka plan kod modelinde geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="ae818-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="ae818-113">Hızla model olarak onu gözden geçirmek için en iyi şekilde varolan ASP.NET'te ASP.NET 2.0 arka plan kod modelde değişiklikler tam olarak anlamak için 1.x.</span><span class="sxs-lookup"><span data-stu-id="ae818-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="ae818-114">ASP.NET arka plan kod modelde 1.x</span><span class="sxs-lookup"><span data-stu-id="ae818-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="ae818-115">ASP.NET'te 1.x, arka plan kod modeli oluşturan bir ASPX dosyası (Webform) ve programlama kodu içeren bir arka plan kod dosyası.</span><span class="sxs-lookup"><span data-stu-id="ae818-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="ae818-116">İki dosyayı kullanarak bağlanmış @Page ASPX dosyasında yönergesi.</span><span class="sxs-lookup"><span data-stu-id="ae818-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="ae818-117">Her denetimi ASPX sayfasına karşılık gelen bir bildirim arka plan kod dosyasında bir örnek değişkeni vardı.</span><span class="sxs-lookup"><span data-stu-id="ae818-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="ae818-118">Arka plan kod dosyasında olay bağlama kodunu yer alan ve oluşturulan kod Visual Studio tasarımcısı için gerekli.</span><span class="sxs-lookup"><span data-stu-id="ae818-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="ae818-119">Bu modeli oldukça iyi çalışan, ancak her ASPX sayfasına ASP.NET öğesinde karşılık gelen kodu arka plan kod dosyasında gerektirdiğinden, kod ve içeriğinin doğru hiçbir ayrım vardı.</span><span class="sxs-lookup"><span data-stu-id="ae818-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="ae818-120">Bir tasarımcı, Visual Studio IDE dışında ASPX dosyasına yeni bir sunucu denetimi eklediyseniz, örneğin, uygulama bu denetim için bir bildirim olmaması nedeniyle arka plan kod dosyasında çalışmamasına neden.</span><span class="sxs-lookup"><span data-stu-id="ae818-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="ae818-121">ASP.NET 2.0 arka plan kod modeli</span><span class="sxs-lookup"><span data-stu-id="ae818-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="ae818-122">ASP.NET 2.0 üzerinde bu model önemli ölçüde artırır.</span><span class="sxs-lookup"><span data-stu-id="ae818-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="ae818-123">ASP.NET 2. 0'da, arka plan kod kullanarak yeni uygulanan *kısmi sınıflar* ASP.NET 2.0 sürümünde sağlanan.</span><span class="sxs-lookup"><span data-stu-id="ae818-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="ae818-124">Arka plan kod ASP.NET 2.0 el sınıf tanımı yalnızca bir kısmını içerdiği anlamına gelen bir kısmi sınıf olarak sınıftır.</span><span class="sxs-lookup"><span data-stu-id="ae818-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="ae818-125">Sınıf tanımının kalan bölümü, çalışma zamanında veya Web sitesi Ön derlenmiş ASPX sayfa kullanarak ASP.NET 2.0 tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ae818-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="ae818-126">Arka plan kod dosyası ve ASPX Sayfası arasındaki bağlantıyı yine de @ sayfa yönergesi kullanarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ae818-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="ae818-127">Ancak, bir CodeBehind veya Src özniteliği yerine ASP.NET 2.0 CodeFile öznitelik şimdi kullanır.</span><span class="sxs-lookup"><span data-stu-id="ae818-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="ae818-128">Inherits özniteliği de sayfa için sınıf adını belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae818-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="ae818-129">Tipik bir @ sayfa yönergesi şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="ae818-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="ae818-130">Bir ASP.NET 2.0 arka plan kod dosyasında bir genel sınıf tanımının şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="ae818-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="ae818-131">C# ve Visual Basic şu anda kısmi sınıflar desteği yalnızca yönetilen diller şunlardır.</span><span class="sxs-lookup"><span data-stu-id="ae818-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="ae818-132">Bu nedenle, geliştiriciler J# kullanarak ASP.NET 2.0 sürümünde arka plan kod modelini kullanmanız mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="ae818-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="ae818-133">Geliştiriciler artık, kullanıcının oluşturduğu kodu içeren kod dosyaları olduğundan yeni modeli arka plan kod modelini geliştirir.</span><span class="sxs-lookup"><span data-stu-id="ae818-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="ae818-134">Arka plan kod dosyasında hiçbir örneği değişken bildirimlerini olduğundan doğru ayrımı kod ve içerik de sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="ae818-135">ASPX sayfa için kısmi sınıf olay bağlama gerçekleştiği olduğundan, Visual Basic geliştiricileri olaylar bağlamak için gerideki kod tanıtıcıları anahtar sözcüğü kullanarak küçük bir performans artışı sağlarsınız.</span><span class="sxs-lookup"><span data-stu-id="ae818-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="ae818-136">C# anahtar sözcük eşdeğer vardır.</span><span class="sxs-lookup"><span data-stu-id="ae818-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="ae818-137">Yeni @ sayfa yönergesi öznitelikler</span><span class="sxs-lookup"><span data-stu-id="ae818-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="ae818-138">ASP.NET 2.0 @ sayfa yönergesi için birçok yeni öznitelikler ekler.</span><span class="sxs-lookup"><span data-stu-id="ae818-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="ae818-139">Aşağıdaki öznitelikler ASP.NET 2.0 sürümünde yenidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="ae818-140">Zaman Uyumsuz</span><span class="sxs-lookup"><span data-stu-id="ae818-140">Async</span></span>

<span data-ttu-id="ae818-141">Zaman uyumsuz özniteliği zaman uyumsuz olarak yürütülecek sayfa yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="ae818-142">Daha sonra bu modüldeki zaman uyumsuz sayfaları da kapsar.</span><span class="sxs-lookup"><span data-stu-id="ae818-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="ae818-143">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="ae818-143">AsyncTimeout</span></span>

<span data-ttu-id="ae818-144">Zaman uyumsuz sayfaları için zaman aşımı belirtildi.</span><span class="sxs-lookup"><span data-stu-id="ae818-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="ae818-145">Varsayılan değer 45 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="ae818-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="ae818-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="ae818-146">CodeFile</span></span>

<span data-ttu-id="ae818-147">CodeFile özniteliği CodeBehind özniteliğinin, Visual Studio 2002/2003 yerini alır.</span><span class="sxs-lookup"><span data-stu-id="ae818-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="ae818-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="ae818-148">CodeFileBaseClass</span></span>

<span data-ttu-id="ae818-149">CodeFileBaseClass öznitelik, tek bir temel sınıftan türetmek için birden çok sayfada istediğiniz durumlarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae818-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="ae818-150">Bu öznitelik olmadan, ASP.NET'te kısmi sınıflar uygulanması nedeniyle bir ASPX sayfa bildirilen denetimleri başvurmak için paylaşılan ortak alanları kullanan bir temel sınıf düzgün olduğundan çalışmak istemiyor ASP. Ağ derleme altyapısı sayfasındaki denetimleri temel alarak yeni üyeleri otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ae818-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="ae818-151">Bu nedenle, iki veya daha fazla ASP.NET sayfaları için genel bir temel sınıf istiyorsanız, tanımlama gerekecektir CodeFileBaseClass özniteliğinde, taban sınıf belirtin ve ardından o temel sınıftan her sayfaları sınıf türetin.</span><span class="sxs-lookup"><span data-stu-id="ae818-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="ae818-152">Bu öznitelik kullanıldığında CodeFile özniteliği de gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="ae818-153">CompilationMode</span><span class="sxs-lookup"><span data-stu-id="ae818-153">CompilationMode</span></span>

<span data-ttu-id="ae818-154">Bu öznitelik ASPX sayfa CompilationMode özelliği ayarlamanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="ae818-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="ae818-155">CompilationMode özellik değerleri içeren bir numaralandırmadır **her zaman**, **otomatik**, ve **hiçbir zaman**.</span><span class="sxs-lookup"><span data-stu-id="ae818-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="ae818-156">Varsayılan değer **her zaman**.</span><span class="sxs-lookup"><span data-stu-id="ae818-156">The default is **Always**.</span></span> <span data-ttu-id="ae818-157">**Otomatik** ayarı, ASP.NET dinamik olarak sayfa mümkünse derleme öğesinden engeller.</span><span class="sxs-lookup"><span data-stu-id="ae818-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="ae818-158">Sayfaları dinamik derlemeden hariç performansı artırır.</span><span class="sxs-lookup"><span data-stu-id="ae818-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="ae818-159">Ancak, hariç bir sayfa derlenmelidir bu kodu içeriyorsa, sayfasına göz atıldığında bir hata oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ae818-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="ae818-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="ae818-160">EnableEventValidation</span></span>

<span data-ttu-id="ae818-161">Bu öznitelik, geri gönderme ve geri çağırma olayları doğrulanır olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae818-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="ae818-162">Bu etkinleştirildiğinde, geri gönderme bağımsız değişkenler veya geri çağırma olayları, ilk işlenen sunucu denetiminden kaynaklı olduğunu emin olmak için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="ae818-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="ae818-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="ae818-163">EnableTheming</span></span>

<span data-ttu-id="ae818-164">Bu öznitelik, bir sayfa üzerinde ASP.NET temaları kullanılan olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae818-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="ae818-165">Varsayılan değer **false**.</span><span class="sxs-lookup"><span data-stu-id="ae818-165">The default is **false**.</span></span> <span data-ttu-id="ae818-166">ASP.NET temaları kapsamdaki [modülü 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="ae818-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="ae818-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="ae818-167">LinePragmas</span></span>

<span data-ttu-id="ae818-168">Bu öznitelik, derleme sırasında satır pragmaları eklenmesi gerekip gerekmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae818-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="ae818-169">Satırı pragmaları kodun belirli bölümlerine işaretlemek için hata ayıklayıcıları tarafından kullanılan seçeneklerdir.</span><span class="sxs-lookup"><span data-stu-id="ae818-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="ae818-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="ae818-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="ae818-171">Bu öznitelik, Geri göndermeler arasında kaydırma konumunu korumak için JavaScript sayfalarına eklenmiş olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae818-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="ae818-172">Bu öznitelik **false** varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="ae818-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="ae818-173">Bu öznitelik olduğunda **true**, ASP.NET ekleyecek bir &lt;betik&gt; şuna benzer bir geri gönderme blok:</span><span class="sxs-lookup"><span data-stu-id="ae818-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="ae818-174">Bu betik bloğu için src WebResource.axd olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ae818-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="ae818-175">Bu kaynak, fiziksel bir yol değil.</span><span class="sxs-lookup"><span data-stu-id="ae818-175">This resource is not a physical path.</span></span> <span data-ttu-id="ae818-176">Bu betik istendiğinde, ASP.NET dinamik olarak betik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ae818-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="ae818-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="ae818-177">MasterPageFile</span></span>

<span data-ttu-id="ae818-178">Bu öznitelik geçerli sayfa için ana sayfa dosyası belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae818-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="ae818-179">Göreli veya mutlak yol olabilir.</span><span class="sxs-lookup"><span data-stu-id="ae818-179">The path can be relative or absolute.</span></span> <span data-ttu-id="ae818-180">Ana sayfalar kapsamdaki [modülü 4](master-pages.md).</span><span class="sxs-lookup"><span data-stu-id="ae818-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="ae818-181">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="ae818-181">StyleSheetTheme</span></span>

<span data-ttu-id="ae818-182">Bu öznitelik, bir ASP.NET 2.0 tema tarafından tanımlanan kullanıcı arabirimi görünüm özellikleri geçersiz kılmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="ae818-183">Temalar kapsamdaki [modülü 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="ae818-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="ae818-184">Tema</span><span class="sxs-lookup"><span data-stu-id="ae818-184">Theme</span></span>

<span data-ttu-id="ae818-185">Sayfa teması belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae818-185">Specifies the theme for the page.</span></span> <span data-ttu-id="ae818-186">Tema özniteliği StyleSheetTheme özniteliği için bir değer belirtilmezse, sayfadaki denetimleri için uygulanan tüm stillerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ae818-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="ae818-187">Başlık</span><span class="sxs-lookup"><span data-stu-id="ae818-187">Title</span></span>

<span data-ttu-id="ae818-188">Sayfanın başlığını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-188">Sets the title for the page.</span></span> <span data-ttu-id="ae818-189">Burada belirtilen değeri görünür &lt;başlık&gt; işlenen sayfanın öğesi.</span><span class="sxs-lookup"><span data-stu-id="ae818-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="ae818-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="ae818-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="ae818-191">ViewStateEncryptionMode numaralandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="ae818-192">Kullanılabilir değerler **her zaman**, **otomatik**, ve **hiçbir zaman**.</span><span class="sxs-lookup"><span data-stu-id="ae818-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="ae818-193">Varsayılan değer **otomatik**. Bu öznitelik değerine ayarlandığında **otomatik**, viewstate şifrelenir denetim istekleri, çağırarak olan **RegisterRequiresViewStateEncryption** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ae818-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="ae818-194">Genel özellik değerleri aracılığıyla @ sayfa yönergesi ayarlama</span><span class="sxs-lookup"><span data-stu-id="ae818-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="ae818-195">ASP.NET 2.0 @ sayfa yönergesi başka bir yeni özellik genel bir temel sınıf özelliklerinin başlangıç değeri ayarlamak da yeteneğidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="ae818-196">Örneğin, ortak bir özellik olan adlı varsayalım **SomeText** temel sınıfınız ve buna benzer d için başlatılması için **Hello** ne zaman bir sayfa yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ae818-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="ae818-197">@ Sayfa yönergesinde yalnızca bir değere ayarlayarak bunu gerçekleştirmenin şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="ae818-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="ae818-198">**SomeText** @ sayfa yönergesi özniteliği için bir temel sınıfta SomeText özelliğinin ilk değeri ayarlar *Merhaba!*.</span><span class="sxs-lookup"><span data-stu-id="ae818-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="ae818-199">Aşağıdaki video @ sayfa yönergesi kullanarak temel bir sınıfta bir genel özelliğinin ilk değeri ayarı bir gözden geçirme ' dir.</span><span class="sxs-lookup"><span data-stu-id="ae818-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="ae818-200">Açık tam ekran görüntü</span><span class="sxs-lookup"><span data-stu-id="ae818-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="ae818-201">Yeni genel özellik sayfası sınıfının</span><span class="sxs-lookup"><span data-stu-id="ae818-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="ae818-202">Aşağıdaki genel özellikleri ASP.NET 2.0 sürümünde yenidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="ae818-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="ae818-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="ae818-204">Sayfa veya denetim için uygulamaya göreli yolunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="ae818-205">Örneğin, konumunda bulunan bir sayfa için http://app/folder/page.aspx, özellik döndürür ~ / folder /.</span><span class="sxs-lookup"><span data-stu-id="ae818-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="ae818-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="ae818-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="ae818-207">Sayfa veya denetim için göreli sanal dizin yolu döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="ae818-208">Örneğin konumunda bulunan bir sayfa için http://app/folder/page.aspx, özellik döndürür ~ / folder/page.aspx.</span><span class="sxs-lookup"><span data-stu-id="ae818-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="ae818-209">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="ae818-209">AsyncTimeout</span></span>

<span data-ttu-id="ae818-210">Alır veya ayarlar sayfası zaman uyumsuz işleme için kullanılan zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ae818-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="ae818-211">(Zaman uyumsuz sayfaları Bu modülün daha sonra ele alınacaktır.)</span><span class="sxs-lookup"><span data-stu-id="ae818-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="ae818-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="ae818-212">ClientQueryString</span></span>

<span data-ttu-id="ae818-213">İstenen URL sorgu dizesi bölümünü döndürür salt okunur özellik.</span><span class="sxs-lookup"><span data-stu-id="ae818-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="ae818-214">Bu değer, URL kodlamalı olur.</span><span class="sxs-lookup"><span data-stu-id="ae818-214">This value is URL encoded.</span></span> <span data-ttu-id="ae818-215">Bu kodu çözülecek HttpServerUtility sınıfının UrlDecode yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae818-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="ae818-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="ae818-216">ClientScript</span></span>

<span data-ttu-id="ae818-217">Bu özellik, istemci tarafı komut dosyası ASP.NETs arabellek yönetmek için kullanılan bir ClientScriptManager nesnesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="ae818-218">(ClientScriptManager sınıfı bu modül içinde ele alınmıştır.)</span><span class="sxs-lookup"><span data-stu-id="ae818-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="ae818-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="ae818-219">EnableEventValidation</span></span>

<span data-ttu-id="ae818-220">Bu özellik, olay doğrulama geri gönderme ve geri çağırma olayları için etkin olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="ae818-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="ae818-221">Etkin olduğunda, bağımsız geri gönderme veya geri çağırma olayları, ilk işlenen sunucu denetiminden kaynaklı olduğunu emin olmak için doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="ae818-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="ae818-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="ae818-222">EnableTheming</span></span>

<span data-ttu-id="ae818-223">Bu özelliği alır veya ASP.NET 2.0 tema sayfasına uygulanıp uygulanmayacağını belirten Boolean bir değer ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="ae818-224">Form</span><span class="sxs-lookup"><span data-stu-id="ae818-224">Form</span></span>

<span data-ttu-id="ae818-225">Bu özellik, HTML form ASPX sayfa üzerinde bir HtmlForm nesne olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="ae818-226">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="ae818-226">Header</span></span>

<span data-ttu-id="ae818-227">Bu özellik, sayfa üstbilgisindeki içeren bir HtmlHead nesnesine bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="ae818-228">Döndürülen HtmlHead nesne get/set için stil sayfaları, Meta etiketler kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae818-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="ae818-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="ae818-229">IdSeparator</span></span>

<span data-ttu-id="ae818-230">Bu salt okunur özelliği, ASP.NET bir sayfadaki denetimleri için benzersiz bir kimlik oluştururken denetim tanımlayıcıları ayırmak için kullanılan karakteri alır.</span><span class="sxs-lookup"><span data-stu-id="ae818-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="ae818-231">Doğrudan kodunuzdan kullanılmaya yönelik değildir.</span><span class="sxs-lookup"><span data-stu-id="ae818-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="ae818-232">Isasync</span><span class="sxs-lookup"><span data-stu-id="ae818-232">IsAsync</span></span>

<span data-ttu-id="ae818-233">Bu özellik için zaman uyumsuz sayfalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="ae818-234">Zaman uyumsuz sayfalar, bu modül içinde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae818-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="ae818-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="ae818-235">IsCallback</span></span>

<span data-ttu-id="ae818-236">Bu salt okunur özellik döndürür **true** sayfa geri arama sonucunu ise.</span><span class="sxs-lookup"><span data-stu-id="ae818-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="ae818-237">Geri aramalar, daha sonra bu modülde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae818-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="ae818-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="ae818-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="ae818-239">Bu salt okunur özellik döndürür **true** sayfa çapraz sayfa geri gönderme parçası olduğunda.</span><span class="sxs-lookup"><span data-stu-id="ae818-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="ae818-240">Çapraz sayfa Geri göndermeler, daha sonra bu modülde ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ae818-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="ae818-241">Öğeler</span><span class="sxs-lookup"><span data-stu-id="ae818-241">Items</span></span>

<span data-ttu-id="ae818-242">Sayfaları bağlam içinde depolanmış tüm nesneler içeren bir IDictionary örneğe bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="ae818-243">Bu IDictionary nesneye öğeleri ekleyebilir ve kullanabileceğiniz bağlamı kullanım ömrü boyunca olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ae818-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="ae818-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="ae818-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="ae818-245">Bu özellik, ASP.NET bir geri gönderme tamamlandıktan sonra konumu tarayıcıda sayfaların kaydırma tutar JavaScript olup olmadığını yayan denetler.</span><span class="sxs-lookup"><span data-stu-id="ae818-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="ae818-246">(Bu özellik ayrıntılarını Bu modülün daha önce bahsedilen.)</span><span class="sxs-lookup"><span data-stu-id="ae818-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="ae818-247">Ana</span><span class="sxs-lookup"><span data-stu-id="ae818-247">Master</span></span>

<span data-ttu-id="ae818-248">Bu salt okunur bir özellik için bir ana sayfa uygulanmış bir sayfa AnaSayfa örneğe bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="ae818-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="ae818-249">MasterPageFile</span></span>

<span data-ttu-id="ae818-250">Alır veya ayarlar sayfası için ana sayfanın dosya adı.</span><span class="sxs-lookup"><span data-stu-id="ae818-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="ae818-251">Bu özellik yalnızca PreInit yöntemi ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae818-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="ae818-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="ae818-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="ae818-253">Bu özelliği alır veya en fazla uzunluk sayfaları durumu için bayt cinsinden ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="ae818-254">Özelliği için pozitif bir sayı ayarlanırsa, belirtilen bayt sayısını aşmadığından emin sayfaları görünüm durumu birden çok gizli alanlarına bölünmesi.</span><span class="sxs-lookup"><span data-stu-id="ae818-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="ae818-255">Özelliği negatif bir sayı ise, Görünüm durumu parçalara bozuk değil.</span><span class="sxs-lookup"><span data-stu-id="ae818-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="ae818-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="ae818-256">PageAdapter</span></span>

<span data-ttu-id="ae818-257">Sayfa için isteyen tarayıcıya değiştirir PageAdapter nesnesine bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="ae818-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="ae818-258">PreviousPage</span></span>

<span data-ttu-id="ae818-259">Bir Server.Transfer veya çapraz sayfa geri gönderme durumlarda önceki sayfaya bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="ae818-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="ae818-260">SkinID</span></span>

<span data-ttu-id="ae818-261">Sayfaya uygulamak için ASP.NET 2.0 dış belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae818-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="ae818-262">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="ae818-262">StyleSheetTheme</span></span>

<span data-ttu-id="ae818-263">Bu özelliği alır veya ayarlar bir sayfaya uygulanan stil sayfası.</span><span class="sxs-lookup"><span data-stu-id="ae818-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="ae818-264">TemplateControl</span><span class="sxs-lookup"><span data-stu-id="ae818-264">TemplateControl</span></span>

<span data-ttu-id="ae818-265">İçeren denetim sayfası için bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="ae818-266">Tema</span><span class="sxs-lookup"><span data-stu-id="ae818-266">Theme</span></span>

<span data-ttu-id="ae818-267">Alır veya ayarlar sayfasına ASP.NET 2.0 tema adı.</span><span class="sxs-lookup"><span data-stu-id="ae818-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="ae818-268">Bu değer PreInit yöntemi önce ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ae818-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="ae818-269">Başlık</span><span class="sxs-lookup"><span data-stu-id="ae818-269">Title</span></span>

<span data-ttu-id="ae818-270">Bu özelliği alır veya sayfaları üst bilgisinden alınan sayfanın başlığını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="ae818-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="ae818-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="ae818-272">Alır veya ayarlar sayfasının ViewStateEncryptionMode.</span><span class="sxs-lookup"><span data-stu-id="ae818-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="ae818-273">Bu özelliğin bu modüldeki ayrıntılı bir tartışma bakın.</span><span class="sxs-lookup"><span data-stu-id="ae818-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="ae818-274">Sayfa sınıfının yeni bir korumalı Özellikler</span><span class="sxs-lookup"><span data-stu-id="ae818-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="ae818-275">ASP.NET 2.0 sayfa sınıfının yeni bir korumalı özellikleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ae818-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="ae818-276">Bağdaştırıcı</span><span class="sxs-lookup"><span data-stu-id="ae818-276">Adapter</span></span>

<span data-ttu-id="ae818-277">İstendiğinde, cihazdaki sayfaya işler ControlAdapter başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="ae818-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="ae818-278">AsyncMode</span></span>

<span data-ttu-id="ae818-279">Bu özellik sayfası zaman uyumsuz olarak işlenen olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="ae818-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="ae818-280">Çalışma zamanı tarafından ve doğrudan kod, kullanım içindir.</span><span class="sxs-lookup"><span data-stu-id="ae818-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="ae818-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="ae818-281">ClientIDSeparator</span></span>

<span data-ttu-id="ae818-282">Bu özellik, benzersiz istemci denetimleri için kimlikleri oluştururken bir ayırıcı olarak kullanılan karakter döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="ae818-283">Çalışma zamanı tarafından ve doğrudan kod, kullanım içindir.</span><span class="sxs-lookup"><span data-stu-id="ae818-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="ae818-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="ae818-284">PageStatePersister</span></span>

<span data-ttu-id="ae818-285">Bu özellik sayfası için PageStatePersister nesnesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="ae818-286">Bu özellik, öncelikli olarak ASP.NET denetimi geliştiriciler tarafından yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae818-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="ae818-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="ae818-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="ae818-288">Bu özellik, tarayıcılar önbelleğe alma için dosya yolu eklenen benzersiz bir suffic döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="ae818-289">Varsayılan değer \_ \_ufps = ve 6 basamaklı bir sayı.</span><span class="sxs-lookup"><span data-stu-id="ae818-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="ae818-290">Sayfası sınıfı için yeni genel yöntemler</span><span class="sxs-lookup"><span data-stu-id="ae818-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="ae818-291">Aşağıdaki genel yöntemleri, ASP.NET 2.0 sayfa sınıfında yenidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="ae818-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="ae818-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="ae818-293">Bu yöntem, zaman uyumsuz sayfa yürütme için olay işleyici temsilcilerini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ae818-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="ae818-294">Zaman uyumsuz sayfalar, bu modül içinde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae818-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="ae818-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="ae818-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="ae818-296">Sayfaları stil sayfası özellikler sayfasına geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="ae818-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="ae818-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="ae818-298">Bu yöntem canlı bir zaman uyumsuz görev.</span><span class="sxs-lookup"><span data-stu-id="ae818-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="ae818-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="ae818-299">GetValidators</span></span>

<span data-ttu-id="ae818-300">Belirtilmezse, belirtilen doğrulama veya varsayılan doğrulama grubunun yönelik Doğrulayıcıların bir koleksiyonunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="ae818-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="ae818-301">RegisterAsyncTask</span></span>

<span data-ttu-id="ae818-302">Bu yöntem, yeni bir zaman uyumsuz görev kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ae818-302">This method registers a new async task.</span></span> <span data-ttu-id="ae818-303">Bu modül zaman uyumsuz sayfalar ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ae818-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="ae818-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="ae818-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="ae818-305">Bu yöntem ASP.NET sayfaları denetim durumu kalıcı olduğunu bildirir.</span><span class="sxs-lookup"><span data-stu-id="ae818-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="ae818-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="ae818-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="ae818-307">Bu yöntem, ASP.NET sayfaları viewstate şifreleme gerektiren söyler.</span><span class="sxs-lookup"><span data-stu-id="ae818-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="ae818-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="ae818-308">ResolveClientUrl</span></span>

<span data-ttu-id="ae818-309">İstemci istekleri görüntüler vb. için kullanılabilecek bir göreli URL döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="ae818-310">SetFocus</span><span class="sxs-lookup"><span data-stu-id="ae818-310">SetFocus</span></span>

<span data-ttu-id="ae818-311">Bu yöntem odağı sayfa ilk yüklendiğinde belirtilen denetimi için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="ae818-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="ae818-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="ae818-313">Bu yöntem artık denetim durumu kalıcılığını gerektiren olarak geçirilen denetim kaldırır.</span><span class="sxs-lookup"><span data-stu-id="ae818-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="ae818-314">Sayfa yaşam döngüsü değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="ae818-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="ae818-315">ASP.NET 2.0 sayfa yaşam döngüsü önemli ölçüde değişmediğinden, ancak farkında olmanız gereken bazı yeni yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="ae818-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="ae818-316">ASP.NET 2.0 sayfa yaşam döngüsü aşağıda ana hatlarıyla açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ae818-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="ae818-317">PreInit (yeni, ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="ae818-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="ae818-318">En erken bir geliştirici erişip yaşam döngüsü aşamasında PreInit olayıdır.</span><span class="sxs-lookup"><span data-stu-id="ae818-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="ae818-319">Bu olay eklenmesi, programlı olarak ASP.NET 2.0 temalar, ana sayfalar değiştirmek için bir ASP.NET 2.0 profili, vb. için özelliklere erişmek mümkün kılar. Önemli Viewstate henüz yaşam döngüsü bu noktada denetimlere uygulandığını değil hayata geçirmek bir geri gönderme durumda olduğunda.</span><span class="sxs-lookup"><span data-stu-id="ae818-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="ae818-320">Bir geliştirici bu aşamada bir denetimin bir özelliğine değişirse, bu nedenle, büyük olasılıkla daha sonra sayfa yaşam döngüsünde üzerine yazılacak.</span><span class="sxs-lookup"><span data-stu-id="ae818-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="ae818-321">Init</span><span class="sxs-lookup"><span data-stu-id="ae818-321">Init</span></span>

<span data-ttu-id="ae818-322">ASP.NET tarafından Init olay değişmedi 1.x.</span><span class="sxs-lookup"><span data-stu-id="ae818-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="ae818-323">Okuma veya sayfanızda denetimlerin özelliklerini açma başlatmak istediğiniz budur.</span><span class="sxs-lookup"><span data-stu-id="ae818-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="ae818-324">Bu aşama, ana sayfalar, temalar vb. zaten sayfasına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ae818-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="ae818-325">InitComplete (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="ae818-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="ae818-326">InitComplete olayı sayfaları başlatma aşamasının sonunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ae818-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="ae818-327">Bu noktada yaşam döngüsünde sayfadaki denetimleri erişebilirsiniz, ancak durumlarını değil henüz doldurulmadı.</span><span class="sxs-lookup"><span data-stu-id="ae818-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="ae818-328">PreLoad (2.0 sürümünde yeni)</span><span class="sxs-lookup"><span data-stu-id="ae818-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="ae818-329">Bu olay, tüm geri gönderme veri uyguladıktan sonra ve sayfa hemen önce çağrılır\_yük.</span><span class="sxs-lookup"><span data-stu-id="ae818-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="ae818-330">Yükleme</span><span class="sxs-lookup"><span data-stu-id="ae818-330">Load</span></span>

<span data-ttu-id="ae818-331">Yükleme olayı ASP.NET tarafından değiştirilmemiştir 1.x.</span><span class="sxs-lookup"><span data-stu-id="ae818-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="ae818-332">LoadComplete (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="ae818-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="ae818-333">LoadComplete olay son sayfaları yük aşamasında etkinliğidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="ae818-334">Bu aşamada, tüm geri gönderme ve viewstate verilerini uygulanmış sayfasına.</span><span class="sxs-lookup"><span data-stu-id="ae818-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="ae818-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="ae818-335">PreRender</span></span>

<span data-ttu-id="ae818-336">Görünüm durumu sayfaya eklenen dinamik olarak denetimleri için düzgün şekilde saklanması için isterseniz PreRender olayını bunları eklemek için son şansınızdır.</span><span class="sxs-lookup"><span data-stu-id="ae818-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="ae818-337">PreRenderComplete (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="ae818-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="ae818-338">PreRenderComplete aşamada, tüm denetimler sayfasına eklenen ve sayfa işlenmek üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="ae818-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="ae818-339">Sayfaları görünüm durumu kaydedilmeden önce tetiklenen son olayı PreRenderComplete olayıdır.</span><span class="sxs-lookup"><span data-stu-id="ae818-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="ae818-340">SaveStateComplete (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="ae818-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="ae818-341">SaveStateComplete olayı, hemen tüm sayfa viewstate ve denetim durumu kaydedildikten sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ae818-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="ae818-342">Sayfa tarayıcıya gerçekten işlenmeden önce son olay budur.</span><span class="sxs-lookup"><span data-stu-id="ae818-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="ae818-343">İşleme</span><span class="sxs-lookup"><span data-stu-id="ae818-343">Render</span></span>

<span data-ttu-id="ae818-344">İşleme yöntemi ASP.NET bu yana değişmemiştir 1.x.</span><span class="sxs-lookup"><span data-stu-id="ae818-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="ae818-345">Burada HtmlTextWriter başlatılır ve tarayıcıda görüntülenen sayfa budur.</span><span class="sxs-lookup"><span data-stu-id="ae818-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="ae818-346">ASP.NET 2.0 çapraz sayfa geri gönderme</span><span class="sxs-lookup"><span data-stu-id="ae818-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="ae818-347">ASP.NET'te 1.x, Geri göndermeler aynı sayfaya göndermek için gerekli.</span><span class="sxs-lookup"><span data-stu-id="ae818-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="ae818-348">Çapraz sayfa Geri göndermeler izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="ae818-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="ae818-349">ASP.NET 2.0 IButtonControl arabirimi üzerinden başka bir sayfaya geri gönderilecek özelliği ekler.</span><span class="sxs-lookup"><span data-stu-id="ae818-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="ae818-350">Bu yeni işlevselliği aracılığıyla PostBackUrl öznitelik kullanımı (düğme, LinkButton ve üçüncü taraf özel denetimler yanı sıra ImageButton) yeni IButtonControl arabirimi uygulayan herhangi bir denetimi yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae818-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="ae818-351">Aşağıdaki kod, ikinci bir sayfaya gönderir düğme denetimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ae818-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="ae818-352">Sayfanın geri gönderildiğinde geri göndermeyi başlatan sayfanın PreviousPage özelliğinin ikinci sayfasında aracılığıyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="ae818-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="ae818-353">Bu işlev yeni WebForm uygulanan\_DoPostBackWithOptions istemci-tarafı bir denetimi başka bir sayfaya gönderileri sayfasına ASP.NET 2.0 işleyen bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ae818-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="ae818-354">Bu JavaScript işlevi, istemci betiği yayan yeni WebResource.axd işleyici tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ae818-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="ae818-355">Aşağıdaki video, çapraz sayfa geri gönderme kılavuz olduğunda.</span><span class="sxs-lookup"><span data-stu-id="ae818-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="ae818-356">Açık tam ekran görüntü</span><span class="sxs-lookup"><span data-stu-id="ae818-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="ae818-357">Çapraz sayfa geri gönderme hakkında daha fazla bilgi</span><span class="sxs-lookup"><span data-stu-id="ae818-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="ae818-358">Görünüm durumu</span><span class="sxs-lookup"><span data-stu-id="ae818-358">Viewstate</span></span>

<span data-ttu-id="ae818-359">Kendiniz zaten viewstate için çapraz sayfa geri gönderme senaryosunda ilk sayfadan olabilecekler sorulan.</span><span class="sxs-lookup"><span data-stu-id="ae818-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="ae818-360">Sonuçta IPostBackDataHandler uygulamıyor herhangi bir denetimi durumuna aracılığıyla görünüm durumu, bu nedenle çapraz sayfa geri gönderme ikinci sayfasında bu denetimin özelliklerini erişimi için açık kalır, sayfanın görünüm durumu için erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ae818-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="ae818-361">ASP.NET 2.0 adlı ikinci sayfasında yeni bir gizli alan kullanarak bu senaryonun üstlenir \_ \_PREVIOUSPAGE.</span><span class="sxs-lookup"><span data-stu-id="ae818-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="ae818-362">\_ \_PREVIOUSPAGE form alanı, böylece tüm denetimlerin özelliklerine erişim ikinci sayfada olabilir ilk sayfanın görünüm durumu içerir.</span><span class="sxs-lookup"><span data-stu-id="ae818-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="ae818-363">FindControl atlanarak</span><span class="sxs-lookup"><span data-stu-id="ae818-363">Circumventing FindControl</span></span>

<span data-ttu-id="ae818-364">Çapraz sayfa geri gönderme video kılavuzda miyim FindControl TextBox denetimi ilk sayfasında bir başvuru almak için kullanılan yöntem.</span><span class="sxs-lookup"><span data-stu-id="ae818-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="ae818-365">Bu yöntem, bu amaç için iyi çalışır, ancak FindControl pahalıdır ve bu ek kod yazma gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae818-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="ae818-366">Neyse ki, ASP.NET 2.0 pek çok senaryoda çalışmaz, bu amaçla FindControl bir alternatif sunar.</span><span class="sxs-lookup"><span data-stu-id="ae818-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="ae818-367">PreviousPageType yönergesi TypeName veya VirtualPath özniteliğini kullanarak türü kesin belirlenmiş bir başvuru önceki sayfaya sahip olmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="ae818-368">TypeName öznitelik sanal yolu kullanarak önceki sayfasına başvurmak VirtualPath öznitelik sağlar, ancak önceki sayfaya türünü belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="ae818-369">PreviousPageType yönergesi ayarladıktan sonra ardından açığa çıkarmalıdır denetimler, genel özelliklerini kullanarak erişim vermek istediğiniz vb.</span><span class="sxs-lookup"><span data-stu-id="ae818-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="ae818-370">Laboratuvar 1 arası sayfa geri gönderme</span><span class="sxs-lookup"><span data-stu-id="ae818-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="ae818-371">Bu laboratuvarda, ASP.NET 2.0 yeni çapraz sayfa geri gönderme işlevlerini kullanan bir uygulama oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="ae818-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="ae818-372">Visual Studio 2005'i açın ve yeni bir ASP.NET Web sitesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ae818-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="ae818-373">Page2.aspx adlı yeni bir Web formu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ae818-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="ae818-374">Default.aspx Tasarım Görünümü'nde açın ve bir düğme denetimi ve bir metin kutusu denetimi ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae818-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="ae818-375">Düğme denetimini kimliği vermek **SubmitButton** ve metin kutusu denetim Kimliğini **UserName**.</span><span class="sxs-lookup"><span data-stu-id="ae818-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="ae818-376">Düğmenin PostBackUrl özelliği için page2.aspx ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ae818-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="ae818-377">Page2.aspx Kaynak Görünümü'nde açın.</span><span class="sxs-lookup"><span data-stu-id="ae818-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="ae818-378">@ PreviousPageType yönergesi, aşağıda gösterildiği gibi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ae818-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="ae818-379">Aşağıdaki kod sayfasına ekleme\_page2.aspx'ın arka plan kod yükü:</span><span class="sxs-lookup"><span data-stu-id="ae818-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="ae818-380">Yapı menüsünde derlemeyi tıklayarak projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="ae818-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="ae818-381">Arka plan kod Default.aspx için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ae818-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="ae818-382">Sayfayı Değiştir\_page2.aspx aşağıdaki yükleme:</span><span class="sxs-lookup"><span data-stu-id="ae818-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="ae818-383">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ae818-383">Build the project.</span></span>
11. <span data-ttu-id="ae818-384">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ae818-384">Run the project.</span></span>
12. <span data-ttu-id="ae818-385">Metin kutusunda adınızı girin ve düğmeye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ae818-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="ae818-386">Sonuç nedir?</span><span class="sxs-lookup"><span data-stu-id="ae818-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="ae818-387">ASP.NET 2.0 sürümünde zaman uyumsuz sayfaları</span><span class="sxs-lookup"><span data-stu-id="ae818-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="ae818-388">ASP.NET'te birçok Çekişme sorunları (örneğin, Web hizmeti veya veritabanı çağrıları) dış aramalar, dosya g/ç gecikme gecikmesine neden olur. Bir ASP.NET uygulaması karşı bir istek yapıldığında, ASP.NET birini kendi çalışan iş parçacıkları Bu isteğe hizmet vermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="ae818-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="ae818-389">Bu istek, istek tamamlandıktan ve yanıtı gönderildi kadar iş parçacığı sahip olur.</span><span class="sxs-lookup"><span data-stu-id="ae818-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="ae818-390">ASP.NET 2.0 sayfalarını zaman uyumsuz olarak yürütmek için özellik ekleyerek bu tür sorunları gecikme sorunlarını gidermek çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ae818-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="ae818-391">Bu iş parçacığı isteği başlatın ve ardından başka bir iş parçacığına, böylece kullanılabilir iş parçacığı havuzuna hızla döndüren ek yürütme devre dışı el anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ae818-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="ae818-392">Yeni bir iş parçacığı, dosya GÇ, veritabanı çağrısı, vb. tamamlandı, istek tamamlamak için iş parçacığı havuzundan elde edilir.</span><span class="sxs-lookup"><span data-stu-id="ae818-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="ae818-393">Zaman uyumsuz yürütülen bir sayfa yapmadan ilk adımı ayarlamaktır **zaman uyumsuz** sayfa yönergesi özniteliği şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="ae818-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="ae818-394">ASP.NET sayfası için IHttpAsyncHandler uygulamak için bu öznitelik bildirir.</span><span class="sxs-lookup"><span data-stu-id="ae818-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="ae818-395">Sonraki adımda, sayfanın PreRender önce yaşam döngüsünde bir noktada AddOnPreRenderCompleteAsync yöntemi çağırmaktır.</span><span class="sxs-lookup"><span data-stu-id="ae818-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="ae818-396">(Bu yöntem genellikle sayfasında çağrılır\_yük.) AddOnPreRenderCompleteAsync yöntem iki parametre alır; bir BeginEventHandler ve bir EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="ae818-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="ae818-397">Ardından EndEventHandler için parametre olarak geçirilen bir IAsyncResult BeginEventHandler döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="ae818-398">Aşağıdaki videoyu bir kılavuz bir zaman uyumsuz sayfa isteğinin ' dir.</span><span class="sxs-lookup"><span data-stu-id="ae818-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="ae818-399">Açık tam ekran görüntü</span><span class="sxs-lookup"><span data-stu-id="ae818-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="ae818-400">EndEventHandler tamamlanana kadar zaman uyumsuz bir sayfa tarayıcıya işlemez.</span><span class="sxs-lookup"><span data-stu-id="ae818-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="ae818-401">Hiçbir şüpheli ancak bazı geliştiriciler zaman uyumsuz isteği zaman uyumsuz geri çağırmalar için benzer olarak düşünebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae818-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="ae818-402">Olmadıklarını bilmeniz önemlidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-402">It's important to realize that they are not.</span></span> <span data-ttu-id="ae818-403">Zaman uyumsuz istekler için ilk çalışan iş parçacığının iş parçacığı havuzu hizmet yeni isteklerini, böylece azaltma Çekişme nedeniyle GÇ bağlı olan, vb. için döndürülebilir avantajdır.</span><span class="sxs-lookup"><span data-stu-id="ae818-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="ae818-404">ASP.NET 2.0 betik geri çağırmaları</span><span class="sxs-lookup"><span data-stu-id="ae818-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="ae818-405">Web geliştiricileri, her zaman bir geri çağırma ile ilişkili titremeyi önlemek yollarını attıktan.</span><span class="sxs-lookup"><span data-stu-id="ae818-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="ae818-406">ASP.NET'te 1.x, SmartNavigation titremeyi kaçınmak için en yaygın yöntem olan, ancak istemci uygulaması karmaşıklığı nedeniyle bazı geliştiriciler için SmartNavigation sorunlarına neden.</span><span class="sxs-lookup"><span data-stu-id="ae818-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="ae818-407">ASP.NET 2.0 betik geri çağırmaları ile bu sorunu giderir.</span><span class="sxs-lookup"><span data-stu-id="ae818-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="ae818-408">Betiği geri çağırmaları JavaScript aracılığıyla Web sunucuya yönelik isteklerde XMLHttp kullanır.</span><span class="sxs-lookup"><span data-stu-id="ae818-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="ae818-409">XMLHttp isteği tarayıcının yerli sonra işlenebilir XML verileri döndürür</span><span class="sxs-lookup"><span data-stu-id="ae818-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="ae818-410">XMLHttp kod kullanıcıdan yeni WebResource.axd işleyicisi tarafından gizlenir.</span><span class="sxs-lookup"><span data-stu-id="ae818-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="ae818-411">ASP.NET 2.0 ile bir betik geri çağırma yapılandırmak için gereken birkaç adım vardır.</span><span class="sxs-lookup"><span data-stu-id="ae818-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="ae818-412">1. adım: ICallbackEventHandler arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="ae818-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="ae818-413">Sayfanız bir betik geri katılan olarak tanınması ASP.NET için sırada ICallbackEventHandler arabirimini uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae818-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="ae818-414">Bu, arka plan kod dosyasında yapabileceğiniz şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="ae818-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="ae818-415">Ayrıca @ Implements yönerge benzeri kullanarak bunu yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ae818-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="ae818-416">Genellikle, satır içi ASP.NET kodu kullanırken @ Implements yönergesinin kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae818-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="ae818-417">2. adım: Çağrı GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="ae818-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="ae818-418">Daha önce belirtildiği gibi XMLHttp çağrı WebResource.axd işleyicisinin içinde kapsüllenir.</span><span class="sxs-lookup"><span data-stu-id="ae818-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="ae818-419">ASP.NET WebForm bir çağrı, sayfa işlendiğinde ekleyeceksiniz\_DoCallback, WebResource.axd tarafından sağlanan bir istemci komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="ae818-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="ae818-420">WebForm\_DoCallback işlevi değiştirir \_ \_doPostBack işlevi için bir geri çağırma.</span><span class="sxs-lookup"><span data-stu-id="ae818-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="ae818-421">Unutmayın \_ \_doPostBack program aracılığıyla sayfasındaki formu gönderir.</span><span class="sxs-lookup"><span data-stu-id="ae818-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="ae818-422">Bu nedenle, bir geri gönderme önlemek istediğiniz bir geri çağırma senaryosunda \_ \_doPostBack değil yeterli.</span><span class="sxs-lookup"><span data-stu-id="ae818-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="ae818-423">\_\_doPostBack hala sayfasına istemci betiği geri çağırma senaryosunda işlenir.</span><span class="sxs-lookup"><span data-stu-id="ae818-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="ae818-424">Bununla birlikte, geri çağırma için kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="ae818-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="ae818-425">Bağımsız değişkenleri WebForm\_DoCallback istemci-tarafı işlevi, normalde sayfa içinde denilen GetCallbackEventReference sunucu tarafı işlev aracılığıyla sağlanan\_yük.</span><span class="sxs-lookup"><span data-stu-id="ae818-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="ae818-426">Tipik bir GetCallbackEventReference çağrı şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="ae818-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="ae818-427">Bu durumda, cm ClientScriptManager örneğidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="ae818-428">ClientScriptManager sınıfı Bu modülün daha sonra ele alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="ae818-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="ae818-429">GetCallbackEventReference birden fazla aşırı yüklenmiş sürümleri vardır.</span><span class="sxs-lookup"><span data-stu-id="ae818-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="ae818-430">Bu durumda, bağımsız değişkenleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ae818-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="ae818-431">Burada GetCallbackEventReference Aranan denetim başvuru.</span><span class="sxs-lookup"><span data-stu-id="ae818-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="ae818-432">Bu durumda, sayfa olur.</span><span class="sxs-lookup"><span data-stu-id="ae818-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="ae818-433">İstemci-tarafı koddan sunucu tarafı olaya geçirilen bir dize bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="ae818-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="ae818-434">Bu durumda, anlık ileti bir açılan değerini geçirme ddlCompany çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ae818-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="ae818-435">Dönüş değeri (dize) sunucu tarafı geri çağırma etkinlikten kabul eder istemci tarafı işlevinin adı.</span><span class="sxs-lookup"><span data-stu-id="ae818-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="ae818-436">Bu işlev, yalnızca sunucu tarafı geri çağırma başarılı olduğunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ae818-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="ae818-437">Bu nedenle, sağlamlık açısından, genellikle bir hatanın olayı içerisinde yürütülmek üzere bir istemci-tarafı işlevin adını belirterek ek dize bağımsız değişken GetCallbackEventReference Aşırı yüklenen sürümünü kullanmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="ae818-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="ae818-438">Sunucuya geri çağırma önce başlatılan bir istemci-tarafı işlevi temsil eden bir dize.</span><span class="sxs-lookup"><span data-stu-id="ae818-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="ae818-439">Bu durumda, bağımsız değişken null, bu nedenle böyle herhangi bir komut yoktur.</span><span class="sxs-lookup"><span data-stu-id="ae818-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="ae818-440">Zaman uyumsuz geri çağırma kuralları gerekip gerekmediğini belirten bir Boole değeri.</span><span class="sxs-lookup"><span data-stu-id="ae818-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="ae818-441">WebForm çağrısı\_DoCallback istemcide, bu bağımsız değişkenler geçecek.</span><span class="sxs-lookup"><span data-stu-id="ae818-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="ae818-442">Bu nedenle, bu sayfayı istemcide işlendiğinde bu kodu görüneceğini şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="ae818-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="ae818-443">İstemci üzerinde işlev imzası biraz farklı olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ae818-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="ae818-444">İstemci tarafı işlevi 5 dize ve Boole geçirir.</span><span class="sxs-lookup"><span data-stu-id="ae818-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="ae818-445">Sunucu tarafı geri çağırma tüm hataları işleyecek istemci-tarafı işlevi (Bu, yukarıdaki örnekte null) olan ek dizeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="ae818-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="ae818-446">3. adım: istemci tarafı denetim olayının bağlama</span><span class="sxs-lookup"><span data-stu-id="ae818-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="ae818-447">Yukarıdaki GetCallbackEventReference dönüş değeri dize değişkenine atandığını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ae818-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="ae818-448">Bu dize, geri çağırmayı başlatan denetimi için istemci tarafı bir olay bağlama için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae818-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="ae818-449">Bağlamak istediğiniz şekilde bu örnekte geri çağırma sayfasında, açılan bir menüde başlatılır *OnChange* olay.</span><span class="sxs-lookup"><span data-stu-id="ae818-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="ae818-450">İstemci tarafında olay bağlama için bir işleyici için istemci tarafı biçimlendirme gibi eklemeniz yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="ae818-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="ae818-451">Bu geri çağırma *cbRef* GetCallbackEventReference çağrıdan dönüş değeridir.</span><span class="sxs-lookup"><span data-stu-id="ae818-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="ae818-452">WebForm çağrısı içerdiği\_yukarıda gösterilen DoCallback.</span><span class="sxs-lookup"><span data-stu-id="ae818-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="ae818-453">4. adım: istemci tarafı komut dosyası kaydetme</span><span class="sxs-lookup"><span data-stu-id="ae818-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="ae818-454">İstemci tarafı komut dosyası adında GetCallbackEventReference çağrısı belirtilen geri çağırma **ShowCompanyName** sunucu tarafı geri çağırma işlemi başarılı olduğunda yürütülmesi.</span><span class="sxs-lookup"><span data-stu-id="ae818-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="ae818-455">Bu betik bir ClientScriptManager örneği kullanarak sayfaya eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae818-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="ae818-456">(Daha sonra bu modüldeki dicussed ClientScriptManager sınıfı olacaktır.) Bu benzer şekilde yapın:</span><span class="sxs-lookup"><span data-stu-id="ae818-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="ae818-457">5. adım: ICallbackEventHandler arabirimin yöntemlerini çağırın</span><span class="sxs-lookup"><span data-stu-id="ae818-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="ae818-458">ICallbackEventHandler kodunuzda uygulamak için gereken iki yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="ae818-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="ae818-459">Bunlar **RaiseCallbackEvent** ve **GetCallbackEvent**.</span><span class="sxs-lookup"><span data-stu-id="ae818-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="ae818-460">**RaiseCallbackEvent** bağımsız değişken olarak bir dize alır ve nothing döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="ae818-461">İstemci tarafı çağrısından WebForm geçirilen dize bağımsız değişken\_DoCallback.</span><span class="sxs-lookup"><span data-stu-id="ae818-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="ae818-462">Bu durumda, bu değeri olduğu *değer* ddlCompany adlı açılan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ae818-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="ae818-463">Sunucu tarafı kodunuzu RaiseCallbackEvent yöntemi yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="ae818-464">Örneğin, bir dış kaynağa karşı WebRequest çağırmanıza yapıyor, kod içinde RaiseCallbackEvent yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="ae818-465">**GetCallbackEvent** istemciye geri dönüşü işlemekten sorumludur.</span><span class="sxs-lookup"><span data-stu-id="ae818-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="ae818-466">Hiçbir bağımsız değişkeni alır ve bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="ae818-467">Döndürdüğü dize bağımsız değişken olarak istemci tarafı işleve, bu durumda geçirilir *ShowCompanyName*.</span><span class="sxs-lookup"><span data-stu-id="ae818-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="ae818-468">Yukarıdaki adımları tamamladıktan sonra ASP.NET 2.0 ile bir betik geri çağırma işlemini gerçekleştirmek hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="ae818-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="ae818-469">Açık tam ekran görüntü</span><span class="sxs-lookup"><span data-stu-id="ae818-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="ae818-470">ASP.NET'te betik geri aramaları yapmayı XMLHttp çağrıları destekleyen herhangi bir tarayıcıda desteklenir.</span><span class="sxs-lookup"><span data-stu-id="ae818-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="ae818-471">Tüm modern tarayıcılarda kullanımda bugün içerir.</span><span class="sxs-lookup"><span data-stu-id="ae818-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="ae818-472">Modern tarayıcılar (gelecek IE 7 dahil) bir iç XMLHttp nesnesine kullanırken Internet Explorer XMLHttp ActiveX nesnesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="ae818-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="ae818-473">Program aracılığıyla bir tarayıcı geri çağırmaları destekliyorsa, kullanabileceğiniz belirlemek için **Request.Browser.SupportCallback** özelliği.</span><span class="sxs-lookup"><span data-stu-id="ae818-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="ae818-474">Bu özellik döndüreceği **true** istekte bulunan istemciye betik geri çağırmaları destekliyorsa.</span><span class="sxs-lookup"><span data-stu-id="ae818-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="ae818-475">ASP.NET 2.0 istemci komut dosyası ile çalışma</span><span class="sxs-lookup"><span data-stu-id="ae818-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="ae818-476">İstemci betiklerini ASP.NET 2.0 ClientScriptManager sınıfının kullanımı yönetilir.</span><span class="sxs-lookup"><span data-stu-id="ae818-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="ae818-477">ClientScriptManager sınıfı, bir türü ve adı'nı kullanarak istemci betikleri izler.</span><span class="sxs-lookup"><span data-stu-id="ae818-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="ae818-478">Bu, aynı komut dosyasını program aracılığıyla bir sayfada birden çok kez eklenen öğesinden engeller.</span><span class="sxs-lookup"><span data-stu-id="ae818-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="ae818-479">Bir komut dosyası başarıyla bir sayfada kaydedildikten sonra aynı komut dosyasını kaydetmek için sonraki denemelere yalnızca ikinci kez kaydedilmemiş betikte neden olur.</span><span class="sxs-lookup"><span data-stu-id="ae818-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="ae818-480">Yinelenen betik eklenir ve hiçbir özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="ae818-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="ae818-481">Gereksiz hesaplama önlemek için böylece birden çok kez kaydetmek çalışmayın bir betik zaten kayıtlı olup olmadığını belirlemek için kullanabileceğiniz yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="ae818-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="ae818-482">ClientScriptManager yöntemlerini tüm geçerli ASP.NET geliştiricilerinin aşina olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="ae818-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="ae818-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="ae818-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="ae818-484">Bu yöntem, işlenen sayfanın üst için bir komut dosyası ekler.</span><span class="sxs-lookup"><span data-stu-id="ae818-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="ae818-485">Bu, istemci üzerinde çağrılır açıkça işlevleri eklemek için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="ae818-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="ae818-486">Bu yöntemin aşırı yüklenmiş iki sürümü vardır.</span><span class="sxs-lookup"><span data-stu-id="ae818-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="ae818-487">Üç dört bağımsız değişken, bunlar arasında ortak olan.</span><span class="sxs-lookup"><span data-stu-id="ae818-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="ae818-488">Bunlar:</span><span class="sxs-lookup"><span data-stu-id="ae818-488">They are:</span></span>

`type (string)`

<span data-ttu-id="ae818-489">***Türü*** bağımsız değişken, komut dosyası için bir tür tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="ae818-490">Genel sayfa türü (Bu. kullanmak iyi bir fikir olduğunu GetType()) türü.</span><span class="sxs-lookup"><span data-stu-id="ae818-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="ae818-491">***Anahtar*** komut dosyası için bir kullanıcı tanımlı anahtar olmayan bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="ae818-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="ae818-492">Bu, her komut dosyası için benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ae818-492">This should be unique for each script.</span></span> <span data-ttu-id="ae818-493">Aynı anahtara ve zaten eklenmiş bir komut dosyası türü ile bir komut dosyası eklemeyi denediğinizde, eklenmeyecek.</span><span class="sxs-lookup"><span data-stu-id="ae818-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="ae818-494">***Betik*** eklemek için gerçek betik içeren bir dizedir.</span><span class="sxs-lookup"><span data-stu-id="ae818-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="ae818-495">Komut dosyası oluşturabilir ve sonra atamak için StringBuilder üzerinde ToString() yöntemini kullanmanız için StringBuilder kullanın önerilir ***betik*** bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="ae818-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="ae818-496">Yalnızca üç bağımsız değişkeni alan aşırı yüklenmiş RegisterClientScriptBlock kullanırsanız, kod öğeleri içermelidir (&lt;betik&gt; ve &lt;/SCRIPT&gt;) betiğinizde.</span><span class="sxs-lookup"><span data-stu-id="ae818-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="ae818-497">Dördüncü bir bağımsız değişken RegisterClientScriptBlock aşırı yüklemesini kullanmayı tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae818-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="ae818-498">Dördüncü bağımsız değişken ASP.NET kod öğeleri, eklemelisiniz olup olmadığını belirten Boolean bir değer var.</span><span class="sxs-lookup"><span data-stu-id="ae818-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="ae818-499">Bu bağımsız değişken ise **true**, kodunuzu betik öğeleri açıkça içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="ae818-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="ae818-500">Bir betik zaten kayıtlı olup olmadığını belirlemek için IsClientScriptBlockRegistered yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="ae818-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="ae818-501">Bu, zaten kayıtlı bir betiği yeniden kaydetme girişiminde önlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="ae818-502">RegisterClientScriptInclude (yeni, 2.0)</span><span class="sxs-lookup"><span data-stu-id="ae818-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="ae818-503">RegisterClientScriptInclude etiketi, harici bir komut dosyasına bağlanan bir betik bloğu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ae818-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="ae818-504">Bu iki aşırı yüklemesi vardır.</span><span class="sxs-lookup"><span data-stu-id="ae818-504">It has two overloads.</span></span> <span data-ttu-id="ae818-505">Bir anahtarı ve bir URL alır.</span><span class="sxs-lookup"><span data-stu-id="ae818-505">One takes a key and a URL.</span></span> <span data-ttu-id="ae818-506">İkinci türünü belirterek üçüncü bir bağımsız değişkeni ekler.</span><span class="sxs-lookup"><span data-stu-id="ae818-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="ae818-507">Örneğin, aşağıdaki kod, bir komut dosyası bloğu jsfunctions.js bağlanan scripts klasörü uygulamasının kök dizininde oluşturur:</span><span class="sxs-lookup"><span data-stu-id="ae818-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="ae818-508">Bu kod aşağıdaki kodda işlenen sayfa üretir:</span><span class="sxs-lookup"><span data-stu-id="ae818-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="ae818-509">Betik bloğundaki sayfasının altında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ae818-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="ae818-510">Bir betik zaten kayıtlı olup olmadığını belirlemek için IsClientScriptIncludeRegistered yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="ae818-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="ae818-511">Bu, bir betiği yeniden kaydetme girişiminde önlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae818-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="ae818-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="ae818-512">RegisterStartupScript</span></span>

<span data-ttu-id="ae818-513">RegisterStartupScript yöntem RegisterClientScriptBlock yöntemi olarak aynı bağımsız değişkenleri alır.</span><span class="sxs-lookup"><span data-stu-id="ae818-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="ae818-514">Sayfa yüklendikten sonra ancak önce yüklendiğinde istemci-tarafı olayı RegisterStartupScript ile kayıtlı bir betik yürütür.</span><span class="sxs-lookup"><span data-stu-id="ae818-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="ae818-515">1.X kapatmadan önce RegisterStartupScript ile kayıtlı bir betikleri yerleştirildi &lt;/form&gt; RegisterClientScriptBlock ile kayıtlı bir betikleri açıldıktan hemen sonra yerleştirildi sırada etiketi &lt;formu&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="ae818-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="ae818-516">ASP.NET 2. 0'da, her ikisi de kapatılmadan hemen önce yerleştirilir &lt;/form&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="ae818-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="ae818-517">Bir işlev ile RegisterStartupScript kaydederseniz, bu işlevi açıkça istemci-tarafı kodunda çağırana kadar yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="ae818-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="ae818-518">Bir betik zaten kayıtlı olup olmadığını belirlemek ve bir betik yeniden kaydetme girişiminde önlemek için IsStartupScriptRegistered yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="ae818-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="ae818-519">Diğer ClientScriptManager yöntemleri</span><span class="sxs-lookup"><span data-stu-id="ae818-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="ae818-520">Diğer kullanışlı yöntem ClientScriptManager sınıfın bazıları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ae818-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>


|  <span data-ttu-id="ae818-521"><strong>GetCallbackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="ae818-521"><strong>GetCallbackEventReference</strong></span></span>   |                                                 <span data-ttu-id="ae818-522">Bu modüldeki betik geri çağırmaları bakın.</span><span class="sxs-lookup"><span data-stu-id="ae818-522">See script callbacks earlier in this module.</span></span>                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <span data-ttu-id="ae818-523"><strong>GetPostBackClientHyperlink</strong></span><span class="sxs-lookup"><span data-stu-id="ae818-523"><strong>GetPostBackClientHyperlink</strong></span></span>  |                <span data-ttu-id="ae818-524">JavaScript başvurusu alır (javascript:&lt;çağrı&gt;) bir istemci-tarafı olayından göndermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ae818-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span>                 |
|  <span data-ttu-id="ae818-525"><strong>GetPostBackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="ae818-525"><strong>GetPostBackEventReference</strong></span></span>   |                                   <span data-ttu-id="ae818-526">Bir posta istemcisinden geri başlatmak için kullanılan bir dize alır.</span><span class="sxs-lookup"><span data-stu-id="ae818-526">Gets a string that can be used to initiate a post back from the client.</span></span>                                    |
|      <span data-ttu-id="ae818-527"><strong>GetWebResourceUrl</strong></span><span class="sxs-lookup"><span data-stu-id="ae818-527"><strong>GetWebResourceUrl</strong></span></span>       | <span data-ttu-id="ae818-528">Bir derlemede gömülü bir kaynak için bir URL döndürür.</span><span class="sxs-lookup"><span data-stu-id="ae818-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="ae818-529">İle birlikte kullanılmalıdır <strong>RegisterClientScriptResource</strong>.</span><span class="sxs-lookup"><span data-stu-id="ae818-529">Must be used in conjunction with <strong>RegisterClientScriptResource</strong>.</span></span> |
| <span data-ttu-id="ae818-530"><strong>RegisterClientScriptResource</strong></span><span class="sxs-lookup"><span data-stu-id="ae818-530"><strong>RegisterClientScriptResource</strong></span></span> |     <span data-ttu-id="ae818-531">Bir Web kaynağı ile sayfaya kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ae818-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="ae818-532">Bu yeni WebResource.axd işleyici tarafından işlenir ve bir derlemede gömülü kaynaklardır.</span><span class="sxs-lookup"><span data-stu-id="ae818-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span>      |
|     <span data-ttu-id="ae818-533"><strong>RegisterHiddenField</strong></span><span class="sxs-lookup"><span data-stu-id="ae818-533"><strong>RegisterHiddenField</strong></span></span>      |                                                 <span data-ttu-id="ae818-534">Gizli bir form alanı sayfasıyla kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ae818-534">Registers a hidden form field with the page.</span></span>                                                 |
|  <span data-ttu-id="ae818-535"><strong>RegisterOnSubmitStatement</strong></span><span class="sxs-lookup"><span data-stu-id="ae818-535"><strong>RegisterOnSubmitStatement</strong></span></span>   |                                  <span data-ttu-id="ae818-536">HTML form gönderildiğinde yürüten istemci-tarafı kodunu kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ae818-536">Registers client-side code that executes when the HTML form is submitted.</span></span>                                   |

