---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: "7. Kısım: Üyeliği ve yetkilendirme | Microsoft Docs"
author: jongalloway
description: "Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 7, üyelik ve yetkilendirme kapsar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: db459de687db862be00a9b59ff5b1b238fa75061
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="337a2-104">7. Kısım: Üyeliği ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="337a2-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="337a2-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="337a2-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="337a2-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="337a2-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="337a2-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="337a2-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="337a2-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="337a2-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="337a2-109">Bölüm 7, üyelik ve yetkilendirme kapsar.</span><span class="sxs-lookup"><span data-stu-id="337a2-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="337a2-110">Bizim mağaza yöneticisi denetleyicisi sitemizi ziyaret eden herkes için şu anda erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="337a2-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="337a2-111">Bu site yöneticileri erişimi kısıtlamak için değiştirelim.</span><span class="sxs-lookup"><span data-stu-id="337a2-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="337a2-112">AccountController ve görünümler ekleme</span><span class="sxs-lookup"><span data-stu-id="337a2-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="337a2-113">Tam ASP.NET MVC 3 Web uygulaması şablonu ve ASP.NET MVC 3 boş Web uygulaması şablonu arasındaki tek fark, boş şablonu bir hesap denetleyicisi içermeyen ' dir.</span><span class="sxs-lookup"><span data-stu-id="337a2-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="337a2-114">Bir hesap denetleyicisi tam ASP.NET MVC 3 Web uygulaması şablondan oluşturulan yeni bir ASP.NET MVC uygulamasında birkaç dosyaları kopyalayarak ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="337a2-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="337a2-115">Tam ASP.NET MVC 3 Web uygulaması şablonu kullanarak yeni bir ASP.NET MVC uygulaması oluşturma ve Projemizin aynı dizinlerde aşağıdaki dosyaları kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="337a2-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="337a2-116">Denetleyicileri dizininde AccountController.cs kopyalama</span><span class="sxs-lookup"><span data-stu-id="337a2-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="337a2-117">Modelleri dizininde AccountModels kopyalama</span><span class="sxs-lookup"><span data-stu-id="337a2-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="337a2-118">Görünümleri dizin içinde bir hesap dizini oluşturun ve tüm dört görünümlerde kopyalayın</span><span class="sxs-lookup"><span data-stu-id="337a2-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="337a2-119">Denetleyici ve Model sınıfları için ad alanı ile MvcMusicStore başlayacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="337a2-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="337a2-120">AccountController sınıfı MvcMusicStore.Controllers ad alanı kullanmalıdır ve AccountModels sınıfı MvcMusicStore.Models ad alanı kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="337a2-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="337a2-121">*Not: Bu dosyalar, biz öğreticinin başında bizim site tasarımı dosyaları kopyalanan MvcMusicStore Assets.zip indirme mevcuttur. Üyelik dosyalar kodu dizininde bulunur.*</span><span class="sxs-lookup"><span data-stu-id="337a2-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="337a2-122">Güncellenen çözümü aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="337a2-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="337a2-123">ASP.NET yapılandırma sitesi olan bir yönetici kullanıcı ekleme</span><span class="sxs-lookup"><span data-stu-id="337a2-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="337a2-124">Biz yetkilendirme, Web sitesinin gerektirmeden önce biz erişimi olan bir kullanıcı oluşturma gerekir.</span><span class="sxs-lookup"><span data-stu-id="337a2-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="337a2-125">Bir kullanıcı oluşturmak için en kolay yolu, yerleşik ASP.NET yapılandırması Web sitesini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="337a2-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="337a2-126">ASP.NET yapılandırma Web sitesi, Çözüm Gezgini'nde simgesini izleyen tıklatarak başlatın.</span><span class="sxs-lookup"><span data-stu-id="337a2-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="337a2-127">Bu yapılandırma Web sitesini başlatır.</span><span class="sxs-lookup"><span data-stu-id="337a2-127">This launches a configuration website.</span></span> <span data-ttu-id="337a2-128">Giriş ekranı Güvenlik sekmesinde, ardından ekranın Center'da "rolleri etkinleştir" bağlantısını tıklatın.</span><span class="sxs-lookup"><span data-stu-id="337a2-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="337a2-129">"Rol oluşturma ve yönetme" bağlantısını tıklatın.</span><span class="sxs-lookup"><span data-stu-id="337a2-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="337a2-130">"Yönetici" rolü adı girin ve Rol Ekle düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="337a2-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="337a2-131">Geri düğmesini tıklatın, ardından sol tarafındaki oluşturma kullanıcı bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="337a2-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="337a2-132">Aşağıdaki bilgileri kullanarak soldaki kullanıcı bilgi alanları doldurun:</span><span class="sxs-lookup"><span data-stu-id="337a2-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="337a2-133">**Alan**</span><span class="sxs-lookup"><span data-stu-id="337a2-133">**Field**</span></span> | <span data-ttu-id="337a2-134">**Değer**</span><span class="sxs-lookup"><span data-stu-id="337a2-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="337a2-135">**Kullanıcı adı**</span><span class="sxs-lookup"><span data-stu-id="337a2-135">**User Name**</span></span> | <span data-ttu-id="337a2-136">Yönetici</span><span class="sxs-lookup"><span data-stu-id="337a2-136">Administrator</span></span> |
| <span data-ttu-id="337a2-137">**Parola**</span><span class="sxs-lookup"><span data-stu-id="337a2-137">**Password**</span></span> | <span data-ttu-id="337a2-138">password123!</span><span class="sxs-lookup"><span data-stu-id="337a2-138">password123!</span></span> |
| <span data-ttu-id="337a2-139">**Parolayı onaylayın**</span><span class="sxs-lookup"><span data-stu-id="337a2-139">**Confirm Password**</span></span> | <span data-ttu-id="337a2-140">password123!</span><span class="sxs-lookup"><span data-stu-id="337a2-140">password123!</span></span> |
| <span data-ttu-id="337a2-141">**E-posta**</span><span class="sxs-lookup"><span data-stu-id="337a2-141">**E-mail**</span></span> | <span data-ttu-id="337a2-142">(herhangi bir e-posta adresi çalışır)</span><span class="sxs-lookup"><span data-stu-id="337a2-142">(any email address will work)</span></span> |
| <span data-ttu-id="337a2-143">**Güvenlik sorusu**</span><span class="sxs-lookup"><span data-stu-id="337a2-143">**Security Question**</span></span> | <span data-ttu-id="337a2-144">(ne olursa olsun, ister)</span><span class="sxs-lookup"><span data-stu-id="337a2-144">(whatever you like)</span></span> |
| <span data-ttu-id="337a2-145">**Güvenlik yanıtı**</span><span class="sxs-lookup"><span data-stu-id="337a2-145">**Security Answer**</span></span> | <span data-ttu-id="337a2-146">(ne olursa olsun, ister)</span><span class="sxs-lookup"><span data-stu-id="337a2-146">(whatever you like)</span></span> |

<span data-ttu-id="337a2-147">*Not: İstediğiniz herhangi bir parola Elbette kullanabilirsiniz. Varsayılan parola güvenlik ayarları 7 karakterden uzun olduğundan ve bir tane alfasayısal olmayan karakter içeren bir parola gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="337a2-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="337a2-148">Bu kullanıcı için yönetici rolünü seçin ve kullanıcı Oluştur düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="337a2-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="337a2-149">Bu noktada, kullanıcının başarıyla oluşturulduğunu belirten bir ileti görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="337a2-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="337a2-150">Bir tarayıcı penceresi artık kapatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="337a2-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="337a2-151">Rol tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="337a2-151">Role-based Authorization</span></span>

<span data-ttu-id="337a2-152">Şimdi biz kullanıcı sınıfı içinde herhangi bir denetleyici işlem erişmek için Yönetici rolü olmalıdır belirterek [Authorize] özniteliğini kullanarak StoreManagerController erişimi kısıtlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="337a2-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="337a2-153">*Not: [Authorize] özniteliği ve denetleyici sınıf düzeyinde aynı zamanda belirli eylem yöntemleri yerleştirilebilir.*</span><span class="sxs-lookup"><span data-stu-id="337a2-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="337a2-154">Şimdi /StoreManager için gözatma bir oturum açma iletişim kutusunu açar:</span><span class="sxs-lookup"><span data-stu-id="337a2-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="337a2-155">Bizim yeni yönetici hesabı kullanarak oturum açtıktan sonra biz önce albümü düzenle ekran çalışmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="337a2-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="337a2-156">[Önceki](mvc-music-store-part-6.md)
[sonraki](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="337a2-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
