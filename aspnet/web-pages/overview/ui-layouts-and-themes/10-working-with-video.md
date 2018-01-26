---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: "Video görüntüleme bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs"
author: tfitzmac
description: "Bu bölümde, bir ASP.NET Web Pages'de Razor sözdizimi sayfasıyla video görüntülemek açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: a14659997d86d1b5cf5381e21e997c1a03a3f57c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="25d2e-103">Bir ASP.NET Web sayfaları (Razor) sitesinde video görüntüleme</span><span class="sxs-lookup"><span data-stu-id="25d2e-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="25d2e-104">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="25d2e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="25d2e-105">Bu makalede, bir video (medya) oynatıcı bir ASP.NET Web sayfaları (Razor) Web sitesinde depolanan videoları izleyin, kullanıcıların izin için nasıl kullanılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="25d2e-106">Razor sözdizimi ile ASP.NET Web sayfaları Flash yürütmek olanak tanır (*.swf*), Media Player (*.wmv*) ve Silverlight (*.xap*) videolar.</span><span class="sxs-lookup"><span data-stu-id="25d2e-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="25d2e-107">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="25d2e-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="25d2e-108">Bir video oynatıcı seçmek nasıl.</span><span class="sxs-lookup"><span data-stu-id="25d2e-108">How to choose a video player.</span></span>
> - <span data-ttu-id="25d2e-109">Video bir web sayfasına ekleme.</span><span class="sxs-lookup"><span data-stu-id="25d2e-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="25d2e-110">Video oynatıcı öznitelikleri nasıl kurulur.</span><span class="sxs-lookup"><span data-stu-id="25d2e-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="25d2e-111">ASP.NET Razor bunlar sayfaları makalesinde sunulan özellikler:</span><span class="sxs-lookup"><span data-stu-id="25d2e-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="25d2e-112">`Video` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="25d2e-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="25d2e-113">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="25d2e-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="25d2e-114">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="25d2e-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="25d2e-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="25d2e-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="25d2e-116">Bu öğretici, WebMatrix 3 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="25d2e-117">Giriş</span><span class="sxs-lookup"><span data-stu-id="25d2e-117">Introduction</span></span>

<span data-ttu-id="25d2e-118">Sitenizdeki bir video görüntülemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="25d2e-118">You might want to display a video on your site.</span></span> <span data-ttu-id="25d2e-119">Bunu yapmak için bir video YouTube'da gibi zaten olan bir siteye bağlamak için yoludur.</span><span class="sxs-lookup"><span data-stu-id="25d2e-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="25d2e-120">Kendi sayfaları doğrudan bu sitelerden bir videoyu katıştırmak istiyorsanız, genellikle HTML biçimlendirmesi sitesinden almak ve sayfanıza kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="25d2e-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="25d2e-121">Örneğin, aşağıdaki örnekte bir YouTube katıştırmak video gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="25d2e-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="25d2e-122">Kendi Web sitesi (olmayan bir sitede genel video paylaşımı) açık bir video oynatmak istiyorsanız, doğrudan şöyle katıştırılmış biçimlendirme kullanarak kendisine bağlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="25d2e-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="25d2e-123">Ancak, videolar sitenizden kullanarak yürütebilirsiniz `Video` media player doğrudan sayfasındaki işler Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="25d2e-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="25d2e-124">Bir Video Oynatıcı seçme</span><span class="sxs-lookup"><span data-stu-id="25d2e-124">Choosing a Video Player</span></span>

<span data-ttu-id="25d2e-125">Çok sayıda video dosyaları için biçimleri vardır ve her biçimi, genellikle farklı bir yürütücü ve player yapılandırmak için farklı bir şekilde gerektirir.</span><span class="sxs-lookup"><span data-stu-id="25d2e-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="25d2e-126">ASP.NET Razor sayfalarında, bir web sayfasını kullanarak bir video yürütebilirsiniz `Video` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="25d2e-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="25d2e-127">`Video` Yardımcı otomatik olarak oluşturması nedeniyle bir web sayfasında videolar katıştırma sürecini basitleştirir `object` ve `embed` normalde video eklemek için kullanılan HTML öğeleri.</span><span class="sxs-lookup"><span data-stu-id="25d2e-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="25d2e-128">`Video` Yardımcı aşağıdaki medya oynatıcıları destekler:</span><span class="sxs-lookup"><span data-stu-id="25d2e-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="25d2e-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="25d2e-129">Adobe Flash</span></span>
- <span data-ttu-id="25d2e-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="25d2e-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="25d2e-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="25d2e-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="25d2e-132">Flash Player</span><span class="sxs-lookup"><span data-stu-id="25d2e-132">The Flash Player</span></span>

<span data-ttu-id="25d2e-133">`Flash` Oynatıcı `Video` yardımcı Flash videoları oynat olanak tanır (*.swf* dosyaları) bir web sayfasında.</span><span class="sxs-lookup"><span data-stu-id="25d2e-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="25d2e-134">En azından bir video dosyası yolunu girmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="25d2e-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="25d2e-135">Yolun ancak hiçbir şey belirtirseniz, player Flash geçerli sürümü tarafından belirlenen varsayılan değerleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="25d2e-136">Genel varsayılan ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="25d2e-136">Typical default settings are:</span></span>

- <span data-ttu-id="25d2e-137">Video varsayılan genişlik ve yükseklik kullanılarak görüntülenir ve olmadan bir arka plan rengi.</span><span class="sxs-lookup"><span data-stu-id="25d2e-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="25d2e-138">Sayfa yüklendiğinde video otomatik olarak oynatılır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="25d2e-139">Sürekli olarak açıkça durdurulana kadar video döngüye girer.</span><span class="sxs-lookup"><span data-stu-id="25d2e-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="25d2e-140">Video tümünü belirli bir boyuta sığması için video kırpma yerine video gösterecek şekilde ölçeklendirilir.</span><span class="sxs-lookup"><span data-stu-id="25d2e-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="25d2e-141">Video bir pencerede oynatılır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="25d2e-142">MediaPlayer Player</span><span class="sxs-lookup"><span data-stu-id="25d2e-142">The MediaPlayer Player</span></span>

<span data-ttu-id="25d2e-143">`MediaPlayer` Oynatıcı `Video` yardımcı Windows Media videoları oynat olanak tanır (*.wmv* dosyaları), Windows Media Ses (*.wma* dosyaları) ve MP3 (*.mp3* bir web sayfasında dosyaları).</span><span class="sxs-lookup"><span data-stu-id="25d2e-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="25d2e-144">Yürütmek için medya dosyasının yolunu içermelidir; diğer tüm parametreler isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="25d2e-145">Yalnızca bir yol belirtirseniz, player MediaPlayer, geçerli sürümü tarafından ayarlamak varsayılan ayarları kullanır:</span><span class="sxs-lookup"><span data-stu-id="25d2e-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="25d2e-146">Video varsayılan genişlik ve yükseklik kullanılarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="25d2e-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="25d2e-147">Sayfa yüklendiğinde video otomatik olarak oynatılır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="25d2e-148">Video kez çalınır (döngü değil).</span><span class="sxs-lookup"><span data-stu-id="25d2e-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="25d2e-149">Player kullanıcı arabiriminde denetimleri kümesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="25d2e-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="25d2e-150">Bir pencerede video oynadığı.</span><span class="sxs-lookup"><span data-stu-id="25d2e-150">The video plays in in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="25d2e-151">Silverlight Player</span><span class="sxs-lookup"><span data-stu-id="25d2e-151">The Silverlight Player</span></span>

<span data-ttu-id="25d2e-152">`Silverlight` Oynatıcı `Video` yardımcı Windows Media çalmasına olanak tanır (*.wmv* dosyaları), Windows Media Ses (*.wma* dosyaları) ve MP3 (*.mp3* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="25d2e-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="25d2e-153">Path parametresi bir Silverlight tabanlı bir uygulama paketi işaret edecek şekilde ayarlamanız gerekir (*.xap* dosyası).</span><span class="sxs-lookup"><span data-stu-id="25d2e-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="25d2e-154">Width ve height parametreleri de ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="25d2e-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="25d2e-155">Diğer tüm parametreler isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-155">All other parameters are optional.</span></span> <span data-ttu-id="25d2e-156">Video için Silverlight player kullandığınızda, yalnızca gerekli parametreleri ayarlarsanız Silverlight player arka plan rengi olmadan videoyu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="25d2e-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="25d2e-157">Silverlight zaten tanımadığınız durumda: *.xap* dosyasıdır düzeni yönergeleri içeren bir sıkıştırılmış dosyayı bir *.xaml* dosya, yönetilen kodda derlemeler ve isteğe bağlı kaynaklar.</span><span class="sxs-lookup"><span data-stu-id="25d2e-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="25d2e-158">Oluşturabileceğiniz bir *.xap* bir Silverlight uygulaması projesi olarak Visual Studio dosyasında.</span><span class="sxs-lookup"><span data-stu-id="25d2e-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="25d2e-159">`Silverlight` Video oynatıcı kullanan iki player için sağladığınız ayarları ve içinde sağlanan ayarları *.xap* dosya.</span><span class="sxs-lookup"><span data-stu-id="25d2e-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="25d2e-160">MIME türleri</span><span class="sxs-lookup"><span data-stu-id="25d2e-160">MIME Types</span></span>
> 
> <span data-ttu-id="25d2e-161">Bir tarayıcı bir dosyayı açtığında, tarayıcı dosya türü için işlenen belge belirtilen MIME türü eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="25d2e-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="25d2e-162">MIME türü içerik türü veya medya dosyasının türüdür.</span><span class="sxs-lookup"><span data-stu-id="25d2e-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="25d2e-163">`Video` Yardımcı aşağıdaki MIME türlerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="25d2e-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="25d2e-164">Flash (.swf) video oynatma</span><span class="sxs-lookup"><span data-stu-id="25d2e-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="25d2e-165">Bu yordamda adlı bir Flash çalmasına gösterilmiştir *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="25d2e-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="25d2e-166">Adlı bir klasör olduğuna yordam varsayar *medya* siteniz ve, *.swf* o klasördeki bir dosyadır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="25d2e-167">ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), onu zaten eklemediniz.</span><span class="sxs-lookup"><span data-stu-id="25d2e-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="25d2e-168">Web sitesi, bir sayfa ekleyin ve adını *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25d2e-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="25d2e-169">Aşağıdaki biçimlendirmede sayfasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="25d2e-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="25d2e-170">Bir tarayıcıda. Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="25d2e-170">Run the page in a browser.</span></span> <span data-ttu-id="25d2e-171">(Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.) Sayfası görüntülenir ve video otomatik olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="25d2e-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="25d2e-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="25d2e-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="25d2e-173">Ayarlayabileceğiniz `quality` parametresi için bir Flash video `low`, `autolow`, `autohigh`, `medium`, `high`, ve `best`:</span><span class="sxs-lookup"><span data-stu-id="25d2e-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="25d2e-174">Bir özel boyutu kullanarak yürütmek için Flash video değiştirebilirsiniz `scale` şu şekilde ayarlayabilirsiniz parametre:</span><span class="sxs-lookup"><span data-stu-id="25d2e-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="25d2e-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="25d2e-175">`showall`.</span></span> <span data-ttu-id="25d2e-176">Bu videonun tamamını görünür özgün en boy oranını koruyarak yapar.</span><span class="sxs-lookup"><span data-stu-id="25d2e-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="25d2e-177">Bununla birlikte, her iki taraftaki Kenarlıklar şunun.</span><span class="sxs-lookup"><span data-stu-id="25d2e-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="25d2e-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="25d2e-178">`noorder`.</span></span> <span data-ttu-id="25d2e-179">Özgün en boy oranını korurken bu videoyu ölçeklendirir ancak kırpılmış.</span><span class="sxs-lookup"><span data-stu-id="25d2e-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="25d2e-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="25d2e-180">`exactfit`.</span></span> <span data-ttu-id="25d2e-181">Bu videonun tamamını görünür özgün en boy oranını koruyarak olmadan sağlar, ancak bozulma oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="25d2e-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="25d2e-182">Belirtmediyseniz bir `scale` parametresi, tüm videoyu görünür olur ve özgün en boy oranını hiçbir kırpma olmadan korunur.</span><span class="sxs-lookup"><span data-stu-id="25d2e-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="25d2e-183">Aşağıdaki örnekte nasıl kullanılacağını gösterir `scale` parametre:</span><span class="sxs-lookup"><span data-stu-id="25d2e-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="25d2e-184">Görüntü modu adlandırılmış ayarı Flash player destekleyen `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="25d2e-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="25d2e-185">Bu ayar `window`, `opaque`, ve `transparent`.</span><span class="sxs-lookup"><span data-stu-id="25d2e-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="25d2e-186">Varsayılan olarak, `windowMode` ayarlanır `window`, web sayfasında ayrı bir pencerede video görüntüleyen.</span><span class="sxs-lookup"><span data-stu-id="25d2e-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="25d2e-187">`opaque` Ayar her şeyi web sayfasında video arkasına gizler.</span><span class="sxs-lookup"><span data-stu-id="25d2e-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="25d2e-188">`transparent` Ayarı, video, herhangi bir bölümünü saydam olduğunu varsayarak video Göster arka plan web sayfasının olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="25d2e-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="25d2e-189">MediaPlayer çalma (*.wmv*) videolar</span><span class="sxs-lookup"><span data-stu-id="25d2e-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="25d2e-190">Aşağıdaki yordamda adlı bir pencere medya çalmasına gösterilmiştir *sample.wmv* alanında *medya* klasör.</span><span class="sxs-lookup"><span data-stu-id="25d2e-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="25d2e-191">ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="25d2e-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="25d2e-192">Adlı yeni bir sayfa oluşturma *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25d2e-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="25d2e-193">Aşağıdaki biçimlendirmede sayfasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="25d2e-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="25d2e-194">Bir tarayıcıda. Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="25d2e-194">Run the page in a browser.</span></span> <span data-ttu-id="25d2e-195">Video yükler ve otomatik olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="25d2e-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="25d2e-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="25d2e-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="25d2e-197">Ayarlayabileceğiniz `playCount` otomatik olarak çalmasına kaç kez belirten bir tamsayı için:</span><span class="sxs-lookup"><span data-stu-id="25d2e-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="25d2e-198">`uiMode` Parametresi hangi denetimlerin kullanıcı arabiriminde gösterilmesi belirtmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="25d2e-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="25d2e-199">Ayarlayabileceğiniz `uiMode` için `invisible`, `none`, `mini`, veya `full`.</span><span class="sxs-lookup"><span data-stu-id="25d2e-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="25d2e-200">Belirtmediyseniz bir `uiMode` parametresi, video olacaktır durum penceresi görüntülenir, arama çubuğunda, düğmeler ve ses denetimlerini video penceresinin yanı sıra denetim.</span><span class="sxs-lookup"><span data-stu-id="25d2e-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="25d2e-201">Bir ses dosyasını oynatmak için player kullanırsanız, bu denetimlerin da görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="25d2e-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="25d2e-202">İşte nasıl kullanılacağına ilişkin bir örnek `uiMode` parametre:</span><span class="sxs-lookup"><span data-stu-id="25d2e-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="25d2e-203">Video yürüttüğünde varsayılan olarak, ses açıktır.</span><span class="sxs-lookup"><span data-stu-id="25d2e-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="25d2e-204">Ayarlayarak sesi `mute` parametresi true olarak:</span><span class="sxs-lookup"><span data-stu-id="25d2e-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="25d2e-205">Ayarlayarak MediaPlayer video ses düzeyini denetleyebilirsiniz `volume` parametresi için 0 ile 100 arasında bir değer.</span><span class="sxs-lookup"><span data-stu-id="25d2e-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="25d2e-206">Varsayılan değer 50'dir.</span><span class="sxs-lookup"><span data-stu-id="25d2e-206">The default value is 50.</span></span> <span data-ttu-id="25d2e-207">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="25d2e-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="25d2e-208">Silverlight video oynatma</span><span class="sxs-lookup"><span data-stu-id="25d2e-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="25d2e-209">Bu yordamda yer alan bir Silverlight çalmasına gösterilmiştir *.xap* bir klasörde adlı sayfa *medya*.</span><span class="sxs-lookup"><span data-stu-id="25d2e-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="25d2e-210">ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="25d2e-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="25d2e-211">Adlı yeni bir sayfa oluşturma *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25d2e-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="25d2e-212">Aşağıdaki biçimlendirmede sayfasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="25d2e-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="25d2e-213">Bir tarayıcıda. Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="25d2e-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="25d2e-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="25d2e-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="25d2e-215">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="25d2e-215">Additional Resources</span></span>


<span data-ttu-id="25d2e-216">[Silverlight genel bakış](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="25d2e-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="25d2e-217">Nesne ve EMBED etiket özniteliklerini flash</span><span class="sxs-lookup"><span data-stu-id="25d2e-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="25d2e-218">[Windows Media Player 11 SDK PARAM etiketleri](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="25d2e-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
