---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: AJAX dinamik güncelleştirmeler sunma | Microsoft Docs
author: microsoft
description: 10. adım uygular RSVP oturum açmış kullanıcıların ilgilendiklerini bir Şimdi Akşam Yemeği ayrıntı içinde tümleşik bir Ajax tabanlı yaklaşımı kullanarak, katılan destek...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: e902881d3dab6a902cb747a197a32f317d199723
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755827"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="5673e-103">AJAX dinamik güncelleştirmeler sunma</span><span class="sxs-lookup"><span data-stu-id="5673e-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="5673e-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5673e-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="5673e-105">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="5673e-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="5673e-106">Adım 10 ücretsiz / budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="5673e-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="5673e-107">10. adım uygular RSVP oturum açmış kullanıcıların ilgilendiklerini bir Şimdi Akşam Yemeği Ayrıntıları sayfasında tümleştirilmiş bir Ajax tabanlı yaklaşımı kullanarak, katılan destekler.</span><span class="sxs-lookup"><span data-stu-id="5673e-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="5673e-108">ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.</span><span class="sxs-lookup"><span data-stu-id="5673e-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="5673e-109">NerdDinner adım 10: AJAX LCV'ler etkinleştirme kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5673e-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="5673e-110">Şimdi bir Akşam Yemeği katılan ilgilendiklerini RSVP oturum açmış kullanıcılar için destek hemen uygulayın.</span><span class="sxs-lookup"><span data-stu-id="5673e-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="5673e-111">Biz, Şimdi Akşam Ayrıntıları sayfasında tümleştirilmiş bir AJAX tabanlı yaklaşımı kullanarak etkinleştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5673e-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="5673e-112">Kullanıcı RSVP'd olup olmadığını belirten</span><span class="sxs-lookup"><span data-stu-id="5673e-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="5673e-113">Kullanıcılar */Dinners/Ayrıntılar / [kimliği*] belirli bir Akşam Yemeği ilgili ayrıntıları görmek için URL:</span><span class="sxs-lookup"><span data-stu-id="5673e-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="5673e-114">Eylem yöntemi gerçekleştirilir Details() şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="5673e-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="5673e-115">RSVP destek uygulamak için ilk adımımız, bizim Dinner nesnesiyle (daha önce oluşturduğumuz Dinner.cs kısmi sınıf) bir "IsUserRegistered(username)" yardımcı yöntem eklemek için olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5673e-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="5673e-116">True veya false kullanıcı Akşam Yemeği için geçerli olup olmadığını RSVP'd bağlı olarak bu yardımcı yöntemini döndürür:</span><span class="sxs-lookup"><span data-stu-id="5673e-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="5673e-117">Biz sonra aşağıdaki kodu kullanıcının kayıtlı olup olmadığını belirten bir uygun bir ileti görüntülemek için sunduğumuz Details.aspx görünüm şablonu veya olay için ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5673e-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="5673e-118">Ve artık bir kullanıcı için kayıtlı bir Akşam Yemeği ziyaret ettiğinde, bu iletiyi görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="5673e-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="5673e-119">Ve bunlar kaydedilmedi görürler için bir Akşam Yemeği ziyaret ettikleri zaman aşağıdaki ileti:</span><span class="sxs-lookup"><span data-stu-id="5673e-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="5673e-120">Uygulama kayıt eylemi yöntemi</span><span class="sxs-lookup"><span data-stu-id="5673e-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="5673e-121">Artık kullanıcılar bir akşam yemeği için RSVP için Ayrıntılar sayfasından etkinleştirmek için gereken işlevselliği ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="5673e-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="5673e-122">Bunu gerçekleştirmek için yeni bir "RSVPController" sınıf \Controllers dizinde sağ tıklayıp Ekle - i seçerek oluşturacağız&gt;denetleyicisi menü komutu.</span><span class="sxs-lookup"><span data-stu-id="5673e-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="5673e-123">Biz bir "Register" eylem yöntemi için bağımsız değişken olarak bir Akşam Yemeği bir kimliği alır, oturum açan kullanıcı için kayıtlı kullanıcılar listesinde şu anda ise ve bakar uygun Dinner nesnesini alır yeni RSVPController sınıf içinde uygulayacaksınız RSVP nesne için bunları ekler değil:</span><span class="sxs-lookup"><span data-stu-id="5673e-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="5673e-124">Nasıl biz basit bir dize eylem yönteminin çıkışını iade ettiğiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="5673e-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="5673e-125">Biz bu iletiyi bir görünüm şablonu – içindeki katıştırılmış ancak kadar küçük olduğundan yalnızca Content() yardımcı yöntem denetleyicisi temel sınıf ve return gibi bir dize iletisi yukarıda kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="5673e-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="5673e-126">AJAX kullanarak RSVPForEvent eylem yöntemi çağırma</span><span class="sxs-lookup"><span data-stu-id="5673e-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="5673e-127">Bizim Ayrıntıları görünümünde kayıt eylemi yöntemi çağırmak için AJAX kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="5673e-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="5673e-128">Bu uygulama oldukça kolaydır.</span><span class="sxs-lookup"><span data-stu-id="5673e-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="5673e-129">İlk iki komut dosyası kitaplığı başvurularını ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="5673e-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="5673e-130">İlk kitaplığı çekirdek ASP.NET AJAX istemci tarafı komut dosyası kitaplığı başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="5673e-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="5673e-131">Bu dosya, yaklaşık 24 k (sıkıştırılmış) boyutu ve çekirdek istemci tarafı AJAX işlevselliği içerir.</span><span class="sxs-lookup"><span data-stu-id="5673e-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="5673e-132">İkinci kitaplık (kısa bir süre sonra kullanacağız) ASP.NET MVC'nin yerleşik AJAX Yardımcısı yöntemleri ile tümleştirmenize yardımcı programını işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="5673e-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="5673e-133">Güncelleştirme outputing bir ", bu olay için kaydedilmeyen" iletisi yerine, biz bunun yerine bir bağlantı, zamanla gönderdiğiniz işlemek için daha önce eklediğimiz kodu görüntüle şablon RSVP denetleyicimizin bizim RSVPForEvent eylem yöntemini çağıran bir AJAX çağrısı gerçekleştirir biz seçebilir ve kullanıcı RSVPs:</span><span class="sxs-lookup"><span data-stu-id="5673e-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="5673e-134">Yukarıda kullanılan Ajax.ActionLink() yardımcı yöntem ASP.NET MVC'nda yerleşik olarak bulunan, bağlantıya tıklandığında bir standart gezinti gerçekleştirmek yerine eylem yöntemi için bir AJAX çağrısı kolaylaştırır Html.ActionLink() yardımcı yöntemine benzer.</span><span class="sxs-lookup"><span data-stu-id="5673e-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="5673e-135">Yukarıdaki biz "RSVP" denetleyicide "Register" eylem yöntemini çağırarak ve DinnerID için "id" parametresi olarak geçirerek.</span><span class="sxs-lookup"><span data-stu-id="5673e-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="5673e-136">Biz kaçı AjaxOptions parametresi son eylem yönteminden döndürülen içeriği alıp HTML güncelleştirme istiyoruz gösterir &lt;div&gt; sayfasında "rsvpmsg" kimliğine sahip öğe.</span><span class="sxs-lookup"><span data-stu-id="5673e-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="5673e-137">Ve Şimdi Akşam Yemeği için bir kullanıcı attığında için kayıtlı değil henüz RSVP onun için bir bağlantı görürler:</span><span class="sxs-lookup"><span data-stu-id="5673e-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="5673e-138">"Bu olayın RSVP" Bağlantısı'na tıklarsanız, kayıt eylem yöntemi için bir AJAX çağrısı RSVP denetleyicisinde yapacaksınız ve tamamlandığında güncelleştirilmiş bir ileti görürler aşağıdaki gibi:</span><span class="sxs-lookup"><span data-stu-id="5673e-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="5673e-139">Bu AJAX çağrısı yaparken ilgili trafik ve ağ bant genişliği gerçekten basit.</span><span class="sxs-lookup"><span data-stu-id="5673e-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="5673e-140">Kullanıcı "RSVP bu olay için" bağlantısına tıkladığında, küçük bir HTTP POST ağ isteği yapılan */Dinners/Register/1* kablo aşağıdaki gibi görünür URL'si:</span><span class="sxs-lookup"><span data-stu-id="5673e-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="5673e-141">Ve bizim kayıt eylemi yöntemi yanıttan yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="5673e-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="5673e-142">Bu basit bir çağrı, hızlı ve daha yavaş bir ağ üzerinden çalışır.</span><span class="sxs-lookup"><span data-stu-id="5673e-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="5673e-143">JQuery bir animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="5673e-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="5673e-144">Uyguladık AJAX işlevselliği iyi ve hızlı bir şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="5673e-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="5673e-145">Bazen hızlı, ancak, bir kullanıcı RSVP bağlantı ile yeni metin değiştirildiğini fark etmeyebilirsiniz emin olabilir.</span><span class="sxs-lookup"><span data-stu-id="5673e-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="5673e-146">Sonucu biraz daha belirgin hale getirmek için güncelleştirme iletisi dikkat çekmek için basit bir animasyon ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5673e-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="5673e-147">' % S'varsayılan ASP.NET MVC proje şablonu, jQuery – da Microsoft tarafından desteklenen bir mükemmel (ve çok popüler) açık kaynak JavaScript kitaplığı içerir.</span><span class="sxs-lookup"><span data-stu-id="5673e-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="5673e-148">bir dizi özellik, iyi bir HTML DOM seçimi ve etkileri kitaplığı dahil olmak üzere jQuery sağlar.</span><span class="sxs-lookup"><span data-stu-id="5673e-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="5673e-149">JQuery kullanmak için önce bir komut dosyası başvuru ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="5673e-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="5673e-150">JQuery içinde çeşitli yerlerde sitemizi içinde kullanılmasını kullanacağız çünkü tüm sayfaları kullanabilmesi betik başvurusu bizim Site.master ana sayfa dosyası içinde ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="5673e-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="5673e-151">*İpucu: VS 2008 SP1'de JavaScript dosyaları (jQuery dahil) için daha zengin IntelliSense desteği sağlayan JavaScript IntelliSense düzeltme yüklediğinizden emin olun. Buradan indirebilirsiniz: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="5673e-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="5673e-152">Genellikle, JQuery kullanılarak yazılmış kod bir genel "$ ()" kullanan bir CSS seçicisini kullanarak bir veya daha fazla HTML öğeleri alır bir JavaScript yöntemini.</span><span class="sxs-lookup"><span data-stu-id="5673e-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="5673e-153">Örneğin, <em>$("#rsvpmsg")</em> herhangi bir HTML öğesi kimliği rsvpmsg, seçer sırada <em>$(".something")</em> "şey" CSS tüm öğelerle seçeceğiniz sınıf adı.</span><span class="sxs-lookup"><span data-stu-id="5673e-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="5673e-154">Ayrıca, "tüm işaretli radyo düğmeleri return gibi" daha gelişmiş sorgular yazabilirsiniz gibi bir seçici sorgu kullanarak: <em>$("Giriş [@typeradyo =] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="5673e-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="5673e-155">Öğeleri seçtikten sonra bunlardaki gizlemeden gibi eylemler gerçekleştiren yöntemleri çağırabilirsiniz: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="5673e-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="5673e-156">RSVP senaryomuz için biz ","rsvpmsg"seçer AnimateRSVPMessage" adlı basit bir JavaScript işlevi tanımlama olasılığınız &lt;div&gt; ve metin içeriğini boyutunu canlandırın.</span><span class="sxs-lookup"><span data-stu-id="5673e-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="5673e-157">Aşağıdaki kodu küçük metin ve ardından nedenleri başlar, bir 400 milisaniyeden zaman çerçevesi içinde artırmak için:</span><span class="sxs-lookup"><span data-stu-id="5673e-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="5673e-158">Biz ardından kablo bu JavaScript işlevi için sunduğumuz Ajax.ActionLink() yardımcı yöntem adını geçirerek müşterilerimizin AJAX çağrısı başarıyla tamamlandıktan sonra çağrılacak Yukarı ("OnSuccess" AjaxOptions aracılığıyla olay özellik):</span><span class="sxs-lookup"><span data-stu-id="5673e-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="5673e-159">Ve artık "Bu olayın RSVP" bağlantısına tıkladı ve bizim AJAX çağrısı gönderilen içerik ileti başarıyla tamamlar arka animasyon ekleme ve büyük büyütmeyi:</span><span class="sxs-lookup"><span data-stu-id="5673e-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="5673e-160">"OnSuccess" olay sağlamanın yanı sıra AjaxOptions nesne (çeşitli diğer özellikleri ve kullanışlı seçenekleri ile birlikte) işleyebileceği OnBegin OnFailure ve OnComplete olayları gösterir.</span><span class="sxs-lookup"><span data-stu-id="5673e-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="5673e-161">Temizlik - out RSVP kısmi görünüm yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="5673e-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="5673e-162">Bizim şablonu ayrıntıları görüntüleme biraz uzun almak hangi mesai bunu anlamak biraz daha zor hale getirecek başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="5673e-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="5673e-163">Kodun okunabilirliğini geliştirmek için şimdi tüm ayrıntıları sayfamıza RSVP görünümü kodu kapsülleyen bir kısmi görünüm – RSVPStatus.ascx – oluşturarak sonlandırın.</span><span class="sxs-lookup"><span data-stu-id="5673e-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="5673e-164">\Views\Dinners klasörü sağ tıklatın ve sonra Add - seçerek bunu yapabilirsiniz&gt;görüntüle menü komutu.</span><span class="sxs-lookup"><span data-stu-id="5673e-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="5673e-165">Biz Şimdi Akşam nesnesi, kesin türü belirtilmiş ViewModel olarak kazandığını sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="5673e-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="5673e-166">Biz ardından kopyalama/RSVP içeriği bizim Details.aspx görünümünden içine yapıştırma.</span><span class="sxs-lookup"><span data-stu-id="5673e-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="5673e-167">Biz yaptıktan sonra ayrıca bizim düzenleme ve silme bağlantısı görünümü kodu kapsülleyen başka bir kısmi görünüm – EditAndDeleteLinks.ascx - oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="5673e-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="5673e-168">Biz de bunu bir Akşam Yemeği nesnesi, kesin türü belirtilmiş ViewModel olarak yararlanın ve Kopyala/Yapıştır, bizim Details.aspx görünümüne düzenleme ve silme mantığından sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="5673e-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="5673e-169">Bizim ayrıntıları şablon can görüntüleyin. ardından hemen altındaki iki Html.RenderPartial() yöntemi çağrılarını içerir:</span><span class="sxs-lookup"><span data-stu-id="5673e-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="5673e-170">Bu kod okunması ve düzenlenmesi daha temiz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5673e-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="5673e-171">Sonraki adım</span><span class="sxs-lookup"><span data-stu-id="5673e-171">Next Step</span></span>

<span data-ttu-id="5673e-172">Şimdi nasıl biz AJAX daha da kullanabilir ve uygulamamıza etkileşimli eşleme desteği eklendi göz atalım.</span><span class="sxs-lookup"><span data-stu-id="5673e-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5673e-173">[Önceki](secure-applications-using-authentication-and-authorization.md)
> [İleri](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="5673e-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
