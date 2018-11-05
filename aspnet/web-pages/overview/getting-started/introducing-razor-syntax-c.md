---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: ASP.NET Web programlama Razor söz dizimini (C#) kullanarak giriş | Microsoft Docs
author: tfitzmac
description: Bu bölümde Razor sözdizimini kullanarak ASP.NET Web sayfaları ile programlamaya genel bir bakış sağlar. ASP.NET dinamik web pa çalıştırmak için Microsoft'un teknolojisidir...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: b242bf52bbd63d726e6ce6ab7be01a1b81c5bf1b
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758264"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="d94f4-104">ASP.NET Web programlama Razor söz dizimini (C#) kullanarak giriş</span><span class="sxs-lookup"><span data-stu-id="d94f4-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>
====================
<span data-ttu-id="d94f4-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d94f4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d94f4-106">Bu makalede Razor sözdizimini kullanarak ASP.NET Web sayfaları ile programlamaya genel bir bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="d94f4-107">ASP.NET dinamik web sayfaları web sunucuları üzerinde çalışan için Microsoft'un teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="d94f4-108">C# programlama dilini kullanarak bu makaleler odaklanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="d94f4-109">**Öğrenecekleriniz**:</span><span class="sxs-lookup"><span data-stu-id="d94f4-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="d94f4-110">Programlama ipuçları, Razor sözdizimini kullanan ASP.NET Web sayfaları programlama ile Başlarken üst 8.</span><span class="sxs-lookup"><span data-stu-id="d94f4-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="d94f4-111">Temel programlama kavramlarını ihtiyacınız olacak.</span><span class="sxs-lookup"><span data-stu-id="d94f4-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="d94f4-112">ASP.NET sunucusu kod ve Razor sözdizimi olan tüm hakkında.</span><span class="sxs-lookup"><span data-stu-id="d94f4-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="d94f4-113">Yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="d94f4-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="d94f4-114">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d94f4-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d94f4-115">Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="d94f4-116">Üst 8 programlama ipuçları</span><span class="sxs-lookup"><span data-stu-id="d94f4-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="d94f4-117">Bu bölümde kesinlikle Razor sözdizimini kullanan ASP.NET sunucusu kod yazmaya başladığınızda bilmeniz gereken birkaç ipucu listelenir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="d94f4-118">Razor sözdizimi C# programlama dilini alır ve, ASP.NET Web sayfaları ile en sık kullanılan dildir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="d94f4-119">Ancak, Visual Basic dili ve her şeyi Visual Basic'te de yapabileceğinizi görün Razor söz dizimi de destekler.</span><span class="sxs-lookup"><span data-stu-id="d94f4-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="d94f4-120">Ek ayrıntılar için bkz [Visual Basic Dil ve sözdizimi](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="d94f4-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="d94f4-121">Makalenin sonraki bölümlerinde çoğu bu programlama teknikleri hakkında daha fazla ayrıntı bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="d94f4-122">1. Kod kullanarak bir sayfa ekleyin @ karakteri</span><span class="sxs-lookup"><span data-stu-id="d94f4-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="d94f4-123">`@` Karakter satır içi ifadeler, tek bir deyim blokları ve birden fazla deyim blokları başlatır:</span><span class="sxs-lookup"><span data-stu-id="d94f4-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="d94f4-124">Bir tarayıcıda sayfa çalıştığında bu deyimler göründüğünü budur:</span><span class="sxs-lookup"><span data-stu-id="d94f4-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="d94f4-126">**HTML kodlama**</span><span class="sxs-lookup"><span data-stu-id="d94f4-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="d94f4-127">Kullanarak bir sayfanın içeriğini görüntülediğinizde `@` karakter, önceki örneklerde olduğu gibi çıkış ASP.NET HTML olarak kodlar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="d94f4-128">Bu ayrılmış HTML karakterlerini değiştirir (gibi `<` ve `>` ve `&`) karakterlerini HTML etiketleri veya varlıklar yorumlanmasını yerine bir web sayfasında karakter olarak görüntülenecek etkinleştirme kodları ile.</span><span class="sxs-lookup"><span data-stu-id="d94f4-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="d94f4-129">Sunucu kodunuz çıktısını HTML kodlaması olmadan düzgün görüntülenmeyebilir ve bir sayfa güvenlik risklerine maruz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="d94f4-130">Amacınız etiketlerini işler biçimlendirmesi olarak HTML biçimlendirmesi çıkış olup olmadığını (örneğin `<p></p>` paragrafı veya `<em></em>` metni vurgulamak için), bölümüne bakın [metin birleştirme, işaretleme ve kod blokları içinde kod](#BM_CombiningTextMarkupAndCode) bu makalenin ilerleyen bölümlerinde.</span><span class="sxs-lookup"><span data-stu-id="d94f4-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="d94f4-131">Daha fazla bilgi edinebilirsiniz, HTML kodlaması hakkında [formlarla çalışma](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="d94f4-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="d94f4-132">2. Kod blokları ayraçlarının içine alın</span><span class="sxs-lookup"><span data-stu-id="d94f4-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="d94f4-133">A *kod bloğu* bir veya daha fazla kod deyimlerini içerir ve parantez içine alınır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="d94f4-134">Bir tarayıcıda görüntülenen sonuç:</span><span class="sxs-lookup"><span data-stu-id="d94f4-134">The result displayed in a browser:</span></span>

![Razor Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="d94f4-136">3. Bir blok içinde her kod deyimi noktalı virgül ile bitmelidir</span><span class="sxs-lookup"><span data-stu-id="d94f4-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="d94f4-137">Bir kod bloğunun içine her tam kod deyimi noktalı virgül ile bitmelidir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="d94f4-138">Satır içi ifadeler, noktalı virgül ile sona ermez.</span><span class="sxs-lookup"><span data-stu-id="d94f4-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="d94f4-139">4. Değerleri depolamak için değişkenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="d94f4-139">4. You use variables to store values</span></span>

<span data-ttu-id="d94f4-140">Değerleri depolayabilir bir *değişkeni*dizeler, sayılar ve tarihler, vb. dahil olmak üzere. Bir değişken kullanarak yeni oluşturduğunuz `var` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="d94f4-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="d94f4-141">Değişken değerleri doğrudan bir sayfasını kullanarak içinde ekleyebilirsiniz `@`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="d94f4-142">Bir tarayıcıda görüntülenen sonuç:</span><span class="sxs-lookup"><span data-stu-id="d94f4-142">The result displayed in a browser:</span></span>

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="d94f4-144">5. Değişmez değer dize değerleri çift tırnak içine alın</span><span class="sxs-lookup"><span data-stu-id="d94f4-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="d94f4-145">A *dize* metin olarak kabul edilir karakterden oluşan bir dizidir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="d94f4-146">Bir dizeyi belirtmek için çift tırnak içine alın:</span><span class="sxs-lookup"><span data-stu-id="d94f4-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="d94f4-147">Görüntülemek istediğiniz dize bir ters eğik çizgi karakteri içeriyorsa ( `\` ) veya çift tırnak işareti ( `"` ), kullanan bir *verbatim dize sabit değeri* ile önek `@` işleci.</span><span class="sxs-lookup"><span data-stu-id="d94f4-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="d94f4-148">(C# ' ta, \ karakterin verbatim dize sabit değeri kullanmadığınız sürece özel bir anlamı vardır.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="d94f4-149">Çift tırnak işaretleri eklemek için verbatim dize sabit değeri kullanın ve tırnak işaretleri yineleyin:</span><span class="sxs-lookup"><span data-stu-id="d94f4-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="d94f4-150">Bu örneklerin her ikisi bir sayfasını kullanarak sonucu şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-150">Here's the result of using both of these examples in a page:</span></span>

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="d94f4-152">Dikkat `@` karakter, hem C# ' deki verbatim dizesi değişmez değerleri işaretlemek için hem de ASP.NET sayfaları kodda işaretlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="d94f4-153">6. Kodu büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="d94f4-153">6. Code is case sensitive</span></span>

<span data-ttu-id="d94f4-154">C# anahtar sözcükleri (gibi `var`, `true`, ve `if`) ve değişken adları büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="d94f4-155">Aşağıdaki kod satırlarını iki farklı değişkenleri oluşturma `lastName` ve `LastName.`</span><span class="sxs-lookup"><span data-stu-id="d94f4-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="d94f4-156">Bir değişken olarak bildirirseniz `var lastName = "Smith";` ve sayfanız olarak bu değişkene başvurmak çalışırsanız `@LastName`, bir hata nedeniyle oluşur `LastName` tanınmaz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="d94f4-157">Visual Basic anahtar sözcükleri ve değişkenleri olan *değil* büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="d94f4-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="d94f4-158">7. Kodlamanızı çoğunu nesneleri içerir</span><span class="sxs-lookup"><span data-stu-id="d94f4-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="d94f4-159">Bir *nesne* ile programlayabileceğiniz bir şeyi temsil &#8212; bir sayfa, bir metin kutusu, bir dosya, görüntü, bir web isteği, bir e-posta iletisi, bir müşteri kaydı (veritabanı satır) vb. Nesnelerin özelliklerini tanımlayan özelliği vardır ve, okuma veya değiştirebilirsiniz, &#8212; bir metin kutusu nesnesine sahip bir `Text` özelliği (diğerlerinin arasında) bir istek nesnesi olan bir `Url` özelliğine sahip bir e-posta iletisi bir `From` özelliği ve bir müşteri nesnesi bir `FirstName` özelliği.</span><span class="sxs-lookup"><span data-stu-id="d94f4-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="d94f4-160">Nesneleri yöntemlerle de &quot;fiilleri&quot; yerine getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="d94f4-161">Örnekler, bir dosya nesnesinin `Save` yöntemi, bir görüntü nesnenin `Rotate` yöntemi ve bir e-posta nesnenin `Send` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="d94f4-162">Genellikle ile çalışacaksınız `Request` , metin kutuları (form alanları) değerleri gibi bilgileri verir ne tür bir tarayıcı, sayfa, kullanıcı kimliği, vb. URL'sini istekte sayfasında, nesne. Aşağıdaki örnek özelliklerine erişmek nasıl gösterir `Request` nesne ve nasıl çağrılacağını `MapPath` yöntemi `Request` sayfasının mutlak yolu sunucu üzerinde size nesnesi:</span><span class="sxs-lookup"><span data-stu-id="d94f4-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="d94f4-163">Bir tarayıcıda görüntülenen sonuç:</span><span class="sxs-lookup"><span data-stu-id="d94f4-163">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="d94f4-165">8. Kararları kod yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="d94f4-166">Bir anahtar dinamik web sayfaları ne koşullara göre yapılacağını belirlemek özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="d94f4-167">Bunu yapmak için en yaygın yolu `if` deyimi (ve isteğe bağlı `else` deyimi).</span><span class="sxs-lookup"><span data-stu-id="d94f4-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="d94f4-168">Deyim `if(IsPost)` yazma toplu yoludur `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="d94f4-169">İle birlikte `if` deyimleri, bir yineleme kod bloklarını koşulları test için çeşitli yollar vardır ve bu şekilde, bu makalenin sonraki bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="d94f4-170">Bir tarayıcıda görüntülenen sonuç (tıkladıktan sonra **Gönder**):</span><span class="sxs-lookup"><span data-stu-id="d94f4-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="d94f4-172">HTTP GET ve POST yöntemleri ve IsPost özelliği</span><span class="sxs-lookup"><span data-stu-id="d94f4-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="d94f4-173">Web sayfaları (HTTP) için kullanılan protokol çok sınırlı sayıda sunucuya isteğinde bulunmak için kullanılan yöntemleri (fiilleri) destekler.</span><span class="sxs-lookup"><span data-stu-id="d94f4-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="d94f4-174">İki en yaygın bir okumak için kullanılan GET ve POST, bir sayfa göndermek için kullanılan olanlardır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="d94f4-175">Genel olarak, ilk kez bir kullanıcı bir sayfa istediğinde, GET kullanarak sayfa istenmektedir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="d94f4-176">Kullanıcı, bir formda doldurur ve sonra Gönder düğmesine tıkladığında tarayıcı bu sunucuya bir POST isteği yapar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="d94f4-177">Web programlama, böylece sayfa işleme bildiğiniz bir sayfaya bir GET veya POST olarak istenen olup olmadığını bilmek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="d94f4-178">ASP.NET Web sayfaları'nda kullanabileceğiniz `IsPost` bir GET veya POST isteği olup olmadığını görmek için özellik.</span><span class="sxs-lookup"><span data-stu-id="d94f4-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="d94f4-179">Bir POST isteğiyse `IsPost` özelliği true döndürür ve bir form üzerinde metin kutuları değerlerini okuma gibi şeyler yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="d94f4-180">Birçok örnekler göreceksiniz sayfanın değerine bağlı olarak farklı şekilde işlemek nasıl Göster `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="d94f4-181">Basit bir kod örneğidir</span><span class="sxs-lookup"><span data-stu-id="d94f4-181">A Simple Code Example</span></span>

<span data-ttu-id="d94f4-182">Bu yordam, temel programlama tekniklerini gösteren bir sayfa oluşturma işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="d94f4-183">Örnekte, kullanıcılar bunları ekler ve sonucu görüntüler iki sayıyı girin sağlayan bir sayfa oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d94f4-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="d94f4-184">Düzenleyicinizde, yeni bir dosya oluşturun ve adlandırın *AddNumbers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d94f4-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="d94f4-185">Aşağıdaki kodu ve biçimlendirmeyi sayfasındaki herhangi bir şey değiştirerek sayfasına kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="d94f4-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="d94f4-186">Not yapabileceğiniz bazı işlemler aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="d94f4-187">`@` Karakter sayfasında ilk kod bloğunu başlar ve kendisinden `totalMessage` sayfanın altına katıştırılmış değişkeni.</span><span class="sxs-lookup"><span data-stu-id="d94f4-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="d94f4-188">Sayfanın üst kısmındaki blok parantez içine alınır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="d94f4-189">Üst blok tüm satırları noktalı virgül ile bitmelidir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="d94f4-190">Değişkenleri `total`, `num1`, `num2`, ve `totalMessage` birkaç sayı ve bir dize depolar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="d94f4-191">Atanan değişmez dize değeri `totalMessage` çift tırnak işareti bulunan bir değişkendir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="d94f4-192">Kod zaman büyük/küçük harfe, olduğundan `totalMessage` değişkeni sayfanın altına kullanılıyor, üst değişkeni adını tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="d94f4-193">İfade `num1.AsInt() + num2.AsInt()` nesneleri ve yöntemleri ile çalışma hakkında bilgi verilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="d94f4-194">`AsInt` Yöntemi her bir değişken üzerinde aritmetik işlemleri, bir sayı (tamsayı) bir kullanıcı tarafından girilen bir dize dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d94f4-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="d94f4-195">`<form>` Etiket içeren bir `method="post"` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d94f4-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="d94f4-196">Kullanıcı tıkladığında bu belirten **Ekle**, sayfanın HTTP POST yöntemini kullanarak sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="d94f4-197">Sayfa gönderildiğinde `if(IsPost)` test değerlendirilen koşul true ile kod çalıştırır, sayıları eklemenin sonucunu görüntüleme.</span><span class="sxs-lookup"><span data-stu-id="d94f4-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="d94f4-198">Sayfayı kaydedin ve bir tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d94f4-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="d94f4-199">(Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.) İki tam sayılar girin ve ardından **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="d94f4-201">Temel programlama kavramları</span><span class="sxs-lookup"><span data-stu-id="d94f4-201">Basic Programming Concepts</span></span>

<span data-ttu-id="d94f4-202">Bu makalede, ASP.NET web programlama bir bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="d94f4-203">Büyük kapsamlı bir incelemesini, en sık kullanacağınız programlama kavramları ile yalnızca hızlı bir tura değildir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="d94f4-204">Bu halde bile hemen ASP.NET Web sayfaları ile kullanmaya başlamak için ihtiyacınız olacak her şeyi kapsar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="d94f4-205">Ancak ilk olarak az teknik arka planı.</span><span class="sxs-lookup"><span data-stu-id="d94f4-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="d94f4-206">Razor sözdizimi, sunucu kodu ve ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d94f4-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="d94f4-207">Razor kod sunucu tabanlı bir web sayfasına eklemek için basit bir programlama sözdizimi sözdizimidir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="d94f4-208">Razor sözdizimini kullanan bir web sayfası içeriği iki tür vardır: istemci içeriği ve sunucu kodu.</span><span class="sxs-lookup"><span data-stu-id="d94f4-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="d94f4-209">İstemci içeriktir için kullandığınız web sayfaları'nda Hizmetler: HTML biçimlendirmesi (öğeleri) ve stil CSS gibi bilgileri belki de JavaScript ve düz metin gibi bazı istemci komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="d94f4-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="d94f4-210">Razor sözdizimi, bu istemci içeriği için sunucu kodu eklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="d94f4-211">Tarayıcıya sayfanın göndermeden önce sunucu, kod sayfasında sunucu kodu varsa, ilk olarak çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="d94f4-212">Sunucu üzerinde çalıştırarak kodu istemci içeriği sunucu tabanlı veritabanlarına erişme gibi tek başına kullanarak yapmak için çok daha karmaşık görevleri gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="d94f4-213">En önemlisi, sunucu kodu istemci içeriği dinamik olarak oluşturabileceğiniz &#8212; HTML biçimlendirmeyi ya da diğer içeriğini hareket halindeyken oluşturmak ve tarayıcı sayfası içerebilecek herhangi bir statik HTML birlikte gönderin.</span><span class="sxs-lookup"><span data-stu-id="d94f4-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="d94f4-214">Tarayıcının açısından bakıldığında, sunucu kodunuz tarafından oluşturulan istemci içeriği herhangi bir istemci içerik farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="d94f4-215">Önceden gördüğünüz gibi gerekli sunucu kodu oldukça basittir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="d94f4-216">Razor sözdizimi içeren ASP.NET web sayfaları, bir özel dosya uzantısına sahip (*.cshtml* veya *.vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="d94f4-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="d94f4-217">Sunucu, bu uzantıları tanır, Razor sözdizimi ile işaretlenir ve sonra sayfa tarayıcıya gönderir kodunu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="d94f4-218">ASP.NET nerelerde uygundur?</span><span class="sxs-lookup"><span data-stu-id="d94f4-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="d94f4-219">Razor sözdizimi, bir Microsoft .NET Framework üzerinde sırayla dayalı olarak ASP.NET adlı Microsoft teknolojisini temel alır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="d94f4-220">Neredeyse her türde bilgisayar uygulaması geliştirmek için büyük ve kapsamlı bir programlama çerçevesi Microsoft.NET Framework var.</span><span class="sxs-lookup"><span data-stu-id="d94f4-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="d94f4-221">ASP.NET web uygulamaları oluşturmak için özel olarak tasarlanmıştır .NET Framework'ün parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="d94f4-222">Geliştiriciler, dünyanın en büyük ve en yüksek trafikli Web sitelerinin çoğu oluşturmak için ASP.NET kullandınız.</span><span class="sxs-lookup"><span data-stu-id="d94f4-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="d94f4-223">(Her zaman dosya adı uzantısı görürsünüz *.aspx* URL bir sitedeki bir parçası olarak site ASP.NET kullanılarak yazılmış olduğundan anlarsınız.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="d94f4-224">Razor sözdizimi, ASP.NET, ancak daha kolay, başlangıç ve daha üretken getiren Uzman olup olmadığınızı öğrenmek basitleştirilmiş bir söz dizimi kullanarak tüm güç sağlar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="d94f4-225">Bu sözdizimi kullanımı basit olsa da, ASP.NET ve .NET Framework ailesi ilişkisini sitelerinizi daha karmaşık bir HAL gibi kullanabileceğiniz daha büyük çerçeveleri gücünü olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="d94f4-227">**Sınıfları ve örnekleri**</span><span class="sxs-lookup"><span data-stu-id="d94f4-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="d94f4-228">ASP.NET sunucusu kod sınıflarının fikrini sırayla oluşturulan nesneleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="d94f4-229">Tanımı veya bir nesne için şablon sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="d94f4-230">Örneğin, bir uygulama içerebilir bir `Customer` gereken herhangi bir müşteri nesnesi yöntemleri ve özellikleri tanımlayan sınıf.</span><span class="sxs-lookup"><span data-stu-id="d94f4-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="d94f4-231">Uygulamayı gerçek müşteri bilgileri ile çalışmak gerektiğinde örneği oluşturur (veya *başlatır*) bir müşteri nesnesi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="d94f4-232">Her bir müşteriye, ayrı bir örneğidir `Customer` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d94f4-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="d94f4-233">Her örnek aynı özellikleri ve yöntemleri destekler, ancak her müşteri nesnesi benzersiz olduğundan her örneği için özellik değerlerini genellikle farklı.</span><span class="sxs-lookup"><span data-stu-id="d94f4-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="d94f4-234">Bir müşteri nesnesi `LastName` özelliği, "Smith" olabilir; başka bir müşteri nesnesi `LastName` özelliği, "Jones." olabilir</span><span class="sxs-lookup"><span data-stu-id="d94f4-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="d94f4-235">Benzer şekilde, herhangi tek tek web sitenizde sayfasıdır bir `Page` örneği nesnesini `Page` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d94f4-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="d94f4-236">Bir düğme sayfasında bir `Button` örneği nesnesini `Button` sınıfı ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="d94f4-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="d94f4-237">Her örnek kendi özellikleri vardır, ancak bunların tümü nesnenin sınıf tanımında belirtilen üzerinde temel alır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="d94f4-238">Temel söz dizimi</span><span class="sxs-lookup"><span data-stu-id="d94f4-238">Basic Syntax</span></span>

<span data-ttu-id="d94f4-239">Daha önce bir ASP.NET Web Pages sayfasında nasıl oluşturulacağını ve sunucu kodunu HTML biçimlendirmesi nasıl ekleyebileceğinizi basit bir örneği gördünüz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="d94f4-240">Burada, Razor sözdizimini kullanan ASP.NET sunucusu kod yazmaya ilişkin temel bilgileri öğreneceksiniz &#8212; diğer bir deyişle, programlama dili kuralları.</span><span class="sxs-lookup"><span data-stu-id="d94f4-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="d94f4-241">(Özellikle, C, C++, C#, Visual Basic veya JavaScript kullandıysanız) programlama deneyimli değilseniz ne burada okuma çoğunu tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="d94f4-242">Yalnızca nasıl sunucu kodu işaretlemede eklenir ile kendinizi alıştırın gerekecektir *.cshtml* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="d94f4-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="d94f4-243">Metni İşaretleme ve kod blokları içinde kod birleştirme</span><span class="sxs-lookup"><span data-stu-id="d94f4-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="d94f4-244">Sunucu kod bloklarında genellikle çıkış metin biçimlendirme (veya her) sayfasına istersiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="d94f4-245">Sunucusu kod bloğu kod değil ve, bunun yerine olarak işleneceğini metin içeriyorsa, ASP.NET koddan metni ayırt etmek mümkün olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="d94f4-246">Bunu yapmanın birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-246">There are several ways to do this.</span></span>

- <span data-ttu-id="d94f4-247">Gibi bir HTML öğesindeki metni alın `<p></p>` veya `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="d94f4-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="d94f4-248">HTML öğesi, metin ve ek HTML öğelerinin sunucu kodu ifadeleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="d94f4-249">ASP.NET, HTML etiketi açılışı gördüğünde (örneğin, `<p>`), öğenin dahil her şeyi işler ve içeriği olarak olduğu sunucu kodu ifadeler çözümleme tarayıcıya olan.</span><span class="sxs-lookup"><span data-stu-id="d94f4-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="d94f4-250">Kullanım `@:` işleci veya `<text>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="d94f4-251">`@:` Tek satırlık bir düz metin veya HTML etiketleri eşleşmeyen; içeren içerik çıkarır `<text>` öğesi çıktısını almak için birden fazla satır alır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="d94f4-252">Bu seçenekler, çıkış bir parçası olarak bir HTML öğesini işlemek istemediğiniz zaman yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="d94f4-253">Çok satırlı metin veya HTML etiketleri eşleşmeyen çıkışı istiyorsanız, her satırın koyabilirsiniz `@:`, veya satır içine alın bir `<text>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="d94f4-254">Gibi `@:` işleci`<text>` etiketleri metin içeriği tanımlamak için ASP.NET tarafından kullanılan ve sayfa çıktıda hiçbir zaman işlenir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="d94f4-255">İlk örnek, önceki örnekte yineler ancak tek bir çift kullanır `<text>` metin işlemek için etiketler.</span><span class="sxs-lookup"><span data-stu-id="d94f4-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="d94f4-256">İkinci örnekte, `<text>` ve `</text>` etiketlerini bazı uncontained metin ve eşleşmeyen HTML etiketleri olması her biri üç satır içine (`<br />`), yanı sıra sunucu kodu ve eşleşen HTML etiketleri.</span><span class="sxs-lookup"><span data-stu-id="d94f4-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="d94f4-257">Yine de her satırın tek tek önünde `@:` işleci; her iki şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d94f4-258">Ne zaman, çıktı metin bu bölümde gösterildiği &#8212; bir HTML öğesini kullanarak `@:` işleci veya `<text>` öğesi &#8212; ASP.NET olmayan HTML olarak kodlanacak çıktı.</span><span class="sxs-lookup"><span data-stu-id="d94f4-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="d94f4-259">(Daha önce belirtildiği gibi ASP.NET sunucusu kod ifadeleri tarafından öncelenen sunucusu kod bloğu ve çıkış kodlama `@`, bu bölümde belirtilen özel durumlar hariç.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="d94f4-260">Boşluk</span><span class="sxs-lookup"><span data-stu-id="d94f4-260">Whitespace</span></span>

<span data-ttu-id="d94f4-261">Fazladan boşlukları deyimindeki (ve bir dize sabit değeri dışındaki) deyim etkilemez:</span><span class="sxs-lookup"><span data-stu-id="d94f4-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="d94f4-262">Satır sonu deyimindeki ifade üzerinde etkiye sahip değildir ve Okunabilirlik için deyimleri kayabilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="d94f4-263">Aşağıdaki deyimler aynıdır:</span><span class="sxs-lookup"><span data-stu-id="d94f4-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="d94f4-264">Ancak, bir dize sabit değeri ortasında bir satır kaydırma olamaz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="d94f4-265">Aşağıdaki örnek çalışmaz:</span><span class="sxs-lookup"><span data-stu-id="d94f4-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="d94f4-266">Yukarıdaki kodu gibi çoklu satırlara sarar uzun bir dize birleştirmek için iki seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="d94f4-267">Birleştirme işlecini kullanabilirsiniz (`+`), bu makalenin sonraki bölümlerinde görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="d94f4-268">Ayrıca `@` bu makalenin önceki bölümlerinde anlatıldığı gibi aynen bir dize değişmez değeri oluşturmak için kullanılan karakter.</span><span class="sxs-lookup"><span data-stu-id="d94f4-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="d94f4-269">Satırlara verbatim dize değişmez değerleri bölünebilir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="d94f4-270">Kod (ve biçimlendirme) açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d94f4-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="d94f4-271">Açıklama notları kendiniz veya başkaları için terk etmenize imkan.</span><span class="sxs-lookup"><span data-stu-id="d94f4-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="d94f4-272">Devre dışı bırakmak de sağlar (*açıklama*) bir bölümünü başladığınız kodun veya çalıştırmak istemiyorsanız, ancak şimdilik sayfanızın tutmak istiyor.</span><span class="sxs-lookup"><span data-stu-id="d94f4-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="d94f4-273">Farklı söz dizimi HTML biçimlendirmesi ve Razor kod için açıklama yoktur.</span><span class="sxs-lookup"><span data-stu-id="d94f4-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="d94f4-274">Tüm Razor kod gibi Razor açıklama işlenen (ve sonra kaldırılır) sayfa tarayıcıya gönderilmeden önce sunucuda.</span><span class="sxs-lookup"><span data-stu-id="d94f4-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="d94f4-275">Bu nedenle, Razor açıklama ekleme söz dizimi, kod (veya hatta işaretleme) dosyasını düzenleyin, ancak kullanıcıların gördüğü yoksa bile sayfa kaynağında gördüğünüz açıklamaları put olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="d94f4-276">ASP.NET Razor yorumlar için açıklamaya başlamadan `@*` ve ile bitmelidir `*@`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="d94f4-277">Bir satır veya çok satırlı açıklama olabilir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="d94f4-278">Bir kod bloğu içinde bir açıklama aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="d94f4-279">Böylece çalışmaz burada aynı kod bloğu kod satırı ile devre dışı bırakılmışsa:</span><span class="sxs-lookup"><span data-stu-id="d94f4-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="d94f4-280">Bir kod bloğunun içine Razor açıklama sözdizimi kullanılarak alternatif olarak, C# gibi kullandığınız programlama diline yorum söz dizimini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d94f4-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="d94f4-281">C# dilinde tek satırlı yorumlar tarafından öncelenen `//` karakter ve çok satırlı yorumlar ile başlayan `/*` ve ile sona erdi `*/`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="d94f4-282">(Razor yorumlarla gibi C# açıklamaları tarayıcıya işlenmez.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="d94f4-283">Bildiğiniz gibi biçimlendirme için bir HTML açıklaması oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d94f4-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="d94f4-284">HTML Yorumlarını başlayarak `<!--` karakterleri ve sonu `-->`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="d94f4-285">HTML Yorumlarını yalnızca metin aynı sayfada saklamak isteyebilirsiniz ancak işlemek istemiyorsanız da herhangi bir HTML biçimlendirmesi kapsamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="d94f4-286">Bu HTML açıklama etiketleri ve içerdikleri metin tüm içeriği gizler:</span><span class="sxs-lookup"><span data-stu-id="d94f4-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="d94f4-287">Razor açıklama, HTML yorumlarında aksine *olan* sayfasına işlenen ve kullanıcı bunları sayfa kaynağı görüntüleyerek görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="d94f4-288">Razor iç içe geçmiş bloklarını C# üzerinde sınırlamalar uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="d94f4-289">Daha fazla bilgi için [adlı C# değişkenleri ve iç içe geçmiş blokları bozuk kod oluştur](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="d94f4-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="d94f4-290">Değişkenler</span><span class="sxs-lookup"><span data-stu-id="d94f4-290">Variables</span></span>

<span data-ttu-id="d94f4-291">Bir değişken verilerini depolamak için kullandığınız adlandırılmış bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="d94f4-292">Herhangi bir şey değişkenleri ad verebilirsiniz ancak adı, alfabetik bir karakter ile başlamalı ve boşluk ya da ayrılmış karakterler içeremez.</span><span class="sxs-lookup"><span data-stu-id="d94f4-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="d94f4-293">Değişkenleri ve veri türleri</span><span class="sxs-lookup"><span data-stu-id="d94f4-293">Variables and Data Types</span></span>

<span data-ttu-id="d94f4-294">Ne tür veriler bir değişkende depolandığını belirtir bir özel veri türü bir değişken olabilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="d94f4-295">Dize değerleri saklayan string değişkeni olabilir (gibi &quot;Merhaba Dünya&quot;), tam sayı değerleri (örneğin, 3 veya 79) depolayan tamsayı değişkenleri ve çeşitli biçimlerde (4/12/2012 veya 2009 yılı Mart gibi tarih değerlerini depolar tarih değişkenleri ).</span><span class="sxs-lookup"><span data-stu-id="d94f4-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="d94f4-296">Ve kullanabileceğiniz diğer birçok veri türleri vardır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="d94f4-297">Ancak, genellikle bir değişken için bir tür belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d94f4-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="d94f4-298">Çoğu zaman, ASP.NET, out değişkeni içinde verilerin nasıl kullanıldığı temel tür anlayabilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="d94f4-299">(Bazen bir türünü belirtmeniz gerekir; bunun true olduğu örnekler göreceksiniz.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="d94f4-300">Bir değişken kullanarak bildirdiğiniz `var` (bir tür belirtmek istemiyorsanız) anahtar sözcüğü veya türün adını kullanarak:</span><span class="sxs-lookup"><span data-stu-id="d94f4-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="d94f4-301">Aşağıdaki örnek, bir web sayfasındaki bazı tipik kullanımları değişkenlerin gösterir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="d94f4-302">Önceki örneklerde bir sayfa birleştirirseniz, bu bir tarayıcıda görüntülenen bakın:</span><span class="sxs-lookup"><span data-stu-id="d94f4-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="d94f4-304">Dönüştürme ve veri türleri test etme</span><span class="sxs-lookup"><span data-stu-id="d94f4-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="d94f4-305">ASP.NET genellikle bir veri türü otomatik olarak belirleyebilir, ancak bazen bunu dönüştürülemez.</span><span class="sxs-lookup"><span data-stu-id="d94f4-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="d94f4-306">Bu nedenle, ASP.NET açık bir dönüştürme gerçekleştirerek yardımcı olacak birçok gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="d94f4-307">Bazen türleri dönüştürmek yoksa bile, ne tür verilere, ile çalışabilir engellemediğini test etmek için yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="d94f4-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="d94f4-308">En yaygın bir tamsayı veya tarih gibi bir dizeyi başka bir türe dönüştürmek sahip olduğu.</span><span class="sxs-lookup"><span data-stu-id="d94f4-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="d94f4-309">Aşağıdaki örnek, bir sayıyı bir dize burada dönüştürmeniz gerekir tipik bir servis talebi gösterir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="d94f4-310">Bir kural olarak, kullanıcı girişini dize olarak size gelir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="d94f4-311">Bir sayı girin kullanıcılardan ve kullanıcı girişi gönderilir ve kodda okuma zaman bir basamak girdikten bile verileri dize biçiminde bile.</span><span class="sxs-lookup"><span data-stu-id="d94f4-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="d94f4-312">Bu nedenle, dizeyi sayıya dönüştürme gerekir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="d94f4-313">Dönüştürmeden değerleri üzerinde aritmetik işlemleri denerseniz, iki dizeyi ASP.NET eklenemiyor çünkü örnekte, şu hata, sonuçları:</span><span class="sxs-lookup"><span data-stu-id="d94f4-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="d94f4-314">*Örtük olarak 'int', ' string' türü dönüştürülemiyor.*</span><span class="sxs-lookup"><span data-stu-id="d94f4-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="d94f4-315">Tam sayılar değerlerini dönüştürmek için çağrı `AsInt` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="d94f4-316">Dönüştürme başarılı olursa, sayıları ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="d94f4-317">Aşağıdaki tablo bazı yaygın dönüştürme ve test yöntemleri değişkenleri listeler.</span><span class="sxs-lookup"><span data-stu-id="d94f4-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="d94f4-318"><strong>Yöntemi</strong></span><span class="sxs-lookup"><span data-stu-id="d94f4-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-319"><strong>Açıklama</strong></span><span class="sxs-lookup"><span data-stu-id="d94f4-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-320"><strong>Örnek</strong></span><span class="sxs-lookup"><span data-stu-id="d94f4-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-321">("593" gibi) bir tam sayı bir tamsayı olarak temsil eden bir dize dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d94f4-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-322">Gibi bir dize dönüştürür &quot;true&quot; veya &quot;false&quot; Boole türü.</span><span class="sxs-lookup"><span data-stu-id="d94f4-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-323">Gibi ondalık bir değeri içeren bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; bir kayan noktalı sayı.</span><span class="sxs-lookup"><span data-stu-id="d94f4-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-324">Gibi ondalık bir değeri içeren bir dize dönüştürür &quot;1.3&quot; veya &quot;7.439&quot; ondalık bir sayı.</span><span class="sxs-lookup"><span data-stu-id="d94f4-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="d94f4-325">(ASP.NET, bir ondalık kayan noktalı sayıdan daha kesin sayıdır.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-326">ASP.NET için bir tarih ve saat değerini temsil eden bir dize dönüştürür `DateTime` türü.</span><span class="sxs-lookup"><span data-stu-id="d94f4-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-327">Herhangi bir veri türü, bir dizeye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d94f4-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="d94f4-328">İşleçler</span><span class="sxs-lookup"><span data-stu-id="d94f4-328">Operators</span></span>

<span data-ttu-id="d94f4-329">Bir anahtar sözcük veya ne tür bir ifadede gerçekleştirilecek komut ASP karakter işlecidir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="d94f4-330">C# dili (ve bunu temel alan bir Razor sözdizimi) birçok işleçleri destekler, ancak yalnızca kullanmaya başlamak için birkaç tanıması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="d94f4-331">En yaygın işleçleri aşağıdaki tabloda özetlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-331">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
    <span data-ttu-id="d94f4-332"><strong>İşleci</strong></span><span class="sxs-lookup"><span data-stu-id="d94f4-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-333"><strong>Açıklama</strong></span><span class="sxs-lookup"><span data-stu-id="d94f4-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-334"><strong>Örnekler</strong></span><span class="sxs-lookup"><span data-stu-id="d94f4-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-335">Sayısal ifadeler kullanılan matematik işleçleri.</span><span class="sxs-lookup"><span data-stu-id="d94f4-335">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-336">Atama.</span><span class="sxs-lookup"><span data-stu-id="d94f4-336">Assignment.</span></span> <span data-ttu-id="d94f4-337">Deyiminin sağ taraftaki değer sol tarafındaki nesnesi atar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-337">Assigns the value on the right side of a statement to the object on the left side.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-338">Eşitlik.</span><span class="sxs-lookup"><span data-stu-id="d94f4-338">Equality.</span></span> <span data-ttu-id="d94f4-339">Döndürür `true` değerler eşitse.</span><span class="sxs-lookup"><span data-stu-id="d94f4-339">Returns `true` if the values are equal.</span></span> <span data-ttu-id="d94f4-340">(Birbirinden fark `=` işleci ve `==` işleci.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-340">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-341">Eşitsizlik.</span><span class="sxs-lookup"><span data-stu-id="d94f4-341">Inequality.</span></span> <span data-ttu-id="d94f4-342">Döndürür `true` değerler eşit değilse.</span><span class="sxs-lookup"><span data-stu-id="d94f4-342">Returns `true` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-343">Daha az-değerinden, büyük-küçük değerinden-veya-eşittir ve daha fazla-veya-eşittir büyüktür.</span><span class="sxs-lookup"><span data-stu-id="d94f4-343">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-344">Birleştirme dizeleri birleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-344">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="d94f4-345">ASP.NET bu operatörü ve ifade veri türüne göre toplama işleci arasındaki farkı bilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-345">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-346">Ekleme ve 1 (sırasıyla) bir değişkenden gelen çıkarmayı artırma ve azaltma işleçleri.</span><span class="sxs-lookup"><span data-stu-id="d94f4-346">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-347">Nokta.</span><span class="sxs-lookup"><span data-stu-id="d94f4-347">Dot.</span></span> <span data-ttu-id="d94f4-348">Nesneleri ve özellikleri ve yöntemleri ayırt etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-348">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-349">Parantezler.</span><span class="sxs-lookup"><span data-stu-id="d94f4-349">Parentheses.</span></span> <span data-ttu-id="d94f4-350">Grup ifadeleri ve yönteme parametreleri geçirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-350">Used to group expressions and to pass parameters to methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-351">Köşeli ayraç.</span><span class="sxs-lookup"><span data-stu-id="d94f4-351">Brackets.</span></span> <span data-ttu-id="d94f4-352">Değer dizileri veya koleksiyonlardaki erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-352">Used for accessing values in arrays or collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-353">Değil.</span><span class="sxs-lookup"><span data-stu-id="d94f4-353">Not.</span></span> <span data-ttu-id="d94f4-354">Tersine çevirir bir `true` değerini `false` ve bunun tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-354">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="d94f4-355">Test etmek için bir toplu şekilde genellikle kullanılan `false` (diğer bir deyişle, için değil `true`).</span><span class="sxs-lookup"><span data-stu-id="d94f4-355">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
    <span data-ttu-id="d94f4-356">Mantıksal AND ve OR koşulları birlikte hangi bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-356">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="d94f4-357">Dosya ve klasör yollarında kod ile çalışma</span><span class="sxs-lookup"><span data-stu-id="d94f4-357">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="d94f4-358">Genellikle, kodunuzda dosya ve klasör yolları ile çalışırsınız.</span><span class="sxs-lookup"><span data-stu-id="d94f4-358">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="d94f4-359">Geliştirme bilgisayarınızda görünebilir gibi bir Web sitesi için fiziksel klasör yapısı örneği aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="d94f4-359">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="d94f4-360">URL'leri ve yolları hakkında bazı önemli ayrıntılar aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-360">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="d94f4-361">Bir URL ile birlikte bir etki alanı adı başlar (`http://www.example.com`) veya bir sunucu adı (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="d94f4-361">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="d94f4-362">Bir URL bir ana bilgisayarın fiziksel yola karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-362">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="d94f4-363">Örneğin, `http://myserver` klasörüne karşılık gelebilir *C:\websites\mywebsite* sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="d94f4-363">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="d94f4-364">Sanal yol tam yolunu belirtmeniz gerek kalmadan kod yollarında temsil etmek için toplu özelliktir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-364">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="d94f4-365">Bu, etki alanı veya sunucu adı bir URL bölümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-365">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="d94f4-366">Sanal yol kullandığınızda, yolları güncelleştirmek zorunda kalmadan farklı bir etki alanı ya da sunucu kodunuzu taşıyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-366">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="d94f4-367">Farklar anlamanıza yardımcı olması için bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-367">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="d94f4-368">Tam URL</span><span class="sxs-lookup"><span data-stu-id="d94f4-368">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="d94f4-369">Sunucu adı</span><span class="sxs-lookup"><span data-stu-id="d94f4-369">Server name</span></span> | <span data-ttu-id="d94f4-370">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="d94f4-370">*mycompanyserver*</span></span> |
| <span data-ttu-id="d94f4-371">Sanal yol</span><span class="sxs-lookup"><span data-stu-id="d94f4-371">Virtual path</span></span> | <span data-ttu-id="d94f4-372">*/HumanResources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="d94f4-372">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="d94f4-373">Fiziksel yol</span><span class="sxs-lookup"><span data-stu-id="d94f4-373">Physical path</span></span> | <span data-ttu-id="d94f4-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="d94f4-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="d94f4-375">Sanal kök / yalnızca C: aynı kök gibi sürücüdür \.</span><span class="sxs-lookup"><span data-stu-id="d94f4-375">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="d94f4-376">(Sanal klasör yolları her zaman eğik çizgilerin kullanılması.) Sanal yolun bir klasörün, fiziksel klasör olarak aynı ada sahip gerekmez; Bu, bir diğer ad olabilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-376">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="d94f4-377">(Üretim sunucularında sanal yolu nadiren tam bir fiziksel yol ile eşleşir.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-377">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="d94f4-378">Bazen kod bulunan dosya ve klasörleri ile çalışırken, fiziksel yolunu ve bazen çalıştığınız hangi nesneleri bağlı olarak bir sanal yola başvurusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-378">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="d94f4-379">ASP.NET, size bu araçları dosya ve klasör yolları kod ile çalışmak için: `Server.MapPath` yöntemi ve `~` işleci ve `Href` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-379">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="d94f4-380">Sanal-fiziksel yolları dönüştürme: Server.MapPath yöntemi</span><span class="sxs-lookup"><span data-stu-id="d94f4-380">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="d94f4-381">`Server.MapPath` Yöntemi, bir sanal yol dönüştürür (gibi */default.cshtml*) mutlak bir fiziksel yola (gibi *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="d94f4-381">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="d94f4-382">Tam fiziksel yolunu ihtiyacınız dilediğiniz zaman bu yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="d94f4-382">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="d94f4-383">Okunurken ya da bir metin dosyası veya web sunucusunda görüntü dosyası yazma tipik bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-383">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="d94f4-384">Genellikle sitenizi barındırma site sunucuda mutlak fiziksel yolu bilmiyorsanız, bu yöntem, yolun dönüştürebilirsiniz şekilde bildiğiniz — sanal yolu — sunucusu üzerinde karşılık gelen yolu.</span><span class="sxs-lookup"><span data-stu-id="d94f4-384">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="d94f4-385">Sanal yol bir dosya veya klasöre yöntemine geçirmeniz ve fiziksel yolu döndürür:</span><span class="sxs-lookup"><span data-stu-id="d94f4-385">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="d94f4-386">Sanal kök başvuran: ~ işleci ve Href yöntemi</span><span class="sxs-lookup"><span data-stu-id="d94f4-386">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="d94f4-387">İçinde bir *.cshtml* veya *.vbhtml* dosyası, kök sanal yolu kullanarak başvurabilirsiniz `~` işleci.</span><span class="sxs-lookup"><span data-stu-id="d94f4-387">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="d94f4-388">Bu sayfaları bir siteye taşıyabilirsiniz ve içerdikleri diğer sayfalara giden bağlantıları kopmuş olmayacak çünkü çok kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-388">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="d94f4-389">Ayrıca, hiç olmadığı kadar Web sitenizi farklı bir konuma taşımanız durumunda kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-389">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="d94f4-390">Bazı örnekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d94f4-390">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="d94f4-391">Web sitesi ise `http://myserver/myapp`, işte sayfa çalıştığında ASP.NET bu yolların nasıl değerlendirir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-391">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="d94f4-392">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="d94f4-392">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="d94f4-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="d94f4-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="d94f4-394">(Bu yolları aslında değişken değerleri olarak göremezsiniz ancak alacağı ne oldukları olan ASP.NET yolları değerlendirir.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-394">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="d94f4-395">Kullanabileceğiniz `~` işleci hem de sunucu kodu (yukarıdaki gibi) bu gibi biçimlendirme içinde:</span><span class="sxs-lookup"><span data-stu-id="d94f4-395">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="d94f4-396">Biçimlendirme içinde kullandığınız `~` yollara görüntü dosyaları, diğer web sayfaları ve CSS dosyaları gibi kaynakları oluşturmak için işleç.</span><span class="sxs-lookup"><span data-stu-id="d94f4-396">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="d94f4-397">Sayfa çalıştığında, ASP.NET (kod ve biçimlendirme) sayfası görünür ve tüm çözümler `~` başvurularını uygun yolu.</span><span class="sxs-lookup"><span data-stu-id="d94f4-397">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="d94f4-398">Koşullu mantık ve döngüler</span><span class="sxs-lookup"><span data-stu-id="d94f4-398">Conditional Logic and Loops</span></span>

<span data-ttu-id="d94f4-399">ASP.NET sunucusu kod koşullara göre görevleri gerçekleştirmenize ve belirli bir sayıda yineler deyimleri kod yazma sağlar (diğer bir deyişle, bir döngü çalışan kod).</span><span class="sxs-lookup"><span data-stu-id="d94f4-399">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="d94f4-400">Test koşulları</span><span class="sxs-lookup"><span data-stu-id="d94f4-400">Testing Conditions</span></span>

<span data-ttu-id="d94f4-401">Kullandığınız basit bir koşulunu test etmek için `if` true veya false değerini döndürür, belirttiğiniz bir test temel deyimi:</span><span class="sxs-lookup"><span data-stu-id="d94f4-401">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="d94f4-402">`if` Anahtar sözcüğü, bir blok başlatır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-402">The `if` keyword starts a block.</span></span> <span data-ttu-id="d94f4-403">Gerçek testi (durum) parantez içine ve true veya false döndürür.</span><span class="sxs-lookup"><span data-stu-id="d94f4-403">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="d94f4-404">Sınama true olduğunda çalıştırılan deyimleri ayraç içine alınır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-404">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="d94f4-405">Bir `if` deyimi içerebilir bir `else` koşul false ise çalıştırılacak deyimleri belirtir engelle:</span><span class="sxs-lookup"><span data-stu-id="d94f4-405">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="d94f4-406">Kullanarak birden çok koşulu ekleyebileceğiniz bir `else if` engelle:</span><span class="sxs-lookup"><span data-stu-id="d94f4-406">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="d94f4-407">İlk koşul ise, bu örnekte, blok true değil `else if` koşul denetlenir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-407">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="d94f4-408">Bu koşul karşılanıyorsa deyimlerinde `else if` bloğu yürütülür.</span><span class="sxs-lookup"><span data-stu-id="d94f4-408">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="d94f4-409">Hiçbir koşul karşılanıyorsa deyimlerinde `else` bloğu yürütülür.</span><span class="sxs-lookup"><span data-stu-id="d94f4-409">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="d94f4-410">Eğer başka herhangi bir sayıda ekleyebilirsiniz engeller ve ile kapatın bir `else` olarak engelleme &quot;diğer her şey&quot; koşul.</span><span class="sxs-lookup"><span data-stu-id="d94f4-410">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="d94f4-411">Çok sayıda koşulları test etmek için bir `switch` engelle:</span><span class="sxs-lookup"><span data-stu-id="d94f4-411">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="d94f4-412">Test etmek için değeri parantez içinde olduğu (örnekte `weekday` değişkeni).</span><span class="sxs-lookup"><span data-stu-id="d94f4-412">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="d94f4-413">Her bir testi kullanan bir `case` iki nokta (:) ile biten deyimi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-413">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="d94f4-414">Varsa değerini bir `case` ifadesi ile eşleşen test değeri, bu büyük/küçük harf bloğundaki kod yürütülür.</span><span class="sxs-lookup"><span data-stu-id="d94f4-414">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="d94f4-415">Her case deyimiyle kapatmak bir `break` deyimi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-415">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="d94f4-416">(Kesme her içerecek şekilde unutursanız `case` sonraki kod bloğu `case` deyimi de çalışır.) A `switch` blok genellikle sahip bir `default` deyimi için son durum olarak bir &quot;diğer her şey&quot; diğer durumlarda hiçbiri doğruysa çalıştırılan seçeneği.</span><span class="sxs-lookup"><span data-stu-id="d94f4-416">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="d94f4-417">Bir tarayıcıda görüntülenen son iki koşullu blokları sonucu:</span><span class="sxs-lookup"><span data-stu-id="d94f4-417">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="d94f4-419">Döngü kod</span><span class="sxs-lookup"><span data-stu-id="d94f4-419">Looping Code</span></span>

<span data-ttu-id="d94f4-420">Genellikle aynı deyimleri tekrar tekrar çalıştırmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-420">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="d94f4-421">Bunun için döngü.</span><span class="sxs-lookup"><span data-stu-id="d94f4-421">You do this by looping.</span></span> <span data-ttu-id="d94f4-422">Örneğin, genellikle her öğe için aynı deyimleri verilerinin bir koleksiyonunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d94f4-422">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="d94f4-423">Tam döngü istediğiniz kaç kez biliyorsanız, kullanabileceğiniz bir `for` döngü.</span><span class="sxs-lookup"><span data-stu-id="d94f4-423">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="d94f4-424">Bu tür bir döngü sayımı veya sayım için kullanışlıdır:</span><span class="sxs-lookup"><span data-stu-id="d94f4-424">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="d94f4-425">Döngü ile başlayan `for` anahtar sözcüğü, parantez içindeki üç deyimi sonrasında, her sonlandırıldı noktalı virgül ile.</span><span class="sxs-lookup"><span data-stu-id="d94f4-425">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="d94f4-426">İlk deyim parantez içine (`var i=10;`) sayaç oluşturur ve 10'a başlatır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-426">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="d94f4-427">Sayaç adı gerekmez `i` &#8212; herhangi bir değişken kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-427">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="d94f4-428">Zaman `for` döngüsü çalıştırıldığında, sayaç otomatik olarak artırılır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-428">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="d94f4-429">İkinci deyim (`i < 21;`) ne kadar saymak istediğiniz bir koşul ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-429">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="d94f4-430">Bu durumda, en fazla 20 gitmek istediğiniz (sayaç 21'den az olsa da diğer bir deyişle, devam edin).</span><span class="sxs-lookup"><span data-stu-id="d94f4-430">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="d94f4-431">Üçüncü deyim (`i++` ) yalnızca sayaç 1 döngü her çalıştığında olarak eklenmiş olması gerektiğini belirtir. bir artış işleci kullanır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-431">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="d94f4-432">Ayraçlar içinde döngü her yinelemede çalıştıracak kodudur.</span><span class="sxs-lookup"><span data-stu-id="d94f4-432">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="d94f4-433">Yeni bir paragraf biçimlendirme oluşturur (`<p>` öğesi) her zaman ve çıktıyı görüntüleme değeri, bir satır ekler `i` (sayacı).</span><span class="sxs-lookup"><span data-stu-id="d94f4-433">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="d94f4-434">Bu sayfa çalıştırdığınızda, örnek çıktı, her satırda öğe sayısını gösteren bir metin ile görüntüleme 11 satırları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d94f4-434">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="d94f4-436">Bir koleksiyon veya dizi ile çalışıyorsanız, sık kullandığınız bir `foreach` döngü.</span><span class="sxs-lookup"><span data-stu-id="d94f4-436">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="d94f4-437">Bir koleksiyon benzer nesnelerin grubudur ve `foreach` döngü, gerçekleştirdiğiniz koleksiyondaki her öğe üzerinde bir görev sağlar.</span><span class="sxs-lookup"><span data-stu-id="d94f4-437">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="d94f4-438">Bu tür bir döngü koleksiyonlar için uygundur çünkü aksine bir `for` döngü yazmanız gerekmez sayaç artırın veya bir sınır belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d94f4-438">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="d94f4-439">Bunun yerine, `foreach` döngü kodunun işlemi tamamlanana kadar toplulukta yalnızca geçer.</span><span class="sxs-lookup"><span data-stu-id="d94f4-439">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="d94f4-440">Örneğin, aşağıdaki kod öğeleri döndürür `Request.ServerVariables` web sunucunuz hakkında bilgi içeren bir nesne koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="d94f4-440">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="d94f4-441">Bunu kullanan bir `foreac` yeni bir oluşturarak her öğenin adını görüntülemek için h döngü `<li>` HTML madde işaretli liste öğesi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-441">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="d94f4-442">`foreach` Anahtar sözcüğü, koleksiyondaki tek bir öğeyi temsil eden bir değişkeni bildirmek burada parantezle izlendiği (örnekte `var item`) ve ardından `in` döngü koleksiyonu ardından anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="d94f4-442">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="d94f4-443">Gövdesinde `foreach` döngü, daha önce bildirilen değişken kullanarak geçerli öğeye erişebilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-443">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="d94f4-445">Daha genel amaçlı bir döngü oluşturmak için kullanın `while` deyimi:</span><span class="sxs-lookup"><span data-stu-id="d94f4-445">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="d94f4-446">A `while` döngü ile başlayan `while` parantez ile ne kadar süreyle döngünün devam belirttiğiniz anahtar sözcüğü (burada için sürece `countNum` 50'den az), ardından yinelemek için blok.</span><span class="sxs-lookup"><span data-stu-id="d94f4-446">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="d94f4-447">Genellikle döngü Artır (Ekle) veya azaltma (Çıkart) bir değişken veya sayım için kullanılan nesne.</span><span class="sxs-lookup"><span data-stu-id="d94f4-447">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="d94f4-448">Örnekte, `+=` işleci ekler 1 `countNum` döngünün her çalıştığında.</span><span class="sxs-lookup"><span data-stu-id="d94f4-448">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="d94f4-449">(Geriye doğru sayan bir döngü içinde bir değişken azaltma için azaltma işleci kullanırsınız `-=`).</span><span class="sxs-lookup"><span data-stu-id="d94f4-449">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="d94f4-450">Nesneler ve Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="d94f4-450">Objects and Collections</span></span>

<span data-ttu-id="d94f4-451">ASP.NET Web sitesi içinde hemen her şeyi web sayfası dahil olmak üzere, bir nesne var.</span><span class="sxs-lookup"><span data-stu-id="d94f4-451">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="d94f4-452">Bu bölümde, ile sık kodunuzda çalışabilecek önemli bazı nesneler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d94f4-452">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="d94f4-453">Sayfa nesneleri</span><span class="sxs-lookup"><span data-stu-id="d94f4-453">Page Objects</span></span>

<span data-ttu-id="d94f4-454">En temel ASP.NET sayfası nesnedir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-454">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="d94f4-455">Herhangi bir hak kazanan nesnesi olmadan doğrudan sayfası nesnenin özelliklerine erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-455">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="d94f4-456">Aşağıdaki kod sayfanın dosya yolunu alır kullanarak `Request` sayfasının nesne:</span><span class="sxs-lookup"><span data-stu-id="d94f4-456">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="d94f4-457">Bunu yapmak için özellikleri ve yöntemleri geçerli sayfa nesnesine başvuran temizleyin, isteğe bağlı olarak anahtar sözcüğünü kullanabilirsiniz `this` kodunuzda sayfa nesnesi temsil etmek için.</span><span class="sxs-lookup"><span data-stu-id="d94f4-457">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="d94f4-458">İle önceki kod örneğinde, işte `this` sayfayı temsil etmek için eklenir:</span><span class="sxs-lookup"><span data-stu-id="d94f4-458">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="d94f4-459">Özelliklerini kullanabilirsiniz `Page` nesne gibi çok bilgi almak için:</span><span class="sxs-lookup"><span data-stu-id="d94f4-459">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="d94f4-460">`Request`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-460">`Request`.</span></span> <span data-ttu-id="d94f4-461">Bu önceden gördüğünüz gibi ne tür bir tarayıcı yapılan istek URL'sini sayfa, kullanıcı kimliği, vb. dahil olmak üzere, geçerli istek hakkındaki bilgiler koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="d94f4-461">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="d94f4-462">`Response`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-462">`Response`.</span></span> <span data-ttu-id="d94f4-463">Sunucu kodu çalıştırma bittiğinde, tarayıcıya gönderilen yanıt (sayfa) hakkında bilgi koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="d94f4-463">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="d94f4-464">Örneğin, yanıtınıza yazmak için bu özelliği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-464">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="d94f4-465">Koleksiyon nesnelerini (diziler ve sözlükleri)</span><span class="sxs-lookup"><span data-stu-id="d94f4-465">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="d94f4-466">A *koleksiyon* koleksiyonu gibi aynı türden nesneleri grubudur `Customer` veritabanı nesneleri.</span><span class="sxs-lookup"><span data-stu-id="d94f4-466">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="d94f4-467">ASP.NET içeren pek çok yerleşik koleksiyonlar gibi `Request.Files` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="d94f4-467">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="d94f4-468">Genellikle, koleksiyonları verilerle çalışırsınız.</span><span class="sxs-lookup"><span data-stu-id="d94f4-468">You'll often work with data in collections.</span></span> <span data-ttu-id="d94f4-469">İki ortak koleksiyon türü *dizi* ve *sözlük*.</span><span class="sxs-lookup"><span data-stu-id="d94f4-469">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="d94f4-470">Bir dizi benzer öğeleri koleksiyonu depolamak istediğiniz ancak her bir öğe tutmak için ayrı bir değişken oluşturmak istemeyen yararlı olur:</span><span class="sxs-lookup"><span data-stu-id="d94f4-470">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="d94f4-471">Dizilerle, bir özel veri türü gibi bildirdiğiniz `string`, `int`, veya `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-471">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="d94f4-472">Bir dizi değişkeni içerebilir, köşeli ayraçlar için bildirimi ekleme belirtmek için (gibi `string[]` veya `int[]`).</span><span class="sxs-lookup"><span data-stu-id="d94f4-472">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="d94f4-473">Öğeleri konumlarına (dizin) kullanarak bir diziye veya kullanarak erişebileceğiniz `foreach` deyimi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-473">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="d94f4-474">Dizi dizinleri sıfır tabanlı &#8212; diğer bir deyişle, birinci öğenin konumundaki konumlandırın 0, ikinci öğesi konum 1 ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="d94f4-474">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="d94f4-475">Bir dizideki öğe sayısını alarak belirleyebilirsiniz, `Length` özelliği.</span><span class="sxs-lookup"><span data-stu-id="d94f4-475">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="d94f4-476">(Diziyi aramak için) dizisinde belirli bir öğenin konumunu almak için kullanın `Array.IndexOf` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-476">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="d94f4-477">Bir dizinin içeriğini de ters gibi şeyler yapabilirsiniz ( `Array.Reverse` yöntemi) veya içeriği sıralama ( `Array.Sort` yöntemi).</span><span class="sxs-lookup"><span data-stu-id="d94f4-477">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="d94f4-478">Bir tarayıcıda görüntülenen dize dizisi kodun çıktısı:</span><span class="sxs-lookup"><span data-stu-id="d94f4-478">The output of the string array code displayed in a browser:</span></span>

![Razor Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="d94f4-480">Bir anahtar/değer çifti koleksiyonunu ayarlayın veya karşılık gelen değeri almak için anahtar (veya ad) sağlarsınız sözlüktür:</span><span class="sxs-lookup"><span data-stu-id="d94f4-480">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="d94f4-481">Bir sözlüğü oluşturmak için kullandığınız `new` yeni bir sözlük nesnesi oluşturduğunuzu göstermek için anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="d94f4-481">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="d94f4-482">Bir değişken kullanarak bir sözlük atayabilirsiniz `var` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="d94f4-482">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="d94f4-483">Açılı ayraçlar kullanarak sözlükteki öğe veri türlerini belirtin ( `< >` ).</span><span class="sxs-lookup"><span data-stu-id="d94f4-483">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="d94f4-484">Bildirimin sonunda, bu gerçekte yeni bir sözlük oluşturur bir yöntem olduğundan, parantez çifti eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-484">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="d94f4-485">Öğeleri sözlüğüne eklenecek çağırabilirsiniz `Add` sözlük değişkeniyle yöntemi (`myScores` bu durumda) ve ardından bir anahtar ve değer belirtin.</span><span class="sxs-lookup"><span data-stu-id="d94f4-485">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="d94f4-486">Alternatif olarak, köşeli ayraç anahtarı belirtmek ve aşağıdaki örnekte olduğu gibi basit bir atama yapmak için kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d94f4-486">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="d94f4-487">Sözlükten bir değer almak için köşeli ayraç içinde anahtarı belirtin:</span><span class="sxs-lookup"><span data-stu-id="d94f4-487">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="d94f4-488">Parametrelere sahip yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="d94f4-488">Calling Methods with Parameters</span></span>

<span data-ttu-id="d94f4-489">Bu makalede daha önce belirtildiği gibi yöntemler ile program nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-489">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="d94f4-490">Örneğin, bir `Database` nesne içerebilir bir `Database.Connect` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d94f4-490">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="d94f4-491">Bir veya daha fazla parametre birçok yöntem de var.</span><span class="sxs-lookup"><span data-stu-id="d94f4-491">Many methods also have one or more parameters.</span></span> <span data-ttu-id="d94f4-492">A *parametresi* bir yöntem için geçirdiğiniz değerdir, görevini tamamlamak üzere yöntemi etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="d94f4-492">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="d94f4-493">Örneğin, bakmak için bir bildirim `Request.MapPath` üç parametre almayan yöntemi:</span><span class="sxs-lookup"><span data-stu-id="d94f4-493">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="d94f4-494">(Satır daha okunabilir yapmak için sarmalanmış.</span><span class="sxs-lookup"><span data-stu-id="d94f4-494">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="d94f4-495">Satır sonları iç dizeleri dışında neredeyse tüm yerleştirin koyabilirsiniz unutmayın tırnak işareti içine alınmış.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-495">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="d94f4-496">Bu yöntem, belirtilen sanal yolu sunucuda karşılık gelen fiziksel yolu döndürür.</span><span class="sxs-lookup"><span data-stu-id="d94f4-496">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="d94f4-497">Yönteminin üç parametreler `virtualPath`, `baseVirtualDir`, ve `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="d94f4-497">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="d94f4-498">(Bildiriminde kabul verilerin veri türleriyle parametreleri listelendiğine dikkat edin.) Bu yöntemi çağırdığınızda, tüm üç parametrelerin değerlerini belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-498">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="d94f4-499">Razor sözdizimi, bir yönteme parametreleri geçirmek için iki seçenek sunar: *konumsal parametreler* ve *adlandırılmış parametreleri*.</span><span class="sxs-lookup"><span data-stu-id="d94f4-499">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="d94f4-500">Konumsal parametreler kullanan bir yöntem çağırmak için yöntem bildiriminde belirtilen katı bir sırayla parametreleri geçirin.</span><span class="sxs-lookup"><span data-stu-id="d94f4-500">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="d94f4-501">(Genellikle bu sırada yöntemi için belgeleri okuyarak bilmeniz.) Sırayı izlemelidir ve herhangi bir parametre atlayamazsınız &#8212; gerekiyorsa, boş bir dize geçirirseniz (`""`) veya `null` için bir değer yoksa, konumsal bir parametre için.</span><span class="sxs-lookup"><span data-stu-id="d94f4-501">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="d94f4-502">Aşağıdaki örnekte adlı bir klasör olduğunu varsayar *betikleri* kullanarak Web sitenizde.</span><span class="sxs-lookup"><span data-stu-id="d94f4-502">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="d94f4-503">Kod çağrıları `Request.MapPath` doğru sırada üç parametre değerlerini yöntemi ve geçirir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-503">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="d94f4-504">Ardından, elde edilen eşlenen yolun görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d94f4-504">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="d94f4-505">Bir yöntem fazla parametre varsa, kodunuzu daha okunabilir adlandırılmış parametreleri kullanarak tutabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-505">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="d94f4-506">Adlandırılmış parametreler kullanılarak bir yöntem çağırmak için ardından iki nokta üst üste (:), ardından değeri tarafından parametre adı belirtin.</span><span class="sxs-lookup"><span data-stu-id="d94f4-506">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="d94f4-507">Adlandırılmış parametreler avantajı, bunları istediğiniz herhangi bir sırada geçirebilirsiniz ' dir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-507">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="d94f4-508">(Bir dezavantajı yöntem çağrısında kompakt olmasıdır.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-508">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="d94f4-509">Aşağıdaki örnek, yukarıdakilerle aynı yöntemini çağırır, ancak adlı değerlerini sağlamak için parametreleri kullanır:</span><span class="sxs-lookup"><span data-stu-id="d94f4-509">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="d94f4-510">Gördüğünüz gibi farklı parametreler geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-510">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="d94f4-511">Ancak, önceki örnekte ve bu örneği çalıştırırsanız, aynı değeri döneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-511">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="d94f4-512">Hataları işleme</span><span class="sxs-lookup"><span data-stu-id="d94f4-512">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="d94f4-513">Try-Catch deyimleri</span><span class="sxs-lookup"><span data-stu-id="d94f4-513">Try-Catch Statements</span></span>

<span data-ttu-id="d94f4-514">Denetiminiz dışındaki bir nedenle başarısız olabilir, kodunuzda genellikle deyimleri sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="d94f4-514">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="d94f4-515">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d94f4-515">For example:</span></span>

- <span data-ttu-id="d94f4-516">Kodunuzu oluşturmak veya bir dosyaya erişmek çalışırsa, her türlü hata oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-516">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="d94f4-517">İstediğiniz dosyayı var olmayabilir, kilitli olabilir, kod değil izinlere sahip ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="d94f4-517">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="d94f4-518">Benzer şekilde, veritabanındaki kayıtları güncelleştirmek kodunuzu çalışırsa, izin sorunları olabilir, veritabanı bağlantısını bırakılabilir, kaydetmek için verileri geçersiz vb. olabilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-518">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="d94f4-519">Programlama dilinde, bu gibi durumlarda çağrılır *özel durumları*.</span><span class="sxs-lookup"><span data-stu-id="d94f4-519">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="d94f4-520">Kodunuz bir özel durumla karşılaşırsa (oluşturur) oluşturur. hata iletisi o, kullanıcılara en iyi sinir bozucu:</span><span class="sxs-lookup"><span data-stu-id="d94f4-520">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="d94f4-522">Durumlarda burada kodunuzu çalıştırdığınızca özel durumlar ve hata iletileri bu tür önlemek için kullanabileceğiniz `try/catch` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="d94f4-522">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="d94f4-523">İçinde `try` deyimi, denetimi kodu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d94f4-523">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="d94f4-524">Bir veya daha fazla `catch` deyimleri için özel konum (belirli tür özel durumlar) hatalar oluşmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-524">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="d94f4-525">Kadar içerebilir `catch` ifadeleri yazarken öngörerek hatalara bakmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-525">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="d94f4-526">Kullanmaktan kaçının öneririz `Response.Redirect` yönteminde `try/catch` deyimleri, bir özel durum sayfanızın neden olabileceği için.</span><span class="sxs-lookup"><span data-stu-id="d94f4-526">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="d94f4-527">Aşağıdaki örnek ilk isteğe bir metin dosyası oluşturur ve ardından kullanıcının dosyayı açma sağlayan bir düğme görüntüleyen bir sayfa görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-527">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="d94f4-528">Örnek kasıtlı olarak hatalı dosya adı kullanır, böylece bir özel durum neden olur.</span><span class="sxs-lookup"><span data-stu-id="d94f4-528">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="d94f4-529">Kodu içerir `catch` deyimleri için iki olası özel durumları: `FileNotFoundException`, dosya adı hatalı olması durumunda gerçekleşir ve `DirectoryNotFoundException`, ASP.NET bile klasörü bulamıyorsanız gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="d94f4-529">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="d94f4-530">(, Örnek bir deyimde her şeyin düzgün çalıştığından, nasıl çalıştığını görmek için açıklama durumundan çıkarabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="d94f4-530">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="d94f4-531">Kodunuzu özel durumu işlemek istemediğiniz ederseniz, önceki ekran görüntüsündeki gibi bir hata sayfası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="d94f4-531">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="d94f4-532">Ancak, `try/catch` bölüm, kullanıcı bu tür hataları görmemesi yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="d94f4-532">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="d94f4-533">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d94f4-533">Additional Resources</span></span>

<span data-ttu-id="d94f4-534">**Visual Basic ile programlama**</span><span class="sxs-lookup"><span data-stu-id="d94f4-534">**Programming with Visual Basic**</span></span>


[<span data-ttu-id="d94f4-535">Ek: Visual Basic Dil ve sözdizimi</span><span class="sxs-lookup"><span data-stu-id="d94f4-535">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


<span data-ttu-id="d94f4-536">**Başvuru belgeleri**</span><span class="sxs-lookup"><span data-stu-id="d94f4-536">**Reference Documentation**</span></span>


[<span data-ttu-id="d94f4-537">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d94f4-537">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="d94f4-538">C# dili</span><span class="sxs-lookup"><span data-stu-id="d94f4-538">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
