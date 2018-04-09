---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu | Microsoft Docs
author: tfitzmac
description: Bu makalede, ASP.NET Web sayfaları (Razor) ve bazı önerilen çözümlerle çalışırken sahip olabileceği sorunlar anlatılmaktadır. Yazılım sürümleri ASP.NET Web sayfa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec51169ccea0016712de3fdb28a16a174150a8bd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="68841-104">ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu</span><span class="sxs-lookup"><span data-stu-id="68841-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="68841-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="68841-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="68841-106">Bu makalede, ASP.NET Web sayfaları (Razor) ve bazı önerilen çözümlerle çalışırken sahip olabileceği sorunlar anlatılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="68841-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="68841-107">Yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="68841-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="68841-108">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="68841-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="68841-109">Bu öğreticide, ASP.NET Web Pages 2 ve ASP.NET Web sayfaları 1.0 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="68841-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="68841-110">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="68841-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="68841-111">Sayfaları çalıştıran sorunlar</span><span class="sxs-lookup"><span data-stu-id="68841-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="68841-112">Razor kod sorunları</span><span class="sxs-lookup"><span data-stu-id="68841-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="68841-113">Güvenlik ve üyeliği ile ilgili sorunlar</span><span class="sxs-lookup"><span data-stu-id="68841-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="68841-114">E-posta gönderme sorunları</span><span class="sxs-lookup"><span data-stu-id="68841-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="68841-115">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="68841-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="68841-116">Genel sorular için bkz: [ASP.NET Web sayfaları (Razor) SSS](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="68841-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="68841-117">Sayfaları çalıştıran sorunlar</span><span class="sxs-lookup"><span data-stu-id="68841-117">Issues with Running Pages</span></span>

<span data-ttu-id="68841-118">Çok çeşitli sorunlar engelleyebilirsiniz *.cshtml* ve *.vbhtml* düzgün çalışmasını sayfaları.</span><span class="sxs-lookup"><span data-stu-id="68841-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="68841-119">Bu bölüm yaygın hata iletileri listelenmiştir ve büyük olasılıkla neden olur.</span><span class="sxs-lookup"><span data-stu-id="68841-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="68841-120">HTTP Hatası 403 - Yasak: Erişim engellendi</span><span class="sxs-lookup"><span data-stu-id="68841-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="68841-121">*Bu dizini veya sayfayı sağladığınız kimlik bilgileriyle görüntüleme izniniz yok.*</span><span class="sxs-lookup"><span data-stu-id="68841-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="68841-122">Sunucu .NET Framework'ün doğru sürümünü çalışmıyorsa bu hata oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="68841-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="68841-123">(Yerel ve uzaktan) server çalıştıran bilgisayarın en az .NET Framework 4 yüklü olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="68841-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="68841-124">Ayrıca uygulamanın kendisinin doğru sürüme çalıştırmak için yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="68841-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="68841-125">Bu sorunu yerel olarak Webmatrix'te çalışırken görürseniz, tıklatın **Site** çalışma alanında ve ardından treeview tıklatın **ayarları**.</span><span class="sxs-lookup"><span data-stu-id="68841-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="68841-126">İçinde **.NET Framework sürüm seçin** listesinde, seçin **.NET 4 (tümleşik)**.</span><span class="sxs-lookup"><span data-stu-id="68841-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="68841-127">Bu sürüm zaten ayarladıysanız, WebMatrix yönetici olarak çalıştırmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="68841-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="68841-128">Web sitenizin kök en az bir tane olduğundan emin olun *.cshtml* içindeki dosya.</span><span class="sxs-lookup"><span data-stu-id="68841-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="68841-129">Web sunucusu uzak bir sunucuda olduğunda bu hatayı görürseniz, sunucu yöneticisine başvurun.</span><span class="sxs-lookup"><span data-stu-id="68841-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="68841-130">Veya üstü yüklü sunucu .NET Framework 4 sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="68841-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="68841-131">Ayrıca uygulamayı,.NET Framework sürümünü kullanmak üzere yapılandırılmış bir uygulama havuzunda çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="68841-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="68841-132">Sunucu üzerinde denetim varsa, .NET Framework doğru sürümü çalıştırdığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="68841-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="68841-133">Çalıştırarak yüklemesini onarmayı deneyebilirsiniz `aspnet_regiis -iru` komutu.</span><span class="sxs-lookup"><span data-stu-id="68841-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="68841-134">(.NET Framework'u yükledikten sonra IIS yüklerseniz, örneğin, IIS doğru ASP.NET sayfaları çalıştırmak için yapılandırılacak değil.) Daha fazla bilgi için bkz: [ASP.NET IIS Kayıt Aracı (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="68841-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="68841-135">HTTP Hatası 403.14 - Yasak</span><span class="sxs-lookup"><span data-stu-id="68841-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="68841-136">*Web sunucusu bu dizinin içindekileri listelemeyecek şekilde yapılandırılmış.*</span><span class="sxs-lookup"><span data-stu-id="68841-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="68841-137">Bu hata, korunan bir kaynağa isteği oluşabilir (gibi *Web.config* dosya) veya korumalı bir klasörde olan (gibi *uygulama\_veri* veya *uygulama\_Kod*).</span><span class="sxs-lookup"><span data-stu-id="68841-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="68841-138">HTTP Hatası 404,17 - bulunamadı</span><span class="sxs-lookup"><span data-stu-id="68841-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="68841-139">*İstenen içerik komut dizisi olarak gözüküyor ve statik dosya işleyici tarafından sunulmayacak.*</span><span class="sxs-lookup"><span data-stu-id="68841-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="68841-140">Sunucu .NET Framework 4 kullanmak için düzgün yapılandırılmamış veya daha sonra ve bu nedenle kodda tanımıyor bu hata oluşabilir `@{ }` engeller.</span><span class="sxs-lookup"><span data-stu-id="68841-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="68841-141">Açıklama için önceki bkz *HTTP Hata 403 - Yasak: erişim reddedildi*.</span><span class="sxs-lookup"><span data-stu-id="68841-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="68841-142">HTTP Hatası 404.7 - bulunamadı</span><span class="sxs-lookup"><span data-stu-id="68841-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="68841-143">*İstek Filtreleme modülü dosya uzantısını reddedecek şekilde yapılandırıldı*</span><span class="sxs-lookup"><span data-stu-id="68841-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="68841-144">Bu hata oluşabilir *.cshtml* veya *.vbhtml* uzantıları sunucuda açıkça da engellenmiş.</span><span class="sxs-lookup"><span data-stu-id="68841-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="68841-145">Bu sorunun belirtisi uzantısı, ancak dahil URL'leri dahil etmeyin URL'leri çalışan olduğunda *.cshtml* veya *.vbhtml* çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="68841-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="68841-146">Sitenin uzantılarında yeniden etkinleştirmek için olası bir çözüm olan *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="68841-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="68841-147">Aşağıdaki örnekte nasıl etkinleştirileceği gösterilmiştir *.cshtml* uzantısı.</span><span class="sxs-lookup"><span data-stu-id="68841-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="68841-148">HTTP Hatası 404.8 - bulunamadı</span><span class="sxs-lookup"><span data-stu-id="68841-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="68841-149">*İstek Filtreleme modülü URL'deki bir hiddenSegment bölümü içeren yolu reddedecek şekilde yapılandırıldı.*</span><span class="sxs-lookup"><span data-stu-id="68841-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="68841-150">Bu hata, korunan bir kaynağa isteği oluşabilir (gibi *Web.config* dosya) veya korumalı bir klasörde olan (gibi *uygulama\_veri* veya *uygulama\_Kod*).</span><span class="sxs-lookup"><span data-stu-id="68841-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="68841-151">Bu sayfa türü ('/' uygulamasında sunucu hatası) sunulmuyor</span><span class="sxs-lookup"><span data-stu-id="68841-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="68841-152">HTTP Hatası 404,17 önceki açıklamasına bakın.</span><span class="sxs-lookup"><span data-stu-id="68841-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="68841-153">Razor kod sorunları</span><span class="sxs-lookup"><span data-stu-id="68841-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="68841-154">Adı '*sınıfı*' geçerli bağlamda mevcut değil</span><span class="sxs-lookup"><span data-stu-id="68841-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="68841-155">Genellikle, bu hata gördüğünüz bir neden olan `class` başvuruları yardımcıyı ancak Yardımcısı yüklü değil.</span><span class="sxs-lookup"><span data-stu-id="68841-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="68841-156">Örneğin, bir yardımcı kullanmayı denerseniz, ancak paket Nuget'ten yüklemediyseniz, bu hatayı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="68841-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="68841-157">WebMatrix Galerisi'nde bulmak ve yardımcı yüklemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="68841-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="68841-158">Yardımcı yüklenir, ancak sayfa hala bu tanımıyor varsa, deneyin ekleme bir `using` kodu ifadesine.</span><span class="sxs-lookup"><span data-stu-id="68841-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="68841-159">İçinde `using` deyimi, başvurusu yardımcı içeren ad alanı.</span><span class="sxs-lookup"><span data-stu-id="68841-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="68841-160">Örneğin, ASP.NET Web Yardımcıları paketteki temel içinde yardımcılardır `System.Web.Helpers` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="68841-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="68841-161">Yardımcıyı kullanmak istediğiniz sayfanın üst kısmında, aşağıdaki satırı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="68841-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="68841-162">Güvenlik ve üyeliği ile ilgili sorunlar</span><span class="sxs-lookup"><span data-stu-id="68841-162">Issues with Security and Membership</span></span>

<span data-ttu-id="68841-163">ASP.NET Web Pages'da (Razor) yerleşik güvenlik (Üyelik) sistemi kullanıyorsanız aşağıdaki sorunlarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68841-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="68841-164">Bu yöntemi çağırabilmeniz için "Membership.Provider" özelliği "ExtendedMembershipProvider" örneği olmalıdır</span><span class="sxs-lookup"><span data-stu-id="68841-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="68841-165">Bu hatayı bildiren hiçbir `AspNetSqlMembershipProvider` sınıfı yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="68841-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="68841-166">(Bir belirti sitesi düzgün yerel olarak çalışır ancak bir barındırma sağlayıcısının sunucuya yayımladığınızda, bu hata oluşturur olur.) Bu sorun için bir düzeltme olduğu sitenin aşağıdakileri ekleyerek basit üyelik açıkça etkinleştirmek için *Web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="68841-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="68841-167">E-posta gönderme sorunları</span><span class="sxs-lookup"><span data-stu-id="68841-167">Issues with Sending Email</span></span>

<span data-ttu-id="68841-168">E-posta gönderme sorunlarını hata ayıklamak için zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="68841-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="68841-169">SMTP sunucusuna bağlanılamıyor ilk bir sorun olabilir.</span><span class="sxs-lookup"><span data-stu-id="68841-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="68841-170">Bağlantı başarılı olursa, ASP.NET ileti kapalı SMTP sunucusuna aktarır.</span><span class="sxs-lookup"><span data-stu-id="68841-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="68841-171">Ancak, SMTP sunucusu, göndermesini engeller ileti kendisini sorunları olabilir.</span><span class="sxs-lookup"><span data-stu-id="68841-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="68841-172">E-posta uygulamanızı başarıyla göndermez, aşağıdakileri deneyin:</span><span class="sxs-lookup"><span data-stu-id="68841-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="68841-173">SMTP sunucusu adı şöyle görülür `smtp.provider.com` veya `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="68841-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="68841-174">Ancak, bir barındırma sağlayıcısına sitenizi yayımlarsanız, SMTP sunucu adı bu noktada olabilir `localhost`.</span><span class="sxs-lookup"><span data-stu-id="68841-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="68841-175">Bu durum, yayımladıktan sonra sitenizi sağlayıcının sunucusunda çalıştığından, SMTP sunucusu uygulamanız açısından bakıldığında yerel olabileceğinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="68841-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="68841-176">Bu değişikliği sunucu adları, yayımlama işleminin bir parçası SMTP sunucu adını değiştirmek zorunda anlamına gelebilir.</span><span class="sxs-lookup"><span data-stu-id="68841-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="68841-177">Bağlantı noktası numarası genellikle 25'tir.</span><span class="sxs-lookup"><span data-stu-id="68841-177">The port number is usually 25.</span></span> <span data-ttu-id="68841-178">Ancak, bazı sağlayıcılar bağlantı noktasını kullanacak biçimde 587 veya bazı başka bir bağlantı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="68841-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="68841-179">SMTP sunucusuna sahip hangi bağlantı noktası numarasını kullanmanızı bekledikleri denetleyin.</span><span class="sxs-lookup"><span data-stu-id="68841-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="68841-180">Doğru kimlik bilgilerini kullandığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="68841-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="68841-181">Bir barındırma sağlayıcısına sitenizi yayımladıysanız, sağlayıcı özellikle e-posta için belirtilir kimlik bilgileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="68841-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="68841-182">Bu kimlik bilgileri yayımlamak için kullandığınız kimlik bilgilerinden farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="68841-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="68841-183">Bazen kimlik bilgilerini hiç gerekmez.</span><span class="sxs-lookup"><span data-stu-id="68841-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="68841-184">Kişisel ISS kullanarak e-posta gönderiyorsanız, e-posta sağlayıcınız kimlik bilgilerinizi zaten biliyor olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68841-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="68841-185">Yayımladığınızda, yerel bilgisayarınızda test ne zaman farklı kimlik bilgileri kullanmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="68841-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="68841-186">E-posta sağlayıcınız şifreleme kullanıyorsa, ayarlamak `WebMail.EnableSsl` için `true`.</span><span class="sxs-lookup"><span data-stu-id="68841-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="68841-187">E-posta gönderilirken bir hata varsa, şuna benzeyen bir standart ASP.NET hata iletisi görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="68841-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![E-posta ile ilgili bir sorun olduğunda ASP.NET hata iletisi](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="68841-189">De kullanarak e-posta gönderme sorunlarını ayıklayabilirsiniz bir `try-catch` aşağıdaki örnekteki gibi bloğu.</span><span class="sxs-lookup"><span data-stu-id="68841-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="68841-190">Kullandığınızda, bir `try-catch` bloğu, ASP.NET, standart hata iletileri görüntülenmez.</span><span class="sxs-lookup"><span data-stu-id="68841-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="68841-191">Bunun yerine, hatayı yakalamak için `catch` blok kısmı.</span><span class="sxs-lookup"><span data-stu-id="68841-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="68841-192">İçin uygun değerleri yerine `your-SMTP-server-name`ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="68841-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="68841-193">Bu şekilde görebileceğiniz hata iletileri bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="68841-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="68841-194">*Posta gönderme başarısız oldu.*</span><span class="sxs-lookup"><span data-stu-id="68841-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="68841-195">veya</span><span class="sxs-lookup"><span data-stu-id="68841-195">-or-</span></span>

    <span data-ttu-id="68841-196">*Bağlı taraf düzgün zaman ya da kurulan bağlantı bağlanılan ana makine yanıt başarısız olduğundan başarısız oldu, bir süre sonra yanıt vermediği için bir bağlantı girişimi başarısız oldu*</span><span class="sxs-lookup"><span data-stu-id="68841-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="68841-197">Bu hata genellikle uygulama SMTP sunucusuna bağlanamadı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="68841-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="68841-198">Sunucu adını denetleyin ve bağlantı noktası numarası.</span><span class="sxs-lookup"><span data-stu-id="68841-198">Check the server name and port number.</span></span>
- <span data-ttu-id="68841-199"><em>Posta kutusu kullanılamıyor. Sunucu yanıt: 5.1.0 &lt; someuser@invaliddomain &gt; gönderen reddedilen: Geçersiz gönderen etki alanı</em></span><span class="sxs-lookup"><span data-stu-id="68841-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="68841-200">Bu iletiyi bildiren `From` adresi doğru değil veya eksik.</span><span class="sxs-lookup"><span data-stu-id="68841-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="68841-201">*Belirtilen dize için bir e-posta adresi gereken biçimde değil.*</span><span class="sxs-lookup"><span data-stu-id="68841-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="68841-202">Bu hata, gösterebilir değerini `To` veya `From` özellikleri e-posta adresleri olarak tanınmadı.</span><span class="sxs-lookup"><span data-stu-id="68841-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="68841-203">(ASP.NET, e-posta adresi doğru biçimde gibi kullanıcının yalnızca geçerli olduğunu denetleyemez *name@domain.com*.)</span><span class="sxs-lookup"><span data-stu-id="68841-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="68841-204">Hatayı görüntüler biçimlendirme Kaldır (`@errorMessage`) canlı bir siteye sayfa yayımlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="68841-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="68841-205">Kullanıcıların bir sunucudan alma hata iletilerine izin vermek için iyi bir fikir değil.</span><span class="sxs-lookup"><span data-stu-id="68841-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="68841-206">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="68841-206">Additional Resources</span></span>

[<span data-ttu-id="68841-207">ASP.NET Web Sayfaları (Razor) SSS</span><span class="sxs-lookup"><span data-stu-id="68841-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="68841-208">[WebMatrix ve ASP.NET Web sayfaları](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET Web sitesi Forumu</span><span class="sxs-lookup"><span data-stu-id="68841-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
