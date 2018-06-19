---
uid: web-api/overview/advanced/http-cookies
title: ASP.NET Web API HTTP tanımlama bilgilerini | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071635"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="a7150-102">ASP.NET Web API HTTP tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="a7150-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="a7150-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a7150-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a7150-104">Bu konu, HTTP tanımlama bilgilerini Web API'si gönderip açıklar.</span><span class="sxs-lookup"><span data-stu-id="a7150-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="a7150-105">Arka plan HTTP tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="a7150-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="a7150-106">Bu bölümde, tanımlama bilgileri HTTP düzeyinde nasıl uygulandığını kısa genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7150-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="a7150-107">Ayrıntılar için başvurun [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="a7150-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="a7150-108">Bir tanımlama bilgisi, bir sunucu HTTP yanıt olarak gönderir veri parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="a7150-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="a7150-109">İstemci (isteğe bağlı) tanımlama bilgisi depolar ve subsequet isteklerinde döndürür.</span><span class="sxs-lookup"><span data-stu-id="a7150-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="a7150-110">Bu, istemci ve sunucu durumu paylaşmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7150-110">This allows the client and server to share state.</span></span> <span data-ttu-id="a7150-111">Bir tanımlama bilgisi ayarlamak için sunucunun yanıtta bir Set-Cookie üstbilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="a7150-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="a7150-112">Bir tanımlama bilgisi isteğe bağlı özniteliklere sahip bir ad-değer çifti biçimidir.</span><span class="sxs-lookup"><span data-stu-id="a7150-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="a7150-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a7150-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="a7150-114">Özniteliklerle örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a7150-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="a7150-115">Bir tanımlama bilgisinin sunucuya döndürülmesi için istemci tanımlama bilgisi üstbilgisi içinde sonraki istekleri içerir.</span><span class="sxs-lookup"><span data-stu-id="a7150-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="a7150-116">Bir HTTP yanıtı birden çok Set-Cookie üst bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a7150-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="a7150-117">İstemciyi tek bir tanımlama bilgisi üstbilgisi kullanarak çok sayıda tanımlama bilgisini döndürür.</span><span class="sxs-lookup"><span data-stu-id="a7150-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="a7150-118">Bir tanımlama bilgisinin süre ve kapsamını Set-Cookie üst bilgisindeki aşağıdaki öznitelikleri tarafından denetlenir:</span><span class="sxs-lookup"><span data-stu-id="a7150-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="a7150-119">**Etki alanı**: hangi etki alanının tanımlama bilgisi alması gereken istemci söyler.</span><span class="sxs-lookup"><span data-stu-id="a7150-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="a7150-120">Örneğin, etki alanı "example.com" ise, istemci her alt etki alanı example.com için tanımlama bilgisi döndürür. Belirtilmezse, kaynak sunucunun etki alanıdır.</span><span class="sxs-lookup"><span data-stu-id="a7150-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com. If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="a7150-121">**Yol**: tanımlama bilgisinin etki alanı içinde belirtilen yol kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="a7150-121">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="a7150-122">Belirtilmezse, istek URI'si yol kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a7150-122">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="a7150-123">**Süresi dolmadan**: tanımlama bilgisinin süre sonu tarihini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a7150-123">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="a7150-124">Süresi dolduğunda, istemci tanımlama bilgisi siler.</span><span class="sxs-lookup"><span data-stu-id="a7150-124">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="a7150-125">**Max-Age**: tanımlama bilgisinin yaş üst sınırını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a7150-125">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="a7150-126">En uzun geçerlilik süresi ulaştığında istemci tanımlama bilgisi siler.</span><span class="sxs-lookup"><span data-stu-id="a7150-126">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="a7150-127">Her iki `Expires` ve `Max-Age` ayarlanır, `Max-Age` önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a7150-127">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="a7150-128">Hiçbiri ayarlanırsa, istemci geçerli oturumu sona erdiğinde tanımlama bilgisi siler.</span><span class="sxs-lookup"><span data-stu-id="a7150-128">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="a7150-129">("Oturum" tam anlamı kullanıcı aracısı tarafından belirlenir.)</span><span class="sxs-lookup"><span data-stu-id="a7150-129">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="a7150-130">Ancak, istemciler tanımlama bilgilerini yoksayabilirsiniz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a7150-130">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="a7150-131">Örneğin, bir kullanıcı gizlilikle ilgili nedenlerden dolayı tanımlama bilgilerini devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7150-131">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="a7150-132">Süresi dolacak veya depolanan tanımlama bilgisi sayısını sınırlamak için önce istemcileri tanımlama bilgilerini silebilir.</span><span class="sxs-lookup"><span data-stu-id="a7150-132">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="a7150-133">Gizlilik nedenlerle istemciler genellikle "üçüncü taraf" tanımlama bilgilerini, kaynak sunucunun nerede etki alanıyla eşleşmeyen reddeder.</span><span class="sxs-lookup"><span data-stu-id="a7150-133">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="a7150-134">Kısacası, sunucunun ayarlar tanımlama bilgilerini geri alamazsınız güvenmemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="a7150-134">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="a7150-135">Web API'de tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="a7150-135">Cookies in Web API</span></span>

<span data-ttu-id="a7150-136">Bir HTTP yanıt olarak bir tanımlama bilgisi eklemek için oluşturma bir **CookieHeaderValue** tanımlama bilgisini temsil eden örneği.</span><span class="sxs-lookup"><span data-stu-id="a7150-136">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="a7150-137">' I çağırın **AddCookies** tanımlanan genişletme yöntemi **System.Net.Http. HttpResponseHeadersExtensions** tanımlama bilgisi eklemek için sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a7150-137">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="a7150-138">Örneğin, aşağıdaki kod bir denetleyici eylemi içinde bir tanımlama bilgisi ekler:</span><span class="sxs-lookup"><span data-stu-id="a7150-138">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="a7150-139">Dikkat **AddCookies** bir dizi alır **CookieHeaderValue** örnekleri.</span><span class="sxs-lookup"><span data-stu-id="a7150-139">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="a7150-140">Bir istemci isteğinden tanımlama bilgileri ayıklamak için çağrı **GetCookies** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a7150-140">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="a7150-141">A **CookieHeaderValue** oluşan bir koleksiyon içeren **CookieState** örnekleri.</span><span class="sxs-lookup"><span data-stu-id="a7150-141">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="a7150-142">Her **CookieState** bir tanımlama bilgisi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="a7150-142">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="a7150-143">Almak için dizin oluşturucu yöntemini kullanın bir **CookieState** gösterildiği gibi ada göre.</span><span class="sxs-lookup"><span data-stu-id="a7150-143">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="a7150-144">Yapılandırılmış tanımlama bilgisi verileri</span><span class="sxs-lookup"><span data-stu-id="a7150-144">Structured Cookie Data</span></span>

<span data-ttu-id="a7150-145">Bunlar depolar kaç tanımlama bilgilerini birçok tarayıcılar sınırlamak&#8212;toplam sayısı hem etki alanı başına sayı.</span><span class="sxs-lookup"><span data-stu-id="a7150-145">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="a7150-146">Bu nedenle, çok sayıda tanımlama bilgisini ayarlamak yerine tek bir çerez yapılandırılmış veri yerleştirin yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="a7150-146">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="a7150-147">RFC 6265 tanımlama bilgisi veri yapısı tanımlamıyor.</span><span class="sxs-lookup"><span data-stu-id="a7150-147">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="a7150-148">Kullanarak **CookieHeaderValue** sınıfı, tanımlama bilgisi verileri için ad-değer çiftlerinin listesini iletebilir.</span><span class="sxs-lookup"><span data-stu-id="a7150-148">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="a7150-149">Bu ad-değer çiftleri Set-Cookie üst bilgisindeki URL kodlanmış form verileri olarak kodlanır:</span><span class="sxs-lookup"><span data-stu-id="a7150-149">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="a7150-150">Önceki kod şu Set-Cookie üstbilgisi üretir:</span><span class="sxs-lookup"><span data-stu-id="a7150-150">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="a7150-151">**CookieState** sınıfı, bir tanımlama bilgisinde istek iletisi alt değerleri okuma için bir dizin oluşturucu yöntemi sağlar:</span><span class="sxs-lookup"><span data-stu-id="a7150-151">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="a7150-152">Örnek: Ayarlamak ve ileti işleyicisi tanımlama bilgilerini alma</span><span class="sxs-lookup"><span data-stu-id="a7150-152">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="a7150-153">Önceki örneklerde Web API denetleyicisi içinde tanımlama bilgilerini kullanmak nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a7150-153">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="a7150-154">Başka bir seçenek kullanmaktır [ileti işleyicileri](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="a7150-154">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="a7150-155">İleti işleyicileri denetleyicileri daha düzenindeki daha önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a7150-155">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="a7150-156">Bir ileti işleyicisini denetleyici istek erişmeden önce tanımlama bilgilerini istekten okumak veya denetleyicisi yanıt oluşturduktan sonra yanıta tanımlama bilgilerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a7150-156">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="a7150-157">Aşağıdaki kod, oturum kimliklerini oluşturmak için bir ileti işleyicisini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a7150-157">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="a7150-158">Oturum kimliği bir tanımlama bilgisinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="a7150-158">The session ID is stored in a cookie.</span></span> <span data-ttu-id="a7150-159">İşleyici istek oturum tanımlama bilgisi için denetler.</span><span class="sxs-lookup"><span data-stu-id="a7150-159">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="a7150-160">İstek tanımlama bilgisi içermiyorsa, işleyicinin yeni bir oturum kimliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a7150-160">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="a7150-161">Her iki durumda da, oturum kimliği işleyici depolar **HttpRequestMessage.Properties** özellik paketi.</span><span class="sxs-lookup"><span data-stu-id="a7150-161">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="a7150-162">Ayrıca HTTP yanıtına oturum tanımlama bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="a7150-162">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="a7150-163">Bu uygulama, istemciden gelen oturum kimliği sunucu tarafından gerçekten verilmiş olduğunu doğrulamaz.</span><span class="sxs-lookup"><span data-stu-id="a7150-163">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="a7150-164">Form kimlik doğrulaması olarak kullanmayın!</span><span class="sxs-lookup"><span data-stu-id="a7150-164">Don't use it as a form of authentication!</span></span> <span data-ttu-id="a7150-165">Örnek HTTP tanımlama bilgisi yönetim göstermek için noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="a7150-165">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="a7150-166">Bir denetleyici oturum kimliği alabilirsiniz **HttpRequestMessage.Properties** özellik paketi.</span><span class="sxs-lookup"><span data-stu-id="a7150-166">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
