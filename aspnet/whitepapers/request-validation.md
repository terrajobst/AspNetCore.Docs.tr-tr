---
uid: whitepapers/request-validation
title: İstek doğrulama - betik saldırılarını önleme | Microsoft Docs
author: rick-anderson
description: Bu incelemede, ASP.NET'in kodlanmamış HTML içerik submitt işlemesini varsayılan olarak, uygulama, engellenir istek doğrulama özelliği anlatılmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 783fb1ae27d88f9c6d6d3484d26d3e206e7f2fba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372935"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="704bf-103">İstek doğrulama - betik saldırılarını önleme</span><span class="sxs-lookup"><span data-stu-id="704bf-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="704bf-104">Bu incelemede, sunucuya gönderilen kodlanmamış HTML içeriğini işlemesini varsayılan olarak, uygulama, engellenir ASP.NET isteği doğrulama özelliği anlatılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="704bf-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="704bf-105">Uygulama HTML verileri güvenli bir şekilde işlemek için tasarlanmıştır, bu isteği doğrulama özelliği devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="704bf-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="704bf-106">ASP.NET 1.1 ve ASP.NET 2.0 için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="704bf-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="704bf-107">İstek doğrulamanın, ASP.NET 1.1 sürümünden bir özelliği, sunucu içeren içerik beklemediğiniz kodlanmış HTML kabul etmesini önler.</span><span class="sxs-lookup"><span data-stu-id="704bf-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="704bf-108">Bu özellik ile istemci komut dosyası kodu veya HTML farkında olmadan bir sunucuya gönderilen, depolanabilir ve ardından diğer kullanıcılara sunulan bazı betik ekleme saldırıları önlemeye yardımcı olmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="704bf-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="704bf-109">Yine de tüm giriş verilerini doğrulamak ve HTML kodlamasına da uygun olduğunda önerilir.</span><span class="sxs-lookup"><span data-stu-id="704bf-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="704bf-110">Örneğin, bir kullanıcının e-posta adresini ve e-posta adresi bir veritabanında depolar ister bir Web sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="704bf-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="704bf-111">Kullanıcı girerse &lt;BETİK&gt;uyarı komut dosyasından ("hello")&lt;/SCRIPT&gt; verileri sunulduğunda içeriği düzgün şekilde kodlanmamış, geçerli bir e-posta adresi yerine, bu betiği yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="704bf-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="704bf-112">ASP.NET isteği doğrulama özelliği bu gerçekleşmesini önler.</span><span class="sxs-lookup"><span data-stu-id="704bf-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="704bf-113">Bu özellik neden yararlıdır</span><span class="sxs-lookup"><span data-stu-id="704bf-113">Why this feature is useful</span></span>

<span data-ttu-id="704bf-114">Basit betik ekleme saldırıları için açık olduklarından emin birçok sitelerini tanımaz.</span><span class="sxs-lookup"><span data-stu-id="704bf-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="704bf-115">HTML görüntüleyerek site deface veya potansiyel olarak kullanıcıyı bir korsanın siteye yeniden yönlendirmek için istemci betiği yürütmek için bu saldırıların amacı olmasına bakılmaksızın, betik ekleme saldırılarını Web geliştiricileri ile azaltması gereken bir sorun var.</span><span class="sxs-lookup"><span data-stu-id="704bf-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="704bf-116">ASP.NET, ASP veya diğer web geliştirme teknolojilerine kullananın betik ekleme saldırılarını tüm web geliştiricileri, bir sorun var.</span><span class="sxs-lookup"><span data-stu-id="704bf-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="704bf-117">ASP.NET isteği doğrulama özelliği proaktif olarak o içeriğe izin vermek Geliştirici etmediğine karar vermedikçe, sunucu tarafından işlenecek kodlanmamış HTML içeriğini vermeyerek bu saldırıları engeller.</span><span class="sxs-lookup"><span data-stu-id="704bf-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="704bf-118">Beklenecekler: hata sayfası</span><span class="sxs-lookup"><span data-stu-id="704bf-118">What to expect: Error Page</span></span>

<span data-ttu-id="704bf-119">Aşağıdaki ekran görüntüsünde, bazı örnek ASP.NET kodunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="704bf-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="704bf-120">Bu kod sonuçları metin kutusunda metin girmesini sağlayan basit bir sayfa çalışan, düğmeye tıklayın ve etiket denetiminde metin görüntüleme:</span><span class="sxs-lookup"><span data-stu-id="704bf-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="704bf-121">Ancak, JavaScript, aşağıdaki gibi olan `<script>alert("hello!")</script>` girilmeli ve aldığımız bir özel durum gönderilen için:</span><span class="sxs-lookup"><span data-stu-id="704bf-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="704bf-122">Hata iletisi 'değeri algılandı tehlikeli Request.Form bir' ve tam olarak neler olduğunu ve davranışın nasıl değiştirildiğini açıklamasında daha fazla ayrıntı sağlar.</span><span class="sxs-lookup"><span data-stu-id="704bf-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="704bf-123">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="704bf-123">For example:</span></span>

<span data-ttu-id="704bf-124">İstek doğrulamanın tehlikeli istemci giriş değeri algıladı ve isteğin işlenmesi iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="704bf-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="704bf-125">Bu değer, siteler arası betik saldırı gibi uygulamanızın güvenliğini tehlikeye bulunulduğunu gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="704bf-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="704bf-126">Ayarlayarak istek doğrulamayı devre dışı bırakabilirsiniz `validateRequest=false` sayfa yönergesi veya yapılandırma bölümü.</span><span class="sxs-lookup"><span data-stu-id="704bf-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="704bf-127">Ancak, uygulamanız açıkça tüm girişleri bu durumda kontrol etmenizi önemle tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="704bf-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="704bf-128">Sayfasında istek doğrulamayı devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="704bf-128">Disabling request validation on a page</span></span>

<span data-ttu-id="704bf-129">İstek doğrulamanın ayarlamalısınız sayfasında devre dışı bırakmak için `validateRequest` özniteliği için sayfa yönergesi `false`:</span><span class="sxs-lookup"><span data-stu-id="704bf-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="704bf-130">İstek doğrulamayı devre dışı bırakıldığında, içeriği, sayfaya gönderilebilir; Bu, o içeriği sağlamak için sayfayı Geliştirici sorumluluğu düzgün şekilde kodlanmış veya işlenen olur.</span><span class="sxs-lookup"><span data-stu-id="704bf-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="704bf-131">Uygulamanız için istek doğrulamayı devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="704bf-131">Disabling request validation for your application</span></span>

<span data-ttu-id="704bf-132">Uygulamanız için istek doğrulamayı devre dışı bırakmak için değiştirmek veya uygulamanız için bir Web.config dosyası oluşturun ve gerekir validateRequest özniteliğini ayarlayın `<pages />` bölümünü `false`:</span><span class="sxs-lookup"><span data-stu-id="704bf-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="704bf-133">Sunucunuzdaki tüm uygulamalar için istek doğrulamayı devre dışı bırakmak istiyorsanız, bu değişikliği Machine.config dosyasına yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="704bf-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="704bf-134">İstek doğrulamanın devre dışı bırakıldığında, uygulamanıza içerik gönderilebilir; Bu içeriği sağlamak için uygulama geliştiricisinin sorumluluğundadır düzgün şekilde kodlanmış veya işlenen olur.</span><span class="sxs-lookup"><span data-stu-id="704bf-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="704bf-135">Aşağıdaki kod, istek doğrulamayı açmak için değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="704bf-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="704bf-136">Şimdi aşağıdaki JavaScript TextBox'a girdiyseniz `<script>alert("hello!")</script>` sonucu:</span><span class="sxs-lookup"><span data-stu-id="704bf-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="704bf-137">Bu, istek doğrulamayı devre dışı olan önlemek için biz içerik HTML kodlama.</span><span class="sxs-lookup"><span data-stu-id="704bf-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="704bf-138">Nasıl HTML içerik kodlama</span><span class="sxs-lookup"><span data-stu-id="704bf-138">How to HTML encode content</span></span>

<span data-ttu-id="704bf-139">İstek doğrulamanın devre dışı bıraktıysanız, gelecekte kullanılmak üzere saklanacak HTML olarak kodlanacak içeriğe uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="704bf-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="704bf-140">HTML kodlaması otomatik olarak yerini alacak herhangi '&lt;'veya'&gt;' (birlikte, çeşitli diğer semboller için) ilgili HTML ile kodlanmış gösterimi.</span><span class="sxs-lookup"><span data-stu-id="704bf-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="704bf-141">Örneğin, '&lt;'ile değiştirilir'&amp;lt;' ve '&gt;'ile değiştirilir'&amp;gt;'.</span><span class="sxs-lookup"><span data-stu-id="704bf-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="704bf-142">Tarayıcılar görüntülemek için bu özel kodları kullanmak '&lt;'veya'&gt;' tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="704bf-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="704bf-143">İçeriği kolayca HTML kullanılarak sunucuda kodlu olabilir `Server.HtmlEncode(string)` API.</span><span class="sxs-lookup"><span data-stu-id="704bf-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="704bf-144">İçeriği de olabilir kolayca HTML-çözülmüş, diğer bir deyişle, geri standart HTML kullanmaya geri `Server.HtmlDecode(string)` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="704bf-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="704bf-145">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="704bf-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
