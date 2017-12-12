---
uid: whitepapers/request-validation
title: "İstek doğrulama - komut dosyası saldırılarını önleme | Microsoft Docs"
author: rick-anderson
description: "Bu yazı, varsayılan olarak, uygulama kodlanmamış HTML içerik submitt işleme engellenmediği ASP.NET istek doğrulama özelliği açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 61a96b75fdc29bdd1510ed689ee0356ef30e03fc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="b1b62-103">İstek doğrulama - komut dosyası saldırılarını önleme</span><span class="sxs-lookup"><span data-stu-id="b1b62-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="b1b62-104">Bu yazı, burada, varsayılan olarak, uygulama sunucuya gönderilen kodlanmamış HTML içerik işleme engellenmediği ASP.NET istek doğrulama özelliğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="b1b62-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="b1b62-105">Uygulama güvenle HTML verileri işlemek için tasarlanmış, bu istek doğrulama özelliği devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="b1b62-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="b1b62-106">ASP.NET 1.1 ve ASP.NET 2.0 için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b1b62-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="b1b62-107">İstek doğrulama, bir ASP.NET özelliği sürüm 1.1 beri sunucunun içerik içeren beklemediğiniz kodlanmış HTML kabul etmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="b1b62-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="b1b62-108">Bu özellik, yapabildiği istemci komut dosyası kodu veya HTML bilmeden bir sunucuya gönderilen, depolanabilir ve diğer kullanıcılara sunulan bazı komut dosyası ekleme saldırıları önlemeye yardımcı olmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b1b62-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="b1b62-109">Hala tüm giriş verilerini doğrulamak ve HTML kodlama, uygun olduğunda öneririz.</span><span class="sxs-lookup"><span data-stu-id="b1b62-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="b1b62-110">Örneğin, bir kullanıcının e-posta adresi ister ve ardından bu e-posta adresi bir veritabanında depolayan bir Web sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b1b62-110">For example, you create a Web page that requests a user's e-mail address and then stores that e-mail address in a database.</span></span> <span data-ttu-id="b1b62-111">Kullanıcı girerse &lt;BETİK&gt;uyarı komut dosyasından ("Merhaba")&lt;/SCRIPT&gt; verileri sunulduğunda, içeriği düzgün şekilde kodlanmamış, geçerli bir e-posta adresi yerine bu komut dosyası yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="b1b62-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid e-mail address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="b1b62-112">ASP.NET istek doğrulama özelliği bu oluşmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="b1b62-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="b1b62-113">Bu özellik neden yararlıdır</span><span class="sxs-lookup"><span data-stu-id="b1b62-113">Why this feature is useful</span></span>

<span data-ttu-id="b1b62-114">Birçok site basit bir komut dosyası ekleme saldırılarına açıktır farkında değildir.</span><span class="sxs-lookup"><span data-stu-id="b1b62-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="b1b62-115">Bu saldırıların amacı HTML görüntüleyerek site deface veya potansiyel olarak kullanıcı bir korsanın sitesine yönlendirmek için istemci komut dosyası yürütme olup, komut dosyası ekleme saldırıları Web geliştiricileri ile yüklüyorsa gereken bir sorun var.</span><span class="sxs-lookup"><span data-stu-id="b1b62-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="b1b62-116">ASP.NET, ASP veya diğer web geliştirme teknolojilerini kullandıkları olsa bile komut dosyası ekleme saldırıları tüm web geliştiricilerin, ilgili bir sorun değildir.</span><span class="sxs-lookup"><span data-stu-id="b1b62-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="b1b62-117">ASP.NET istek doğrulama özelliği Geliştirici içeriğin izin vermeye karar sürece sunucu tarafından işlenmek üzere kodlanmamış HTML içeriğine izin vermeyerek bu saldırıların proaktif olarak engeller.</span><span class="sxs-lookup"><span data-stu-id="b1b62-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="b1b62-118">Beklenmesi gerekenler: hata sayfası</span><span class="sxs-lookup"><span data-stu-id="b1b62-118">What to expect: Error Page</span></span>

<span data-ttu-id="b1b62-119">Aşağıdaki ekran bazı örnek ASP.NET kodu gösterir:</span><span class="sxs-lookup"><span data-stu-id="b1b62-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="b1b62-120">Bu kod sonuçları metin kutusuna bazı metinler girmenizi sağlayan basit bir sayfa çalışan, düğmesini tıklatın ve etiket denetiminde metni görüntüle:</span><span class="sxs-lookup"><span data-stu-id="b1b62-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="b1b62-121">Ancak, JavaScript, aşağıdaki gibi olan `<script>alert("hello!")</script>` girilmeli ve şu özel durum almak gönderilen için:</span><span class="sxs-lookup"><span data-stu-id="b1b62-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="b1b62-122">Hata iletisi 'değeri algılandı potansiyel olarak tehlikeli olabilecek Request.Form bir' ve tam olarak ne olduğunu ve davranışını değiştirmek nasıl açıklamasında daha fazla ayrıntı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b1b62-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="b1b62-123">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b1b62-123">For example:</span></span>

<span data-ttu-id="b1b62-124">İstek doğrulama potansiyel olarak tehlikeli olabilecek istemci giriş değeri algıladı ve isteğin işlenmesi iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="b1b62-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="b1b62-125">Bu değer siteler arası komut dosyası saldırısı gibi uygulamanızın güvenliğini tehlikeye yönelik bir girişim olduğunu gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="b1b62-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="b1b62-126">Ayarlayarak istek doğrulamayı devre dışı bırakabilirsiniz `validateRequest=false` sayfa yönergesi veya yapılandırma bölümü.</span><span class="sxs-lookup"><span data-stu-id="b1b62-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="b1b62-127">Ancak, uygulamanızın açıkça tüm girişleri bu durumda denetlemenizi önemle tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="b1b62-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="b1b62-128">İstek doğrulama sayfasında devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="b1b62-128">Disabling request validation on a page</span></span>

<span data-ttu-id="b1b62-129">İstek doğrulama ayarlamalısınız sayfasında devre dışı bırakmak için `validateRequest` sayfa yönergesi özniteliği `false`:</span><span class="sxs-lookup"><span data-stu-id="b1b62-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="b1b62-130">İstek doğrulamayı devre dışı bırakıldığında, içeriği bir sayfaya gönderilebilir; Bu, o içeriği sağlamak için sayfa Geliştirici sorumluluğunda düzgün şekilde kodlanmış veya işlenen gösterir.</span><span class="sxs-lookup"><span data-stu-id="b1b62-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="b1b62-131">Uygulamanız için istek doğrulamayı devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="b1b62-131">Disabling request validation for your application</span></span>

<span data-ttu-id="b1b62-132">Uygulamanız için istek doğrulamayı devre dışı bırakmak için değiştirmek veya bir Web.config dosyası, uygulamanız için oluşturmalı ve validateRequest özniteliğini ayarlayın `<pages />` için bölüm `false`:</span><span class="sxs-lookup"><span data-stu-id="b1b62-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="b1b62-133">Sunucunuzdaki tüm uygulamalar için istek doğrulamayı devre dışı bırakmak istiyorsanız, bu değişikliği Machine.config dosyanıza yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b1b62-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="b1b62-134">İstek doğrulamayı devre dışı bırakıldığında, içerik uygulamanıza gönderilebilir; Bu, o içeriği sağlamak için uygulama geliştiricisi sorumluluğunda düzgün şekilde kodlanmış veya işlenen gösterir.</span><span class="sxs-lookup"><span data-stu-id="b1b62-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="b1b62-135">Aşağıdaki kod, istek doğrulamayı devre dışı bırakma değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="b1b62-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="b1b62-136">Şu JavaScript metin kutusuna girdiyseniz şimdi `<script>alert("hello!")</script>` sonuç olur:</span><span class="sxs-lookup"><span data-stu-id="b1b62-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="b1b62-137">Bu, istek doğrulamayı devre dışı sahip olmaması için biz içeriği HTML olarak kodlamak.</span><span class="sxs-lookup"><span data-stu-id="b1b62-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="b1b62-138">HTML olarak nasıl içerik kodlama</span><span class="sxs-lookup"><span data-stu-id="b1b62-138">How to HTML encode content</span></span>

<span data-ttu-id="b1b62-139">İstek doğrulamayı devre dışı bıraktıysanız, gelecekte kullanılmak üzere depolanır HTML olarak kodlanacak içeriğe iyi bir uygulama olur.</span><span class="sxs-lookup"><span data-stu-id="b1b62-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="b1b62-140">HTML kodlaması otomatik olarak yerini alacak herhangi '&lt;'veya'&gt;' (birlikte birkaç diğer simge için) ilgili HTML ile kodlanmış gösterimi.</span><span class="sxs-lookup"><span data-stu-id="b1b62-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="b1b62-141">Örneğin, '&lt;'ile değiştirilir'&amp;lt;' ve '&gt;'ile değiştirilir'&amp;gt;'.</span><span class="sxs-lookup"><span data-stu-id="b1b62-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="b1b62-142">Tarayıcılar görüntülemek için bu özel kodları kullanın '&lt;'veya'&gt;' tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="b1b62-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="b1b62-143">İçeriği kolayca HTML kullanarak sunucu üzerinde kodlu olabilir `Server.HtmlEncode(string)` API.</span><span class="sxs-lookup"><span data-stu-id="b1b62-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="b1b62-144">İçerik de olabilir kolayca HTML-kodunu çözdü, diğer bir deyişle, geri standart HTML kullanmaya geri `Server.HtmlDecode(string)` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b1b62-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="b1b62-145">Bunun sonucunda:</span><span class="sxs-lookup"><span data-stu-id="b1b62-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
