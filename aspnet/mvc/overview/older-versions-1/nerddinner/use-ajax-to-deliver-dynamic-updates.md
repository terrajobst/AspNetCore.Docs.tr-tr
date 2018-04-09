---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Dinamik güncelleştirmeleri göndermek için AJAX kullanma | Microsoft Docs
author: microsoft
description: Adım 10 Implements oturum açan kullanıcılar için RSVP içinde Yemeği ayrıntı tümleşik bir Ajax tabanlı yaklaşımı kullanarak, bir Yemeği katılan içinde kendi ilgi destek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7cea3ee2ec52261521941efac484e91a53f6310b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="876c2-103">Dinamik güncelleştirmeleri göndermek için AJAX kullanma</span><span class="sxs-lookup"><span data-stu-id="876c2-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="876c2-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="876c2-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="876c2-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="876c2-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="876c2-106">Bu 10 ücretsiz bir adımdır ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="876c2-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="876c2-107">Adım 10 Implements oturum açan kullanıcılar için RSVP Yemeği Ayrıntıları sayfasında tümleşik bir Ajax tabanlı yaklaşımı kullanarak, bir Yemeği katılan içinde kendi ilgi destekler.</span><span class="sxs-lookup"><span data-stu-id="876c2-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="876c2-108">ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.</span><span class="sxs-lookup"><span data-stu-id="876c2-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="876c2-109">NerdDinner 10. adım: LCV'ler etkinleştirme AJAX kabul eder</span><span class="sxs-lookup"><span data-stu-id="876c2-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="876c2-110">Şimdi bir Yemeği katılan içinde kendi ilgi RSVP oturum açan kullanıcılar için destek şimdi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="876c2-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="876c2-111">Biz bu Yemeği Ayrıntıları sayfasında tümleşik bir AJAX tabanlı yaklaşım kullanarak etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="876c2-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="876c2-112">Kullanıcı RSVP'd olup olmadığını belirten</span><span class="sxs-lookup"><span data-stu-id="876c2-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="876c2-113">Kullanıcıların ziyaret */Dinners/Ayrıntılar / [kimliği*] belirli Yemeği ayrıntılarını görmek için URL:</span><span class="sxs-lookup"><span data-stu-id="876c2-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="876c2-114">Eylem yöntemi uygulanır Details() sözlüğüdür:</span><span class="sxs-lookup"><span data-stu-id="876c2-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="876c2-115">RSVP destek uygulamak için ilk adım, bir "IsUserRegistered(username)" yardımcı yöntem bizim Yemeği nesnesiyle (daha önce oluşturduğumuz Dinner.cs parçalı sınıf) eklemek için olacaktır.</span><span class="sxs-lookup"><span data-stu-id="876c2-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="876c2-116">True veya false kullanıcı yemeği için geçerli olup olmadığını RSVP'd bağlı olarak bu yardımcı yöntemini döndürür:</span><span class="sxs-lookup"><span data-stu-id="876c2-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="876c2-117">Biz sonra aşağıdaki kodu kullanıcı kayıtlı olup olmadığını belirten bir uygun iletisi görüntülenecek bizim Details.aspx şablonu görüntüleme veya olay için ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="876c2-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="876c2-118">Ve artık kullanıcı için kayıtlı bir Yemeği ziyaret ettiğinde, bu iletiyi görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="876c2-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="876c2-119">Ve bunlar kaydedilmedi bunlar görürsünüz için yemeği ziyaret ettiğinizde ileti altında:</span><span class="sxs-lookup"><span data-stu-id="876c2-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="876c2-120">Kayıt eylemi yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="876c2-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="876c2-121">Artık kullanıcıların bir Yemeği RSVP için Ayrıntılar sayfasından etkinleştirmek için gereken işlevleri ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="876c2-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="876c2-122">Bu uygulama için yeni bir "RSVPController" sınıf \Controllers dizinde sağ tıklayıp seçerek Ekle - oluşturacağız&gt;denetleyicisi menü komutu.</span><span class="sxs-lookup"><span data-stu-id="876c2-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="876c2-123">"Register" eylem yöntemi için bağımsız değişken olarak bir Yemeği kimliği alır, uygun Yemeği nesnesine oturum açma kullanıcı şu an için kayıtlı kullanıcıların listesini kullanılıyorsa ve bakar alır yeni RSVPController sınıf içinde uygulamak RSVP nesneyi bunları ekler değil:</span><span class="sxs-lookup"><span data-stu-id="876c2-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="876c2-124">Nasıl biz basit bir dize eylem yönteminin çıkışını iade ettiğiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="876c2-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="876c2-125">Biz bu iletiyi bir görünüm şablonu içinde– katıştırılmış ancak kadar küçük olduğundan yalnızca Content() yardımcı yöntem denetleyicisi temel sınıf ve dize ileti gibi yukarıda return kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="876c2-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="876c2-126">AJAX kullanarak RSVPForEvent eylem yöntemini çağırma</span><span class="sxs-lookup"><span data-stu-id="876c2-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="876c2-127">Bizim Ayrıntıları görünümünden kayıt eylemi yöntemi çağırmak için AJAX kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="876c2-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="876c2-128">Bu uygulama oldukça kolaydır.</span><span class="sxs-lookup"><span data-stu-id="876c2-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="876c2-129">İlk iki komut dosyası kitaplığı başvuru ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="876c2-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="876c2-130">İlk kitaplığı çekirdek ASP.NET AJAX istemci tarafı komut dosyası kitaplığı başvurur.</span><span class="sxs-lookup"><span data-stu-id="876c2-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="876c2-131">Bu dosya, yaklaşık 24 k (sıkıştırılmış) boyutu ve çekirdek istemci tarafı AJAX işlevselliği içerir.</span><span class="sxs-lookup"><span data-stu-id="876c2-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="876c2-132">İkinci kitaplığı (kısa bir süre sonra kullanacağız) ASP.NET MVC'ın yerleşik AJAX yardımcı yöntemler ile tümleştirmenize yardımcı işlevlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="876c2-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="876c2-133">Bizim RSVP denetleyicisinde bizim RSVPForEvent eylem yöntemini çağırır bir AJAX çağrısı güncelleştirme outputing ", bu olay için kayıtlı olmayan" iletisi yerine biz bunun yerine bir bağlantı, basıldığında işlemek için daha önce eklediğimiz görünüm şablonu kodu gerçekleştirir sonra geçebiliriz ve kullanıcı RSVPs:</span><span class="sxs-lookup"><span data-stu-id="876c2-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="876c2-134">Yukarıda kullanılan Ajax.ActionLink() yardımcı yöntemi yerleşik-ASP.NET MVC ve bağlantı tıklatıldığında standart gezinti gerçekleştirmek yerine eylem yöntemi için bir AJAX çağrısı kolaylaştırır Html.ActionLink() yardımcı yöntemine benzerdir.</span><span class="sxs-lookup"><span data-stu-id="876c2-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="876c2-135">Yukarıdaki biz "RSVP" denetleyicisinde "Register" eylem yöntemini çağırma ve "id" parametresi olarak kendisine DinnerID geçirme.</span><span class="sxs-lookup"><span data-stu-id="876c2-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="876c2-136">Eylem yönteminden döndürülen içeriği almak ve HTML güncelleştirmek istiyoruz geçirme son AjaxOptions parametre gösterir &lt;div&gt; kimliğine sahip "rsvpmsg" sayfasında öğesi.</span><span class="sxs-lookup"><span data-stu-id="876c2-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="876c2-137">Ve şimdi bir kullanıcı bir Yemeği attığında için kayıtlı değil henüz RSVP onun için bir bağlantı göreceksiniz:</span><span class="sxs-lookup"><span data-stu-id="876c2-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="876c2-138">"Bu olay için RSVP" bağlantısına tıklarsanız kayıt eylem yöntemi için bir AJAX çağrısı RSVP denetleyicisinde yaptıkları ve tamamlandığında güncelleştirilmiş bir ileti görürsünüz aşağıdaki gibi:</span><span class="sxs-lookup"><span data-stu-id="876c2-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="876c2-139">Ağ bant genişliği ve trafik bu AJAX çağrısı yaparak gerçekten hafif olduğunda söz konusu.</span><span class="sxs-lookup"><span data-stu-id="876c2-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="876c2-140">Kullanıcı "RSVP bu olay için" bağlantısına tıkladığında, küçük bir HTTP POST ağ isteği yapılan */Dinners/Register/1* kablo aşağıdaki gibi görünen URL'si:</span><span class="sxs-lookup"><span data-stu-id="876c2-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="876c2-141">Ve bizim kayıt eylemi yöntemi yanıttan yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="876c2-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="876c2-142">Bu basit aramayı hızlıdır ve hatta bir yavaş ağ üzerinden çalışır.</span><span class="sxs-lookup"><span data-stu-id="876c2-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="876c2-143">JQuery animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="876c2-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="876c2-144">Biz uygulanan AJAX işlevselliği, düzgün ve hızlı bir şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="876c2-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="876c2-145">Bazen hızlı, yine de bir kullanıcı yeni metinle RSVP bağlantı değiştirildi fark etmeyebilirsiniz emin olabilir.</span><span class="sxs-lookup"><span data-stu-id="876c2-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="876c2-146">Sonucu biraz daha belirgin yapmak için güncelleştirme iletisi dikkat çekmek için basit bir animasyon ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="876c2-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="876c2-147">ASP.NET MVC proje şablonu varsayılan jQuery – ayrıca Microsoft tarafından desteklenen bir mükemmel (ve çok yaygın) açık kaynak JavaScript kitaplığı içerir.</span><span class="sxs-lookup"><span data-stu-id="876c2-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="876c2-148">jQuery iyi bir HTML DOM seçimi ve etkileri kitaplığı gibi özellikleri de sağlar.</span><span class="sxs-lookup"><span data-stu-id="876c2-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="876c2-149">JQuery kullanmak için öncelikle bir komut dosyası başvuru ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="876c2-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="876c2-150">JQuery yerler, çeşitli içinde sitemizi içinde kullanılmasını kalacaklarını olduğundan, tüm sayfaları kullanabilmesi komut dosyası başvurusunu bizim Site.master ana sayfa dosyası içinde ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="876c2-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="876c2-151">*İpucu: VS 2008 SP1'de, JavaScript dosyaları (jQuery dahil) için daha zengin IntelliSense desteği sağlayan JavaScript IntelliSense düzeltmesinin yüklü olup olmadığını denetleyin. Buradan indirebilirsiniz: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="876c2-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="876c2-152">JQuery genellikle kullanılarak yazılan kodu kullanan bir genel "$ ()" bir CSS seçicisini kullanarak bir veya daha fazla HTML öğeleri alır JavaScript yöntemi.</span><span class="sxs-lookup"><span data-stu-id="876c2-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="876c2-153">Örneğin, <em>$("#rsvpmsg")</em> rsvpmsg, kimliği herhangi bir HTML öğesi seçer sırada <em>$(".something")</em> şeyle "" CSS tüm öğeleri seçeceğiniz sınıf adı.</span><span class="sxs-lookup"><span data-stu-id="876c2-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="876c2-154">"Tüm checked radyo düğmeleri return"gibi daha gelişmiş sorgular da yazabilirsiniz gibi bir seçici sorgu kullanarak: <em>$("Giriş [@typeradyo =] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="876c2-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="876c2-155">Öğeleri seçtikten sonra kendilerine gizleyip gibi eyleme geçmek için yöntemleri çağırabilirsiniz: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="876c2-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="876c2-156">RSVP senaryomuz için biz "" rsvpmsg"seçer AnimateRSVPMessage" adlı basit bir JavaScript işlevi tanımlarsınız &lt;div&gt; ve metin içeriğini boyutunu canlandırır.</span><span class="sxs-lookup"><span data-stu-id="876c2-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="876c2-157">Kod, metni küçük ve daha sonra nedenleri başlatır, 400 milisaniye zaman çerçevesi içinde artırmak için:</span><span class="sxs-lookup"><span data-stu-id="876c2-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="876c2-158">Biz sonra kablo bizim Ajax.ActionLink() yardımcı yöntem adını geçirerek bizim AJAX çağrısı başarıyla tamamlandıktan sonra çağrılacak bu JavaScript işlevi Yukarı ("OnSuccess" AjaxOptions aracılığıyla olay özellik):</span><span class="sxs-lookup"><span data-stu-id="876c2-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="876c2-159">Ve şimdi "RSVP bu olay için" Bağlantı tıklatıldığında ve bizim AJAX çağrısı gönderilen içerik ileti başarıyla tamamlandığında, geri hale getirmeyi ve büyük büyütmeyi:</span><span class="sxs-lookup"><span data-stu-id="876c2-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="876c2-160">Bir "OnSuccess" olay sağlamanın yanı sıra, AjaxOptions nesne (çeşitli diğer özellikleri ve yararlı seçenekleri ile birlikte) işleyebilir OnBegin, OnFailure ve OnComplete olayları gösterir.</span><span class="sxs-lookup"><span data-stu-id="876c2-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="876c2-161">Temizleme - RSVP kısmi görünüm çıkışı düzenleme</span><span class="sxs-lookup"><span data-stu-id="876c2-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="876c2-162">Bizim Ayrıntılar görünümü şablon biraz uzun almak hangi mesai anlamak biraz daha zor yapacak başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="876c2-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="876c2-163">Kodun okunabilirliğini artırmak için şimdi tüm ayrıntıları sayfamızı RSVP görünüm kodunu kapsülleyen bir kısmi görünüm – RSVPStatus.ascx – oluşturarak sonlandırın.</span><span class="sxs-lookup"><span data-stu-id="876c2-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="876c2-164">Bu \Views\Dinners klasöre sağ tıklayarak ve ardından Ekle - yapabiliriz&gt;görüntülemek menü komutu.</span><span class="sxs-lookup"><span data-stu-id="876c2-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="876c2-165">Biz bunu Yemeği nesnesi, kesin türü belirtilmiş ViewModel olarak ele sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="876c2-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="876c2-166">Biz sonra kopyalayıp RSVP içerik içine bizim Details.aspx görünümünden yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="876c2-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="876c2-167">Biz yaptıktan sonra ayrıca bizim düzenleme ve silme bağlantı Görünüm Kodu yalıtır başka bir kısmi görünüm – EditAndDeleteLinks.ascx - oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="876c2-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="876c2-168">Biz de Yemeği nesnesi, kesin türü belirtilmiş ViewModel olarak alın ve kopyalayıp yapıştırın bizim Details.aspx görünümüne, düzenleme ve silme mantığından sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="876c2-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="876c2-169">Bizim ayrıntıları şablon can görüntüleyin. ardından hemen altındaki iki Html.RenderPartial() yöntemi çağrılarını içerir:</span><span class="sxs-lookup"><span data-stu-id="876c2-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="876c2-170">Bu kod okuyun ve korumak için Temizleyici hale getirir.</span><span class="sxs-lookup"><span data-stu-id="876c2-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="876c2-171">Sonraki adım</span><span class="sxs-lookup"><span data-stu-id="876c2-171">Next Step</span></span>

<span data-ttu-id="876c2-172">Şimdi nasıl biz AJAX daha kullanın ve uygulamamız için etkileşimli eşleme desteği eklemek bakalım.</span><span class="sxs-lookup"><span data-stu-id="876c2-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="876c2-173">[Önceki](secure-applications-using-authentication-and-authorization.md)
> [sonraki](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="876c2-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
