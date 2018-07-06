---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Video görüntüleme bir ASP.NET Web sayfaları (Razor) sitesinde | Microsoft Docs
author: tfitzmac
description: Bu bölümde, bir ASP.NET Web Pages'de Razor söz dizimi sayfası ile videoyu görüntülemek açıklanmaktadır.
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 43a806bb5419d32eb8b03e3fc6de05af5d9e2427
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826111"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="24df7-103">Bir ASP.NET Web sayfaları (Razor) sitesinde video görüntüleme</span><span class="sxs-lookup"><span data-stu-id="24df7-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="24df7-104">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="24df7-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="24df7-105">Bu makalede, kullanıcıların sitesinde depolanan videosu görüntüleyin izin vermek için bir ASP.NET Web sayfaları (Razor) Web sitesinde (ortam) video oynatıcı kullanmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="24df7-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="24df7-106">Razor sözdizimi olan ASP.NET Web sayfaları Flash play olanak tanır (*.swf*), Media Player (*.wmv*) ve Silverlight (*.xap*) videolar.</span><span class="sxs-lookup"><span data-stu-id="24df7-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="24df7-107">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="24df7-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="24df7-108">Bir video oynatıcı nasıl seçeceğinizi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="24df7-108">How to choose a video player.</span></span>
> - <span data-ttu-id="24df7-109">Video bir web sayfasına ekleme.</span><span class="sxs-lookup"><span data-stu-id="24df7-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="24df7-110">Nasıl video oynatıcı özniteliklerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="24df7-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="24df7-111">Bunlar, ASP.NET Razor sayfaları makalesinde sunulan özellikleri:</span><span class="sxs-lookup"><span data-stu-id="24df7-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="24df7-112">`Video` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="24df7-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="24df7-113">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="24df7-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="24df7-114">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="24df7-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="24df7-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="24df7-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="24df7-116">Bu öğreticide, WebMatrix 3'ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="24df7-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="24df7-117">Giriş</span><span class="sxs-lookup"><span data-stu-id="24df7-117">Introduction</span></span>

<span data-ttu-id="24df7-118">Sitenizde bir videoyu görüntülemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="24df7-118">You might want to display a video on your site.</span></span> <span data-ttu-id="24df7-119">Bunu yapmanın bir yolu gibi YouTube video zaten bir siteye bağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="24df7-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="24df7-120">Kendi sayfaları doğrudan bu sitelerden video eklemek istiyorsanız, genellikle bir HTML biçimlendirmesi sitesinden almak ve sayfanıza kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="24df7-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="24df7-121">Örneğin, aşağıdaki örnekte bir YouTube ekleme video gösterir:</span><span class="sxs-lookup"><span data-stu-id="24df7-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="24df7-122">Kendi Web (değil bir sitede genel bir video paylaşım) olan bir videoyu oynatmak istiyorsanız, doğrudan katıştırılmış biçimlendirme şunun gibi kullanarak kendisine bağlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="24df7-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="24df7-123">Sitenizden kullanarak videoları ancak oynatabilen `Video` bir medya oynatıcı sayfasındaki doğrudan işleyen Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="24df7-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="24df7-124">Bir Video Oynatıcı seçme</span><span class="sxs-lookup"><span data-stu-id="24df7-124">Choosing a Video Player</span></span>

<span data-ttu-id="24df7-125">Video dosyaları için biçimler çok sayıda vardır ve her biçim genellikle farklı bir yürütücü ve oyuncu yapılandırmak için farklı bir yol gereklidir.</span><span class="sxs-lookup"><span data-stu-id="24df7-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="24df7-126">ASP.NET Razor sayfaları'nda bir görüntü kullanarak bir web sayfası yürütebilirsiniz `Video` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="24df7-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="24df7-127">`Video` Yardımcı otomatik olarak oluşturduğundan bir web sayfasında videoları ekleme işlemini basitleştiren `object` ve `embed` normalde video eklemek için kullanılan HTML öğeleri.</span><span class="sxs-lookup"><span data-stu-id="24df7-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="24df7-128">`Video` Yardımcısı aşağıdaki medya oynatıcıları destekler:</span><span class="sxs-lookup"><span data-stu-id="24df7-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="24df7-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="24df7-129">Adobe Flash</span></span>
- <span data-ttu-id="24df7-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="24df7-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="24df7-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="24df7-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="24df7-132">Flash Player</span><span class="sxs-lookup"><span data-stu-id="24df7-132">The Flash Player</span></span>

<span data-ttu-id="24df7-133">`Flash` Oyuncusu `Video` Yardımcısı, Flash videoları oynatın olanak tanır (*.swf* dosyaları) bir web sayfasında.</span><span class="sxs-lookup"><span data-stu-id="24df7-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="24df7-134">En az bir görüntü dosyasının yolunu sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="24df7-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="24df7-135">Yolun ancak hiçbir şey belirtirseniz, oyuncunun Flash geçerli sürümü tarafından belirlenen varsayılan değerleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="24df7-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="24df7-136">Genel varsayılan ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="24df7-136">Typical default settings are:</span></span>

- <span data-ttu-id="24df7-137">Varsayılan genişlik ve yükseklik kullanarak video görüntülenir ve arka plan rengi.</span><span class="sxs-lookup"><span data-stu-id="24df7-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="24df7-138">Sayfa yüklendiğinde videonun otomatik olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="24df7-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="24df7-139">Video, sürekli olarak açıkça durdurulana kadar döngüde kalır.</span><span class="sxs-lookup"><span data-stu-id="24df7-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="24df7-140">Video tümünü belirli bir boyuta sığması için videoyu kırpma yerine video gösterecek şekilde ölçeklendirilir.</span><span class="sxs-lookup"><span data-stu-id="24df7-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="24df7-141">Bir pencerede bir video yürütür.</span><span class="sxs-lookup"><span data-stu-id="24df7-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="24df7-142">MediaPlayer Player</span><span class="sxs-lookup"><span data-stu-id="24df7-142">The MediaPlayer Player</span></span>

<span data-ttu-id="24df7-143">`MediaPlayer` Oyuncusu `Video` Yardımcısı Windows Media videoları oynatın olanak tanır (*.wmv* dosyaları), Windows Media Ses (*.wma* dosyaları) ve MP3 (*.mp3* bir web sayfasında dosyalar).</span><span class="sxs-lookup"><span data-stu-id="24df7-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="24df7-144">Oynatmak için medya dosyasının yolunu içermelidir; diğer tüm parametreler isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="24df7-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="24df7-145">Yalnızca bir yol belirtirseniz, oyuncunun MediaPlayer, geçerli sürümü tarafından ayarlamak varsayılan ayarları kullanır:</span><span class="sxs-lookup"><span data-stu-id="24df7-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="24df7-146">Video, varsayılan genişlik ve yükseklik kullanılarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="24df7-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="24df7-147">Sayfa yüklendiğinde videonun otomatik olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="24df7-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="24df7-148">Video kez çalınır (döngü değil).</span><span class="sxs-lookup"><span data-stu-id="24df7-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="24df7-149">Oyuncu denetimleri kümesini kullanıcı arabiriminin görüntüler.</span><span class="sxs-lookup"><span data-stu-id="24df7-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="24df7-150">Bir pencerede bir video yürütür.</span><span class="sxs-lookup"><span data-stu-id="24df7-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="24df7-151">Silverlight Player</span><span class="sxs-lookup"><span data-stu-id="24df7-151">The Silverlight Player</span></span>

<span data-ttu-id="24df7-152">`Silverlight` Oyuncusu `Video` Yardımcısı Windows Media Video oynatın olanak tanır (*.wmv* dosyaları), Windows Media Ses (*.wma* dosyaları) ve MP3 (*.mp3* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="24df7-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="24df7-153">Path parametresi bir Silverlight tabanlı bir uygulama paketi için işaret edecek şekilde ayarlamanız gerekir (*.xap* dosyası).</span><span class="sxs-lookup"><span data-stu-id="24df7-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="24df7-154">Genişlik ve yükseklik parametreleri de ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="24df7-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="24df7-155">Diğer tüm parametreler isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="24df7-155">All other parameters are optional.</span></span> <span data-ttu-id="24df7-156">Video için Silverlight player kullandığınızda, yalnızca gerekli parametreler ayarlarsanız Silverlight player arka plan rengi olmadan videoyu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="24df7-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="24df7-157">Silverlight zaten bilinmiyor durumunda: *.xap* dosyasıdır Düzen yönergeleri içeren bir sıkıştırılmış dosyayı bir *.xaml* derlemeleri ve isteğe bağlı kaynakları yönetilen kodu dosyası.</span><span class="sxs-lookup"><span data-stu-id="24df7-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="24df7-158">Oluşturabileceğiniz bir *.xap* dosyasını Visual Studio'da Silverlight uygulaması projesi olarak.</span><span class="sxs-lookup"><span data-stu-id="24df7-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="24df7-159">`Silverlight` Video oynatıcı kullanan iki player için sağladığınız ve sağlanan ayarlarını *.xap* dosya.</span><span class="sxs-lookup"><span data-stu-id="24df7-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="24df7-160">MIME türleri</span><span class="sxs-lookup"><span data-stu-id="24df7-160">MIME Types</span></span>
> 
> <span data-ttu-id="24df7-161">Tarayıcı, bir tarayıcı bir dosya yüklediğinde, dosya türü işlenen belge için belirtilmiştir MIME türü eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="24df7-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="24df7-162">MIME içerik türü veya medya dosyasının türünü türüdür.</span><span class="sxs-lookup"><span data-stu-id="24df7-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="24df7-163">`Video` Yardımcısı aşağıdaki MIME türlerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="24df7-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="24df7-164">Flash (.swf) video oynatma</span><span class="sxs-lookup"><span data-stu-id="24df7-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="24df7-165">Bu yordamda adlı bir Flash videoyu oynatmak gösterilmiştir *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="24df7-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="24df7-166">Adlı bir klasör kendinizi yordam varsayar *medya* siteniz ve, *.swf* bu klasörde dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="24df7-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="24df7-167">ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), bunu zaten eklemediniz.</span><span class="sxs-lookup"><span data-stu-id="24df7-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="24df7-168">Web sitesi, bir sayfa ekleyin ve adlandırın *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="24df7-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="24df7-169">Sayfaya aşağıdaki işaretlemeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24df7-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="24df7-170">Sayfanın tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24df7-170">Run the page in a browser.</span></span> <span data-ttu-id="24df7-171">(Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.) Sayfa görüntülenir ve video otomatik olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="24df7-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="24df7-172">![[image] ] (10-working-with-video/_static/image1.jpg "ch08_video 1.jpg")</span><span class="sxs-lookup"><span data-stu-id="24df7-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="24df7-173">Ayarlayabileceğiniz `quality` parametresi için Flash video `low`, `autolow`, `autohigh`, `medium`, `high`, ve `best`:</span><span class="sxs-lookup"><span data-stu-id="24df7-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="24df7-174">Bir boyutta kullanarak oynatmak için Flash video değiştirebilirsiniz `scale` parametresini aşağıdaki şekilde ayarlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="24df7-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="24df7-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="24df7-175">`showall`.</span></span> <span data-ttu-id="24df7-176">Bu videonun tamamını görünür özgün en boy oranını koruyarak sağlar.</span><span class="sxs-lookup"><span data-stu-id="24df7-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="24df7-177">Bununla birlikte, her iki taraftaki kenarlıklı çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="24df7-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="24df7-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="24df7-178">`noorder`.</span></span> <span data-ttu-id="24df7-179">Özgün en boy oranını koruyarak bu videoyu ölçeklenen, ancak kırpılmış.</span><span class="sxs-lookup"><span data-stu-id="24df7-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="24df7-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="24df7-180">`exactfit`.</span></span> <span data-ttu-id="24df7-181">Bu videonun tamamını görünür özgün en boy oranını koruyarak gerek kalmadan kolaylaştırır, ancak bozulma ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="24df7-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="24df7-182">Belirtmezseniz bir `scale` parametresi, tüm video görünür olur ve herhangi bir kırpma olmadan özgün en boy oranı korunur.</span><span class="sxs-lookup"><span data-stu-id="24df7-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="24df7-183">Aşağıdaki örnek nasıl kullanılacağını gösterir `scale` parametresi:</span><span class="sxs-lookup"><span data-stu-id="24df7-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="24df7-184">Flash player adlı ayar video bir modu destekler `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="24df7-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="24df7-185">Bu ayar `window`, `opaque`, ve `transparent`.</span><span class="sxs-lookup"><span data-stu-id="24df7-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="24df7-186">Varsayılan olarak, `windowMode` ayarlanır `window`, web sayfasında ayrı bir pencerede bir video görüntüleyen.</span><span class="sxs-lookup"><span data-stu-id="24df7-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="24df7-187">`opaque` Ayar her şeyi web sayfasındaki videoyu arkasına gizler.</span><span class="sxs-lookup"><span data-stu-id="24df7-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="24df7-188">`transparent` Ayarı, saydam herhangi bir videonun parçası olduğunu varsayarak video içinde Göster arka plan web sayfasının olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="24df7-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="24df7-189">MediaPlayer Yürütülüyor (*.wmv*) videoları</span><span class="sxs-lookup"><span data-stu-id="24df7-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="24df7-190">Aşağıdaki yordamda adlı bir Windows Media videoyu oynatmak gösterilmiştir *sample.wmv* alanında *medya* klasör.</span><span class="sxs-lookup"><span data-stu-id="24df7-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="24df7-191">ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="24df7-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="24df7-192">Adlı yeni bir sayfa oluşturun *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="24df7-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="24df7-193">Sayfaya aşağıdaki işaretlemeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24df7-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="24df7-194">Sayfanın tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24df7-194">Run the page in a browser.</span></span> <span data-ttu-id="24df7-195">Video yükler ve otomatik olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="24df7-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="24df7-196">![[image] ] (10-working-with-video/_static/image2.jpg "ch08_video 2.jpg")</span><span class="sxs-lookup"><span data-stu-id="24df7-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="24df7-197">Ayarlayabileceğiniz `playCount` için otomatik olarak videoyu oynatmak için kaç kez gösteren bir tam sayı:</span><span class="sxs-lookup"><span data-stu-id="24df7-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="24df7-198">`uiMode` Parametresi kullanıcı arabiriminin hangi denetimlerin görünmesini belirtmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="24df7-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="24df7-199">Ayarlayabileceğiniz `uiMode` için `invisible`, `none`, `mini`, veya `full`.</span><span class="sxs-lookup"><span data-stu-id="24df7-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="24df7-200">Belirtmezseniz bir `uiMode` parametresi, videoyu olacak durum penceresi görüntülenir, arama çubuğu, düğmeler ve ses denetimlerini video penceresinin yanı sıra denetim.</span><span class="sxs-lookup"><span data-stu-id="24df7-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="24df7-201">Bir ses dosyasını oynatmak için player'ı kullanırsanız, bu denetimleri de görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="24df7-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="24df7-202">İşte nasıl kullanılacağına ilişkin bir örnek `uiMode` parametresi:</span><span class="sxs-lookup"><span data-stu-id="24df7-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="24df7-203">Video yürütüldüğünde, varsayılan ses açıktır.</span><span class="sxs-lookup"><span data-stu-id="24df7-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="24df7-204">Ayarlayarak sesi `mute` parametresi true:</span><span class="sxs-lookup"><span data-stu-id="24df7-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="24df7-205">Ayarlayarak MediaPlayer videonun ses düzeyini denetleyebilirsiniz `volume` parametresi için 0 ile 100 arasında bir değer.</span><span class="sxs-lookup"><span data-stu-id="24df7-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="24df7-206">Varsayılan değer 50'dir.</span><span class="sxs-lookup"><span data-stu-id="24df7-206">The default value is 50.</span></span> <span data-ttu-id="24df7-207">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="24df7-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="24df7-208">Silverlight video oynatma</span><span class="sxs-lookup"><span data-stu-id="24df7-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="24df7-209">Bu yordamda bir Silverlight'ta bulunan bir videoyu oynatmak gösterilmiştir *.xap* bir klasörde adlı sayfa *medya*.</span><span class="sxs-lookup"><span data-stu-id="24df7-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="24df7-210">ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="24df7-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="24df7-211">Adlı yeni bir sayfa oluşturun *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="24df7-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="24df7-212">Sayfaya aşağıdaki işaretlemeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="24df7-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="24df7-213">Sayfanın tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24df7-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="24df7-214">![[image] ] (10-working-with-video/_static/image3.jpg "ch08_video 3.jpg")</span><span class="sxs-lookup"><span data-stu-id="24df7-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="24df7-215">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="24df7-215">Additional Resources</span></span>


<span data-ttu-id="24df7-216">[Silverlight genel bakış](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="24df7-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="24df7-217">Nesne ve ekleme etiketi öznitelikleri flash</span><span class="sxs-lookup"><span data-stu-id="24df7-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="24df7-218">[Windows Media Player 11 SDK PARAM etiketleri](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="24df7-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
