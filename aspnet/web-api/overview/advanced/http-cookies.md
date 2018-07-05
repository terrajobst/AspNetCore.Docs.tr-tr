---
uid: web-api/overview/advanced/http-cookies
title: ASP.NET Web API'de HTTP tanımlama bilgileri | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 5885586df1d0f67d4e7e04ad88bc4fd1af71dc80
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368411"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="08c2c-102">ASP.NET Web API'de HTTP tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="08c2c-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="08c2c-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="08c2c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="08c2c-104">Bu konu, göndermek ve Web API'de HTTP tanımlama bilgileri almak açıklar.</span><span class="sxs-lookup"><span data-stu-id="08c2c-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="08c2c-105">Arka planda HTTP tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="08c2c-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="08c2c-106">Bu bölüm, tanımlama bilgisi HTTP düzeyinde nasıl uygulandığını kısa genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="08c2c-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="08c2c-107">Ayrıntılar için başvurun [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="08c2c-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="08c2c-108">Bir tanımlama bilgisi, HTTP yanıtında bir sunucuya gönderdiği verilerin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="08c2c-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="08c2c-109">İstemci (isteğe bağlı) tanımlama bilgisi depolar ve subsequet isteklerinde döndürür.</span><span class="sxs-lookup"><span data-stu-id="08c2c-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="08c2c-110">Bu, istemci ve sunucu durumu paylaşmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="08c2c-110">This allows the client and server to share state.</span></span> <span data-ttu-id="08c2c-111">Bir tanımlama bilgisi ayarlamak için sunucunun yanıtta bir Set-Cookie üst bilgisini içerir.</span><span class="sxs-lookup"><span data-stu-id="08c2c-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="08c2c-112">Bir tanımlama bilgisinin biçimi isteğe bağlı öznitelikleri ile bir ad-değer çiftidir.</span><span class="sxs-lookup"><span data-stu-id="08c2c-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="08c2c-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="08c2c-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="08c2c-114">Özniteliklere sahip bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="08c2c-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="08c2c-115">Bir tanımlama bilgisinin sunucuya döndürülmesi için istemcinin sonraki istekleri tanımlama bilgisi üstbilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="08c2c-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="08c2c-116">Bir HTTP yanıtı birden çok Set-Cookie üst bilgi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="08c2c-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="08c2c-117">İstemciyi tek bir tanımlama bilgisi üstbilgisi kullanarak çok sayıda tanımlama bilgisini döndürür.</span><span class="sxs-lookup"><span data-stu-id="08c2c-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="08c2c-118">Projenin kapsamına ve süresine bir tanımlama bilgisinin Set-Cookie üst bilgisindeki aşağıdaki öznitelikleri tarafından denetlenir:</span><span class="sxs-lookup"><span data-stu-id="08c2c-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="08c2c-119">**Etki alanı**: etki alanı tanımlama bilgisi alması gereken istemci söyler.</span><span class="sxs-lookup"><span data-stu-id="08c2c-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="08c2c-120">Örneğin, etki alanı "example.com" ise, istemci her alt etki alanının example.com tanımlama bilgisini döndürür.</span><span class="sxs-lookup"><span data-stu-id="08c2c-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="08c2c-121">Belirtilmezse, kaynak sunucu etki alanıdır.</span><span class="sxs-lookup"><span data-stu-id="08c2c-121">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="08c2c-122">**Yol**: tanımlama bilgisinin etki alanı içinde belirtilen yol kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="08c2c-122">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="08c2c-123">Belirtilmezse, istek URI'si yolu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="08c2c-123">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="08c2c-124">**Süresi dolmadan**: bir sona erme tarihi tanımlama bilgisi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="08c2c-124">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="08c2c-125">Belirtecin süresi dolduğunda, istemcinin tanımlama bilgisi siler.</span><span class="sxs-lookup"><span data-stu-id="08c2c-125">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="08c2c-126">**Max-Age**: tanımlama bilgisinin yaş üst sınırını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="08c2c-126">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="08c2c-127">En uzun geçerlilik süresi ulaştığında istemci tanımlama bilgisi siler.</span><span class="sxs-lookup"><span data-stu-id="08c2c-127">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="08c2c-128">Her iki `Expires` ve `Max-Age` ayarlanması `Max-Age` önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="08c2c-128">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="08c2c-129">Ayarlanmış ise, geçerli oturumu sona erdiğinde istemci tanımlama bilgisi siler.</span><span class="sxs-lookup"><span data-stu-id="08c2c-129">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="08c2c-130">("Oturumu" tam anlamı kullanıcı aracısı tarafından belirlenir.)</span><span class="sxs-lookup"><span data-stu-id="08c2c-130">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="08c2c-131">Ancak, istemciler tanımlama bilgilerini yoksayabilirsiniz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="08c2c-131">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="08c2c-132">Örneğin, kullanıcı gizlilik nedenleriyle tanımlama bilgilerini devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="08c2c-132">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="08c2c-133">Süresi dolacak veya depolanan tanımlama bilgisi sayısını sınırlamak için önce istemcileri tanımlama bilgilerini silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="08c2c-133">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="08c2c-134">Gizlilik nedeniyle istemciler genellikle "üçüncü taraf" tanımlama bilgilerini, kaynak sunucu etki alanı burada eşleşmiyor reddeder.</span><span class="sxs-lookup"><span data-stu-id="08c2c-134">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="08c2c-135">Kısacası, sunucu tanımlama bilgilerini ayarlar geri alamazsınız güvenmemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="08c2c-135">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="08c2c-136">Web API'de tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="08c2c-136">Cookies in Web API</span></span>

<span data-ttu-id="08c2c-137">Bir HTTP yanıtına bir tanımlama bilgisi eklemek için oluşturun bir **CookieHeaderValue** tanımlama bilgisini temsil eden örneği.</span><span class="sxs-lookup"><span data-stu-id="08c2c-137">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="08c2c-138">Ardından çağırın **AddCookies** alanında tanımlanan genişletme yöntemi **System.Net.Http. HttpResponseHeadersExtensions** tanımlama bilgisi eklemek için sınıfı.</span><span class="sxs-lookup"><span data-stu-id="08c2c-138">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="08c2c-139">Örneğin, aşağıdaki kod, içinde bir denetleyici eylemi bir tanımlama bilgisi ekler:</span><span class="sxs-lookup"><span data-stu-id="08c2c-139">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="08c2c-140">Dikkat **AddCookies** dizisi alır **CookieHeaderValue** örnekleri.</span><span class="sxs-lookup"><span data-stu-id="08c2c-140">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="08c2c-141">Bir istemci isteğinde tanımlama bilgileri ayıklamak için çağrı **GetCookies** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="08c2c-141">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="08c2c-142">A **CookieHeaderValue** koleksiyonunu içeren **CookieState** örnekleri.</span><span class="sxs-lookup"><span data-stu-id="08c2c-142">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="08c2c-143">Her **CookieState** bir tanımlama bilgisi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="08c2c-143">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="08c2c-144">Almak için dizin oluşturucu yöntemi kullanın. bir **CookieState** adıyla gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="08c2c-144">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="08c2c-145">Yapılandırılmış bir tanımlama bilgisi verisi</span><span class="sxs-lookup"><span data-stu-id="08c2c-145">Structured Cookie Data</span></span>

<span data-ttu-id="08c2c-146">Bunlar depolar kaç tanımlama bilgilerini çoğu tarayıcısı sınırlamak&#8212;toplam hem etki alanı sayısı.</span><span class="sxs-lookup"><span data-stu-id="08c2c-146">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="08c2c-147">Bu nedenle, çok sayıda tanımlama bilgisini ayarlamak yerine tek bir tanımlama, yapılandırılmış veriler yerleştirmenin yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="08c2c-147">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="08c2c-148">RFC 6265 tanımlama bilgisi veri yapısı tanımlamıyor.</span><span class="sxs-lookup"><span data-stu-id="08c2c-148">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="08c2c-149">Kullanarak **CookieHeaderValue** sınıfı, tanımlama bilgisi verisi için ad-değer çiftlerinin listesini iletebilir.</span><span class="sxs-lookup"><span data-stu-id="08c2c-149">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="08c2c-150">Bu ad-değer çiftleri Set-Cookie üst bilgisindeki formu URL kodlanmış veriler olarak kodlanmış:</span><span class="sxs-lookup"><span data-stu-id="08c2c-150">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="08c2c-151">Önceki kod, şu Set-Cookie üst bilgisini üretir:</span><span class="sxs-lookup"><span data-stu-id="08c2c-151">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="08c2c-152">**CookieState** sınıfı bir tanımlama bilgisinde istek iletisi alt değerleri okumak için bir dizin oluşturucu yöntemi sağlar:</span><span class="sxs-lookup"><span data-stu-id="08c2c-152">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="08c2c-153">Örnek: Ayarlayın ve bir ileti işleyicisi tanımlama bilgilerini alma</span><span class="sxs-lookup"><span data-stu-id="08c2c-153">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="08c2c-154">Önceki örneklerde, bir Web API denetleyicisi içinde tanımlama bilgilerini kullanma gösterdi.</span><span class="sxs-lookup"><span data-stu-id="08c2c-154">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="08c2c-155">Başka bir seçenek kullanmaktır [ileti işleyicileri](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="08c2c-155">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="08c2c-156">İleti işleyicileri denetleyicileri daha işlem hattındaki daha önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="08c2c-156">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="08c2c-157">Bir ileti işleyicisi isteği ulaşır denetleyicisi önce tanımlama bilgilerini istekten okuyun veya denetleyicisi yanıt oluşturduktan sonra yanıta tanımlama bilgileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="08c2c-157">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="08c2c-158">Aşağıdaki kod, oturum kimliklerini oluşturmaya yönelik bir ileti işleyicisi gösterir.</span><span class="sxs-lookup"><span data-stu-id="08c2c-158">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="08c2c-159">Oturum kimliği, bir tanımlama bilgisinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="08c2c-159">The session ID is stored in a cookie.</span></span> <span data-ttu-id="08c2c-160">Oturum tanımlama bilgisi için istek işleyicisi denetler.</span><span class="sxs-lookup"><span data-stu-id="08c2c-160">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="08c2c-161">İstek tanımlama bilgisi içermiyorsa, işleyici, yeni bir oturum kimliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="08c2c-161">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="08c2c-162">Her iki durumda da, oturum kimliği işleyici depolar **HttpRequestMessage.Properties** özellik paketi.</span><span class="sxs-lookup"><span data-stu-id="08c2c-162">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="08c2c-163">Ayrıca HTTP yanıtı oturum tanımlama bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="08c2c-163">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="08c2c-164">Bu uygulama, istemciden gelen oturum kimliği, sunucu tarafından gerçekten verildiğini doğrulamaz.</span><span class="sxs-lookup"><span data-stu-id="08c2c-164">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="08c2c-165">Form kimlik doğrulaması olarak kullanmayın!</span><span class="sxs-lookup"><span data-stu-id="08c2c-165">Don't use it as a form of authentication!</span></span> <span data-ttu-id="08c2c-166">HTTP tanımlama bilgisi yönetim noktası örneğin göstermektir.</span><span class="sxs-lookup"><span data-stu-id="08c2c-166">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="08c2c-167">Bir denetleyici oturum kimliği alabilirsiniz **HttpRequestMessage.Properties** özellik paketi.</span><span class="sxs-lookup"><span data-stu-id="08c2c-167">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
