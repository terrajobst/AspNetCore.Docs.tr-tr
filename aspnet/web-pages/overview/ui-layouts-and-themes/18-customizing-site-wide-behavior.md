---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Site genelinde davranış (Razor) ASP.NET Web sayfaları için özelleştirme | Microsoft Docs
author: tfitzmac
description: Bu bölümde, Web sitenizin tamamını veya bir klasörün tamamına yerine yalnızca bir sayfa ayarları yapmak açıklanmaktadır.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 4457318bcf1d2886eb8ed68fdd795eea7905368b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899207"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a><span data-ttu-id="7169b-103">ASP.NET Web sayfaları (Razor) sitesi için site genelinde davranışını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="7169b-103">Customizing Site-Wide Behavior for ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="7169b-104">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7169b-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7169b-105">Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sayfaları için site tarafı ayarlarını yapma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="7169b-105">This article explains how to make site-side settings for pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="7169b-106">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="7169b-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7169b-107">Kodu çalıştırmak için izin veren nasıl kümesi bir sitedeki tüm sayfalar için (global değerleri veya Yardımcısı ayarları) değerleri.</span><span class="sxs-lookup"><span data-stu-id="7169b-107">How to run code that lets you set values (global values or helper settings) for all pages in a site.</span></span>
> - <span data-ttu-id="7169b-108">Bir klasördeki tüm sayfalar için değerleri ayarlamanıza olanak tanır kodu çalıştırmak nasıl.</span><span class="sxs-lookup"><span data-stu-id="7169b-108">How to run code that lets you set values for all pages in a folder.</span></span>
> - <span data-ttu-id="7169b-109">Önce ve sonra bir sayfa kodu çalıştırmak nasıl yükler.</span><span class="sxs-lookup"><span data-stu-id="7169b-109">How to run code before and after a page loads.</span></span>
> - <span data-ttu-id="7169b-110">Bir merkezi hata sayfası hataları göndermek nasıl.</span><span class="sxs-lookup"><span data-stu-id="7169b-110">How to send errors to a central error page.</span></span>
> - <span data-ttu-id="7169b-111">Bir klasördeki tüm sayfalar için kimlik doğrulaması ekleme yapma.</span><span class="sxs-lookup"><span data-stu-id="7169b-111">How to add authentication to all pages in a folder.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7169b-112">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="7169b-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7169b-113">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="7169b-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="7169b-114">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="7169b-114">WebMatrix 3</span></span>
> - <span data-ttu-id="7169b-115">ASP.NET Web Yardımcıları kitaplığı (NuGet paketi)</span><span class="sxs-lookup"><span data-stu-id="7169b-115">ASP.NET Web Helpers Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="7169b-116">Bu öğretici ASP.NET Web Pages 3 ile de çalışır ve Visual Studio 2013 (veya Visual Studio Express 2013 Web için) dışında ASP.NET Web Yardımcıları kitaplığı kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="7169b-116">This tutorial also works with ASP.NET Web Pages 3 and Visual Studio 2013 (or Visual Studio Express 2013 for Web), except you cannot use the ASP.NET Web Helpers Library.</span></span>


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a><span data-ttu-id="7169b-117">ASP.NET Web sayfaları için Web sitesi başlangıç kod ekleme</span><span class="sxs-lookup"><span data-stu-id="7169b-117">Adding Website Startup Code for ASP.NET Web Pages</span></span>

<span data-ttu-id="7169b-118">Bu sayfa için gerekli olan tüm kod çok ASP.NET Web Pages'da yazdığınız kodun belirli bir sayfayı içerebilir.</span><span class="sxs-lookup"><span data-stu-id="7169b-118">For much of the code that you write in ASP.NET Web Pages, an individual page can contain all the code that's required for that page.</span></span> <span data-ttu-id="7169b-119">Örneğin, bir sayfa bir e-posta iletisi gönderirse, bu işlem için tüm kod tek bir sayfayla put mümkündür.</span><span class="sxs-lookup"><span data-stu-id="7169b-119">For example, if a page sends an email message, it's possible to put all the code for that operation in a single page.</span></span> <span data-ttu-id="7169b-120">Bu e-posta göndermek için ayarları başlatmak için kod içerebilir (diğer bir deyişle, SMTP sunucusu için) ve e-posta iletisi göndermek için.</span><span class="sxs-lookup"><span data-stu-id="7169b-120">This can include the code to initialize the settings for sending email (that is, for the SMTP server) and for sending the email message.</span></span>

<span data-ttu-id="7169b-121">Ancak, bazı durumlarda, sitenin herhangi bir sayfasında çalışmadan önce bazı kodlar çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7169b-121">However, in some situations, you might want to run some code before any page on the site runs.</span></span> <span data-ttu-id="7169b-122">Bu sitede herhangi bir kullanılabilir değerler için faydalı (olarak adlandırılan *global değerleri*.) Örneğin, bazı Yardımcıları e-posta ayarları veya hesabı anahtarları gibi değerler sağlamanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7169b-122">This is useful for setting values that can be used anywhere in the site (referred to as *global values*.) For example, some helpers require you to provide values like email settings or account keys.</span></span> <span data-ttu-id="7169b-123">Bu ayarlar genel değerleri tutmak için kullanışlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="7169b-123">It can be handy to keep these settings in global values.</span></span>

<span data-ttu-id="7169b-124">Adlı bir sayfa oluşturarak bunu yapabilirsiniz  *\_AppStart.cshtml* sitenin kök.</span><span class="sxs-lookup"><span data-stu-id="7169b-124">You can do this by creating a page named *\_AppStart.cshtml* in the root of the site.</span></span> <span data-ttu-id="7169b-125">Bu sayfa varsa, sitedeki herhangi bir sayfayı istenen ilk kez çalışır.</span><span class="sxs-lookup"><span data-stu-id="7169b-125">If this page exists, it runs the first time any page in the site is requested.</span></span> <span data-ttu-id="7169b-126">Bu nedenle, genel değerlerini ayarlamak için kodu çalıştırmak iyi yerdir.</span><span class="sxs-lookup"><span data-stu-id="7169b-126">Therefore, it's a good place to run code to set global values.</span></span> <span data-ttu-id="7169b-127">(Çünkü  *\_AppStart.cshtml* bir alt çizgi ön ekine sahip ASP.NET olmaz Gönder sayfa tarayıcıya kullanıcıların bunu doğrudan istemesi durumunda bile.)</span><span class="sxs-lookup"><span data-stu-id="7169b-127">(Because *\_AppStart.cshtml* has an underscore prefix, ASP.NET won't send the page to a browser even if users request it directly.)</span></span>

<span data-ttu-id="7169b-128">Aşağıdaki diyagramda gösterildiği nasıl  *\_AppStart.cshtml* sayfasında çalışır.</span><span class="sxs-lookup"><span data-stu-id="7169b-128">The following diagram shows how the *\_AppStart.cshtml* page works.</span></span> <span data-ttu-id="7169b-129">Bir sayfa için bir istek geldiğinde ve bu ise ilk istek herhangi sayfasında site, ASP.NET ilk denetimleri olup bir  *\_AppStart.cshtml* sayfa bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="7169b-129">When a request comes in for a page, and if this is the first request for any page in the site, ASP.NET first checks whether a *\_AppStart.cshtml* page exists.</span></span> <span data-ttu-id="7169b-130">Bu durumda, herhangi bir kod  *\_AppStart.cshtml* sayfasında çalıştırır ve ardından istenen sayfa çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="7169b-130">If so, any code in the *\_AppStart.cshtml* page runs, and then the requested page runs.</span></span>

![[Görüntü]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a><span data-ttu-id="7169b-132">Web siteniz için genel değerleri ayarlama</span><span class="sxs-lookup"><span data-stu-id="7169b-132">Setting Global Values for Your Website</span></span>

1. <span data-ttu-id="7169b-133">Bir WebMatrix Web sitesinin kök klasöründe adlı bir dosya oluşturun  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7169b-133">In the root folder of a WebMatrix website, create a file named *\_AppStart.cshtml*.</span></span> <span data-ttu-id="7169b-134">Dosyayı sitenin kök dizininde olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7169b-134">The file must be in the root of the site.</span></span>
2. <span data-ttu-id="7169b-135">Varolan içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-135">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    <span data-ttu-id="7169b-136">Bu kod bir değer depolar `AppState` sitesindeki tüm sayfalara otomatik olarak kullanılabilir sözlük.</span><span class="sxs-lookup"><span data-stu-id="7169b-136">This code stores a value in the `AppState` dictionary, which is automatically available to all pages in the site.</span></span> <span data-ttu-id="7169b-137">Dikkat  *\_AppStart.cshtml* dosya yok herhangi biçimlendirmesi içinde.</span><span class="sxs-lookup"><span data-stu-id="7169b-137">Notice that the *\_AppStart.cshtml* file does not have any markup in it.</span></span> <span data-ttu-id="7169b-138">Sayfa kodu çalıştırın ve başlangıçta istenen sayfasına yeniden yönlendir.</span><span class="sxs-lookup"><span data-stu-id="7169b-138">The page will run the code and then redirect to the page that was originally requested.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7169b-139">Kod geçirdiğinizde dikkatli olun  *\_AppStart.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="7169b-139">Be careful when you put code in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="7169b-140">Kod içinde herhangi bir hata oluşursa  *\_AppStart.cshtml* olmaz dosyası, Web sitesini Başlat.</span><span class="sxs-lookup"><span data-stu-id="7169b-140">If any errors occur in code in the *\_AppStart.cshtml* file, the website won't start.</span></span>
3. <span data-ttu-id="7169b-141">Kök klasöründe adlı yeni bir sayfa oluşturma *AppName.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7169b-141">In the root folder, create a new page named *AppName.cshtml*.</span></span>
4. <span data-ttu-id="7169b-142">Varsayılan biçimlendirme ve kodun aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-142">Replace the default markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    <span data-ttu-id="7169b-143">Bu kod değerinden ayıklar `AppState` içinde ayarladığınız nesne  *\_AppStart.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="7169b-143">This code extracts the value from the `AppState` object that you set in the *\_AppStart.cshtml* page.</span></span>
5. <span data-ttu-id="7169b-144">Çalıştırma *AppName.cshtml* sayfasını bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="7169b-144">Run the *AppName.cshtml* page in a browser.</span></span> <span data-ttu-id="7169b-145">(Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.) Sayfa genel değeri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="7169b-145">(Make sure the page is selected in the **Files** workspace before you run it.) The page displays the global value.</span></span> 

    ![[Görüntü]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a><span data-ttu-id="7169b-147">Ayar değerleri için Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="7169b-147">Setting Values for Helpers</span></span>

<span data-ttu-id="7169b-148">İçin iyi bir kullanım  *\_AppStart.cshtml* dosyasıdır sitenizdeki kullanan ve sahip başlatılması Yardımcıları değerlerini ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="7169b-148">A good use for the *\_AppStart.cshtml* file is to set values for helpers that you use in your site and that have to be initialized.</span></span> <span data-ttu-id="7169b-149">Tipik örnekleridir e-posta ayarlarını `WebMail` Yardımcısı ve özel ve ortak anahtarları `ReCaptcha` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="7169b-149">Typical examples are email settings for the `WebMail` helper and the private and public keys for the `ReCaptcha` helper.</span></span> <span data-ttu-id="7169b-150">Bu gibi durumlarda, bir kez değerler ayarlayabileceğiniz  *\_AppStart.cshtml* ve sonra bunlar zaten tüm sayfalar için sitenizdeki hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="7169b-150">In cases like these, you can set the values once in the *\_AppStart.cshtml* and then they're already set for all the pages in your site.</span></span>

<span data-ttu-id="7169b-151">Bu yordam nasıl ayarlanacağını gösterir `WebMail` ayarları genel.</span><span class="sxs-lookup"><span data-stu-id="7169b-151">This procedure shows you how to set `WebMail` settings globally.</span></span> <span data-ttu-id="7169b-152">(Kullanma hakkında daha fazla bilgi için `WebMail` Yardımcısı, bkz: [e-posta bir ASP.NET Web sayfaları Site Ekleme](../getting-started/11-adding-email-to-your-web-site.md).)</span><span class="sxs-lookup"><span data-stu-id="7169b-152">(For more information about using the `WebMail` helper, see [Adding Email to an ASP.NET Web Pages Site](../getting-started/11-adding-email-to-your-web-site.md).)</span></span>

1. <span data-ttu-id="7169b-153">ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), onu zaten eklemediniz.</span><span class="sxs-lookup"><span data-stu-id="7169b-153">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="7169b-154">Henüz yoksa bir  *\_AppStart.cshtml* dosya, bir Web sitesinin kök klasöründe adlı bir dosya oluşturun  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7169b-154">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
3. <span data-ttu-id="7169b-155">Aşağıdakileri ekleyin `WebMail` ayarlar  *\_AppStart.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7169b-155">Add the following `WebMail` settings to the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    <span data-ttu-id="7169b-156">Değiştirme aşağıdaki e-posta kodu ilgili ayarları:</span><span class="sxs-lookup"><span data-stu-id="7169b-156">Modify the following email related settings in the code:</span></span>

   - <span data-ttu-id="7169b-157">Ayarlama `your-SMTP-host` erişiminiz SMTP sunucusunun adı.</span><span class="sxs-lookup"><span data-stu-id="7169b-157">Set `your-SMTP-host` to the name of the SMTP server that you have access to.</span></span>
   - <span data-ttu-id="7169b-158">Ayarlama `your-user-name-here` SMTP sunucusu hesabının kullanıcı adı.</span><span class="sxs-lookup"><span data-stu-id="7169b-158">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
   - <span data-ttu-id="7169b-159">Ayarlama `your-account-password` SMTP sunucusu hesabının parolasına.</span><span class="sxs-lookup"><span data-stu-id="7169b-159">Set `your-account-password` to the password for your SMTP server account.</span></span>
   - <span data-ttu-id="7169b-160">Ayarlama `your-email-address-here` kendi e-posta adresi.</span><span class="sxs-lookup"><span data-stu-id="7169b-160">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="7169b-161">Bu, ileti gönderilen e-posta adresidir.</span><span class="sxs-lookup"><span data-stu-id="7169b-161">This is the email address that the message is sent from.</span></span> <span data-ttu-id="7169b-162">(Bazı e-posta sağlayıcısı farklı bir belirtmenize izin vermeyin `From` adres ve kullanıcı adı olarak kullanacağı `From` adresi.)</span><span class="sxs-lookup"><span data-stu-id="7169b-162">(Some email providers don't let you specify a different `From` address and will use your user name as the `From` address.)</span></span>

     <span data-ttu-id="7169b-163">SMTP ayarları hakkında daha fazla bilgi için bkz: [e-posta ayarlarını yapılandırma](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) makalede [bir ASP.NET Web sayfaları (Razor) sitesinden e-posta gönderme](https://go.microsoft.com/fwlink/?LinkID=202899) ve [gönderme epostaileilgilisorunları](https://go.microsoft.com/fwlink/?LinkId=253001#email)içinde [ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu](https://go.microsoft.com/fwlink/?LinkId=253001).</span><span class="sxs-lookup"><span data-stu-id="7169b-163">For more information about SMTP settings, see [Configuring Email Settings](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) in the article [Sending Email from an ASP.NET Web Pages (Razor) Site](https://go.microsoft.com/fwlink/?LinkID=202899) and [Issues with Sending Email](https://go.microsoft.com/fwlink/?LinkId=253001#email) in the [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span></span>
4. <span data-ttu-id="7169b-164">Kaydet  *\_AppStart.cshtml* dosya ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="7169b-164">Save the *\_AppStart.cshtml* file and close it.</span></span>
5. <span data-ttu-id="7169b-165">Adlı yeni bir sayfa bir Web sitesinin kök klasörü oluşturmak *TestEmail.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7169b-165">In the root folder of a website, create new page named *TestEmail.cshtml*.</span></span>
6. <span data-ttu-id="7169b-166">Varolan içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-166">Replace the existing content with the following:</span></span> 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. <span data-ttu-id="7169b-167">Çalıştırma *TestEmail.cshtml* sayfasını bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="7169b-167">Run the *TestEmail.cshtml* page in a browser.</span></span>
8. <span data-ttu-id="7169b-168">Kendinize bir e-posta iletisi gönderin ve ardından alanları doldurun **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="7169b-168">Fill in the fields to send yourself an email message and then click **Send**.</span></span>
9. <span data-ttu-id="7169b-169">İleti kabulünüzü emin olmak için e-postanızı kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="7169b-169">Check your email to make sure you've gotten the message.</span></span>

<span data-ttu-id="7169b-170">Bu örnek önemli bir parçası, genellikle değişmez ayarları olan — adını, SMTP sunucunuza ve e-posta kimlik bilgilerinizi ister — ayarlanır  *\_AppStart.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="7169b-170">The important part of this example is that the settings that you don't usually change — like the name of your SMTP server and your email credentials — are set in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="7169b-171">Böylece bunları burada size e-posta göndermek yeniden her sayfasında ayarlamanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="7169b-171">That way you don't need to set them again in each page where you send email.</span></span> <span data-ttu-id="7169b-172">(Bazı nedenlerden dolayı bu ayarları değiştirmeniz gerekiyorsa, bunları tek tek bir sayfada ayarlayabilirsiniz rağmen.) Sayfasında, yalnızca genellikle alıcı ve e-posta iletisinin gövdesi gibi her zaman değiştirme değerlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7169b-172">(Although if for some reason you need to change those settings, you can set them individually in a page.) In the page, you only set the values that typically change each time, like the recipient and the body of the email message.</span></span>

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a><span data-ttu-id="7169b-173">Önce ve sonra bir klasördeki dosyaları çalışan kod</span><span class="sxs-lookup"><span data-stu-id="7169b-173">Running Code Before and After Files in a Folder</span></span>

<span data-ttu-id="7169b-174">Hemen kullanabileceğiniz gibi  *\_AppStart.cshtml* sitedeki sayfalar çalıştırmadan önce kod yazmak için önce (ve sonra) çalışan bir kod yazabilirsiniz çalıştırmak belirli bir klasörde herhangi bir sayfayı.</span><span class="sxs-lookup"><span data-stu-id="7169b-174">Just like you can use *\_AppStart.cshtml* to write code before pages in the site run, you can write code that runs before (and after) any page in a particular folder run.</span></span> <span data-ttu-id="7169b-175">Bu, bir kullanıcı bir sayfa klasöründe çalıştırmadan önce oturum aynı düzeni sayfa bir klasördeki tüm sayfalar için veya denetleme ayarlama gibi şeyler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="7169b-175">This is useful for things like setting the same layout page for all the pages in a folder, or for checking that a user is logged in before running a page in the folder.</span></span>

<span data-ttu-id="7169b-176">Sayfalar için özellikle klasörleri kod adındaki bir dosyada oluşturabileceğiniz  *\_PageStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7169b-176">For pages in particular folders, you can create code in a file named *\_PageStart.cshtml*.</span></span> <span data-ttu-id="7169b-177">Aşağıdaki diyagramda gösterildiği nasıl  *\_PageStart.cshtml* sayfasında çalışır.</span><span class="sxs-lookup"><span data-stu-id="7169b-177">The following diagram shows how the *\_PageStart.cshtml* page works.</span></span> <span data-ttu-id="7169b-178">Bir istek için bir sayfa geldiğinde, ASP.NET ilk denetler bir  *\_AppStart.cshtml* sayfasında ve çalıştıran.</span><span class="sxs-lookup"><span data-stu-id="7169b-178">When a request comes in for a page, ASP.NET first checks for a *\_AppStart.cshtml* page and runs that.</span></span> <span data-ttu-id="7169b-179">ASP.NET olup olmadığını denetler sonra bir  *\_PageStart.cshtml* sayfasında ve Öyleyse, çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="7169b-179">Then ASP.NET checks whether there's a *\_PageStart.cshtml* page, and if so, runs that.</span></span> <span data-ttu-id="7169b-180">Ardından, istenen sayfa çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="7169b-180">It then runs the requested page.</span></span>

<span data-ttu-id="7169b-181">İçinde  *\_PageStart.cshtml* sayfasında, istenen sayfanın dahil ederek çalıştırmasına istediğiniz işleme sırasında nerede belirtebilirsiniz bir `RunPage` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7169b-181">Inside the *\_PageStart.cshtml* page, you can specify where during processing you want the requested page to run by including a `RunPage` method.</span></span> <span data-ttu-id="7169b-182">Bu, istenen sayfa çalışmadan önce kodu çalıştırmak sağlar ve daha sonra yeniden sonra.</span><span class="sxs-lookup"><span data-stu-id="7169b-182">This lets you run code before the requested page runs and then again after it.</span></span> <span data-ttu-id="7169b-183">Eklemezseniz, `RunPage`, tüm kodda  *\_PageStart.cshtml* çalıştırır ve istenen sayfa çalışır otomatik olarak.</span><span class="sxs-lookup"><span data-stu-id="7169b-183">If you don't include `RunPage`, all the code in *\_PageStart.cshtml* runs, and then the requested page runs automatically.</span></span>

![[Görüntü]](18-customizing-site-wide-behavior/_static/image3.jpg)

<span data-ttu-id="7169b-185">ASP.NET hiyerarşisini oluşturmanıza olanak tanır  *\_PageStart.cshtml* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="7169b-185">ASP.NET lets you create a hierarchy of *\_PageStart.cshtml* files.</span></span> <span data-ttu-id="7169b-186">Koyabilirsiniz bir  *\_PageStart.cshtml* sitesinin kök ve herhangi bir alt dosya.</span><span class="sxs-lookup"><span data-stu-id="7169b-186">You can put a *\_PageStart.cshtml* file in the root of the site and in any subfolder.</span></span> <span data-ttu-id="7169b-187">Bir sayfa istendiğinde  *\_PageStart.cshtml* en üst düzey (en yakın bir site kökünde) çalışır ve ardından, dosyayı  *\_PageStart.cshtml* sonraki dosyasında İstenen sayfaya içeren klasörü istek ulaşana kadar vb. alt yapısı aşağı alt.</span><span class="sxs-lookup"><span data-stu-id="7169b-187">When a page is requested, the *\_PageStart.cshtml* file at the top-most level (nearest to the site root) runs, followed by the *\_PageStart.cshtml* file in the next subfolder, and so on down the subfolder structure until the request reaches the folder that contains the requested page.</span></span> <span data-ttu-id="7169b-188">Tüm geçerli sonra  *\_PageStart.cshtml* dosyaları çalıştırıp, istenen sayfa çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7169b-188">After all the applicable *\_PageStart.cshtml* files have run, the requested page runs.</span></span>

<span data-ttu-id="7169b-189">Örneğin, aşağıdaki bileşimi olabilir  *\_PageStart.cshtml* dosyaları ve *Default.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7169b-189">For example, you might have the following combination of *\_PageStart.cshtml* files and *Default.cshtml* file:</span></span>

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

<span data-ttu-id="7169b-190">Çalıştırdığınızda */myfolder/default.cshtml*, aşağıdaki görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="7169b-190">When you run */myfolder/default.cshtml*, you'll see the following:</span></span>

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a><span data-ttu-id="7169b-191">Bir klasördeki tüm sayfalar için başlatma kodu çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7169b-191">Running Initialization Code for All Pages in a Folder</span></span>

<span data-ttu-id="7169b-192">İçin iyi bir kullanım  *\_PageStart.cshtml* dosyaları olan tek bir klasördeki tüm dosyalar aynı düzen sayfasını başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="7169b-192">A good use for *\_PageStart.cshtml* files is to initialize the same layout page for all files in a single folder.</span></span>

1. <span data-ttu-id="7169b-193">Kök klasöründe adlı yeni bir klasör oluşturun *InitPages*.</span><span class="sxs-lookup"><span data-stu-id="7169b-193">In the root folder, create a new folder named *InitPages*.</span></span>
2. <span data-ttu-id="7169b-194">İçinde *InitPages* Web sitenizin klasör adında bir dosya oluşturun  *\_PageStart.cshtml* ve varsayılan biçimlendirme ve kodun şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-194">In the *InitPages* folder of your website, create a file named *\_PageStart.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. <span data-ttu-id="7169b-195">Web sitesinin kök dizininde adlı bir klasör oluşturun *paylaşılan*.</span><span class="sxs-lookup"><span data-stu-id="7169b-195">In the root of the website, create a folder named *Shared*.</span></span>
4. <span data-ttu-id="7169b-196">İçinde *paylaşılan* klasör adında bir dosya oluşturun  *\_Layout1.cshtml* ve varsayılan biçimlendirme ve kodun şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-196">In the *Shared* folder, create a file named *\_Layout1.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. <span data-ttu-id="7169b-197">İçinde *InitPages* klasör adında bir dosya oluşturun *Content1.cshtml* ve varolan içeriği şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-197">In the *InitPages* folder, create a file named *Content1.cshtml* and replace the existing content with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. <span data-ttu-id="7169b-198">İçinde *InitPages* klasör adında başka bir dosya oluşturun *Content2.cshtml* ve varsayılan biçimlendirme şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-198">In the *InitPages* folder, create another file named *Content2.cshtml* and replace the default markup with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. <span data-ttu-id="7169b-199">Çalıştırma *Content1.cshtml* bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="7169b-199">Run *Content1.cshtml* in a browser.</span></span> 

    ![[Görüntü]](18-customizing-site-wide-behavior/_static/image4.jpg)

    <span data-ttu-id="7169b-201">Zaman *Content1.cshtml* sayfa çalıştığında,  *\_PageStart.cshtml* dosya kümeleri `Layout` ve ayrıca ayarlar `PageData["MyBackground"]` bir renk.</span><span class="sxs-lookup"><span data-stu-id="7169b-201">When the *Content1.cshtml* page runs, the *\_PageStart.cshtml* file sets `Layout` and also sets `PageData["MyBackground"]` to a color.</span></span> <span data-ttu-id="7169b-202">İçinde *Content1.cshtml*, rengini ve düzenini uygulanır.</span><span class="sxs-lookup"><span data-stu-id="7169b-202">In *Content1.cshtml*, the layout and color are applied.</span></span>
8. <span data-ttu-id="7169b-203">Görüntü *Content2.cshtml* bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="7169b-203">Display *Content2.cshtml* in a browser.</span></span> 

    <span data-ttu-id="7169b-204">Her iki sayfa içinde başlatılan gibi aynı düzen sayfasını ve renk kullan düzeni aynı olduğundan  *\_PageStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7169b-204">The layout is the same, because both pages use the same layout page and color as initialized in *\_PageStart.cshtml*.</span></span>

## <a name="using-pagestartcshtml-to-handle-errors"></a><span data-ttu-id="7169b-205">Kullanarak \_hataları işlemek için PageStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="7169b-205">Using \_PageStart.cshtml to Handle Errors</span></span>

<span data-ttu-id="7169b-206">Başka bir iyi kullanmak için  *\_PageStart.cshtml* dosyasıdır (özel durumlar) birinde oluşabilir programlama hataları işlemek için bir yol oluşturmak için *.cshtml* bir klasörde sayfa.</span><span class="sxs-lookup"><span data-stu-id="7169b-206">Another good use for the *\_PageStart.cshtml* file is to create a way to handle programming errors (exceptions) that might occur in any *.cshtml* page in a folder.</span></span> <span data-ttu-id="7169b-207">Bu örnek, bunu yapmanın bir yolu gösterir.</span><span class="sxs-lookup"><span data-stu-id="7169b-207">This example shows you one way to do this.</span></span>

1. <span data-ttu-id="7169b-208">Kök klasöründe adlı bir klasör oluşturun *InitCatch*.</span><span class="sxs-lookup"><span data-stu-id="7169b-208">In the root folder, create a folder named *InitCatch*.</span></span>
2. <span data-ttu-id="7169b-209">İçinde *InitCatch* Web sitenizin klasör adında bir dosya oluşturun  *\_PageStart.cshtml* ve varolan biçimlendirme ve kodun şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-209">In the *InitCatch* folder of your website, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    <span data-ttu-id="7169b-210">Bu kodda, istenen sayfa açıkça çağırarak çalıştırmayı deneyin. `RunPage` yönteminin içinde bir `try` bloğu.</span><span class="sxs-lookup"><span data-stu-id="7169b-210">In this code, you try running the requested page explicitly by calling the `RunPage` method inside a `try` block.</span></span> <span data-ttu-id="7169b-211">Herhangi bir programlama hata istenen oluşursa sayfası, içinde kod `catch` engelleme çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="7169b-211">If any programming errors occur in the requested page, the code inside the `catch` block runs.</span></span> <span data-ttu-id="7169b-212">Bu durumda, kodu bir sayfasına yönlendirir (*Error.cshtml*) ve URL parçası olarak bir hatayla karşılaştı dosyasının adını geçirir.</span><span class="sxs-lookup"><span data-stu-id="7169b-212">In this case, the code redirects to a page (*Error.cshtml*) and passes the name of the file that experienced the error as part of the URL.</span></span> <span data-ttu-id="7169b-213">(Kısa süre içinde sayfa oluşturacaksınız.)</span><span class="sxs-lookup"><span data-stu-id="7169b-213">(You'll create the page shortly.)</span></span>
3. <span data-ttu-id="7169b-214">İçinde *InitCatch* Web sitenizin klasör adında bir dosya oluşturun *Exception.cshtml* ve varolan biçimlendirme ve kodun şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-214">In the *InitCatch* folder of your website, create a file named *Exception.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    <span data-ttu-id="7169b-215">Bu örneğin amaçları doğrultusunda, bu sayfada gerçekleştirmekte olduğunuz kasıtlı olarak bir hata var olmayan bir veritabanı dosyasını açmaya çalışırken tarafından oluşturuyor.</span><span class="sxs-lookup"><span data-stu-id="7169b-215">For purposes of this example, what you're doing in this page is deliberately creating an error by trying to open a database file that doesn't exist.</span></span>
4. <span data-ttu-id="7169b-216">Kök klasöründe adlı bir dosya oluşturun *Error.cshtml* ve varolan biçimlendirme ve kodun şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-216">In the root folder, create a file named *Error.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    <span data-ttu-id="7169b-217">Bu sayfadaki ifade `@Request["source"]` URL dışında bir değer alır ve görüntüler.</span><span class="sxs-lookup"><span data-stu-id="7169b-217">In this page, the expression `@Request["source"]` gets the value out of the URL and displays it.</span></span>
5. <span data-ttu-id="7169b-218">Araç çubuğunda **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="7169b-218">In the toolbar, click **Save**.</span></span>
6. <span data-ttu-id="7169b-219">Çalıştırma *Exception.cshtml* bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="7169b-219">Run *Exception.cshtml* in a browser.</span></span> 

    ![[Görüntü]](18-customizing-site-wide-behavior/_static/image5.jpg)

    <span data-ttu-id="7169b-221">Bir hata oluştuğundan *Exception.cshtml*,  *\_PageStart.cshtml* sayfa yeniden yönlendirmeleri için *Error.cshtml* iletisini görüntüler dosya.</span><span class="sxs-lookup"><span data-stu-id="7169b-221">Because an error occurs in *Exception.cshtml*, the *\_PageStart.cshtml* page redirects to the *Error.cshtml* file, which displays the message.</span></span>

    <span data-ttu-id="7169b-222">Özel durumlar hakkında daha fazla bilgi için bkz: [ASP.NET Web sayfalarını programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkID=251587).</span><span class="sxs-lookup"><span data-stu-id="7169b-222">For more information about exceptions, see [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).</span></span>

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a><span data-ttu-id="7169b-223">Kullanarak \_PageStart.cshtml klasörüne erişimi kısıtlama</span><span class="sxs-lookup"><span data-stu-id="7169b-223">Using \_PageStart.cshtml to Restrict Folder Access</span></span>

<span data-ttu-id="7169b-224">Aynı zamanda  *\_PageStart.cshtml* bir klasördeki tüm dosyalara erişimi kısıtlamak için dosya.</span><span class="sxs-lookup"><span data-stu-id="7169b-224">You can also use the *\_PageStart.cshtml* file to restrict access to all the files in a folder.</span></span>

1. <span data-ttu-id="7169b-225">Kullanarak yeni bir Web sitesini Webmatrix'te oluşturmak **Site şablondan** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="7169b-225">In WebMatrix, create a new website using the **Site From Template** option.</span></span>
2. <span data-ttu-id="7169b-226">Kullanılabilir şablonları arasından seçim **başlangıç sitesi**.</span><span class="sxs-lookup"><span data-stu-id="7169b-226">From the available templates, select **Starter Site**.</span></span>
3. <span data-ttu-id="7169b-227">Kök klasöründe adlı bir klasör oluşturun *AuthenticatedContent*.</span><span class="sxs-lookup"><span data-stu-id="7169b-227">In the root folder, create a folder named *AuthenticatedContent*.</span></span>
4. <span data-ttu-id="7169b-228">İçinde *AuthenticatedContent* klasör adında bir dosya oluşturun  *\_PageStart.cshtml* ve varolan biçimlendirme ve kodun şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-228">In the *AuthenticatedContent* folder, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    <span data-ttu-id="7169b-229">Kod, klasördeki tüm dosyaları önbelleğe alınması engelleyerek başlatır.</span><span class="sxs-lookup"><span data-stu-id="7169b-229">The code starts by preventing all files in the folder from being cached.</span></span> <span data-ttu-id="7169b-230">(Bu ortak bilgisayarlar, bir kullanıcının önbelleğe alınan sayfaları sonraki kullanıcı için kullanılabilir olmasını istediğiniz yok gibi senaryolar için gereklidir.) Ardından, kod sayfalar klasöründe görüntüleyebilmeniz için önce kullanıcı siteye oturum açtığı olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="7169b-230">(This is required for scenarios like public computers, where you don't want one user's cached pages to be available to the next user.) Next, the code determines whether the user has signed in to the site before they can view any of the pages in the folder.</span></span> <span data-ttu-id="7169b-231">Kullanıcı imzalı değilse, kod oturum açma sayfasına yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="7169b-231">If the user is not signed in, the code redirects to the login page.</span></span> <span data-ttu-id="7169b-232">Oturum açma sayfasına kullanıcı adlı bir sorgu dizesi değeri eklerseniz, başlangıçta istenen sayfaya dönmek `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="7169b-232">The login page can return the user to the page that was originally requested if you include a query string value named `ReturnUrl`.</span></span>
5. <span data-ttu-id="7169b-233">Yeni bir sayfa oluşturma *AuthenticatedContent* adlı klasörü *Page.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7169b-233">Create a new page in the *AuthenticatedContent* folder named *Page.cshtml*.</span></span>
6. <span data-ttu-id="7169b-234">Varsayılan biçimlendirme aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7169b-234">Replace the default markup with the following:</span></span>  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. <span data-ttu-id="7169b-235">Çalıştırma *Page.cshtml* bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="7169b-235">Run *Page.cshtml* in a browser.</span></span> <span data-ttu-id="7169b-236">Kod bir oturum açma sayfasına yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="7169b-236">The code redirects you to a login page.</span></span> <span data-ttu-id="7169b-237">Oturum açmayı önce kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="7169b-237">You must register before logging in.</span></span> <span data-ttu-id="7169b-238">Kayıtlı ve oturum açtıktan sonra sayfasına gidin ve içeriğini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="7169b-238">After you've registered and logged in, you can navigate to the page and view its contents.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="7169b-239">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7169b-239">Additional Resources</span></span>

[<span data-ttu-id="7169b-240">ASP.NET Web sayfaları Razor sözdizimini kullanarak programlama giriş</span><span class="sxs-lookup"><span data-stu-id="7169b-240">Introduction to ASP.NET Web Pages Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=251587)
