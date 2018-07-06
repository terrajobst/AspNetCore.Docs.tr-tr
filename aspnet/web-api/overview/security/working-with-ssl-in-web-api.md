---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Web API'de SSL ile çalışma | Microsoft Docs
author: MikeWasson
description: SSL SSL istemci sertifikaları kullanma dahil olmak üzere ASP.NET Web API ile kullanma işlemi gösterilmektedir.
ms.author: aspnetcontent
ms.date: 12/12/2012
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0e0ca75c6ff1af397fce91079bcd8e9304b025ef
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829816"
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="4ec1d-103">Web API'de SSL ile çalışma</span><span class="sxs-lookup"><span data-stu-id="4ec1d-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="4ec1d-104">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4ec1d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4ec1d-105">Birçok yaygın kimlik doğrulama düzenleri düz HTTP üzerinden güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="4ec1d-106">Özellikle temel kimlik doğrulaması ve form kimlik doğrulaması şifrelenmemiş kimlik bilgileri gönderin.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="4ec1d-107">Güvenli olması için bu kimlik doğrulama düzenleri *gerekir* SSL kullanın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="4ec1d-108">Ayrıca, SSL istemci sertifikaları, istemcilerin kimliğini doğrulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="4ec1d-109">Sunucuda SSL etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="4ec1d-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="4ec1d-110">SSL kurma IIS 7 veya üzeri için:</span><span class="sxs-lookup"><span data-stu-id="4ec1d-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="4ec1d-111">Oluşturun veya bir sertifika alın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-111">Create or get a certificate.</span></span> <span data-ttu-id="4ec1d-112">Test için otomatik olarak imzalanan bir sertifika oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="4ec1d-113">Bir HTTPS bağlaması ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="4ec1d-114">Ayrıntılar için bkz [SSL ayarlama IIS 7 üzerinde nasıl](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="4ec1d-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="4ec1d-115">Yerel test etmek için Visual Studio'dan IIS Express SSL etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="4ec1d-116">Özellikler penceresinde ayarlayın **SSL etkin** için **True**.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="4ec1d-117">Değerini not edin **SSL URL**; HTTPS bağlantıları test etmek için bu URL'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="4ec1d-118">Bir Web API denetleyicisi, SSL'yi zorunlu tutma</span><span class="sxs-lookup"><span data-stu-id="4ec1d-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="4ec1d-119">Bir HTTPS hem bir HTTP bağlaması varsa, istemciler siteye erişmek için HTTP yine de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="4ec1d-120">Bazı kaynaklar diğer kaynaklara SSL gerektirirken HTTP kullanılabilir olmasını sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="4ec1d-121">Bu durumda, bir eyleme eylem filtresi korumalı kaynaklar için SSL gerektirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="4ec1d-122">Aşağıdaki kod, SSL için denetleyen bir Web API kimlik doğrulaması filtre gösterir:</span><span class="sxs-lookup"><span data-stu-id="4ec1d-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="4ec1d-123">Bu filtre, SSL iste herhangi bir Web API eylemlerini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4ec1d-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="4ec1d-124">SSL istemci sertifikaları</span><span class="sxs-lookup"><span data-stu-id="4ec1d-124">SSL Client Certificates</span></span>

<span data-ttu-id="4ec1d-125">SSL, ortak anahtar altyapısı sertifikalarını kullanarak kimlik doğrulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="4ec1d-126">Sunucu, sunucuda istemci kimliğini doğrulayan bir sertifika sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="4ec1d-127">Sunucu için bir sertifika sağlamak için istemcinin daha az yaygın bir durumdur, ancak istemcilerin kimliğini doğrulamak için bir seçenek budur.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="4ec1d-128">İstemci sertifikaları, SSL ile kullanmak için kullanıcılarınız için otomatik imzalı sertifikaları dağıtmak için bir yol gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="4ec1d-129">Birçok uygulama türü, bu iyi bir kullanıcı deneyimi olmayacak, ancak bazı ortamlarda (örneğin, Kurumsal) uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="4ec1d-130">Yararları</span><span class="sxs-lookup"><span data-stu-id="4ec1d-130">Advantages</span></span> | <span data-ttu-id="4ec1d-131">Dezavantajlar</span><span class="sxs-lookup"><span data-stu-id="4ec1d-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="4ec1d-132">-Kullanıcı adı/parola daha güçlü sertifika kimlik bilgileridir.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="4ec1d-133">-Kimlik doğrulama, ileti bütünlüğü ve ileti şifreleme tam güvenli kanalı SSL sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="4ec1d-134">-Edinmeli ve PKI sertifikalarını yönetin.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="4ec1d-135">-İstemci Platformu SSL istemci sertifikaları desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="4ec1d-136">İstemci sertifikalarını kabul etmek üzere IIS'yi yapılandırmak için IIS Yöneticisi'ni açın ve aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="4ec1d-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="4ec1d-137">Ağaç görünümünde site düğümünü tıklatın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="4ec1d-138">Çift **SSL ayarları** Orta bölmede özelliği.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="4ec1d-139">Altında **istemci sertifikaları**, aşağıdaki seçeneklerden birini seçin:</span><span class="sxs-lookup"><span data-stu-id="4ec1d-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="4ec1d-140">**Kabul**: IIS istemci sertifika kabul eder, ancak bir gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="4ec1d-141">**Gerekli**: bir istemci sertifikası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="4ec1d-142">(Bu seçeneği etkinleştirmek için de "SSL iste" seçmeniz gerekir)</span><span class="sxs-lookup"><span data-stu-id="4ec1d-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="4ec1d-143">Bu gibi durumlarda, bu seçenekler ayrıca ApplicationHost.config dosyasında ayarlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4ec1d-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="4ec1d-144">**SslNegotiateCert** bayrağı anlamına gelir IIS istemci sertifika kabul eder, ancak biri ("Kabul et" seçeneğini IIS Yöneticisi'nde eşdeğer) gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="4ec1d-145">Bir sertifika gerektirecek şekilde **SslRequireCert** bayrağı.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="4ec1d-146">Test etmek için de bu seçenekler IIS Express'te URL'i yerel applicationhost içinde ayarlayabilirsiniz. "Documents\IISExpress\config" içinde bulunan yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="4ec1d-147">Test etmek için bir istemci sertifikası oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ec1d-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="4ec1d-148">Test amacıyla kullanabilirsiniz [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) bir istemci sertifikası oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="4ec1d-149">İlk olarak, bir test kök yetkilisi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="4ec1d-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="4ec1d-150">MakeCert özel anahtarı için bir parola girmenizi ister.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="4ec1d-151">Ardından, sertifika sunucunun "güvenilen kök sertifika yetkilileri" mağazasından, aşağıdaki şekilde test ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4ec1d-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="4ec1d-152">MMC'yi açın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-152">Open MMC.</span></span>
2. <span data-ttu-id="4ec1d-153">Altında **dosya**seçin **Ekle/Kaldır ek bileşenini**.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="4ec1d-154">Seçin **bilgisayar hesabı**.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="4ec1d-155">Seçin **yerel bilgisayar** ve Sihirbazı tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="4ec1d-156">Altındaki gezinme bölmesinde, "Güvenilen kök sertifika yetkilileri" düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="4ec1d-157">Üzerinde **eylem** menüsünde **tüm görevler**ve ardından **alma** Sertifika Alma Sihirbazı'nı başlatın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="4ec1d-158">TempCA.cer sertifika dosyasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="4ec1d-159">Tıklayın **açık**, ardından **sonraki** ve Sihirbazı tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="4ec1d-160">(, Parolayı yeniden girmeniz istenir.)</span><span class="sxs-lookup"><span data-stu-id="4ec1d-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="4ec1d-161">Şimdi ilk sertifika tarafından imzalanmış bir istemci sertifikası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="4ec1d-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="4ec1d-162">İstemci sertifikaları Web API'si kullanma</span><span class="sxs-lookup"><span data-stu-id="4ec1d-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="4ec1d-163">Sunucu tarafında çağırarak istemci sertifikası edinebilirsiniz [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) istek iletisinde.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="4ec1d-164">Yöntemi, istemci sertifikası yok ise null döndürür.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="4ec1d-165">Aksi halde bir **X509Certificate2** örneği.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="4ec1d-166">Sertifikadan konu ve veren gibi bilgileri almak için bu nesneyi kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="4ec1d-167">Ardından bu bilgileri kimlik doğrulaması ve/veya yetkilendirme için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ec1d-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
