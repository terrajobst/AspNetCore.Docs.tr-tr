---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: ASP.NET Web sayfaları sunarak - WebMatrix kullanarak bir Site yayımlama | Microsoft Docs
author: tfitzmac
description: Bu öğretici ASP.NET Web sayfaları ve Microsoft WebMatrix tanıtır öğretici kümesi son yüklemede ' dir. Sitenizi t yayımlamak nasıl ele...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 7b9bffac5cc72e1bea3f1b211cc03be2ccb8e499
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30899596"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a><span data-ttu-id="fe871-104">ASP.NET Web sayfaları sunarak - WebMatrix kullanarak bir Site yayımlama</span><span class="sxs-lookup"><span data-stu-id="fe871-104">Introducing ASP.NET Web Pages - Publishing a Site by Using WebMatrix</span></span>
====================
<span data-ttu-id="fe871-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fe871-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fe871-106">Bu öğretici ASP.NET Web sayfaları ve Microsoft WebMatrix tanıtır öğretici kümesi son yüklemede ' dir.</span><span class="sxs-lookup"><span data-stu-id="fe871-106">This tutorial is the final installment in the tutorial set that introduces ASP.NET Web Pages and Microsoft WebMatrix.</span></span> <span data-ttu-id="fe871-107">Diğerleri ile çalışabilmeniz için Internet'e, sitenizi yayımlama açıklanır.</span><span class="sxs-lookup"><span data-stu-id="fe871-107">It discusses how to publish your site to the Internet so that others can work with it.</span></span> <span data-ttu-id="fe871-108">Seri aracılığıyla tamamladığınızdan varsayar [tutarlı aramak için ASP.NET Web Pages sitelerinde oluşturma](https://go.microsoft.com/fwlink/?LinkId=251585).</span><span class="sxs-lookup"><span data-stu-id="fe871-108">It assumes you have completed the series through [Creating a Consistent Look for ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=251585).</span></span>
> 
> <span data-ttu-id="fe871-109">Site kullanarak yayımlamayı öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="fe871-109">You'll learn how to publish your site using:</span></span>
> 
> - <span data-ttu-id="fe871-110">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="fe871-110">Microsoft Azure</span></span>
> - <span data-ttu-id="fe871-111">Web barındırma şirketi</span><span class="sxs-lookup"><span data-stu-id="fe871-111">Web Hosting Company</span></span>


## <a name="about-publishing-your-site"></a><span data-ttu-id="fe871-112">Sitenizi yayımlama hakkında</span><span class="sxs-lookup"><span data-stu-id="fe871-112">About Publishing Your Site</span></span>

<span data-ttu-id="fe871-113">Şimdiye kadar sayfalarınızı test de dahil olmak üzere yerel bir bilgisayardaki tüm iş yaptık.</span><span class="sxs-lookup"><span data-stu-id="fe871-113">Up to now, you've done all your work on a local computer, including testing your pages.</span></span> <span data-ttu-id="fe871-114">Çalıştırmak için<em>.cshtml</em> sayfaları, WebMatrix, yani IIS Express yerleşik web sunucusu kullandığınız.</span><span class="sxs-lookup"><span data-stu-id="fe871-114">To run your<em>.cshtml</em> pages, you've used the web server that's built into WebMatrix, namely IIS Express.</span></span> <span data-ttu-id="fe871-115">Ancak hiç kimsenin dışında oluşturduğunuz site Elbette görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe871-115">But of course no one can see the site you've created except you.</span></span> <span data-ttu-id="fe871-116">Başkalarının sitenizle çalışmak için Internet'e yayımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe871-116">To let others work with your site, you have to publish it to the Internet.</span></span>

<span data-ttu-id="fe871-117">Bir ortak web sunucusuna erişim zaten yoksa, hesabınız zorunda yayımlama anlamına gelir bir *bulut platformu* veya *barındırma sağlayıcısına*.</span><span class="sxs-lookup"><span data-stu-id="fe871-117">Unless you have access to a public web server already, publishing means that you have to have an account with a *cloud platform* or a *hosting provider*.</span></span> <span data-ttu-id="fe871-118">Microsoft Azure gibi bir bulut platformu uygulamalarınız için isteğe bağlı altyapı sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe871-118">A cloud platform, such as Microsoft Azure, provides on-demand infrastructure for your applications.</span></span> <span data-ttu-id="fe871-119">Bir barındırma sağlayıcısı, genel olarak erişilebilir web sunucuları sahibi ve, kira şirket siteniz için boşluk ' dir.</span><span class="sxs-lookup"><span data-stu-id="fe871-119">A hosting provider is a company that owns publicly accessible web servers and that will rent you space for your site.</span></span> <span data-ttu-id="fe871-120">Barındırma planları ay birkaç dolar çalıştırılabilen (veya hatta ücretsiz) yüzlerce yüksek hacimli ticari Web siteleri için ayda bir ABD doları için küçük siteler için.</span><span class="sxs-lookup"><span data-stu-id="fe871-120">Hosting plans run from a few dollars a month (or even free) for small sites to many hundreds of dollars a month for high-volume commercial websites.</span></span>

> [!NOTE]
> <span data-ttu-id="fe871-121">Internet servis evde almak için kullandığınız Internet servis sağlayıcısı (ISS) üzerinden ortak web sunucusuna erişim olabilir.</span><span class="sxs-lookup"><span data-stu-id="fe871-121">You might have access to a public web server via the internet service provider (ISP) that you use to get internet service at home.</span></span> <span data-ttu-id="fe871-122">Ancak, barındırma sağlayıcınız ASP.NET Web sayfaları desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe871-122">However, your hosting provider must support ASP.NET Web Pages.</span></span> <span data-ttu-id="fe871-123">Birçok ISS yoktur, ancak bunu her zaman denetleme değerindedir.</span><span class="sxs-lookup"><span data-stu-id="fe871-123">Many ISPs don't, but it's always worth checking.</span></span>


<span data-ttu-id="fe871-124">Bu öğreticide, nasıl yayımlanacağını genel bakış sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="fe871-124">In this tutorial, we'll give you an overview of how to publish.</span></span> <span data-ttu-id="fe871-125">İşlem her barındırma sağlayıcısı için biraz farklıdır olduğundan tam Ayrıntılar için her şeyi, sağlamak mümkün değil.</span><span class="sxs-lookup"><span data-stu-id="fe871-125">It's not practical to provide exact details for everything, because the process differs a bit for every hosting provider.</span></span> <span data-ttu-id="fe871-126">Ancak işleminin nasıl çalıştığı, iyi bir fikir elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="fe871-126">But you'll get a good idea of how the process works.</span></span>

<span data-ttu-id="fe871-127">Bu öğretici dört bölüm içerir:</span><span class="sxs-lookup"><span data-stu-id="fe871-127">This tutorial contains four sections:</span></span>

1. [<span data-ttu-id="fe871-128">Varsayılan sayfa ayarlama</span><span class="sxs-lookup"><span data-stu-id="fe871-128">Setting up the default page</span></span>](#defaultpage)
2. <span data-ttu-id="fe871-129">Yayımlama (aşağıdakilerden birini seçin)</span><span class="sxs-lookup"><span data-stu-id="fe871-129">Publishing (choose one of the following)</span></span>  
 <span data-ttu-id="fe871-130">a.</span><span class="sxs-lookup"><span data-stu-id="fe871-130">a.</span></span> [<span data-ttu-id="fe871-131">Siteniz için Microsoft Azure yayımlama</span><span class="sxs-lookup"><span data-stu-id="fe871-131">Publishing Your Site to Microsoft Azure</span></span>](#azure)  
 <span data-ttu-id="fe871-132">b.</span><span class="sxs-lookup"><span data-stu-id="fe871-132">b.</span></span> [<span data-ttu-id="fe871-133">Şirket barındıran bir Web sitenizi yayımlama</span><span class="sxs-lookup"><span data-stu-id="fe871-133">Publishing Your Site to a Web Hosting Company</span></span>](#host)
3. [<span data-ttu-id="fe871-134">Canlı Site güncelleştiriliyor: yeniden yayımlama</span><span class="sxs-lookup"><span data-stu-id="fe871-134">Updating the Live Site: Republishing</span></span>](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a><span data-ttu-id="fe871-135">Varsayılan sayfa ayarlama</span><span class="sxs-lookup"><span data-stu-id="fe871-135">Setting up the default page</span></span>

<span data-ttu-id="fe871-136">Bir kullanıcı web siteniz için temel adres gittiğinde, siteniz için varsayılan sayfa kullanıcıya görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fe871-136">When a user navigates to the base address for your web site, the default page for your site is displayed to the user.</span></span> <span data-ttu-id="fe871-137">Default.htm www.contoso.com sırasında site için varsayılan sayfa olarak ayarlandığında, örneğin, ardından giderek <strong>www.contoso.com</strong> giderek aynı <strong>www.contoso.com/Default.htm</strong>.</span><span class="sxs-lookup"><span data-stu-id="fe871-137">For example, when Default.htm is set as the default page for the site at www.contoso.com, then navigating to <strong>www.contoso.com</strong> is the same as navigating to <strong>www.contoso.com/Default.htm</strong>.</span></span>

<span data-ttu-id="fe871-138">Siteniz şu anda kullandığı **Default.cshtml** varsayılan sayfa olarak.</span><span class="sxs-lookup"><span data-stu-id="fe871-138">Currently, your site uses **Default.cshtml** as the default page.</span></span> <span data-ttu-id="fe871-139">Bu sayfa için varsayılan sayfanızı sorun yoktur, ancak boş bir sayfa görüntüleyecektir şekilde Bu öğreticide, herhangi bir içerik için bu sayfayı eklemediniz.</span><span class="sxs-lookup"><span data-stu-id="fe871-139">This page is fine for your default page, but in this tutorial you have not added any content to that page so it would display a blank page.</span></span> <span data-ttu-id="fe871-140">Default.cshtml açın ve içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fe871-140">Open Default.cshtml and replace the content with the following code.</span></span>

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

<span data-ttu-id="fe871-141">Artık, sitenizi yayımlama için hazırdır.</span><span class="sxs-lookup"><span data-stu-id="fe871-141">Now your site is ready for publication.</span></span> <span data-ttu-id="fe871-142">İlk olarak, site Azure'a dağıtma ve bir web barındırma şirketi dağıtma görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="fe871-142">First, you will see how to deploy the site to Azure, and then how to deploy it to a web hosting company.</span></span> <span data-ttu-id="fe871-143">Web sitenizi ve için iki seçenek çalışır, yalnızca dağıtım seçeneklerinden birini gerçekleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe871-143">Either option works for your web site, and you only need to follow one of the deployment options.</span></span>

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a><span data-ttu-id="fe871-144">Siteniz için Microsoft Azure yayımlama</span><span class="sxs-lookup"><span data-stu-id="fe871-144">Publishing Your Site to Microsoft Azure</span></span>

<span data-ttu-id="fe871-145">Bu öğretici ilk sitenizi Microsoft Azure'a dağıtmak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="fe871-145">This tutorial will first show you how to deploy your site to Microsoft Azure.</span></span> <span data-ttu-id="fe871-146">Bir Microsoft hesabıyla oturum açarak, Azure üzerinde en fazla 10 ücretsiz siteleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe871-146">By signing in with a Microsoft account, you can create up to 10 free sites on Azure.</span></span> <span data-ttu-id="fe871-147">Bu ücretsiz sitelere sitelerinizi sınamak için kolay bir yol sağlamak.</span><span class="sxs-lookup"><span data-stu-id="fe871-147">These free sites provide a convenient way to test your sites.</span></span> <span data-ttu-id="fe871-148">Bu örnek site daha sonra tüm ücretsiz sitelerinizin kullanarak önlemek için her zaman silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe871-148">You can always delete this example site later to avoid using all of your free sites.</span></span> <span data-ttu-id="fe871-149">Yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe871-149">You can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="fe871-150">Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="fe871-150">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="fe871-151">WebMatrix Şeritte tıklatın **Yayımla** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="fe871-151">In the WebMatrix ribbon, click the **Publish** button.</span></span>

!['WebMatrix Şeritte Yayımla düğmesi'](publishing/_static/image1.png)

<span data-ttu-id="fe871-153">**Sitenizin yayımlama** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fe871-153">The **Publish Your Site** dialog box is displayed.</span></span> <span data-ttu-id="fe871-154">Microsoft hesabınızda oturum açmanızdan değil, iletişim kutusu içerecek bir **Azure ile çalışmaya başlama** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="fe871-154">If you have not signed in to your Microsoft account, the dialog box will contain a **Get started with Azure** link.</span></span> <span data-ttu-id="fe871-155">Bu bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fe871-155">Click this link.</span></span>

![Sitenizi yayımlayın](publishing/_static/image2.png)

<span data-ttu-id="fe871-157">Bir Microsoft hesabı ile oturum açmanızdan değil, oturum açmak için Fırsat yeniden verilir.</span><span class="sxs-lookup"><span data-stu-id="fe871-157">If you have not signed in to a Microsoft account, you are again given the opportunity to sign in.</span></span> <span data-ttu-id="fe871-158">Bir Microsoft hesabına sitenizi Azure yayımlama oturum gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe871-158">You must sign in to a Microsoft account to publish your site on Azure.</span></span>

![Oturum Aç](publishing/_static/image3.png)

<span data-ttu-id="fe871-160">Microsoft hesabınızda oturum açtıktan sonra iletişim kutusu Azure üzerinde yeni bir site oluşturmak veya var olan Azure sitelerinizde birine bağlanmak için bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="fe871-160">After signing in to your Microsoft account, the dialog box contains links to create a new site on Azure or connect to one of your existing sites on Azure.</span></span>

![Yeni Site oluşturma](publishing/_static/image4.png)

<span data-ttu-id="fe871-162">Seçin **yeni bir site oluştur**.</span><span class="sxs-lookup"><span data-stu-id="fe871-162">Select **Create a new site**.</span></span>

<span data-ttu-id="fe871-163">Projenizi adlandırırsanız **WebPagesMovies**, siteniz için varsayılan adı **webpagesmovies.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="fe871-163">If you named your project **WebPagesMovies**, the default name for your site will be **webpagesmovies.azurewebsites.net**.</span></span> <span data-ttu-id="fe871-164">Bu varsayılan adı büyük olasılıkla kullanılamıyor, kırmızı bir ünlem işareti tarafından belirtildiği şekilde.</span><span class="sxs-lookup"><span data-stu-id="fe871-164">This default name is most likely not available, as indicated by the red exclamation mark.</span></span>

![Varsayılan Web sitesi adı](publishing/_static/image5.png)

<span data-ttu-id="fe871-166">Kullanılabilir olan bir site adı değiştirin ve konumunuz yakın bir konum seçin.</span><span class="sxs-lookup"><span data-stu-id="fe871-166">Change the site name to something that is available, and select a location that is close to your location.</span></span>

![Değiştirilen site adı](publishing/_static/image6.png)

<span data-ttu-id="fe871-168">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fe871-168">Click **OK**.</span></span>

<span data-ttu-id="fe871-169">WebMatrix performss sunucu sitenizle uyumlu olup olmadığını belirlemek için bir test.</span><span class="sxs-lookup"><span data-stu-id="fe871-169">WebMatrix performss a test to determine if the server is compatible with your site.</span></span>

![Uyumluluk testi](publishing/_static/image7.png)

<span data-ttu-id="fe871-171">Seçin **devam**.</span><span class="sxs-lookup"><span data-stu-id="fe871-171">Select **Continue**.</span></span>

<span data-ttu-id="fe871-172">Uyumluluk test sonuçlarını görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fe871-172">The results of the compatibility test are displayed.</span></span>

![Uyumluluk sonucu](publishing/_static/image8.png)

<span data-ttu-id="fe871-174">Seçin **devam**.</span><span class="sxs-lookup"><span data-stu-id="fe871-174">Select **Continue**.</span></span>

<span data-ttu-id="fe871-175">WebMatrix, dosyalar ve siteye yayımlanan veritabanları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fe871-175">WebMatrix displays the files and databases that will be published to the site.</span></span> <span data-ttu-id="fe871-176">Bu sitenin yayımlama ilk kez olduğuna göre tüm dosyalar listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="fe871-176">Since this is the first time you are publishing the site, all of the files are listed.</span></span> <span data-ttu-id="fe871-177">Yayımlanmaya hazır olmayan bir dosya kutusunun işaretini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="fe871-177">You can uncheck a file that is not ready to be published.</span></span> <span data-ttu-id="fe871-178">Sonraki yayınlarda yalnızca değişen dosyaları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fe871-178">In the subsequent publications, only the files that have changed will be displayed.</span></span> <span data-ttu-id="fe871-179">Bkz: [Canlı Site güncelleştiriliyor: yeniden yayımlanması](#update).</span><span class="sxs-lookup"><span data-stu-id="fe871-179">See [Updating the Live Site: Republishing](#update).</span></span>

![Yayımlama önizlemesi](publishing/_static/image9.png)

<span data-ttu-id="fe871-181">Seçin **devam**.</span><span class="sxs-lookup"><span data-stu-id="fe871-181">Select **Continue**.</span></span>

<span data-ttu-id="fe871-182">Site için Azure dağıtıldıktan sonra dağıtımın tamamlandığını bir ileti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fe871-182">After the site has been deployed to Azure, a message is displayed that indicates the deployment has completed.</span></span>

![tam yayımlama](publishing/_static/image10.png)

<span data-ttu-id="fe871-184">Site ve veritabanı için Azure yayımlandı ve ortak kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="fe871-184">Your site and database have been published to Azure, and are now available to the public.</span></span> <span data-ttu-id="fe871-185">Yayımlama tamamladı ve dağıtılan siteniz şimdi göreceksiniz belirten iletiyi bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fe871-185">Click the link in the message indicating publishing has completed, and you will now see your deployed site.</span></span> <span data-ttu-id="fe871-186">Siz veya Internet erişimi olan herkes, ekleyin veya veritabanındaki kayıtları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fe871-186">You or anyone with Internet access can add or modify records in the database.</span></span>

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a><span data-ttu-id="fe871-187">Şirket barındıran bir Web sitenizi yayımlama</span><span class="sxs-lookup"><span data-stu-id="fe871-187">Publishing Your Site to a Web Hosting Company</span></span>

<span data-ttu-id="fe871-188">İçin Azure yayımlama değil karar verirseniz, bunun yerine şirket barındıran bir web sitenizi yayımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe871-188">If you decide to not publish to Azure, you can instead publish your site to a web hosting company.</span></span>

<span data-ttu-id="fe871-189">Tıklatın **web barındırma Bul** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="fe871-189">Click the **Find web hosting** link.</span></span>

![Yayımlama Ayarları iletişim kutusunda 'web barındırma Bul' düğmesini](publishing/_static/image12.png)

<span data-ttu-id="fe871-191">ASP.NET'i destekleyen barındırma hizmeti sağlayıcıları listeler Microsoft sitesinde bir sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="fe871-191">You go to a page on the Microsoft site that lists hosting providers that support ASP.NET.</span></span>

![Barındırma hizmeti sağlayıcıları listeler Microsoft sitesinde sayfası](publishing/_static/image13.png)

<span data-ttu-id="fe871-193">Belli ki, tam olarak uzun vadeli gerektirebilir barındırma hangi özelliklerin biliyorsunuz zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="fe871-193">Obviously, it can be difficult to know now exactly what hosting features you might require over the long term.</span></span> <span data-ttu-id="fe871-194">Birkaç göz önünde bulundurmanız gerekenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="fe871-194">Here are a couple of things to consider:</span></span>

- <span data-ttu-id="fe871-195">WebPagesMovies site amaçları doğrultusunda, genellikle çok maliyetleri SQL Server için ayrı bir eklenti olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fe871-195">For purposes of the WebPagesMovies site, you don't have to have a separate add-on for SQL Server, which often costs extra.</span></span> <span data-ttu-id="fe871-196">Sitenizdeki, kendi içinde bulunan olduğu SQL Server Compact Edition, kullanmakta olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="fe871-196">In your site, you're using SQL Server Compact Edition, which is self-contained.</span></span> <span data-ttu-id="fe871-197">Ancak, bunu bazı gelecekteki Web sitesi iş için SQL Server erişimi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="fe871-197">However, you might need SQL Server access for some future website work you do.</span></span> <span data-ttu-id="fe871-198">Olabilirsiniz düşünüyorsanız, SQL Server özelliği daha sonra ekleyebilirsiniz emin olun.</span><span class="sxs-lookup"><span data-stu-id="fe871-198">If you think you might, make sure that you can add SQL Server capability later.</span></span>
- <span data-ttu-id="fe871-199">Barındırma sağlayıcısı Web dağıtımı yayımlama protokolünü destekleyip desteklemediğini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="fe871-199">Check whether the hosting provider supports the Web Deploy publishing protocol.</span></span> <span data-ttu-id="fe871-200">FTP protokolünü kullanarak yayımlayabilirsiniz, ancak Web dağıtımı kullanabilmeniz daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="fe871-200">You can publish by using FTP protocol, but it's more convenient to use Web Deploy.</span></span>

<span data-ttu-id="fe871-201">Bazı siteler ücretsiz bir deneme süresi sunar.</span><span class="sxs-lookup"><span data-stu-id="fe871-201">Some sites offer a free trial period.</span></span> <span data-ttu-id="fe871-202">Ücretsiz deneme yayımlama denemek için iyi bir yoldur ve basılı barındırma hala denemeler WebMatrix ve ASP.NET Web sayfaları.</span><span class="sxs-lookup"><span data-stu-id="fe871-202">A free trial is a good way to try publishing and hosting while you're still experimenting with WebMatrix and ASP.NET Web Pages.</span></span>

<span data-ttu-id="fe871-203">İstediğiniz bir tane seçin.</span><span class="sxs-lookup"><span data-stu-id="fe871-203">Pick one that you like.</span></span> <span data-ttu-id="fe871-204">Bu öğreticide, öğretici oluşturma olsa da, bu şirket için birkaç ay ücretsiz bir siteyi barındıracak kişilere bir yükseltme sahip olduğundan DiscountASP.NET, seçtik.</span><span class="sxs-lookup"><span data-stu-id="fe871-204">For this tutorial, we selected DiscountASP.NET, because while we were creating the tutorial, that company had a promotion that let people host a site free for a few months.</span></span>

> [!NOTE]
> <span data-ttu-id="fe871-205">Bu öğretici için bir barındırma sağlayıcısına bizim seçimi o şirket bir onay diğer yorumlanacağını döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="fe871-205">Our choice of a hosting provider for this tutorial shouldn't be interpreted as an endorsement of that company over any other.</span></span> <span data-ttu-id="fe871-206">Ancak biz bir çizim için çekme gerekiyordu ve yayımlama için ASP.NET Web sayfaları ve Web dağıtımı protokolünü destekleyen birçok şirket, DiscountASP.NET biridir.</span><span class="sxs-lookup"><span data-stu-id="fe871-206">But we had to pick one for illustration, and DiscountASP.NET is one of the many companies that supports ASP.NET Web Pages and the Web Deploy protocol for publishing.</span></span>


<span data-ttu-id="fe871-207">Genellikle, barındırma sağlayıcı ile oturum açtığınız sonra şirket, bir kullanıcı adı ve parola, web sunucusu ve benzeri URL'sini içeren bir e-posta gönderir.</span><span class="sxs-lookup"><span data-stu-id="fe871-207">Typically, after you've signed up with the hosting provider, the company sends you an email that contains a user name and password, the URL of the web server, and so on.</span></span> <span data-ttu-id="fe871-208">Barındırma şirketi Web dağıtımı protokolünü destekliyorsa, içeren bir dosyayı yayımlama ayarları veya, bir indirmenizi sağlar gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="fe871-208">If the hosting company supports Web Deploy protocol, they might send you a file that contains publish settings, or let you download one.</span></span> <span data-ttu-id="fe871-209">Yayımlama ayarları dosyası işlemi sizin için basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="fe871-209">A publish settings file simplifies the process for you.</span></span>

<span data-ttu-id="fe871-210">Oturum açtığınız ve yayımlamaya hazır olduğunuzda tıklatın **Yayımla** WebMatrix düğmesini.</span><span class="sxs-lookup"><span data-stu-id="fe871-210">When you've signed up and are ready to publish, click the **Publish** button in the WebMatrix ribbon.</span></span> <span data-ttu-id="fe871-211">**Yayımlama ayarları** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fe871-211">The **Publish Settings** dialog box is displayed.</span></span>

<span data-ttu-id="fe871-212">Barındırma sağlayıcısı yayımlama ayarları dosyası gönderirse tıklatın **yayımlama ayarlarını içeri aktarmak** bağlamak ve dosyayı içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="fe871-212">If the hosting provider sent you a publish settings file, click the **Import publish settings** link and import the file.</span></span> <span data-ttu-id="fe871-213">Yayımlama ayarları dosyası yoksa, barındırma şirket e-posta ile gönderilen değerler kullanarak alanları doldurun.</span><span class="sxs-lookup"><span data-stu-id="fe871-213">If you don't have a publish settings file, fill in the fields by using the values that the hosting company sent you in email.</span></span> <span data-ttu-id="fe871-214">İşte **yayımlama ayarları** iletişim kutusu, ne zaman bitirdiniz gibi görünebilir:</span><span class="sxs-lookup"><span data-stu-id="fe871-214">Here's what the **Publish Settings** dialog box might look like when you're done:</span></span>

!['Yayımlama Ayarları' iletişim kutusuna doldurulmuş yayımlama ayarları](publishing/_static/image14.png)

<span data-ttu-id="fe871-216">Tıklatın **bağlantısı doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="fe871-216">Click **Validate Connection**.</span></span> <span data-ttu-id="fe871-217">Her şeyi Tamam olup olmadığını, iletişim kutusunu raporlarını **başarıyla bağlandı**, yani barındırma sağlayıcısının sunucusu ile iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="fe871-217">If everything is ok, the dialog box reports **Connected successfully**, which means it can communicate with the hosting provider's server.</span></span>

![Başarılı olursa iletiyi yayımlama ayarları doğru](publishing/_static/image15.png)

<span data-ttu-id="fe871-219">Bir sorun varsa, WebMatrix sorunun ne olduğunu bildirmek için en iyi yapar:</span><span class="sxs-lookup"><span data-stu-id="fe871-219">If there's a problem, WebMatrix does its best to tell you what the problem is:</span></span>

![Bir sorun olduğunda hata iletisi yayımlama ayarları](publishing/_static/image16.png)

<span data-ttu-id="fe871-221">Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="fe871-221">Click **Save** to save your settings.</span></span> <span data-ttu-id="fe871-222">WebMatrix, bunu doğru şekilde barındırma site ile iletişim kurabildiğinden emin olmak için bir test gerçekleştirmek sunar:</span><span class="sxs-lookup"><span data-stu-id="fe871-222">WebMatrix offers to perform a test to make sure that it can communicate correctly with the hosting site:</span></span>

![İleti yayımlama işlemini bir testi yapmak için sunumu](publishing/_static/image17.png)

<span data-ttu-id="fe871-224">**Evet**'i tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fe871-224">Click **Yes**.</span></span> <span data-ttu-id="fe871-225">WebMatrix, barındırma sağlayıcısının bazı örnek dosya karşıya yükleme.</span><span class="sxs-lookup"><span data-stu-id="fe871-225">WebMatrix uploads some sample files to the hosting provider.</span></span> <span data-ttu-id="fe871-226">Uyumluluk sınama işiniz bittiğinde, WebMatrix sonuçları raporları:</span><span class="sxs-lookup"><span data-stu-id="fe871-226">When the compatibility test is done, WebMatrix reports the results:</span></span>

![Yayımlama test sonuçları](publishing/_static/image18.png)

<span data-ttu-id="fe871-228">Gitmek için hazır olduğunuzda, bir tane tıklatın **devam** yayımlama işlemi gerçekte başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="fe871-228">If you're ready to go, go ahead and click **Continue** to start the publish process for real.</span></span> <span data-ttu-id="fe871-229">WebMatrix, ne dosyalar sitenizdeki ve ana bilgisayar sunucusunda (şu anda hiçbiri) zaten var ve yayımlama işlemi önizlemesini verir çıkışı rakamları:</span><span class="sxs-lookup"><span data-stu-id="fe871-229">WebMatrix figures out what files are in your site and are already on the host server (right now, none) and gives you a preview of the publish process:</span></span>

![Yayımlama işlemi karşıya yükleyecek hangi dosyaların önizlemesini](publishing/_static/image19.png)

<span data-ttu-id="fe871-231">Yayımlamak için dosya listesini gibi oluşturduğunuz web sayfalarını içerir *Movies.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fe871-231">The list of files to publish includes the web pages that you've created like *Movies.cshtml*.</span></span> <span data-ttu-id="fe871-232">Bu liste, dosyaları, yüklediğiniz, Yardımcıları için veritabanınızı SQL Server Compact Edition çalıştırın ve benzeri dosyaları da içerir.</span><span class="sxs-lookup"><span data-stu-id="fe871-232">The list also includes files for helpers that you've installed, the files to run SQL Server Compact Edition for your database, and so on.</span></span> <span data-ttu-id="fe871-233">Sonuç olarak, ilk yayımlama işlemi olabilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fe871-233">As a result, the initial publish process can be substantial.</span></span>

<span data-ttu-id="fe871-234">
              **Devam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fe871-234">Click **Continue**.</span></span> <span data-ttu-id="fe871-235">WebMatrix, dosyalarınızı barındırma sağlayıcısının sunucusuna kopyalar.</span><span class="sxs-lookup"><span data-stu-id="fe871-235">WebMatrix copies your files to the hosting provider's server.</span></span> <span data-ttu-id="fe871-236">Tamamlandığında, sonuçları durum çubuğunda bildirilen:</span><span class="sxs-lookup"><span data-stu-id="fe871-236">When it's done, the results are reported in the status bar:</span></span>

![Yayımlama işlemi başarıyla tamamlandığında durum çubuğu iletisi](publishing/_static/image20.png)

<span data-ttu-id="fe871-238">Canlı sitenizi görmek için durum çubuğunda bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fe871-238">To see your live site, click the link in the status bar.</span></span> <span data-ttu-id="fe871-239">Ekleme *filmler* URL'ye görürsünüz *Movies.cshtml* oluşturduğunuz dosyası:</span><span class="sxs-lookup"><span data-stu-id="fe871-239">Add *Movies* to the URL, and you'll see the *Movies.cshtml* file that you created:</span></span>

![Film sayfasını gösteren Canlı site](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a><span data-ttu-id="fe871-241">Canlı Site güncelleştiriliyor: yeniden yayımlama</span><span class="sxs-lookup"><span data-stu-id="fe871-241">Updating the Live Site: Republishing</span></span>

<span data-ttu-id="fe871-242">(Azure veya şirket barındıran bir web için), sitenizi yayımladıktan sonra iki kopyasını vardır &mdash; bilgisayarınız ve hizmet sağlayıcı sürümünde sürümüne.</span><span class="sxs-lookup"><span data-stu-id="fe871-242">Once you've published your site (to either Azure or a web hosting company), there are two copies of it &mdash; the version on your computer and the version on the service provider.</span></span> <span data-ttu-id="fe871-243">Site geliştirmeye devam istersiniz (başka bir şey varsa sonraki öğretici kümesinin bir parçası olarak).</span><span class="sxs-lookup"><span data-stu-id="fe871-243">You'll probably want to continue developing the site (if nothing else, as part of the next tutorial set).</span></span> <span data-ttu-id="fe871-244">Bunu yaptığınızda, değişiklikler hizmet sağlayıcısına bilgisayarınızdan kopyalamak için sitenizin yeniden yayımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe871-244">When you do, you have to republish your site in order to copy changes from your computer to the service provider.</span></span> <span data-ttu-id="fe871-245">WebMatrix yayımlama işleminde ne sitenizde dosyalar değiştirilmiş belirlemek ve yalnızca bu dosyaları yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="fe871-245">The publish process in WebMatrix can determine what files have changed on your site and publish just those files.</span></span>

<span data-ttu-id="fe871-246">Yeniden yayımlanması nasıl çalıştığını görmek için açın *Movies.cshtml* site, bazı küçük değişiklikler yapın ve ardından dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="fe871-246">To see how republishing works, open the *Movies.cshtml* site, make some small change, and then save the file.</span></span> <span data-ttu-id="fe871-247">Örneğin, başlık değiştirme `Movies - Updated`.</span><span class="sxs-lookup"><span data-stu-id="fe871-247">For example, change the title to `Movies - Updated`.</span></span>

<span data-ttu-id="fe871-248">Tıklatın **Yayımla** düğmesini.</span><span class="sxs-lookup"><span data-stu-id="fe871-248">Click the **Publish** button in the ribbon.</span></span> <span data-ttu-id="fe871-249">WebMatrix, hangi değiştirilir ve bu yayımlayacak dosyaları önizlemesini belirler.</span><span class="sxs-lookup"><span data-stu-id="fe871-249">WebMatrix determines what's changed and shows you a preview of the files it will publish.</span></span>

![Yeniden yayımlanması için hazır dosyalar değiştirilmiş gösteren 'Yayımla' iletişim kutusu](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> <span data-ttu-id="fe871-251">Varsayılan olarak, WebMatrix veritabanınızı yayımlar (*.sdf* dosyası) yalnızca ilk kez site yayımlama.</span><span class="sxs-lookup"><span data-stu-id="fe871-251">By default, WebMatrix publishes your database (*.sdf* file) only the first time you publish the site.</span></span> <span data-ttu-id="fe871-252">Sitenizi yayımlanır ve kişiler Web sitesi ile etkileşim sonra dinamik site veritabanında genellikle sitenin gerçek veri vardır.</span><span class="sxs-lookup"><span data-stu-id="fe871-252">Once your site is published and people are interacting with the website, the database on the live site typically has the site's real data.</span></span> <span data-ttu-id="fe871-253">Canlı veritabanı ile üzerine çok dikkatli olmak zorunda *.sdf* genellikle yalnızca test verilerini içeren, bilgisayarınızda dosya.</span><span class="sxs-lookup"><span data-stu-id="fe871-253">You have to be very careful not to overwrite the live database with the *.sdf* file that's on your computer, which usually contains only test data.</span></span> <span data-ttu-id="fe871-254">İşte bu nedenle uyarı gördüğünüz **yayımlama uzak tüm veritabanlarının üzerine yazılacak**, ve neden onay kutusunu *WebPagesMovies.sdf* varsayılan olarak işaretli değildir.</span><span class="sxs-lookup"><span data-stu-id="fe871-254">That's why you see the warning **Publishing will overwrite any remote databases**, and why the check box for *WebPagesMovies.sdf* is cleared by default.</span></span>


<span data-ttu-id="fe871-255">
              **Devam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fe871-255">Click **Continue**.</span></span> <span data-ttu-id="fe871-256">WebMatrix, değişen dosyaları yayımlar ve yayımladığınız ilk kez olduğu gibi bir başarı iletisi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="fe871-256">WebMatrix publishes the changed files and shows you a success message, like it did the first time you published.</span></span>

<span data-ttu-id="fe871-257">(Tıklayabilirsiniz başarı iletisi bağlantıyı hala gösteriliyorsa) dinamik site gidin ve değişikliğinizi yayınlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="fe871-257">Go to the live site (you can click the link in the success message if it's still showing) and verify that your change has been published.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="fe871-258">**Uzaktan dosyalarını düzenleme**</span><span class="sxs-lookup"><span data-stu-id="fe871-258">**Editing Files Remotely**</span></span>
> 
> <span data-ttu-id="fe871-259">Sitenizi değiştirme ve ardından yeniden yayımlanması için alternatif olarak, WebMatrix uzak dosyalara doğrudan düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe871-259">As an alternative to changing your site and then republishing, you can edit remote files directly in WebMatrix.</span></span> <span data-ttu-id="fe871-260">Bu senaryoda, hizmet sağlayıcısında bir dosya açın ve bir kopyasını düzenleyebilmeniz için WebMatrix indirir.</span><span class="sxs-lookup"><span data-stu-id="fe871-260">In this scenario, you open a file that's on the service provider, and WebMatrix downloads a copy of it for you to edit.</span></span> <span data-ttu-id="fe871-261">Dosyayı kaydedin her zaman, WebMatrix değişiklikleri siteye gönderir.</span><span class="sxs-lookup"><span data-stu-id="fe871-261">Every time you save the file, WebMatrix sends the changes to the site.</span></span>
> 
> <span data-ttu-id="fe871-262">Uzak düzenleme, Canlı sitenize değişiklik yapmak için kolay bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="fe871-262">Remote editing is an easy way to make changes to your live site.</span></span> <span data-ttu-id="fe871-263">Ancak, bu şekilde yaptığınız değişiklikler yerel sitenizi dosyaları ile eşitlenmedi.</span><span class="sxs-lookup"><span data-stu-id="fe871-263">However, the changes you make this way aren't synchronized with the files in your local site.</span></span> <span data-ttu-id="fe871-264">Yerel dosyaları uzak siteyle eşitlemek için Uzak dosyaları indirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe871-264">To synchronize the local files with the remote site, you can download the remote files.</span></span> <span data-ttu-id="fe871-265">Bu işlem yayımlama, benzer dışında geriye doğru çalışır.</span><span class="sxs-lookup"><span data-stu-id="fe871-265">This process works much like publishing, except in reverse.</span></span>
> 
> <span data-ttu-id="fe871-266">Biz daha uzaktan düzenleme ve Uzaktan yükleme özellikleri, WebMatrix hakkında burada açıklayın olmaz.</span><span class="sxs-lookup"><span data-stu-id="fe871-266">We won't describe more about the remote-editing and remote-download facilities of WebMatrix here.</span></span> <span data-ttu-id="fe871-267">Bunlar birden çok kişi farklı bilgisayarlarda aynı sitedeki çalışmak zorunda oldukça yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="fe871-267">They're quite useful if multiple people have to work on the same site on different computers.</span></span> <span data-ttu-id="fe871-268">Daha fazla bilgi için bkz: [yayımlamak ve WebMatrix 2 Beta ile bir uzak Site Düzenle](https://go.microsoft.com/fwlink/?LinkId=251591).</span><span class="sxs-lookup"><span data-stu-id="fe871-268">For more information, see [Publish and Edit a Remote Site with WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="fe871-269">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fe871-269">Additional Resources</span></span>

- <span data-ttu-id="fe871-270">[ASP.NET WebMatrix ASP.NET Web sayfaları Forumu](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), post için uygun bir yerdir sorular ve yanıtlar alın.</span><span class="sxs-lookup"><span data-stu-id="fe871-270">[ASP.NET WebMatrix ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), a great place to post questions and get answers.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fe871-271">Önceki</span><span class="sxs-lookup"><span data-stu-id="fe871-271">Previous</span></span>](layouts.md)
