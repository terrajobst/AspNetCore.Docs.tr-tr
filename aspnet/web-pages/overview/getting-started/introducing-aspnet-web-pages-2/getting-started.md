---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: "ASP.NET Web sayfaları Başlarken - giriş | Microsoft Docs"
author: tfitzmac
description: "WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Visual Studio veya Visual Studio kodunu kullanın. Bu kılavuz bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: a6789ee75b4ca6e9443681cc7ec0bd3ab94cedcd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="introducing-aspnet-web-pages---getting-started"></a><span data-ttu-id="4dc8a-105">ASP.NET Web sayfalarını - Başlarken tanıtma</span><span class="sxs-lookup"><span data-stu-id="4dc8a-105">Introducing ASP.NET Web Pages - Getting Started</span></span>
====================
<span data-ttu-id="4dc8a-106">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4dc8a-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> > [!NOTE] 
> > 
> > <span data-ttu-id="4dc8a-107">WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-107">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="4dc8a-108">Kullanım [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) veya [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="4dc8a-108">Use [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>
> 
> 
> <span data-ttu-id="4dc8a-109">Bu yönerge ve uygulama size genel bir bakış, ASP.NET Web sayfaları (sürüm 2 veya sonrası) ve Razor sözdizimi, dinamik Web siteleri oluşturmak için basit bir çerçeve.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-109">This guidance and application gives you an overview of ASP.NET Web Pages (version 2 or later) and Razor syntax, a lightweight framework for creating dynamic websites.</span></span> <span data-ttu-id="4dc8a-110">Aynı zamanda, WebMatrix, sayfaları ve siteleri oluşturmak için bir araç sunar.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-110">It also introduces WebMatrix, a tool for creating pages and sites.</span></span>
> 
> <span data-ttu-id="4dc8a-111">**Düzey**: ASP.NET Web sayfaları için yeni.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-111">**Level**: New to ASP.NET Web Pages.</span></span>  
> <span data-ttu-id="4dc8a-112">**Kabul becerileri**: HTML, temel geçişli stil sayfaları (CSS).</span><span class="sxs-lookup"><span data-stu-id="4dc8a-112">**Skills assumed**: HTML, basic cascading style sheets (CSS).</span></span>
> 
> <span data-ttu-id="4dc8a-113">Kümenin ilk öğreticide öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-113">What you'll learn in the first tutorial of the set:</span></span>
> 
> - <span data-ttu-id="4dc8a-114">İçin nedir ve ne ASP.NET Web sayfaları teknolojidir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-114">What ASP.NET Web Pages technology is and what it's for.</span></span>
> - <span data-ttu-id="4dc8a-115">WebMatrix nedir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-115">What WebMatrix is.</span></span>
> - <span data-ttu-id="4dc8a-116">Her şeyi yükleme.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-116">How to install everything.</span></span>
> - <span data-ttu-id="4dc8a-117">WebMatrix kullanarak Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4dc8a-117">How to create a website by using WebMatrix.</span></span>
>   
> 
> <span data-ttu-id="4dc8a-118">Özellikler/teknolojilerini ele alınan:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-118">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="4dc8a-119">Microsoft Web Platformu yükleyicisi.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-119">Microsoft Web Platform Installer.</span></span>
> - <span data-ttu-id="4dc8a-120">WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-120">WebMatrix.</span></span>
> - <span data-ttu-id="4dc8a-121">*.cshtml* sayfaları</span><span class="sxs-lookup"><span data-stu-id="4dc8a-121">*.cshtml* pages</span></span>
>   
> 
> <span data-ttu-id="4dc8a-122">Can Pope ilk olarak Bu öğretici yazıldı.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-122">Mike Pope originally wrote this tutorial.</span></span> <span data-ttu-id="4dc8a-123">Zel FitzMacken için Microsoft WebMatrix 3 güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-123">Tom FitzMacken updated it for Microsoft WebMatrix 3.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4dc8a-124">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="4dc8a-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4dc8a-125">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="4dc8a-125">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="4dc8a-126">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="4dc8a-126">WebMatrix 3</span></span>


## <a name="what-should-you-know"></a><span data-ttu-id="4dc8a-127">Bilmeniz gerekenler?</span><span class="sxs-lookup"><span data-stu-id="4dc8a-127">What Should You Know?</span></span>

<span data-ttu-id="4dc8a-128">Alışık olduğunuz varsayılarak:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-128">We're assuming that you're familiar with:</span></span>

- <span data-ttu-id="4dc8a-129">**HTML**.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-129">**HTML**.</span></span> <span data-ttu-id="4dc8a-130">Hiçbir ayrıntılı uzmanlık gereklidir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-130">No in-depth expertise is required.</span></span> <span data-ttu-id="4dc8a-131">HTML açıklayan çalışmaz, ancak biz de karmaşık bir şey kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-131">We won't explain HTML, but we also don't use anything complex.</span></span> <span data-ttu-id="4dc8a-132">Burada yararlı oldukları düşünüyoruz HTML öğreticileri bağlantılar sağlarız.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-132">We'll provide links to HTML tutorials where we think they're useful.</span></span>
- <span data-ttu-id="4dc8a-133">**Geçişli stil sayfaları (CSS)**.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-133">**Cascading style sheets (CSS)**.</span></span> <span data-ttu-id="4dc8a-134">Aynı HTML ile.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-134">Same as with HTML.</span></span>
- <span data-ttu-id="4dc8a-135">**Temel veritabanı fikirleri**.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-135">**Basic database ideas**.</span></span> <span data-ttu-id="4dc8a-136">Artık veriler için bir elektronik tablo kullanılan ve sıralanmış ve uzmanlık düzeyini verileri, filtre, biz Bu öğretici kümesi için genellikle varsayılarak.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-136">If you've used a spreadsheet for data and sorted and filtered the data, that's the level of expertise we're generally assuming for this tutorial set.</span></span>

<span data-ttu-id="4dc8a-137">Biz de temel programlama öğrenme ilgilenen varsayılarak.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-137">We're also assuming that you're interested in learning basic programming.</span></span> <span data-ttu-id="4dc8a-138">ASP.NET Web sayfaları adlı C# programlama diliyle kullanın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-138">ASP.NET Web Pages use a programming language called C#.</span></span> <span data-ttu-id="4dc8a-139">Programlama, yalnızca bu ilgi içindeki tüm arka plan olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-139">You don't have to have any background in programming, just an interest in it.</span></span> <span data-ttu-id="4dc8a-140">Bir web sayfasında önce tüm JavaScript hiç yazdıysanız arka plan Eskinin açıyor.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-140">If you've ever written any JavaScript in a web page before, you've got plenty of background.</span></span>

<span data-ttu-id="4dc8a-141">Biz yeni programcıları hızlıca Getir sırada ile programlama hakkında bilginiz varsa, bu öğreticiyi tamamlarken belirlenir fark edebilirsiniz Not yavaş taşır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-141">Note that if you are familiar with programming, you might find that this tutorial set initially moves slowly while we bring new programmers up to speed.</span></span> <span data-ttu-id="4dc8a-142">Biz ilk birkaç öğreticileri aldıkça, yine de olacaktır açıklamak için daha az temel programlama ve şeyler bir daha hızlı klibi taşınır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-142">As we get past the first few tutorials, though, there will be less basic programming to explain and things will move along at a faster clip.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="4dc8a-143">Ne ihtiyacınız var?</span><span class="sxs-lookup"><span data-stu-id="4dc8a-143">What Do You Need?</span></span>

<span data-ttu-id="4dc8a-144">İşte gerekenler:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-144">Here's what you'll need:</span></span>

- <span data-ttu-id="4dc8a-145">Windows 8, Windows 7, Windows Server 2008 veya Windows Server 2012 çalıştıran bir bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-145">A computer that is running Windows 8, Windows 7, Windows Server 2008, or Windows Server 2012.</span></span>
- <span data-ttu-id="4dc8a-146">Canlı bir Internet bağlantısı.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-146">A live internet connection.</span></span>
- <span data-ttu-id="4dc8a-147">Yönetici ayrıcalıkları (yükleme işlemi için gereklidir).</span><span class="sxs-lookup"><span data-stu-id="4dc8a-147">Administrator privileges (required for the installation process).</span></span>

## <a name="what-is-aspnet-web-pages"></a><span data-ttu-id="4dc8a-148">ASP.NET Web sayfaları nedir?</span><span class="sxs-lookup"><span data-stu-id="4dc8a-148">What Is ASP.NET Web Pages?</span></span>

<span data-ttu-id="4dc8a-149">ASP.NET Web sayfaları, dinamik web sayfaları oluşturmak için kullanabileceğiniz bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-149">ASP.NET Web Pages is a framework that you can use to create dynamic web pages.</span></span> <span data-ttu-id="4dc8a-150">Basit bir HTML web sayfası statik; içeriği sayfa sabit HTML biçimlendirmesi tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-150">A simple HTML web page is static; its content is determined by the fixed HTML markup that's in the page.</span></span> <span data-ttu-id="4dc8a-151">ASP.NET Web sayfaları ile oluşturduğunuz gelenler dinamik sayfalar kod kullanarak hızla, sayfa içeriği oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-151">Dynamic pages like those you create with ASP.NET Web Pages let you create the page content on the fly, by using code.</span></span>

<span data-ttu-id="4dc8a-152">Dinamik sayfalar, her türlü şey yapmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-152">Dynamic pages let you do all sorts of things.</span></span> <span data-ttu-id="4dc8a-153">Bir kullanıcı girişi için form kullanarak isteyin ve sonra ne sayfasını görüntüler veya nasıl göründüğünü değiştirin.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-153">You can ask a user for input by using a form and then change what the page displays or how it looks.</span></span> <span data-ttu-id="4dc8a-154">Bir kullanıcıdan bilgi alın, bir veritabanına kaydetme ve daha sonra listesi.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-154">You can take information from a user, save it in a database, and then list it later.</span></span> <span data-ttu-id="4dc8a-155">Sitenizden e-posta gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-155">You can send email from your site.</span></span> <span data-ttu-id="4dc8a-156">(Örneğin, bir eşleme hizmeti) Web'de diğer hizmetlerle etkileşim ve bu kaynaklardan bilgi tümleştirmek sayfaları üretir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-156">You can interact with other services on the web (for example, a mapping service) and produce pages that integrate information from those sources.</span></span>

## <a name="what-is-webmatrix"></a><span data-ttu-id="4dc8a-157">WebMatrix nedir?</span><span class="sxs-lookup"><span data-stu-id="4dc8a-157">What is WebMatrix?</span></span>

<span data-ttu-id="4dc8a-158">WebMatrix tümleşen bir web sayfası Düzenleyicisi, veritabanı yardımcı programı, sayfalar ve Internet'e Web sitenizi yayımlama özellikleri test etmek için bir web sunucusu bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-158">WebMatrix is a tool that integrates a web page editor, a database utility, a web server for testing pages, and features for publishing your website to the Internet.</span></span> <span data-ttu-id="4dc8a-159">WebMatrix ücretsiz ve yüklemek kolay ve kullanımı kolay olur.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-159">WebMatrix is free, and it's easy to install and easy to use.</span></span> <span data-ttu-id="4dc8a-160">(Ayrıca PHP gibi diğer teknolojileri yanı sıra, yalnızca düz HTML sayfaları için çalışır.)</span><span class="sxs-lookup"><span data-stu-id="4dc8a-160">(It also works for just plain HTML pages, as well as for other technologies like PHP.)</span></span>

<span data-ttu-id="4dc8a-161">Gerçekte yok *sahip* WebMatrix ASP.NET Web sayfaları ile çalışmak için kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-161">You don't actually *have* to use WebMatrix to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="4dc8a-162">Sayfaları bir metin kullanarak Düzenleyicisi oluşturabilir örneğin ve erişimi olan bir web sunucusu kullanarak sayfaları test.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-162">You can create pages by using a text editor, for example, and test pages by using a web server that you have access to.</span></span> <span data-ttu-id="4dc8a-163">Bu öğreticileri boyunca WebMatrix kullanacak şekilde ancak, WebMatrix tümünü çok, kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-163">However, WebMatrix makes it all very easy, so these tutorials will use WebMatrix throughout.</span></span>

## <a name="about-these-tutorials"></a><span data-ttu-id="4dc8a-164">Bu öğreticiler hakkında</span><span class="sxs-lookup"><span data-stu-id="4dc8a-164">About These Tutorials</span></span>

<span data-ttu-id="4dc8a-165">Bu öğretici ASP.NET Web sayfalarının nasıl kullanılacağını giriş kümesidir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-165">This tutorial set is an introduction to how to use ASP.NET Web Pages.</span></span> <span data-ttu-id="4dc8a-166">Bu Tanıtım öğretici kümesinde toplam 9 öğreticileri vardır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-166">There are 9 tutorials total in this introductory tutorial set.</span></span> <span data-ttu-id="4dc8a-167">Gerçek, profesyonel görünümlü Web siteleri oluşturmak için ASP.NET Web sayfaları yeni başlayan alan bir dizi öğretici kümeleri parçası kullanıcının.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-167">It's part of a series of tutorial sets that takes you from ASP.NET Web Pages novice to creating real, professional-looking websites.</span></span>

<span data-ttu-id="4dc8a-168">Bu ilk öğreticide, ASP.NET Web sayfaları ile çalışma konusunda temelleri gösteren concentrates ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-168">This first tutorial set concentrates on showing you the basics of how to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="4dc8a-169">İşiniz bittiğinde, burada bunu sona erer ve daha ayrıntılı olarak, Web sayfaları keşfedin almak ek öğretici kümeleri ile çalışabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-169">When you're done, you can work with additional tutorial sets that pick up where this one ends and that explore Web Pages in more depth.</span></span>

<span data-ttu-id="4dc8a-170">Biz kasıtlı olarak hakkında ayrıntılı açıklamalar kolay gidin.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-170">We deliberately go easy on the in-depth explanations.</span></span> <span data-ttu-id="4dc8a-171">Ve bir şey gösteriyoruz olduğunda, Bu öğretici kümesi için her zaman düşünüyoruz şekilde anlaşılması kolay seçeneğini belirledik.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-171">And whenever we show something, for this tutorial set we always choose the way that we think is easiest to understand.</span></span> <span data-ttu-id="4dc8a-172">Sonraki öğretici kümeleri daha kapsamlı gidin ve daha verimli ya da daha esnek yaklaşımlar (aynı zamanda daha eğlenceli) gösterir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-172">Later tutorial sets go into more depth and show you more efficient or more flexible approaches (also more fun).</span></span> <span data-ttu-id="4dc8a-173">Ancak bu öğreticiler öncelikle temel kavramları anlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-173">But those tutorials require you to understand the basics first.</span></span>

<span data-ttu-id="4dc8a-174">Bu özellikler, ASP.NET Web sayfaları yalnızca başladıktan öğretici kümesi kapsar:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-174">The tutorial set you've just started covers these features of ASP.NET Web Pages:</span></span>

- <span data-ttu-id="4dc8a-175">Giriş ve yüklü her şeyi alınıyor.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-175">Introduction and getting everything installed.</span></span> <span data-ttu-id="4dc8a-176">(Makaleyi okuduğunuz öğreticide olmasıdır.)</span><span class="sxs-lookup"><span data-stu-id="4dc8a-176">(That's in the tutorial you're reading.)</span></span>
- <span data-ttu-id="4dc8a-177">ASP.NET Web sayfaları Programlama temelleri.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-177">The basics of ASP.NET Web Pages programming.</span></span>
- <span data-ttu-id="4dc8a-178">Veritabanı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-178">Creating a database.</span></span>
- <span data-ttu-id="4dc8a-179">Oluşturma ve bir kullanıcı giriş formunu işleme.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-179">Creating and processing a user input form.</span></span>
- <span data-ttu-id="4dc8a-180">Ekleme, güncelleştirme ve veritabanındaki verileri silme.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-180">Adding, updating, and deleting data in the database.</span></span>

## <a name="what-will-you-create"></a><span data-ttu-id="4dc8a-181">Ne oluşturacaksınız?</span><span class="sxs-lookup"><span data-stu-id="4dc8a-181">What Will You Create?</span></span>

<span data-ttu-id="4dc8a-182">Bu öğretici ayarlayın ve sonraki olanları istediğiniz filmler listelediğiniz yerde bir Web sitesi Uzayda Döndür.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-182">This tutorial set and subsequent ones revolve around a website where you can list movies that you like.</span></span> <span data-ttu-id="4dc8a-183">Film girin, bunları düzenleyin ve bunları listesinde görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-183">You'll be able to enter movies, edit them, and list them.</span></span> <span data-ttu-id="4dc8a-184">Bu öğretici kümesinde oluşturacaksınız sayfaları birkaç şunlardır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-184">Here are a couple of the pages you'll create in this tutorial set.</span></span> <span data-ttu-id="4dc8a-185">Birinci oluşturacağınız sayfa listeleme film gösterir:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-185">The first one shows the movie listing page that you'll create:</span></span>

![Film listesini gösteren tamamladı film uygulaması](getting-started/_static/image1.png)

<span data-ttu-id="4dc8a-187">Ve yeni film bilgileri sitenize eklemenize olanak sağlayan sayfa şöyledir:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-187">And here's the page that lets you add new movie information to your site:</span></span>

![Film ekleme sayfasını gösteren tamamlanmış film uygulaması](getting-started/_static/image2.png)

<span data-ttu-id="4dc8a-189">Bu sonraki öğretici kümeleri yapı ayarlayın ve resimleri karşıya yükleme, oturum açma kişiler izin vererek, e-posta göndermek ve sosyal medya ile tümleştirme gibi daha fazla işlevsellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-189">Subsequent tutorial sets build on this set and add more functionality, like uploading pictures, letting people log in, sending email, and integrating with social media.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="4dc8a-190">Azure üzerinde çalışan bu uygulamayı bakın</span><span class="sxs-lookup"><span data-stu-id="4dc8a-190">See this App Running on Azure</span></span>

<span data-ttu-id="4dc8a-191">Canlı web uygulaması çalışan tamamlanmış site görmek ister misiniz?</span><span class="sxs-lookup"><span data-stu-id="4dc8a-191">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="4dc8a-192">Aşağıdaki düğmeyi tıklatarak, uygulama tam sürümü Azure hesabınızda dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-192">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

<span data-ttu-id="4dc8a-193">Bu çözüm Azure'a dağıtmak için bir Azure hesabınız olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-193">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="4dc8a-194">Bir hesap zaten yoksa, aşağıdaki seçenekler vardır:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-194">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="4dc8a-195">[Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-195">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="4dc8a-196">[MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-196">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="installing-everything"></a><span data-ttu-id="4dc8a-197">Her şeyi yükleme</span><span class="sxs-lookup"><span data-stu-id="4dc8a-197">Installing Everything</span></span>

<span data-ttu-id="4dc8a-198">Her şeyi, Microsoft Web Platformu Yükleyicisi'ni kullanarak yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-198">You can install everything by using the Web Platform Installer from Microsoft.</span></span> <span data-ttu-id="4dc8a-199">Aslında, yükleyici yükleyin ve şey yüklemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-199">In effect, you install the installer, and then use it to install everything else.</span></span>

<span data-ttu-id="4dc8a-200">Web sayfaları kullanmak için olması en az olması yüklü SP3 ile Windows XP veya Windows Server 2008 veya üstü.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-200">To use Web Pages, you have to be have at least Windows XP with SP3 installed, or Windows Server 2008 or later.</span></span>

<span data-ttu-id="4dc8a-201">Üzerinde [Web Pages sayfası](../../../index.md) ASP.NET Web sitesi tıklatın **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-201">On the [Web Pages page](../../../index.md) of the ASP.NET website, click **Install**.</span></span>

![ASP.NET Web sitesi gösteren &quot;Webmatrix'i yükleyin&quot; düğmesi](getting-started/_static/image3.png)

<span data-ttu-id="4dc8a-203">Lisans koşullarını ve WebMatrix yüklemeden önce gizlilik bildirimini kabul istenir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-203">You are asked to accept the license terms and privacy statement before installing WebMatrix.</span></span>

![yüklemeye başlamak için koşulu kabul](getting-started/_static/image4.png)

<span data-ttu-id="4dc8a-205">Tıklatın **çalıştırmak** yükleme başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-205">Click **Run** to start the installation.</span></span> <span data-ttu-id="4dc8a-206">(Yükleyici kaydetmek istiyorsanız, tıklatın **kaydetmek** ve kaydettiğiniz bu klasörden yükleyiciyi çalıştırın.)</span><span class="sxs-lookup"><span data-stu-id="4dc8a-206">(If you want to save the installer, click **Save** and then run the installer from the folder where you saved it.)</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="4dc8a-207">Web Platformu yükleyicisi görünür, WebMatrix yüklemek için hazır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-207">The Web Platform Installer appears, ready to install WebMatrix.</span></span> <span data-ttu-id="4dc8a-208">**Yükle**'ye tıklatın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-208">Click **Install**.</span></span>

![](getting-started/_static/image6.png)

<span data-ttu-id="4dc8a-209">Yükleme işleminin ne onu bilgisayarınıza yüklemek üzere olduğunu rakamları ve işlemi başlatır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-209">The installation process figures out what it has to install on your computer and starts the process.</span></span> <span data-ttu-id="4dc8a-210">Ne tam olarak yüklü olması gereken bağlı olarak, işlem herhangi bir yere birkaç dakika sonra birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-210">Depending on what exactly has to be installed, the process can take anywhere from a few moments to several minutes.</span></span> <span data-ttu-id="4dc8a-211">Seçin **kabul ediyorum** lisans koşullarını kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-211">Select **I Accept** to accept the license terms.</span></span>

## <a name="hello-webmatrix"></a><span data-ttu-id="4dc8a-212">Merhaba, WebMatrix</span><span class="sxs-lookup"><span data-stu-id="4dc8a-212">Hello, WebMatrix</span></span>

<span data-ttu-id="4dc8a-213">Yükleme işlemi tamamlandığında, WebMatrix otomatik olarak başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-213">When it's done, the installation process can launch WebMatrix automatically.</span></span> <span data-ttu-id="4dc8a-214">Windows, gelen içermiyorsa **Başlat** menüsü, başlatma **Microsoft WebMatrix**.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-214">If it doesn't, in Windows, from the **Start** menu, launch **Microsoft WebMatrix**.</span></span>

<span data-ttu-id="4dc8a-215">WebMatrix ilk kez başlattığınızda, Microsoft Azure için Microsoft hesabınızla oturum açmak için bir fırsat verilir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-215">When you launch WebMatrix for the first time, you are given a chance to sign in to Microsoft Azure with your Microsoft account.</span></span> <span data-ttu-id="4dc8a-216">Oturum açma tarafından 10 ücretsiz web uygulamaları Azure aracılığıyla alırsınız.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-216">By signing in, you will receive 10 free web apps through Azure.</span></span> <span data-ttu-id="4dc8a-217">Bu ücretsiz web uygulamaları uygulamalarınızı test etmek için kolay bir yol sağlamak.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-217">These free web apps provide a convenient way to test your apps.</span></span> <span data-ttu-id="4dc8a-218">Zaten bir Azure hesabınız yoksa, ancak bir MSDN aboneliğiniz varsa, şunları yapabilirsiniz [MSDN abonelik Avantajlarınızı etkinleştirebilir](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="4dc8a-218">If you don't already have an Azure account, but you do have an MSDN subscription, you can [activate your MSDN subscription benefits](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="4dc8a-219">Aksi takdirde, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-219">Otherwise, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4dc8a-220">Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="4dc8a-220">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="4dc8a-221">Bu öğretici ile devam etmek şu anda oturum gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-221">You do not have to sign in right now to continue with this tutorial.</span></span> <span data-ttu-id="4dc8a-222">Şimdi oturum değil, daha sonra oturum açmak için seçeneği hala gerekir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-222">If you do not sign in now, you will still have the option to sign in later.</span></span> <span data-ttu-id="4dc8a-223">Son [konu](publishing.md) Bu öğreticide serisi için Azure Web sitenizi dağıtmak alınmaktadır; bu nedenle, bu konuda tamamlamak oturum açmanız.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-223">The last [topic](publishing.md) in this tutorial series covers how to deploy your website to Azure; therefore, you would need to sign in to complete that topic.</span></span>

<span data-ttu-id="4dc8a-224">Bu noktada, ya da, Microsoft hesabı veya Seç oturum açmanız **şimdi değil** alt köşedeki.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-224">At this point, either sign in with your Microsoft account or select **Not Now** in the lower right corner.</span></span>

![Oturum Aç](getting-started/_static/image7.png)

<span data-ttu-id="4dc8a-226">Başlamak için boş bir Web sitesi oluşturmak ve bir sayfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-226">To begin, you'll create a blank website and add a page.</span></span> <span data-ttu-id="4dc8a-227">Sonraki Öğreticide bu kümesindeki yerleşik Web şablonlarından birini yürütmek.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-227">In a later tutorial in this set you'll play with one of the built-in website templates.</span></span>

<span data-ttu-id="4dc8a-228">Başlangıç penceresinde **yeni**.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-228">In the start window, click **New**.</span></span>

![WebMatrix başlangıç ekranı](getting-started/_static/image8.png)

<span data-ttu-id="4dc8a-230">Önceden oluşturulmuş dosyalar ve farklı türde Web siteleri için sayfaları şablonlarıdır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-230">Templates are prebuilt files and pages for different types of websites.</span></span> <span data-ttu-id="4dc8a-231">Tüm varsayılan olarak kullanılabilir şablonları görmek için Şablon Galerisi seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-231">To see all of the templates that are available by default, select the Template Gallery option.</span></span>

![Select Şablon Galerisi](getting-started/_static/image9.png)

<span data-ttu-id="4dc8a-233">İçinde **Hızlı Başlangıç** penceresinde, seçin **boş Site** gelen **ASP.NET** grup ve "WebPagesMovies" Yeni sitesini adlandırın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-233">In the **Quick Start** window, select **Empty Site** from the **ASP.NET** group and name the new site "WebPagesMovies".</span></span>

![WebMatrix hızlı başlangıç penceresinde seçili boş Site şablonuyla](getting-started/_static/image10.png)

<span data-ttu-id="4dc8a-235">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-235">Click **Next**.</span></span>

<span data-ttu-id="4dc8a-236">Microsoft hesabınızda oturum açmanızdan, Azure'da site oluşturma fırsatınız olur.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-236">If you have signed in to your Microsoft account, you will be given the opportunity to create the site on Azure.</span></span> <span data-ttu-id="4dc8a-237">Varsayılan adı sitenizin adını temel alarak **WebPagesMovies.azurewebsites.net** önerilir; ancak, bu adı Windows Azure üzerinde kullanılabilir değil ünlem gösterir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-237">Based on the name of your site, the default name of **WebPagesMovies.azurewebsites.net** is suggested; however, the exclamation point indicates that this name is not available on Windows Azure.</span></span> <span data-ttu-id="4dc8a-238">Kolaylık olması için seçin **atla** Azure üzerinde şu anda web sitesi oluşturma atlamak için.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-238">For simplicity, select **Skip** to bypass creating the web site on Azure right now.</span></span> <span data-ttu-id="4dc8a-239">Bu seri biz site Azure'a yayımlayacak.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-239">Later in this series, we will publish the site to Azure.</span></span>

![Azure sitesi oluştur](getting-started/_static/image11.png)

<span data-ttu-id="4dc8a-241">WebMatrix oluşturur ve site açar:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-241">WebMatrix creates and opens the site:</span></span>

![Yeni WebPagesMovies sitesini Webmatrix'te açın](getting-started/_static/image12.png)

<span data-ttu-id="4dc8a-243">En üstte bir hızlı erişim araç çubuğu ve Şerit yoktur.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-243">At the top, there's a Quick Access Toolbar and a ribbon.</span></span> <span data-ttu-id="4dc8a-244">Altındaki sol çalışma alanı seçicisinde görevleri arasında geçiş Burada gördüğünüz (**Site**, **dosyaları**, **veritabanları**, **raporları**).</span><span class="sxs-lookup"><span data-stu-id="4dc8a-244">At the bottom left, you see the workspace selector where you switch between tasks (**Site**, **Files**, **Databases**, **Reports**).</span></span> <span data-ttu-id="4dc8a-245">Sağ tarafta içerik bölmesinde Düzenleyicisi ve raporları içindir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-245">On the right is the content pane for the editor and for reports.</span></span> <span data-ttu-id="4dc8a-246">Ve alt arasında bazen iletiler için bir bildirim çubuğu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-246">And across the bottom you'll occasionally see a notification bar for messages.</span></span>

<span data-ttu-id="4dc8a-247">Hakkında daha fazla WebMatrix ve özelliklerini bu öğreticileri ilerledikçe öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-247">You'll learn more about WebMatrix and its features as you go through these tutorials.</span></span>

## <a name="creating-a-web-page"></a><span data-ttu-id="4dc8a-248">Web sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="4dc8a-248">Creating a Web Page</span></span>

<span data-ttu-id="4dc8a-249">WebMatrix ve ASP.NET Web sayfaları konusunda bilgi sahibi olmak için basit bir sayfa oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-249">To become familiar with WebMatrix and ASP.NET Web Pages, you'll create a simple page.</span></span>

<span data-ttu-id="4dc8a-250">Çalışma alanı seçicisinde seçin **dosyaları** çalışma.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-250">In the workspace selector, select the **Files** workspace.</span></span> <span data-ttu-id="4dc8a-251">Bu çalışma alanı, dosyalar ve klasörlerle çalışmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-251">This workspace lets you work with files and folders.</span></span> <span data-ttu-id="4dc8a-252">Sol bölmede, sitenizin dosya yapısı gösterir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-252">The left pane shows the file structure of your site.</span></span> <span data-ttu-id="4dc8a-253">Dosya ile ilgili görevleri göstermek için Şerit değişir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-253">The ribbon changes to show file-related tasks.</span></span>

![WebMatrix çalışma dosyası](getting-started/_static/image13.png)

<span data-ttu-id="4dc8a-255">Şeritte altında oku **yeni** ve ardından **yeni dosya**.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-255">In the ribbon, click the arrow under **New** and then click **New File**.</span></span>

![Kullanarak &quot;yeni&quot; yeni bir dosya oluşturmak için Şerit'te komutu](getting-started/_static/image14.png)

<span data-ttu-id="4dc8a-257">WebMatrix dosya türlerinin bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-257">WebMatrix displays a list of file types.</span></span> <span data-ttu-id="4dc8a-258">Seçin **CSHTML**hem de **adı** "HelloWorld" yazın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-258">Select **CSHTML**, and in the **Name** box, type "HelloWorld".</span></span> <span data-ttu-id="4dc8a-259">Bir ASP.NET Web Pages sayfası CSHTML sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-259">A CSHTML page is an ASP.NET Web Pages page.</span></span>

![HelloWorld.cshtml adlı yeni bir CSHTML sayfa oluşturma](getting-started/_static/image15.png)

<span data-ttu-id="4dc8a-261">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-261">Click **OK**.</span></span>

<span data-ttu-id="4dc8a-262">WebMatrix, sayfa oluşturur ve Düzenleyicisi'nde açar.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-262">WebMatrix creates the page and opens it in the editor.</span></span>

![WebMatrix Düzenleyicisi'nde yeni HelloWorld sayfası](getting-started/_static/image16.png)

<span data-ttu-id="4dc8a-264">Gördüğünüz gibi sayfa şöyle üst blok dışında sıradan HTML biçimlendirmesi çoğunlukla içerir:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-264">As you can see, the page contains mostly ordinary HTML markup, except for a block at the top that looks like this:</span></span>

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

<span data-ttu-id="4dc8a-265">Bu kod, ekleme için kısa bir süre içinde anlatıldığı gibi.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-265">That's for adding code, as you'll see shortly.</span></span>

<span data-ttu-id="4dc8a-266">Dikkat sayfasının farklı bölümleri &mdash; öğe adları, öznitelikleri ve metin artı üst blok — farklı renkler tüm yazılımında.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-266">Notice that the different parts of the page &mdash; the element names, attributes, and text, plus the block at the top — are all in different colors.</span></span> <span data-ttu-id="4dc8a-267">Bu adlı *sözdizimi vurgulama*, ve her şeyi açık tutmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-267">This is called *syntax highlighting*, and it makes it easier to keep everything clear.</span></span> <span data-ttu-id="4dc8a-268">WebMatrix web sayfalarında çalışmak kolaylaştırır özelliklerden biridir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-268">It's one of the features that makes it easy to work with web pages in WebMatrix.</span></span>

<span data-ttu-id="4dc8a-269">İçerik için ekleme `<head>` ve `<body>` aşağıdaki örnekte gibi öğeler.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-269">Add content for the `<head>` and `<body>` elements like in the following example.</span></span> <span data-ttu-id="4dc8a-270">(İstediğiniz yaparsanız, yalnızca aşağıdaki bloğu kopyalayın ve tüm mevcut sayfa bu kod ile değiştirin.)</span><span class="sxs-lookup"><span data-stu-id="4dc8a-270">(If you want, you can just copy the following block and replace the entire existing page with this code.)</span></span>

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

<span data-ttu-id="4dc8a-271">Hızlı Erişim Araç veya **dosya** menüsünde tıklatın **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-271">In the Quick Access Toolbar or in the **File** menu, click **Save**.</span></span>

![WebMatrix hızlı erişim araç çubuğunda Kaydet düğmesi](getting-started/_static/image17.png)

## <a name="testing-the-page"></a><span data-ttu-id="4dc8a-273">Sayfa test etme</span><span class="sxs-lookup"><span data-stu-id="4dc8a-273">Testing the Page</span></span>

<span data-ttu-id="4dc8a-274">İçinde **dosyaları** çalışma alanında, sağ *HelloWorld.cshtml* sayfasında ve ardından **başlatma tarayıcıda**.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-274">In the **Files** workspace, right-click the *HelloWorld.cshtml* page and then click **Launch in browser**.</span></span>

![WebMatrix Şeritte Çalıştır düğmesini kullanarak bir sayfa çalıştırma](getting-started/_static/image18.png)

<span data-ttu-id="4dc8a-276">WebMatrix, bilgisayarınızdaki sayfaları test etmek için kullanabileceğiniz bir yerleşik web sunucusu (IIS Express) başlatır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-276">WebMatrix starts a built-in web server (IIS Express) that you can use to test pages on your computer.</span></span> <span data-ttu-id="4dc8a-277">(WebMatrix, IIS Express, test edebilirsiniz önce sayfanızı bir web sunucusuna herhangi bir yerde yayımlamanız gerekir.) Sayfa varsayılan tarayıcınızda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-277">(Without IIS Express in WebMatrix, you'd have to publish your page to a web server somewhere before you could test it.) The page is displayed in your default browser.</span></span>

![&quot;Merhaba Dünya&quot; tarayıcıda çalışan sayfası](getting-started/_static/image19.png)

<span data-ttu-id="4dc8a-279">Webmatrix'te bir sayfayı test ettiğinizde, tarayıcıda URL şöyle olduğuna dikkat edin `http://localhost:33651/HelloWorld.cshtml.` adı *localhost* sayfa kendi bilgisayarınızda bir web sunucusu tarafından hizmet verilen anlamına gelir, yerel bir sunucuya başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-279">Notice that when you test a page in WebMatrix, the URL in the browser is something like `http://localhost:33651/HelloWorld.cshtml.` The name *localhost* refers to a local server, meaning that the page is served by a web server that's on your own computer.</span></span> <span data-ttu-id="4dc8a-280">WebMatrix belirtildiği gibi bir sayfa başlattığında çalıştırılan IIS Express adlı bir web sunucusu programı içerir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-280">As noted, WebMatrix includes a web server program named IIS Express that runs when you launch a page.</span></span>

<span data-ttu-id="4dc8a-281">Sonra sayı *localhost* (örneğin, *localhost:33651*) başvurduğu bir *bağlantı noktası numarası* bilgisayarınızda.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-281">The number after *localhost* (for example, *localhost:33651*) refers to a *port number* on your computer.</span></span> <span data-ttu-id="4dc8a-282">"Bu belirli Web sitesi için IIS Express kullanan kanal" sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-282">This is the number of the "channel" that IIS Express uses for this particular website.</span></span> <span data-ttu-id="4dc8a-283">Bağlantı noktası numarası rastgele 1024 ile 65536 aralığından sitenizi oluşturmak ve oluşturduğunuz her site için farklı seçilir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-283">The port number is selected at random from the range 1024 through 65536 when you create your site, and it's different for every site that you create.</span></span> <span data-ttu-id="4dc8a-284">(Kendi site test ettiğinizde, bağlantı noktası numarasını neredeyse kesinlikle 33561 daha farklı bir numara olacaktır.) Her Web sitesi için farklı bir bağlantı noktası kullanarak, IIS Express için Konuşmayı sitelerinizi hangisinin düz kullanmaya devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-284">(When you test your own site, the port number will almost certainly be a different number than 33561.) By using a different port for each website, IIS Express can keep straight which of your sites it's talking to.</span></span>

<span data-ttu-id="4dc8a-285">Artık görmek sitenizi bir ortak web sunucusuna yayımladığınızda sonraki *localhost* URL.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-285">Later when you publish your site to a public web server, you no longer see *localhost* in the URL.</span></span> <span data-ttu-id="4dc8a-286">Bu noktada, daha genel bir URL gibi görürsünüz `http://myhostingsite/mywebsite/HelloWorld.cshtml` veya ne olursa olsun sayfasıdır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-286">At that point, you'll see a more typical URL like `http://myhostingsite/mywebsite/HelloWorld.cshtml` or whatever the page is.</span></span> <span data-ttu-id="4dc8a-287">Bu öğretici serisinde daha sonra bir site yayımlama hakkında daha fazla bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-287">You'll learn more about publishing a site later in this tutorial series.</span></span>

## <a name="adding-some-server-side-code"></a><span data-ttu-id="4dc8a-288">Bazı sunucu tarafı kod ekleme</span><span class="sxs-lookup"><span data-stu-id="4dc8a-288">Adding Some Server-Side Code</span></span>

<span data-ttu-id="4dc8a-289">Tarayıcıyı kapatın ve WebMatrix sayfasına geri dönün.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-289">Close the browser and go back to the page in WebMatrix.</span></span>

<span data-ttu-id="4dc8a-290">Aşağıdaki kod gibi görünüyor. böylece bir satır kod bloğunu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-290">Add a line to the code block so that it looks like the following code:</span></span>

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

<span data-ttu-id="4dc8a-291">Biraz Razor kodunun budur.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-291">This is a little bit of Razor code.</span></span> <span data-ttu-id="4dc8a-292">Geçerli tarih ve saati alır ve bu değeri içine yerleştirir büyük olasılıkla boş olduğundan bir *değişkeni* adlı `currentDateTime`.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-292">It's probably clear that it gets the current date and time and puts that value into a *variable* named `currentDateTime`.</span></span> <span data-ttu-id="4dc8a-293">Daha fazla okuyacaksınız sonraki öğretici Razor söz dizimi hakkında.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-293">You'll read more about Razor syntax in the next tutorial.</span></span>

<span data-ttu-id="4dc8a-294">Sayfasının gövdesindeki sonra `<p>Hello World!</p>` öğesi, aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-294">In the body of the page, after the `<p>Hello World!</p>` element, add the following:</span></span>

[!code-html[Main](getting-started/samples/sample4.html)]

<span data-ttu-id="4dc8a-295">Bu kod, yerleştirin değerini alır `currentDateTime` üst değişken ve sayfanın biçimlendirmesine ekler.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-295">This code gets the value that you put into the `currentDateTime` variable at the top and inserts it into the markup of the page.</span></span> <span data-ttu-id="4dc8a-296">`@` Karakteri sayfasında ASP.NET Web Pages kodunu işaretler.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-296">The `@` character marks the ASP.NET Web Pages code in the page.</span></span>

<span data-ttu-id="4dc8a-297">(Sayfanın çalıştırılmadan önce WebMatrix değişiklikleri sizin için kaydeder) sayfasını tekrar çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-297">Run the page again (WebMatrix saves the changes for you before it runs the page).</span></span> <span data-ttu-id="4dc8a-298">Bu süre, tarih ve saat sayfasında görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-298">This time you see the date and time in the page.</span></span>

![&quot;Merhaba Dünya&quot; tarayıcı dinamik olarak üretilen zaman görüntü ile çalışan sayfası](getting-started/_static/image20.png)

<span data-ttu-id="4dc8a-300">Birkaç dakika bekleyin ve sonra tarayıcıda sayfayı yenileyin.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-300">Wait a few moments and then refresh the page in the browser.</span></span> <span data-ttu-id="4dc8a-301">Tarih ve saat gösterimi güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-301">The date and time display is updated.</span></span>

<span data-ttu-id="4dc8a-302">Tarayıcıda, sayfa kaynağında arayın.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-302">In the browser, look at the page source.</span></span> <span data-ttu-id="4dc8a-303">Aşağıdaki biçimlendirmede gibi görünüyor:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-303">It looks like the following markup:</span></span>

[!code-html[Main](getting-started/samples/sample5.html)]

<span data-ttu-id="4dc8a-304">Dikkat `@{ }` üst blok değil vardır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-304">Notice that the `@{ }` block at the top isn't there.</span></span> <span data-ttu-id="4dc8a-305">Ayrıca tarih ve saat gösterimi gerçek bir karakter dizesi gösterdiğine dikkat edin (`1/18/2012 2:49:50 PM` veya herhangi) değil `@currentDateTime` vardı gibi *.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-305">Also notice that the date and time display shows an actual string of characters (`1/18/2012 2:49:50 PM` or whatever), not `@currentDateTime` like you had in the *.cshtml* page.</span></span> <span data-ttu-id="4dc8a-306">İşte, sayfa çalıştırdığınızda, ASP.NET ile işaretlenmiş tüm kodu (çok az bu durumda) işlenen ne `@`.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-306">What happened here is that when you ran the page, ASP.NET processed all the code (very little in this case) that was marked with `@`.</span></span> <span data-ttu-id="4dc8a-307">Kod çıkışı üretir ve bu çıkışı sayfasına eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-307">The code produces output, and that output was inserted into the page.</span></span>

## <a name="this-is-what-aspnet-web-pages-are-about"></a><span data-ttu-id="4dc8a-308">ASP.NET Web sayfaları üzeresiniz budur</span><span class="sxs-lookup"><span data-stu-id="4dc8a-308">This Is What ASP.NET Web Pages Are About</span></span>

<span data-ttu-id="4dc8a-309">ASP.NET Web sayfaları dinamik web içeriği üretir okurken ne Burada gördüğünüz olur.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-309">When you read that ASP.NET Web Pages produces dynamic web content, what you've seen here is the idea.</span></span> <span data-ttu-id="4dc8a-310">Yeni oluşturduğunuz sayfa önce gördünüz aynı HTML biçimlendirmesi içerir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-310">The page you just created contains the same HTML markup that you've seen before.</span></span> <span data-ttu-id="4dc8a-311">Ayrıca, her türlü görevleri gerçekleştirebilir kod de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-311">It can also contain code that can perform all sorts of tasks.</span></span> <span data-ttu-id="4dc8a-312">Bu örnekte, geçerli tarih ve saati alma Önemsiz görevini yaptınız.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-312">In this example, it did the trivial task of getting the current date and time.</span></span> <span data-ttu-id="4dc8a-313">Gördüğünüz gibi sayfasında bir çıktı oluşturmak için HTML kod intersperse.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-313">As you saw, you can intersperse code with the HTML to produce output in the page.</span></span> <span data-ttu-id="4dc8a-314">Birisi isteğinde bulunduğunda bir *.cshtml* sayfasını tarayıcıda, ASP.NET web sunucusu elinizde hala durumdayken sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-314">When someone requests a *.cshtml* page in the browser, ASP.NET processes the page while it's still in the hands of the web server.</span></span> <span data-ttu-id="4dc8a-315">ASP.NET kodunun çıkış (varsa) sayfasına HTML olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-315">ASP.NET inserts the output of the code (if any) into the page as HTML.</span></span> <span data-ttu-id="4dc8a-316">Kod işlem tamamlandığında, ASP.NET ortaya çıkacak sayfasında tarayıcıya gönderir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-316">When the code processing is done, ASP.NET sends the resulting page to the browser.</span></span> <span data-ttu-id="4dc8a-317">Tüm tarayıcı hiç alır HTML olabilir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-317">All the browser ever gets is HTML.</span></span> <span data-ttu-id="4dc8a-318">Bir diyagram şöyledir:</span><span class="sxs-lookup"><span data-stu-id="4dc8a-318">Here's a diagram:</span></span>

![Kavramsal akışı nasıl ASP.NET HTML dinamik olarak oluşturur](getting-started/_static/image21.png)

<span data-ttu-id="4dc8a-320">Düşünce basittir ancak kodu gerçekleştirebileceğiniz birçok ilginç görevleri vardır ve hangi dinamik olarak HTML içeriğini sayfaya ekleyebileceğiniz ilginç birçok yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-320">The idea is simple, but there are many interesting tasks that the code can perform, and there are many interesting ways in which you can dynamically add HTML content to the page.</span></span> <span data-ttu-id="4dc8a-321">Ve ASP.NET *.cshtml* herhangi bir HTML sayfası gibi sayfaları da tarayıcının kendi (JavaScript ve jQuery kodu) çalışan bir kod içerir.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-321">And ASP.NET *.cshtml* pages, like any HTML page, can also include code that runs in the browser itself (JavaScript and jQuery code).</span></span> <span data-ttu-id="4dc8a-322">Tüm bunlar Bu öğretici kümesi ve sonraki olanları ele alacağız.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-322">You'll explore all of these things in this tutorial set and in subsequent ones.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="4dc8a-323">Sıradaki gelen</span><span class="sxs-lookup"><span data-stu-id="4dc8a-323">Coming Up Next</span></span>

<span data-ttu-id="4dc8a-324">Bu serideki sonraki öğretici ASP.NET Web sayfaları biraz daha programlama keşfedin.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-324">In the next tutorial in this series, you explore ASP.NET Web Pages programming a little more.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4dc8a-325">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4dc8a-325">Additional Resources</span></span>

<span data-ttu-id="4dc8a-326">[ASP.NET Web sitesi sıfırdan oluşturma](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch).</span><span class="sxs-lookup"><span data-stu-id="4dc8a-326">[Create an ASP.NET website from scratch](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch).</span></span> <span data-ttu-id="4dc8a-327">Bu, özellikle bir öğreticidir WebMatrix (ASP.NET Web sayfaları değil) kullanma hakkında.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-327">This is a tutorial that's specifically about using WebMatrix (not ASP.NET Web Pages).</span></span> <span data-ttu-id="4dc8a-328">Bu biraz bazı ek özelliklerini Bu öğretici kümesinde kapak olmaz WebMatrix hakkında daha fazla ayrıntı girmeyeceğini.</span><span class="sxs-lookup"><span data-stu-id="4dc8a-328">It goes into a little more detail about some of the additional features of WebMatrix that we won't cover in this tutorial set.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="4dc8a-329">Next</span><span class="sxs-lookup"><span data-stu-id="4dc8a-329">Next</span></span>](intro-to-web-pages-programming.md)
