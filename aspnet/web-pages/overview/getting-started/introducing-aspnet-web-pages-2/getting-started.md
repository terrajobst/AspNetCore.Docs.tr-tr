---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Karşınızda ASP.NET Web sayfaları - kullanmaya başlama | Microsoft Docs
author: tfitzmac
description: WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Visual Studio veya Visual Studio Code'u kullanın. Bu kılavuz bir...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 12878082306cf51f8ea08ae614d9420251ecb587
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482963"
---
<a name="introducing-aspnet-web-pages---getting-started"></a><span data-ttu-id="a2e5b-105">Karşınızda ASP.NET Web sayfaları - kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a2e5b-105">Introducing ASP.NET Web Pages - Getting Started</span></span>
====================
<span data-ttu-id="a2e5b-106">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a2e5b-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE[](~/includes/rp.md)]

> > [!NOTE] 
> > 
> > <span data-ttu-id="a2e5b-107">WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-107">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="a2e5b-108">Kullanım [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) veya [Visual Studio Code'u](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a2e5b-108">Use [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>
> 
> 
> <span data-ttu-id="a2e5b-109">Bu kılavuzu ve uygulama size bir ASP.NET Web sayfaları (sürüm 2 veya sonraki sürümler), genel bakış ve Razor sözdizimi, dinamik Web siteleri oluşturmak için basit bir çerçeve.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-109">This guidance and application gives you an overview of ASP.NET Web Pages (version 2 or later) and Razor syntax, a lightweight framework for creating dynamic websites.</span></span> <span data-ttu-id="a2e5b-110">WebMatrix, sayfalar ve site oluşturmak için bir araç da tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-110">It also introduces WebMatrix, a tool for creating pages and sites.</span></span>
> 
> <span data-ttu-id="a2e5b-111">**Düzey**: yeni ASP.NET Web sayfaları için.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-111">**Level**: New to ASP.NET Web Pages.</span></span>  
> <span data-ttu-id="a2e5b-112">**Kabul becerileri**: HTML, temel geçişli stil sayfaları (CSS).</span><span class="sxs-lookup"><span data-stu-id="a2e5b-112">**Skills assumed**: HTML, basic cascading style sheets (CSS).</span></span>
> 
> <span data-ttu-id="a2e5b-113">Kümenin ilk öğreticide öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-113">What you'll learn in the first tutorial of the set:</span></span>
> 
> - <span data-ttu-id="a2e5b-114">İçin nedir ve hangi ASP.NET Web Pages teknolojidir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-114">What ASP.NET Web Pages technology is and what it's for.</span></span>
> - <span data-ttu-id="a2e5b-115">WebMatrix nedir?</span><span class="sxs-lookup"><span data-stu-id="a2e5b-115">What WebMatrix is.</span></span>
> - <span data-ttu-id="a2e5b-116">Her şeyi yükleme.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-116">How to install everything.</span></span>
> - <span data-ttu-id="a2e5b-117">WebMatrix kullanarak Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a2e5b-117">How to create a website by using WebMatrix.</span></span>
>   
> 
> <span data-ttu-id="a2e5b-118">Ele alınan özelliklerin/teknolojiler:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-118">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="a2e5b-119">Microsoft Web Platformu yükleyicisi.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-119">Microsoft Web Platform Installer.</span></span>
> - <span data-ttu-id="a2e5b-120">WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-120">WebMatrix.</span></span>
> - <span data-ttu-id="a2e5b-121">*.cshtml* sayfaları</span><span class="sxs-lookup"><span data-stu-id="a2e5b-121">*.cshtml* pages</span></span>
>   
> 
> <span data-ttu-id="a2e5b-122">Mike Pope, bu öğreticide ilk olarak yazıldı.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-122">Mike Pope originally wrote this tutorial.</span></span> <span data-ttu-id="a2e5b-123">Tom FitzMacken Microsoft WebMatrix 3'için güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-123">Tom FitzMacken updated it for Microsoft WebMatrix 3.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a2e5b-124">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="a2e5b-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a2e5b-125">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="a2e5b-125">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="a2e5b-126">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="a2e5b-126">WebMatrix 3</span></span>


## <a name="what-should-you-know"></a><span data-ttu-id="a2e5b-127">Bilmeniz gerekenler?</span><span class="sxs-lookup"><span data-stu-id="a2e5b-127">What Should You Know?</span></span>

<span data-ttu-id="a2e5b-128">Biz, alışık olduğunuz varsayılır:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-128">We're assuming that you're familiar with:</span></span>

- <span data-ttu-id="a2e5b-129">**HTML**.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-129">**HTML**.</span></span> <span data-ttu-id="a2e5b-130">Hiçbir kapsamlı uzmanlığını gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-130">No in-depth expertise is required.</span></span> <span data-ttu-id="a2e5b-131">Biz HTML açıklayan olmaz, ancak biz de karmaşık bir şey kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-131">We won't explain HTML, but we also don't use anything complex.</span></span> <span data-ttu-id="a2e5b-132">Burada yararlı oldukları düşünüyoruz HTML öğreticiler bağlantılarını sağlarız.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-132">We'll provide links to HTML tutorials where we think they're useful.</span></span>
- <span data-ttu-id="a2e5b-133">**Geçişli stil sayfaları (CSS)**.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-133">**Cascading style sheets (CSS)**.</span></span> <span data-ttu-id="a2e5b-134">Aynı HTML.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-134">Same as with HTML.</span></span>
- <span data-ttu-id="a2e5b-135">**Temel veritabanı fikirleri**.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-135">**Basic database ideas**.</span></span> <span data-ttu-id="a2e5b-136">Bu öğretici kümesi için biz genellikle, Elektronik tablolardaki verileri için kullanılan ve sıralanmış ve uzmanlık düzeyi verilere, filtre varsayılır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-136">If you've used a spreadsheet for data and sorted and filtered the data, that's the level of expertise we're generally assuming for this tutorial set.</span></span>

<span data-ttu-id="a2e5b-137">Biz de, temel programlama edinmek istiyorsanız da varsayılır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-137">We're also assuming that you're interested in learning basic programming.</span></span> <span data-ttu-id="a2e5b-138">ASP.NET Web Pages adlı C# programlama dilini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-138">ASP.NET Web Pages use a programming language called C#.</span></span> <span data-ttu-id="a2e5b-139">Programlama, yalnızca bir ilgi içindeki herhangi bir arka plana sahip gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-139">You don't have to have any background in programming, just an interest in it.</span></span> <span data-ttu-id="a2e5b-140">Bir web sayfasında önce hiç olmadığı kadar herhangi bir JavaScript yazdıysanız, arka plan bolca aradığınızı bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-140">If you've ever written any JavaScript in a web page before, you've got plenty of background.</span></span>

<span data-ttu-id="a2e5b-141">Ancak biz kısa sürede yeni programcılar Getir ile programlama hakkında bilginiz varsa, Bu öğretici başlangıçta kümesine bulabilirsiniz, Not yavaş taşır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-141">Note that if you are familiar with programming, you might find that this tutorial set initially moves slowly while we bring new programmers up to speed.</span></span> <span data-ttu-id="a2e5b-142">İlk birkaç öğreticiler aldığımız gibi yine de olmayacaktır açıklayın daha az temel programlama ve şeyler bir daha hızlı klibi taşınır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-142">As we get past the first few tutorials, though, there will be less basic programming to explain and things will move along at a faster clip.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="a2e5b-143">Ne gerekiyor?</span><span class="sxs-lookup"><span data-stu-id="a2e5b-143">What Do You Need?</span></span>

<span data-ttu-id="a2e5b-144">İşte gerekenler:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-144">Here's what you'll need:</span></span>

- <span data-ttu-id="a2e5b-145">Windows 8, Windows 7, Windows Server 2008 veya Windows Server 2012 çalıştıran bir bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-145">A computer that is running Windows 8, Windows 7, Windows Server 2008, or Windows Server 2012.</span></span>
- <span data-ttu-id="a2e5b-146">Canlı bir internet bağlantısı.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-146">A live internet connection.</span></span>
- <span data-ttu-id="a2e5b-147">Yönetici ayrıcalıkları (yükleme işlemi için gereklidir).</span><span class="sxs-lookup"><span data-stu-id="a2e5b-147">Administrator privileges (required for the installation process).</span></span>

## <a name="what-is-aspnet-web-pages"></a><span data-ttu-id="a2e5b-148">ASP.NET Web sayfaları nedir?</span><span class="sxs-lookup"><span data-stu-id="a2e5b-148">What Is ASP.NET Web Pages?</span></span>

<span data-ttu-id="a2e5b-149">ASP.NET Web sayfaları, dinamik web sayfaları oluşturmak için kullanabileceğiniz bir altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-149">ASP.NET Web Pages is a framework that you can use to create dynamic web pages.</span></span> <span data-ttu-id="a2e5b-150">Basit bir HTML web sayfası statiktir; içeriği, sayfaya sabit HTML biçimlendirme tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-150">A simple HTML web page is static; its content is determined by the fixed HTML markup that's in the page.</span></span> <span data-ttu-id="a2e5b-151">Dinamik sayfaları, ASP.NET Web sayfaları ile oluşturduğunuz benzer kod kullanarak hızla, sayfa içeriğini oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-151">Dynamic pages like those you create with ASP.NET Web Pages let you create the page content on the fly, by using code.</span></span>

<span data-ttu-id="a2e5b-152">Dinamik sayfalar çok şey yapmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-152">Dynamic pages let you do all sorts of things.</span></span> <span data-ttu-id="a2e5b-153">Bir kullanıcı giriş formu kullanarak isteyin ve ne sayfası görüntüler veya şöyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-153">You can ask a user for input by using a form and then change what the page displays or how it looks.</span></span> <span data-ttu-id="a2e5b-154">Bir kullanıcıdan bilgi al, bir veritabanına kaydetme ve daha sonra liste.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-154">You can take information from a user, save it in a database, and then list it later.</span></span> <span data-ttu-id="a2e5b-155">Sitenizden e-posta gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-155">You can send email from your site.</span></span> <span data-ttu-id="a2e5b-156">Diğer Hizmetleri Web (örneğin, bir eşleme hizmeti) ile etkileşim kurabilir ve bu kaynaklardan bilgi tümleştirme sayfaları oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-156">You can interact with other services on the web (for example, a mapping service) and produce pages that integrate information from those sources.</span></span>

## <a name="what-is-webmatrix"></a><span data-ttu-id="a2e5b-157">WebMatrix nedir?</span><span class="sxs-lookup"><span data-stu-id="a2e5b-157">What is WebMatrix?</span></span>

<span data-ttu-id="a2e5b-158">WebMatrix tümleşen bir web sayfası Düzenleyicisi, bir veritabanı yardımcı programı, sayfalarını ve Web sitenizi Internet'e yayımlama özelliklerini test etmek için bir web sunucusu bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-158">WebMatrix is a tool that integrates a web page editor, a database utility, a web server for testing pages, and features for publishing your website to the Internet.</span></span> <span data-ttu-id="a2e5b-159">WebMatrix, ücretsiz ve kolay yükleme ve kullanımı kolay.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-159">WebMatrix is free, and it's easy to install and easy to use.</span></span> <span data-ttu-id="a2e5b-160">(Ayrıca PHP gibi diğer teknolojileri yanı sıra, yalnızca düz HTML sayfaları için çalışır.)</span><span class="sxs-lookup"><span data-stu-id="a2e5b-160">(It also works for just plain HTML pages, as well as for other technologies like PHP.)</span></span>

<span data-ttu-id="a2e5b-161">Aslında yoksa *sahip* WebMatrix ASP.NET Web sayfaları ile çalışmak için kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-161">You don't actually *have* to use WebMatrix to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="a2e5b-162">Sayfaları metin kullanarak Düzenleyicisi, oluşturabilir ve erişimi olan bir web sunucusunu kullanarak sayfaları test.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-162">You can create pages by using a text editor, for example, and test pages by using a web server that you have access to.</span></span> <span data-ttu-id="a2e5b-163">Bu öğretici boyunca WebMatrix kullanabilirsiniz ancak WebMatrix tüm çok, kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-163">However, WebMatrix makes it all very easy, so these tutorials will use WebMatrix throughout.</span></span>

## <a name="about-these-tutorials"></a><span data-ttu-id="a2e5b-164">Bu öğreticileri hakkında</span><span class="sxs-lookup"><span data-stu-id="a2e5b-164">About These Tutorials</span></span>

<span data-ttu-id="a2e5b-165">Bu öğretici kümesi, ASP.NET Web sayfalarının nasıl kullanılacağını giriş niteliğindedir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-165">This tutorial set is an introduction to how to use ASP.NET Web Pages.</span></span> <span data-ttu-id="a2e5b-166">Bu giriş niteliğindeki öğretici kümesinde toplam 9 öğreticiler vardır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-166">There are 9 tutorials total in this introductory tutorial set.</span></span> <span data-ttu-id="a2e5b-167">ASP.NET Web Pages Acemi kullanıcıdan gerçek ve profesyonel görünümlü bir Web siteleri oluşturmak için gereken bir dizi öğretici kümeleri gereksinimlerimizim bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-167">It's part of a series of tutorial sets that takes you from ASP.NET Web Pages novice to creating real, professional-looking websites.</span></span>

<span data-ttu-id="a2e5b-168">Bu ilk öğreticide, ASP.NET Web sayfaları ile çalışmaya ilişkin temel bilgileri gösteren concentrates ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-168">This first tutorial set concentrates on showing you the basics of how to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="a2e5b-169">İşiniz bittiğinde, burada bunu sona erer ve, Web sayfalarını daha derinlemesine keşfedin öğrenilip ek öğretici kümeleri ile çalışabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-169">When you're done, you can work with additional tutorial sets that pick up where this one ends and that explore Web Pages in more depth.</span></span>

<span data-ttu-id="a2e5b-170">Biz kasıtlı olarak üzerinde ayrıntılı açıklamalar kolay gidin.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-170">We deliberately go easy on the in-depth explanations.</span></span> <span data-ttu-id="a2e5b-171">Ve bir şeyi göstereceğiz her durumda, Bu öğretici kümesi için size her zaman düşünüyoruz bir şekilde anlaşılması kolay seçin.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-171">And whenever we show something, for this tutorial set we always choose the way that we think is easiest to understand.</span></span> <span data-ttu-id="a2e5b-172">Sonraki öğretici kümeleri, daha ayrıntılı gidin ve daha verimli ya da daha esnek yaklaşımları (Ayrıca daha eğlenceli) gösterir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-172">Later tutorial sets go into more depth and show you more efficient or more flexible approaches (also more fun).</span></span> <span data-ttu-id="a2e5b-173">Ancak bu öğreticileri öncelikle temellerini anlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-173">But those tutorials require you to understand the basics first.</span></span>

<span data-ttu-id="a2e5b-174">Bu özellikler, ASP.NET Web sayfaları yalnızca başlangıç öğretici kümesi içerir:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-174">The tutorial set you've just started covers these features of ASP.NET Web Pages:</span></span>

- <span data-ttu-id="a2e5b-175">Giriş ve yüklü olan her şeyi alma.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-175">Introduction and getting everything installed.</span></span> <span data-ttu-id="a2e5b-176">(Bu makaleyi okuduğunuz öğreticide içindir.)</span><span class="sxs-lookup"><span data-stu-id="a2e5b-176">(That's in the tutorial you're reading.)</span></span>
- <span data-ttu-id="a2e5b-177">ASP.NET Web sayfaları Programlama temelleri.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-177">The basics of ASP.NET Web Pages programming.</span></span>
- <span data-ttu-id="a2e5b-178">Veritabanı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-178">Creating a database.</span></span>
- <span data-ttu-id="a2e5b-179">Oluşturma ve bir kullanıcı giriş formu işleme.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-179">Creating and processing a user input form.</span></span>
- <span data-ttu-id="a2e5b-180">Ekleme, güncelleştirme, veritabanındaki verileri siliniyor.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-180">Adding, updating, and deleting data in the database.</span></span>

## <a name="what-will-you-create"></a><span data-ttu-id="a2e5b-181">Ne oluşturacaksınız?</span><span class="sxs-lookup"><span data-stu-id="a2e5b-181">What Will You Create?</span></span>

<span data-ttu-id="a2e5b-182">Bu öğreticide ayarlayabilir ve sonraki olanları çalışmalarınızı istediğiniz film burada listeleyebilirsiniz bir Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-182">This tutorial set and subsequent ones revolve around a website where you can list movies that you like.</span></span> <span data-ttu-id="a2e5b-183">Filmler girin, düzenleyebilir ve bunları listelemek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-183">You'll be able to enter movies, edit them, and list them.</span></span> <span data-ttu-id="a2e5b-184">Bu öğretici kümesinde oluşturacaksınız sayfaları birkaç aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-184">Here are a couple of the pages you'll create in this tutorial set.</span></span> <span data-ttu-id="a2e5b-185">İlki, oluşturacağınız sayfa listeleme film gösterir:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-185">The first one shows the movie listing page that you'll create:</span></span>

![Film listesini gösteren bitirdi film uygulaması](getting-started/_static/image1.png)

<span data-ttu-id="a2e5b-187">Ve İşte yeni film bilgileri sitenize eklemenize olanak sağlayan sayfası:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-187">And here's the page that lets you add new movie information to your site:</span></span>

![Ekleme film sayfasını gösteren tamamlanmış film uygulaması](getting-started/_static/image2.png)

<span data-ttu-id="a2e5b-189">Bu sonraki öğretici kümeleri yapı ayarlayın ve resimleri karşıya yükleme, oturum kişilere izin vererek, e-posta göndermek ve sosyal medya ile tümleştirme gibi daha fazla işlevsellik ekler.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-189">Subsequent tutorial sets build on this set and add more functionality, like uploading pictures, letting people log in, sending email, and integrating with social media.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="a2e5b-190">Azure üzerinde çalışan bu uygulamayı bakın</span><span class="sxs-lookup"><span data-stu-id="a2e5b-190">See this App Running on Azure</span></span>

<span data-ttu-id="a2e5b-191">Canlı web uygulaması olarak çalışan tamamlanmış site görmek ister misiniz?</span><span class="sxs-lookup"><span data-stu-id="a2e5b-191">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="a2e5b-192">Aşağıdaki düğmeye tıklayarak Azure hesabınızda bir tam sürümü uygulama dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-192">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

<span data-ttu-id="a2e5b-193">Bu çözüm, Azure'a dağıtmak için bir Azure hesabına ihtiyacınız var.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-193">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="a2e5b-194">Bir hesap zaten yoksa, aşağıdaki seçenekleriniz:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-194">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="a2e5b-195">[Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-195">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="a2e5b-196">[MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-196">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="installing-everything"></a><span data-ttu-id="a2e5b-197">Her şeyi yükleme</span><span class="sxs-lookup"><span data-stu-id="a2e5b-197">Installing Everything</span></span>

<span data-ttu-id="a2e5b-198">Her şey, Microsoft Web Platformu Yükleyicisi'ni kullanarak yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-198">You can install everything by using the Web Platform Installer from Microsoft.</span></span> <span data-ttu-id="a2e5b-199">Aslında, yükleyici yükleme ve diğer her şey yüklemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-199">In effect, you install the installer, and then use it to install everything else.</span></span>

<span data-ttu-id="a2e5b-200">Web sayfaları kullanmak için sahip en az zorunda yüklü SP3, Windows XP veya Windows Server 2008 veya üstü.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-200">To use Web Pages, you have to be have at least Windows XP with SP3 installed, or Windows Server 2008 or later.</span></span>

<span data-ttu-id="a2e5b-201">Üzerinde [Web Pages sayfası](../../../index.md) ASP.NET Web sitesini, tıklayın **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-201">On the [Web Pages page](../../../index.md) of the ASP.NET website, click **Install**.</span></span>

![ASP.NET Web sitesini gösteren &quot;Webmatrix'i yükleyin&quot; düğmesi](getting-started/_static/image3.png)

<span data-ttu-id="a2e5b-203">Lisans koşullarını ve gizlilik bildirimi, WebMatrix yüklemeden önce kabul istenir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-203">You are asked to accept the license terms and privacy statement before installing WebMatrix.</span></span>

![yüklemeye başlamak için koşulu kabul](getting-started/_static/image4.png)

<span data-ttu-id="a2e5b-205">Tıklayın **çalıştırma** yüklemeyi başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-205">Click **Run** to start the installation.</span></span> <span data-ttu-id="a2e5b-206">(Yükleyici kaydetmek istiyorsanız, tıklayın **Kaydet** ve ardından kaydettiğiniz bu klasörden yükleyiciyi çalıştırın.)</span><span class="sxs-lookup"><span data-stu-id="a2e5b-206">(If you want to save the installer, click **Save** and then run the installer from the folder where you saved it.)</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="a2e5b-207">Web Platformu yükleyicisi görüntülenir, WebMatrix yüklemek için hazır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-207">The Web Platform Installer appears, ready to install WebMatrix.</span></span> <span data-ttu-id="a2e5b-208">**Yükle**'ye tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-208">Click **Install**.</span></span>

![](getting-started/_static/image6.png)

<span data-ttu-id="a2e5b-209">Yükleme işlemi, ne, bilgisayarınıza yüklemek sahip rakamları ve işlemini başlatır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-209">The installation process figures out what it has to install on your computer and starts the process.</span></span> <span data-ttu-id="a2e5b-210">Hangi tam olarak yüklenmesi gerekir bağlı olarak, işlem her yerde birkaç dakika için birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-210">Depending on what exactly has to be installed, the process can take anywhere from a few moments to several minutes.</span></span> <span data-ttu-id="a2e5b-211">Seçin **kabul ediyorum** lisans koşullarını kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-211">Select **I Accept** to accept the license terms.</span></span>

## <a name="hello-webmatrix"></a><span data-ttu-id="a2e5b-212">Merhaba, WebMatrix</span><span class="sxs-lookup"><span data-stu-id="a2e5b-212">Hello, WebMatrix</span></span>

<span data-ttu-id="a2e5b-213">İşlem tamamlandığında, yükleme işlemi otomatik olarak WebMatrix başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-213">When it's done, the installation process can launch WebMatrix automatically.</span></span> <span data-ttu-id="a2e5b-214">Windows gelen eşleşmiyorsa **Başlat** menüsü, başlatma **Microsoft WebMatrix**.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-214">If it doesn't, in Windows, from the **Start** menu, launch **Microsoft WebMatrix**.</span></span>

<span data-ttu-id="a2e5b-215">WebMatrix için ilk kez başlattığınızda, Microsoft Azure için Microsoft hesabınızla oturum açmak için bir fırsat sunulur.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-215">When you launch WebMatrix for the first time, you are given a chance to sign in to Microsoft Azure with your Microsoft account.</span></span> <span data-ttu-id="a2e5b-216">Oturum açarak Azure üzerinden 10 ücretsiz web uygulaması elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-216">By signing in, you will receive 10 free web apps through Azure.</span></span> <span data-ttu-id="a2e5b-217">Bu ücretsiz web apps, uygulamalarınızı test etmek için kullanışlı bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-217">These free web apps provide a convenient way to test your apps.</span></span> <span data-ttu-id="a2e5b-218">Bir Azure hesabınız yoksa, ancak bir MSDN aboneliğiniz varsa [MSDN abonelik Avantajlarınızı etkinleştirin](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="a2e5b-218">If you don't already have an Azure account, but you do have an MSDN subscription, you can [activate your MSDN subscription benefits](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="a2e5b-219">Aksi takdirde, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-219">Otherwise, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a2e5b-220">Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="a2e5b-220">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="a2e5b-221">Bu öğretici ile devam etmek şu anda oturum gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-221">You do not have to sign in right now to continue with this tutorial.</span></span> <span data-ttu-id="a2e5b-222">Artık oturum değil ise, yine de daha sonra oturum açmayı seçeneğine sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-222">If you do not sign in now, you will still have the option to sign in later.</span></span> <span data-ttu-id="a2e5b-223">Son [konu](publishing.md) Bu öğreticide serisi Azure'da Web sitenizi dağıtmak nasıl etkinleştireceğinizi de açıklar; bu nedenle, bu konuyu tamamlamak oturum açmanız.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-223">The last [topic](publishing.md) in this tutorial series covers how to deploy your website to Azure; therefore, you would need to sign in to complete that topic.</span></span>

<span data-ttu-id="a2e5b-224">Bu noktada, ya da, Microsoft hesabı ile veya select oturum **şimdi değil** sağ alt köşedeki.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-224">At this point, either sign in with your Microsoft account or select **Not Now** in the lower right corner.</span></span>

![Oturum Aç](getting-started/_static/image7.png)

<span data-ttu-id="a2e5b-226">Başlamak için boş bir Web sitesi oluşturma ve bir sayfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-226">To begin, you'll create a blank website and add a page.</span></span> <span data-ttu-id="a2e5b-227">Bir sonraki Öğreticide bu yerleşik Web şablonlarından biriyle oynatın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-227">In a later tutorial in this set you'll play with one of the built-in website templates.</span></span>

<span data-ttu-id="a2e5b-228">Başlangıç pencerede **yeni**.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-228">In the start window, click **New**.</span></span>

![WebMatrix başlangıç ekranı](getting-started/_static/image8.png)

<span data-ttu-id="a2e5b-230">Önceden oluşturulmuş dosyalar ve sayfalar için Web siteleri farklı türde şablonlardır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-230">Templates are prebuilt files and pages for different types of websites.</span></span> <span data-ttu-id="a2e5b-231">Tüm varsayılan olarak kullanılabilir şablonları görmek için Şablon Galerisi seçeneğini seçin.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-231">To see all of the templates that are available by default, select the Template Gallery option.</span></span>

![Şablon Galerisi seçin](getting-started/_static/image9.png)

<span data-ttu-id="a2e5b-233">İçinde **Hızlı Başlangıç** penceresinde **boş Site** gelen **ASP.NET** grubu ve yeni site "WebPagesMovies" olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-233">In the **Quick Start** window, select **Empty Site** from the **ASP.NET** group and name the new site "WebPagesMovies".</span></span>

![Boş Site şablonu seçili penceresiyle WebMatrix hızlı başlangıç](getting-started/_static/image10.png)

<span data-ttu-id="a2e5b-235">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-235">Click **Next**.</span></span>

<span data-ttu-id="a2e5b-236">Microsoft hesabınızda oturum açmanızdan, Azure'da site oluşturma fırsatı verilir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-236">If you have signed in to your Microsoft account, you will be given the opportunity to create the site on Azure.</span></span> <span data-ttu-id="a2e5b-237">Varsayılan adını sitenizin adına dayalı **WebPagesMovies.azurewebsites.net** önerilir; ancak, bu adı Windows Azure üzerinde kullanılabilir değil ünlem gösterir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-237">Based on the name of your site, the default name of **WebPagesMovies.azurewebsites.net** is suggested; however, the exclamation point indicates that this name is not available on Windows Azure.</span></span> <span data-ttu-id="a2e5b-238">Kolaylık olması için seçin **atla** Azure'da web sitesi oluşturma, şu anda atlamak için.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-238">For simplicity, select **Skip** to bypass creating the web site on Azure right now.</span></span> <span data-ttu-id="a2e5b-239">Bu seri için Azure site yayımlarız.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-239">Later in this series, we will publish the site to Azure.</span></span>

![Azure site oluşturma](getting-started/_static/image11.png)

<span data-ttu-id="a2e5b-241">WebMatrix oluşturur ve bu sitenin açılır:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-241">WebMatrix creates and opens the site:</span></span>

![Webmatrix'te yeni WebPagesMovies sitesini açın](getting-started/_static/image12.png)

<span data-ttu-id="a2e5b-243">En üstünde, hızlı erişim araç çubuğu ve bir Şerit yoktur.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-243">At the top, there's a Quick Access Toolbar and a ribbon.</span></span> <span data-ttu-id="a2e5b-244">Alt sol, çalışma alanı Seçici görevler arasında geçiş Burada gördüğünüz (**Site**, **dosyaları**, **veritabanları**, **raporları**).</span><span class="sxs-lookup"><span data-stu-id="a2e5b-244">At the bottom left, you see the workspace selector where you switch between tasks (**Site**, **Files**, **Databases**, **Reports**).</span></span> <span data-ttu-id="a2e5b-245">Sağ tarafta içerik bölmesinde Düzenleyicisi ve raporları içindir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-245">On the right is the content pane for the editor and for reports.</span></span> <span data-ttu-id="a2e5b-246">Ve alt arasında bazen iletiler için bir bildirim çubuğu göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-246">And across the bottom you'll occasionally see a notification bar for messages.</span></span>

<span data-ttu-id="a2e5b-247">Hakkında daha fazla WebMatrix ve özelliklerini bu öğreticileri gibi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-247">You'll learn more about WebMatrix and its features as you go through these tutorials.</span></span>

## <a name="creating-a-web-page"></a><span data-ttu-id="a2e5b-248">Bir Web sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="a2e5b-248">Creating a Web Page</span></span>

<span data-ttu-id="a2e5b-249">WebMatrix ve ASP.NET Web sayfaları ile ilgili bilgi sahibi olmak için basit bir sayfa oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-249">To become familiar with WebMatrix and ASP.NET Web Pages, you'll create a simple page.</span></span>

<span data-ttu-id="a2e5b-250">Çalışma alanı seçiciden seçin **dosyaları** çalışma.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-250">In the workspace selector, select the **Files** workspace.</span></span> <span data-ttu-id="a2e5b-251">Bu çalışma alanı, dosyalar ve klasörler ile çalışmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-251">This workspace lets you work with files and folders.</span></span> <span data-ttu-id="a2e5b-252">Sol bölmede, sitenizin dosya yapısı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-252">The left pane shows the file structure of your site.</span></span> <span data-ttu-id="a2e5b-253">Dosya ile ilgili görevleri göstermek için Şerit değişir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-253">The ribbon changes to show file-related tasks.</span></span>

![Webmatrix'te dosya çalışma](getting-started/_static/image13.png)

<span data-ttu-id="a2e5b-255">Şeritte altında oku **yeni** ve ardından **yeni dosya**.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-255">In the ribbon, click the arrow under **New** and then click **New File**.</span></span>

![Kullanarak &quot;yeni&quot; yeni bir dosya oluşturmak için Şeritte komutu](getting-started/_static/image14.png)

<span data-ttu-id="a2e5b-257">WebMatrix, dosya türlerinin bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-257">WebMatrix displays a list of file types.</span></span> <span data-ttu-id="a2e5b-258">Seçin **CSHTML**hem de **adı** "HelloWorld" yazın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-258">Select **CSHTML**, and in the **Name** box, type "HelloWorld".</span></span> <span data-ttu-id="a2e5b-259">Bir ASP.NET Web Pages sayfasında CSHTML sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-259">A CSHTML page is an ASP.NET Web Pages page.</span></span>

![HelloWorld.cshtml adlı yeni bir CSHTML sayfası oluşturma](getting-started/_static/image15.png)

<span data-ttu-id="a2e5b-261">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-261">Click **OK**.</span></span>

<span data-ttu-id="a2e5b-262">WebMatrix sayfası oluşturur ve düzenleyicide açılır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-262">WebMatrix creates the page and opens it in the editor.</span></span>

![WebMatrix Düzenleyicisi'nde yeni HelloWorld sayfası](getting-started/_static/image16.png)

<span data-ttu-id="a2e5b-264">Gördüğünüz gibi sayfa şuna benzer bir üst bloğu dışında sıradan HTML biçimlendirmesi çoğunlukla içerir:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-264">As you can see, the page contains mostly ordinary HTML markup, except for a block at the top that looks like this:</span></span>

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

<span data-ttu-id="a2e5b-265">Bu kısa bir süre içinde anlatıldığı gibi kodu eklemek için.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-265">That's for adding code, as you'll see shortly.</span></span>

<span data-ttu-id="a2e5b-266">Dikkat sayfasının farklı bölümlerini &mdash; öğe adları, öznitelikleri ve metnin yanı sıra üst kısmındaki blok — tüm de farklı renklerde.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-266">Notice that the different parts of the page &mdash; the element names, attributes, and text, plus the block at the top — are all in different colors.</span></span> <span data-ttu-id="a2e5b-267">Bu adlandırılır *söz dizimi vurgulama*, ve her şeyi NET tutmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-267">This is called *syntax highlighting*, and it makes it easier to keep everything clear.</span></span> <span data-ttu-id="a2e5b-268">Webmatrix'te web sayfalarıyla çalışma kolaylaştırır özelliklerden biridir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-268">It's one of the features that makes it easy to work with web pages in WebMatrix.</span></span>

<span data-ttu-id="a2e5b-269">İçin içerik ekleme `<head>` ve `<body>` aşağıdaki örnekte gibi öğeler.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-269">Add content for the `<head>` and `<body>` elements like in the following example.</span></span> <span data-ttu-id="a2e5b-270">(İsterseniz, yalnızca aşağıdaki bloğunu kopyalayın ve mevcut sayfanın tamamını şu kodla değiştirin.)</span><span class="sxs-lookup"><span data-stu-id="a2e5b-270">(If you want, you can just copy the following block and replace the entire existing page with this code.)</span></span>

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

<span data-ttu-id="a2e5b-271">Hızlı Erişim Araç çubuğu veya **dosya** menüsünde tıklatın **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-271">In the Quick Access Toolbar or in the **File** menu, click **Save**.</span></span>

![WebMatrix hızlı erişim araç çubuğunda Kaydet düğmesi](getting-started/_static/image17.png)

## <a name="testing-the-page"></a><span data-ttu-id="a2e5b-273">Sayfasını test etme</span><span class="sxs-lookup"><span data-stu-id="a2e5b-273">Testing the Page</span></span>

<span data-ttu-id="a2e5b-274">İçinde **dosyaları** çalışma alanında, sağ *HelloWorld.cshtml* sayfasında ve ardından **tarayıcıda Başlat**.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-274">In the **Files** workspace, right-click the *HelloWorld.cshtml* page and then click **Launch in browser**.</span></span>

![WebMatrix Şeritte Çalıştır düğmesini kullanarak bir sayfa çalıştırma](getting-started/_static/image18.png)

<span data-ttu-id="a2e5b-276">WebMatrix, bilgisayarınızda sayfaları test etmek için kullanabileceğiniz bir yerleşik web sunucusu (IIS Express) başlatır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-276">WebMatrix starts a built-in web server (IIS Express) that you can use to test pages on your computer.</span></span> <span data-ttu-id="a2e5b-277">(Webmatrix'te IIS Express, test edebilirsiniz önce sayfanıza bir web sunucusuna yere yayımlamanız gerekir.) Sayfa varsayılan tarayıcınızda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-277">(Without IIS Express in WebMatrix, you'd have to publish your page to a web server somewhere before you could test it.) The page is displayed in your default browser.</span></span>

![&quot;Merhaba Dünya&quot; sayfasını tarayıcıda çalışıyor](getting-started/_static/image19.png)

<span data-ttu-id="a2e5b-279">Webmatrix'te bir sayfayı test ettiğinizde, tarayıcı URL aşağıdakine benzer olduğunu fark `http://localhost:33651/HelloWorld.cshtml.` adı *localhost* sayfa kendi bilgisayarınızda bir web sunucusu tarafından sunulan, yani, yerel bir sunucuya ifade eder.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-279">Notice that when you test a page in WebMatrix, the URL in the browser is something like `http://localhost:33651/HelloWorld.cshtml.` The name *localhost* refers to a local server, meaning that the page is served by a web server that's on your own computer.</span></span> <span data-ttu-id="a2e5b-280">WebMatrix, belirtildiği gibi bir sayfa başlatıldığında çalıştırılan IIS Express adlı bir web sunucusu programı içerir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-280">As noted, WebMatrix includes a web server program named IIS Express that runs when you launch a page.</span></span>

<span data-ttu-id="a2e5b-281">Sonra sayı *localhost* (örneğin, *localhost:33651*) başvurduğu bir *bağlantı noktası numarası* bilgisayarınızda.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-281">The number after *localhost* (for example, *localhost:33651*) refers to a *port number* on your computer.</span></span> <span data-ttu-id="a2e5b-282">Bu "IIS Express kullandığından bu belirli bir Web sitesi için kanal" sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-282">This is the number of the "channel" that IIS Express uses for this particular website.</span></span> <span data-ttu-id="a2e5b-283">Bağlantı noktası numarasını rastgele bir aralıktan 1024 ile 65536 sitenizi oluşturmak ve oluşturduğunuz her site için farklı seçilir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-283">The port number is selected at random from the range 1024 through 65536 when you create your site, and it's different for every site that you create.</span></span> <span data-ttu-id="a2e5b-284">(Kendi site test ettiğinizde, bağlantı noktası numarasını neredeyse kesindir 33561 değerinden farklı bir numara olacaktır.) Her Web sitesi için farklı bir bağlantı noktası kullanarak IIS Express için Bahsediyor sitelerinizi hangisinin düz tutabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-284">(When you test your own site, the port number will almost certainly be a different number than 33561.) By using a different port for each website, IIS Express can keep straight which of your sites it's talking to.</span></span>

<span data-ttu-id="a2e5b-285">Artık bkz: Genel web sunucusuna, sitenizi yayımladığınızda sonraki *localhost* URL.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-285">Later when you publish your site to a public web server, you no longer see *localhost* in the URL.</span></span> <span data-ttu-id="a2e5b-286">Bu noktada, gibi daha genel bir URL göreceğiniz `http://myhostingsite/mywebsite/HelloWorld.cshtml` veya herhangi bir sayfa.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-286">At that point, you'll see a more typical URL like `http://myhostingsite/mywebsite/HelloWorld.cshtml` or whatever the page is.</span></span> <span data-ttu-id="a2e5b-287">Bu öğretici serisinde daha sonra bir site yayımlama hakkında daha fazla bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-287">You'll learn more about publishing a site later in this tutorial series.</span></span>

## <a name="adding-some-server-side-code"></a><span data-ttu-id="a2e5b-288">Bazı sunucu tarafındaki kod ekleme</span><span class="sxs-lookup"><span data-stu-id="a2e5b-288">Adding Some Server-Side Code</span></span>

<span data-ttu-id="a2e5b-289">Tarayıcıyı kapatın ve WebMatrix sayfasına geri dönün.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-289">Close the browser and go back to the page in WebMatrix.</span></span>

<span data-ttu-id="a2e5b-290">Şu kod gibi görünüyor. böylece kod bloğu için bir satır ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-290">Add a line to the code block so that it looks like the following code:</span></span>

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

<span data-ttu-id="a2e5b-291">Razor kod biraz budur.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-291">This is a little bit of Razor code.</span></span> <span data-ttu-id="a2e5b-292">Geçerli tarih ve saati alır ve bu değeri içine yerleştirir büyük olasılıkla boş olduğundan bir *değişkeni* adlı `currentDateTime`.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-292">It's probably clear that it gets the current date and time and puts that value into a *variable* named `currentDateTime`.</span></span> <span data-ttu-id="a2e5b-293">Daha fazla bilgi edinin sonraki öğreticide Razor söz dizimi hakkında.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-293">You'll read more about Razor syntax in the next tutorial.</span></span>

<span data-ttu-id="a2e5b-294">Sayfanın gövdesindeki sonra `<p>Hello World!</p>` öğesi, aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-294">In the body of the page, after the `<p>Hello World!</p>` element, add the following:</span></span>

[!code-html[Main](getting-started/samples/sample4.html)]

<span data-ttu-id="a2e5b-295">Bu kod içine yerleştirdiğiniz değeri alır `currentDateTime` üst değişken ve sayfanın biçimlendirmesine ekler.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-295">This code gets the value that you put into the `currentDateTime` variable at the top and inserts it into the markup of the page.</span></span> <span data-ttu-id="a2e5b-296">`@` ASP.NET Web Pages kod sayfasında karakter işaretler.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-296">The `@` character marks the ASP.NET Web Pages code in the page.</span></span>

<span data-ttu-id="a2e5b-297">(Sayfa çalıştırılmadan önce WebMatrix değişiklikleri sizin için kaydeder) sayfasını tekrar çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-297">Run the page again (WebMatrix saves the changes for you before it runs the page).</span></span> <span data-ttu-id="a2e5b-298">Bu kez tarih ve saat sayfasında bakın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-298">This time you see the date and time in the page.</span></span>

![&quot;Merhaba Dünya&quot; tarayıcı dinamik olarak üretilen bir saati görüntüleme ile çalışan sayfası](getting-started/_static/image20.png)

<span data-ttu-id="a2e5b-300">Birkaç dakika bekleyin ve ardından sayfanın tarayıcıda yenileyin.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-300">Wait a few moments and then refresh the page in the browser.</span></span> <span data-ttu-id="a2e5b-301">Tarih ve saat görüntüleme güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-301">The date and time display is updated.</span></span>

<span data-ttu-id="a2e5b-302">Tarayıcıda, sayfa kaynağında arayın.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-302">In the browser, look at the page source.</span></span> <span data-ttu-id="a2e5b-303">Aşağıdaki biçimlendirme gibi görünüyor:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-303">It looks like the following markup:</span></span>

[!code-html[Main](getting-started/samples/sample5.html)]

<span data-ttu-id="a2e5b-304">Dikkat `@{ }` üst blok değil vardır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-304">Notice that the `@{ }` block at the top isn't there.</span></span> <span data-ttu-id="a2e5b-305">Ayrıca, tarih ve saat görüntüleme gerçek bir karakter dizesi gösterir dikkat edin (`1/18/2012 2:49:50 PM` veya) değil `@currentDateTime` olduğu gibi *.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-305">Also notice that the date and time display shows an actual string of characters (`1/18/2012 2:49:50 PM` or whatever), not `@currentDateTime` like you had in the *.cshtml* page.</span></span> <span data-ttu-id="a2e5b-306">İşte sayfa çalıştırdığınızda, ASP.NET ile işaretlenmiş tüm kod (çok az bu durumda) işlenen ne `@`.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-306">What happened here is that when you ran the page, ASP.NET processed all the code (very little in this case) that was marked with `@`.</span></span> <span data-ttu-id="a2e5b-307">Çıkış kodu üretir ve bu çıkışı sayfasına eklenir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-307">The code produces output, and that output was inserted into the page.</span></span>

## <a name="this-is-what-aspnet-web-pages-are-about"></a><span data-ttu-id="a2e5b-308">ASP.NET Web sayfaları üzeresiniz budur</span><span class="sxs-lookup"><span data-stu-id="a2e5b-308">This Is What ASP.NET Web Pages Are About</span></span>

<span data-ttu-id="a2e5b-309">ASP.NET Web Pages dinamik web içeriği üretir okuduğunuzda ne Burada gördüğünüz olur.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-309">When you read that ASP.NET Web Pages produces dynamic web content, what you've seen here is the idea.</span></span> <span data-ttu-id="a2e5b-310">Yeni oluşturduğunuz sayfasında önce gördüğünüz aynı HTML biçimlendirmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-310">The page you just created contains the same HTML markup that you've seen before.</span></span> <span data-ttu-id="a2e5b-311">Ayrıca, çok çeşitli görevler gerçekleştiren kod de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-311">It can also contain code that can perform all sorts of tasks.</span></span> <span data-ttu-id="a2e5b-312">Bu örnekte, geçerli tarih ve saat almaya ilişkin basit bir görev yaptım.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-312">In this example, it did the trivial task of getting the current date and time.</span></span> <span data-ttu-id="a2e5b-313">Gördüğünüz gibi HTML sayfasındaki çıktı üretmek için kod aralarına koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-313">As you saw, you can intersperse code with the HTML to produce output in the page.</span></span> <span data-ttu-id="a2e5b-314">Birisi istediğinde bir *.cshtml* sayfasını tarayıcıda, ASP.NET web sunucusunun hala kişiye olsa sayfa işler.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-314">When someone requests a *.cshtml* page in the browser, ASP.NET processes the page while it's still in the hands of the web server.</span></span> <span data-ttu-id="a2e5b-315">ASP.NET kodun çıktısı (varsa) sayfasına HTML olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-315">ASP.NET inserts the output of the code (if any) into the page as HTML.</span></span> <span data-ttu-id="a2e5b-316">Kod işlem tamamlandığında, ASP.NET elde edilen sayfanın tarayıcısına gönderir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-316">When the code processing is done, ASP.NET sends the resulting page to the browser.</span></span> <span data-ttu-id="a2e5b-317">Tüm tarayıcı hiç olmadığı kadar alır HTML budur.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-317">All the browser ever gets is HTML.</span></span> <span data-ttu-id="a2e5b-318">Bir diyagramda şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="a2e5b-318">Here's a diagram:</span></span>

![Kavramsal akışı nasıl ASP.NET HTML dinamik olarak oluşturur](getting-started/_static/image21.png)

<span data-ttu-id="a2e5b-320">Basit bir uygulamadır ancak kod gerçekleştirebileceğiniz birçok ilgi çekici görevleri vardır ve hangi dinamik olarak HTML içeriği sayfaya ekleyebileceğiniz birçok ilgi çekici yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-320">The idea is simple, but there are many interesting tasks that the code can perform, and there are many interesting ways in which you can dynamically add HTML content to the page.</span></span> <span data-ttu-id="a2e5b-321">Ve ASP.NET *.cshtml* sayfaları, herhangi bir HTML sayfası gibi tarayıcı kendisi (JavaScript ve jQuery kodu) çalışan kod da içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-321">And ASP.NET *.cshtml* pages, like any HTML page, can also include code that runs in the browser itself (JavaScript and jQuery code).</span></span> <span data-ttu-id="a2e5b-322">Tüm bunlar, Bu öğretici kümesinde ve sonraki olanları hakkında bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-322">You'll explore all of these things in this tutorial set and in subsequent ones.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="a2e5b-323">Sıradaki gelen</span><span class="sxs-lookup"><span data-stu-id="a2e5b-323">Coming Up Next</span></span>

<span data-ttu-id="a2e5b-324">Bu serideki sonraki öğretici, ASP.NET Web Pages biraz daha programlama keşfedin.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-324">In the next tutorial in this series, you explore ASP.NET Web Pages programming a little more.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2e5b-325">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a2e5b-325">Additional Resources</span></span>

<span data-ttu-id="a2e5b-326">[Sıfırdan ASP.NET Web sitesi oluşturma](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch).</span><span class="sxs-lookup"><span data-stu-id="a2e5b-326">[Create an ASP.NET website from scratch](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch).</span></span> <span data-ttu-id="a2e5b-327">Bu, özellikle bir öğretici WebMatrix (ASP.NET Web sayfaları değil) kullanarak hakkında.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-327">This is a tutorial that's specifically about using WebMatrix (not ASP.NET Web Pages).</span></span> <span data-ttu-id="a2e5b-328">Bu biraz bazı ek özellikleri Bu öğretici kümesinde ele olmaz WebMatrix hakkında daha fazla ayrıntı geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a2e5b-328">It goes into a little more detail about some of the additional features of WebMatrix that we won't cover in this tutorial set.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a2e5b-329">Next</span><span class="sxs-lookup"><span data-stu-id="a2e5b-329">Next</span></span>](intro-to-web-pages-programming.md)
