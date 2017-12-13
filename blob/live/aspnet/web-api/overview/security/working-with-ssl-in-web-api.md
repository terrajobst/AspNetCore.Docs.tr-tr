---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: "Web API'de SSL ile çalışma | Microsoft Docs"
author: MikeWasson
description: "SSL istemci sertifikalarını kullanan dahil olmak üzere ASP.NET Web API ile SSL kullanmayı gösterir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8c631900c8c5ab6097e0cb9fd4a71abbcba1c88b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="ef387-103">Web API'de SSL ile çalışma</span><span class="sxs-lookup"><span data-stu-id="ef387-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="ef387-104">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ef387-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ef387-105">Birçok ortak kimlik doğrulama şemasını düz HTTP üzerinden güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="ef387-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="ef387-106">Özellikle, temel kimlik doğrulaması ve forms kimlik doğrulaması şifrelenmemiş kimlik bilgilerini gönderir.</span><span class="sxs-lookup"><span data-stu-id="ef387-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="ef387-107">Güvenli olması için bu kimlik doğrulama şemasını *gerekir* SSL kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef387-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="ef387-108">Buna ek olarak, SSL istemci sertifikaları, istemcilerin kimliğini doğrulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef387-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="ef387-109">Sunucuda SSL etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ef387-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="ef387-110">IIS 7 veya sonraki sürümlerde SSL kurma için:</span><span class="sxs-lookup"><span data-stu-id="ef387-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="ef387-111">Oluşturun veya bir sertifika alın.</span><span class="sxs-lookup"><span data-stu-id="ef387-111">Create or get a certificate.</span></span> <span data-ttu-id="ef387-112">Test etmek için otomatik olarak imzalanan sertifika oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef387-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="ef387-113">Bir HTTPS bağlaması ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ef387-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="ef387-114">Ayrıntılar için bkz [SSL ayarlamak IIS 7 üzerinde nasıl](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="ef387-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="ef387-115">Yerel test etmek için IIS Express Visual Studio'dan SSL etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef387-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="ef387-116">Özellikler penceresinde ayarlayın **SSL etkin** için **doğru**.</span><span class="sxs-lookup"><span data-stu-id="ef387-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="ef387-117">Değeri Not **SSL URL**; HTTPS bağlantıları test etmek için bu URL'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef387-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="ef387-118">Bir Web API denetleyicisi SSL zorlama</span><span class="sxs-lookup"><span data-stu-id="ef387-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="ef387-119">Bir HTTPS ve HTTP bağlaması varsa, istemciler siteye erişmek için HTTP hala kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef387-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="ef387-120">Diğer kaynaklar SSL gerektirirken HTTP kullanılabilir olması bazı kaynaklar sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="ef387-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="ef387-121">Bu durumda, korunan kaynaklar için SSL gerektirmek için bir eylem filtresi kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef387-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="ef387-122">Aşağıdaki kod, SSL için denetler bir Web API kimlik doğrulama filtre gösterir:</span><span class="sxs-lookup"><span data-stu-id="ef387-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="ef387-123">Bu filtre SSL gereken tüm Web API eylemler ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ef387-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="ef387-124">SSL istemci sertifikaları</span><span class="sxs-lookup"><span data-stu-id="ef387-124">SSL Client Certificates</span></span>

<span data-ttu-id="ef387-125">SSL, ortak anahtar altyapısı sertifikalarını kullanarak kimlik doğrulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="ef387-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="ef387-126">Sunucu istemciye kimlik doğrulamasını bir sertifika sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef387-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="ef387-127">İstemci sunucuya bir sertifika sağlamak daha az yaygın bir durumdur, ancak istemcilerin kimliğini doğrulamak için bir seçenek budur.</span><span class="sxs-lookup"><span data-stu-id="ef387-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="ef387-128">SSL ile istemci sertifikaları kullanmak için kullanıcılarınız için imzalanmış sertifikaları dağıtmak için bir yönteme ihtiyacınız vardır.</span><span class="sxs-lookup"><span data-stu-id="ef387-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="ef387-129">Birçok uygulama türü için bu iyi bir kullanıcı deneyimi olmayacak, ancak bazı ortamlarda (örneğin, Kurumsal) uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="ef387-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="ef387-130">Yararları</span><span class="sxs-lookup"><span data-stu-id="ef387-130">Advantages</span></span> | <span data-ttu-id="ef387-131">Dezavantajlar</span><span class="sxs-lookup"><span data-stu-id="ef387-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="ef387-132">-Kullanıcı adı/parola güçlü sertifika kimlik bilgileridir.</span><span class="sxs-lookup"><span data-stu-id="ef387-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="ef387-133">-SSL kimlik doğrulaması, ileti bütünlüğü ve ileti şifreleme ile tam bir güvenli kanal sağlar.</span><span class="sxs-lookup"><span data-stu-id="ef387-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="ef387-134">-Edinmeli ve PKI sertifikaları yönetin.</span><span class="sxs-lookup"><span data-stu-id="ef387-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="ef387-135">-İstemci Platformu SSL istemci sertifikalarını desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef387-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="ef387-136">İstemci sertifikalarını kabul etmek üzere IIS'yi yapılandırmak için IIS Yöneticisi'ni açın ve aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="ef387-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="ef387-137">Ağaç görünümünde site düğümünü tıklatın.</span><span class="sxs-lookup"><span data-stu-id="ef387-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="ef387-138">Çift **SSL ayarları** Orta bölmede özelliği.</span><span class="sxs-lookup"><span data-stu-id="ef387-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="ef387-139">Altında **istemci sertifikalarını**, aşağıdaki seçeneklerden birini seçin:</span><span class="sxs-lookup"><span data-stu-id="ef387-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="ef387-140">**Kabul**: IIS istemci sertifikası kabul eder, ancak bir gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="ef387-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="ef387-141">**Gerektiren**: bir istemci sertifikası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ef387-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="ef387-142">(Bu seçeneği etkinleştirmek için de "SSL iste" seçmeniz gerekir)</span><span class="sxs-lookup"><span data-stu-id="ef387-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="ef387-143">Bu gibi durumlarda, bu seçenekler ayrıca ApplicationHost.config dosyasında ayarlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ef387-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="ef387-144">**SslNegotiateCert** bayrağı anlamına gelir IIS istemci sertifikası kabul eder, ancak bir ("Kabul et" seçeneğini IIS Yöneticisi'nde eşdeğer) gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="ef387-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="ef387-145">Bir sertifika istemek için ayarlayın **sıklıkla** bayrağı.</span><span class="sxs-lookup"><span data-stu-id="ef387-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="ef387-146">Test etmek için Ayrıca bu seçenekler IIS Express'te yerel applicationhost ayarlayabilirsiniz. "Documents\IISExpress\config" içinde bulunan yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="ef387-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="ef387-147">Test etmek için bir istemci sertifikası oluşturma</span><span class="sxs-lookup"><span data-stu-id="ef387-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="ef387-148">Test amacıyla kullanabilirsiniz [MakeCert.exe](https://msdn.microsoft.com/en-US/library/bfsktky3.aspx) bir istemci sertifikası oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="ef387-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/en-US/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="ef387-149">İlk olarak, bir test kök yetkilisi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ef387-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="ef387-150">MakeCert, özel anahtarı bir parola girmenizi ister.</span><span class="sxs-lookup"><span data-stu-id="ef387-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="ef387-151">Ardından, sertifika sunucunun "güvenilen kök sertifika yetkilileri" depolamak, aşağıdaki gibi test ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ef387-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="ef387-152">MMC'yi açın.</span><span class="sxs-lookup"><span data-stu-id="ef387-152">Open MMC.</span></span>
2. <span data-ttu-id="ef387-153">Altında **dosya**seçin **Ekle/Kaldır ek bileşenini**.</span><span class="sxs-lookup"><span data-stu-id="ef387-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="ef387-154">Seçin **bilgisayar hesabı**.</span><span class="sxs-lookup"><span data-stu-id="ef387-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="ef387-155">Seçin **yerel bilgisayar** ve Sihirbazı tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="ef387-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="ef387-156">Gezinti bölmesinin altında "Güvenilen kök sertifika yetkilileri" düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="ef387-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="ef387-157">Üzerinde **eylem** menüsündeki **tüm görevler**ve ardından **alma** Sertifika Alma Sihirbazı'nı başlatın.</span><span class="sxs-lookup"><span data-stu-id="ef387-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="ef387-158">TempCA.cer sertifika dosyasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="ef387-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="ef387-159">Tıklatın **açık**, ardından **sonraki** ve Sihirbazı tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="ef387-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="ef387-160">(Parola girmeniz istenir.)</span><span class="sxs-lookup"><span data-stu-id="ef387-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="ef387-161">Artık ilk sertifikası tarafından imzalanmış bir istemci sertifikası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ef387-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="ef387-162">İstemci sertifikalarını Web API kullanma</span><span class="sxs-lookup"><span data-stu-id="ef387-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="ef387-163">Sunucu tarafında çağırarak istemci sertifikasını alabilirsiniz [GetClientCertificate](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) üzerinde istek iletisi.</span><span class="sxs-lookup"><span data-stu-id="ef387-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="ef387-164">Yöntemi istemci sertifikası ise null döndürür.</span><span class="sxs-lookup"><span data-stu-id="ef387-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="ef387-165">Aksi takdirde, döndürür bir **X509Certificate2** örneği.</span><span class="sxs-lookup"><span data-stu-id="ef387-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="ef387-166">Bu nesne, sertifika veren ve konu gibi bilgi almak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef387-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="ef387-167">Ardından kimlik doğrulama ve/veya yetkilendirme için bu bilgileri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef387-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
