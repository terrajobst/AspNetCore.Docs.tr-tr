---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: "ASP.NET kimliği kullanıcılar için birincil anahtar değiştirme | Microsoft Docs"
author: tfitzmac
description: "Visual Studio 2013'te varsayılan web uygulaması için kullanıcı hesapları anahtarının bir dize değeri kullanır. ASP.NET Identity türünü değiştirmenizi sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="f8b1d-104">ASP.NET kimliği kullanıcılar için birincil anahtarı Değiştir</span><span class="sxs-lookup"><span data-stu-id="f8b1d-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="f8b1d-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f8b1d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f8b1d-106">Visual Studio 2013'te varsayılan web uygulaması için kullanıcı hesapları anahtarının bir dize değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="f8b1d-107">ASP.NET Identity veri gereksinimlerinizi karşılamak için anahtar türü değiştirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="f8b1d-108">Örneğin, bir tamsayıya bir dizeden anahtarın türünü değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="f8b1d-109">Bu konu, başlama varsayılan web uygulaması ve kullanıcı hesap anahtarı tamsayıya değiştirmek gösterir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="f8b1d-110">Projenizde anahtar herhangi bir türde uygulamak için aynı değişikliği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="f8b1d-111">Varsayılan web uygulamasında bu değişiklikleri yapmak nasıl gösterir, ancak özelleştirilmiş bir uygulamaya benzer değişiklikler uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="f8b1d-112">MVC veya Web Forms ile çalışırken gerekli değişiklikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f8b1d-113">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="f8b1d-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f8b1d-114">Visual Studio 2013 güncelleştirme 2 (veya üstü)</span><span class="sxs-lookup"><span data-stu-id="f8b1d-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="f8b1d-115">ASP.NET Identity 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="f8b1d-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="f8b1d-116">Bu öğreticide adımları gerçekleştirmek için Visual Studio 2013 güncelleştirme 2 (veya üstü) ve ASP.NET Web uygulaması şablondan oluşturulan bir web uygulaması olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="f8b1d-117">Güncelleştirme 3'te değiştirilen şablonu.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-117">The template changed in Update 3.</span></span> <span data-ttu-id="f8b1d-118">Bu konu, güncelleştirme 2 ve güncelleştirme 3'te şablonu değiştirmek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="f8b1d-119">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="f8b1d-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f8b1d-120">Kimlik kullanıcı sınıfı anahtar türünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="f8b1d-121">Anahtar türü kullanmak özelleştirilmiş kimlik sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="f8b1d-122">Anahtar türü kullanılacak bağlam sınıfı ve kullanıcı yöneticisini değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="f8b1d-123">Anahtar türü kullanmak için başlangıç yapılandırmasını değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="f8b1d-124">Güncelleştirme 2 ile MVC için AccountController anahtar türü geçirilecek değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="f8b1d-125">Güncelleştirme 3 ile MVC için AccountController ve ManageController anahtar türü geçirilecek değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="f8b1d-126">Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="f8b1d-127">Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="f8b1d-128">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="f8b1d-128">Run application</span></span>](#run)
- [<span data-ttu-id="f8b1d-129">Diğer kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f8b1d-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="f8b1d-130">Kimlik kullanıcı sınıfı anahtar türünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="f8b1d-131">ASP.NET Web uygulaması şablondan oluşturulan projenizde ApplicationUser sınıfı, kullanıcı hesapları için anahtar tamsayı kullandığını belirtin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="f8b1d-132">Bir türü IdentityUser devralacak şekilde ApplicationUser sınıfı IdentityModels.cs içinde değiştirmek **int** TKey genel parametresi için.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="f8b1d-133">Henüz uygulamaya olmayan üç özelleştirilmiş sınıf adlarını ayrıca geçirdiğiniz.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="f8b1d-134">Anahtar türü değiştirildi, ancak varsayılan olarak, uygulamanın geri kalanına hala bir dize anahtar olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="f8b1d-135">Açıkça bir dize varsayar kod anahtarında türünü belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="f8b1d-136">İçinde **ApplicationUser** sınıfı, değişiklik **GenerateUserIdentityAsync** yöntemi int, aşağıda vurgulanan kodda gösterildiği gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="f8b1d-137">Bu değişiklik güncelleştirme 3'ü şablonuyla Web Forms projeler için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="f8b1d-138">Anahtar türü kullanmak özelleştirilmiş kimlik sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="f8b1d-139">Diğer kimlik sınıfları, IdentityUserRole, gibi IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore RoleStore, hala bir dize anahtarı kullanacak şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="f8b1d-140">Anahtar için bir tamsayı belirtin Bu sınıfların yeni sürümlerini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="f8b1d-141">Bu sınıfların kadar uygulama kodunda sağlamak zorunda değildir, int anahtar olarak birincil olarak yalnızca ayarlama.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="f8b1d-142">Aşağıdaki sınıflar IdentityModels.cs dosyanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="f8b1d-143">Anahtar türü kullanılacak bağlam sınıfı ve kullanıcı yöneticisini değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="f8b1d-144">IdentityModels.cs içinde tanımını değiştirin **ApplicationDbContext** yeni kullanılacak sınıfı özelleştirilmiş sınıfları ve bir **int** vurgulanan kodda gösterildiği gibi anahtar.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="f8b1d-145">ThrowIfV1Schema parametresi artık oluşturucuda geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="f8b1d-146">ThrowIfV1Schema değeri iletmez şekilde Oluşturucusu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="f8b1d-147">IdentityConfig.cs açın ve değiştirme **ApplicationUserManger** , yeni bir kullanıcı için sınıf sınıfı için kalıcı veri depolamak ve bir **int** anahtar.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="f8b1d-148">Güncelleştirme 3'ü şablonunda ApplicationSignInManager sınıfı değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="f8b1d-149">Anahtar türü kullanmak için başlangıç yapılandırmasını değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="f8b1d-150">Startup.Auth.cs içinde Onvalidateıdentity kod aşağıda vurgulandığı gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="f8b1d-151">GetUserIdCallback tanımı bir tamsayı, dize değeri ayrıştırır dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="f8b1d-152">Projenizi genel uygulamasını tanımıyor varsa **getuserıd öğesini** yöntemi, ASP.NET Identity NuGet paketini sürüm 2.1 güncelleştirmeniz gerekebilir</span><span class="sxs-lookup"><span data-stu-id="f8b1d-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="f8b1d-153">ASP.NET kimliği tarafından kullanılan altyapı sınıflar için çok fazla değişiklik yaptınız.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="f8b1d-154">Proje derleme çalışırsanız, çok sayıda hata fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="f8b1d-155">Neyse ki, kalan tüm benzer hatalardır.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="f8b1d-156">Identity sınıfı anahtar için bir tamsayı bekliyor, ancak bir dize değeri denetleyici (veya Web formu) geçirme.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="f8b1d-157">Her durumda, bir dizeye ve tamsayı çağırarak dönüştürmeniz gerekir. **getuserıd öğesini&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="f8b1d-158">Derleme hata listesinden aracılığıyla iş veya aşağıdaki değişiklikleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="f8b1d-159">Kalan değişikliklerini oluşturmakta olduğunuz ve Visual Studio yüklü hangi güncelleştirme proje türüne göre değişir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="f8b1d-160">Aşağıdaki bağlantılar aracılığıyla doğrudan ilgili bölümüne gidin</span><span class="sxs-lookup"><span data-stu-id="f8b1d-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="f8b1d-161">Güncelleştirme 2 ile MVC için AccountController anahtar türü geçirilecek değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="f8b1d-162">Güncelleştirme 3 ile MVC için AccountController ve ManageController anahtar türü geçirilecek değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="f8b1d-163">Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="f8b1d-164">Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="f8b1d-165">Güncelleştirme 2 ile MVC için AccountController anahtar türü geçirilecek değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="f8b1d-166">AccountController.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="f8b1d-167">Aşağıdaki yöntemlerden değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-167">You need to change the following methods.</span></span>

<span data-ttu-id="f8b1d-168">**ConfirmEmail** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="f8b1d-169">**İlişkisini** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="f8b1d-170">**Manage(ManageUserViewModel)** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="f8b1d-171">**LinkLoginCallback** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="f8b1d-172">**RemoveAccountList** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="f8b1d-173">**HasPassword** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="f8b1d-174">Artık [uygulamayı çalıştırın](#run) ve yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="f8b1d-175">Güncelleştirme 3 ile MVC için AccountController ve ManageController anahtar türü geçirilecek değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="f8b1d-176">AccountController.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="f8b1d-177">Aşağıdaki yöntem değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-177">You need to change the following method.</span></span>

<span data-ttu-id="f8b1d-178">**ConfirmEmail** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="f8b1d-179">**SendCode** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="f8b1d-180">ManageController.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="f8b1d-181">Aşağıdaki yöntemlerden değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-181">You need to change the following methods.</span></span>

<span data-ttu-id="f8b1d-182">**Dizin** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="f8b1d-183">**RemoveLogin** yöntemleri</span><span class="sxs-lookup"><span data-stu-id="f8b1d-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="f8b1d-184">**AddPhoneNumber** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="f8b1d-185">**EnableTwoFactorAuthentication** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="f8b1d-186">**DisableTwoFactorAuthentication** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="f8b1d-187">**VerifyPhoneNumber** yöntemleri</span><span class="sxs-lookup"><span data-stu-id="f8b1d-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="f8b1d-188">**RemovePhoneNumber** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="f8b1d-189">**Parola değiştirme** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="f8b1d-190">**SetPassword** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="f8b1d-191">**ManageLogins** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="f8b1d-192">**LinkLoginCallback** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="f8b1d-193">**HasPassword** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="f8b1d-194">**HasPhoneNumber** yöntemi</span><span class="sxs-lookup"><span data-stu-id="f8b1d-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="f8b1d-195">Artık [uygulamayı çalıştırın](#run) ve yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="f8b1d-196">Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="f8b1d-197">Güncelleştirme 2 ile Web formları için şu sayfaları değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="f8b1d-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="f8b1d-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="f8b1d-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="f8b1d-201">Artık [uygulamayı çalıştırın](#run) ve yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="f8b1d-202">Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="f8b1d-203">Güncelleştirme 3 ile Web formları için şu sayfaları değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="f8b1d-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="f8b1d-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="f8b1d-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="f8b1d-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="f8b1d-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="f8b1d-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="f8b1d-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="f8b1d-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f8b1d-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="f8b1d-212">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="f8b1d-212">Run application</span></span>

<span data-ttu-id="f8b1d-213">Varsayılan Web uygulaması şablonu için gerekli tüm değişiklikleri tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="f8b1d-214">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-214">Run the application and register a new user.</span></span> <span data-ttu-id="f8b1d-215">Kullanıcı kaydolduktan sonra AspNetUsers tablo bir tamsayıdır bir kimlik sütunu olduğunu fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![Yeni birincil anahtar](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="f8b1d-217">Daha önce ASP.NET Identity tablolar ile farklı bir birincil anahtar oluşturduysanız, bazı ek değişiklikler yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="f8b1d-218">Mümkünse, varolan bir veritabanını yalnızca silin.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="f8b1d-219">Web uygulaması çalıştırdığınızda ve yeni bir kullanıcı hesabı veritabanı doğru tasarım ile yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="f8b1d-220">Tabloları değiştirmek için code first geçişleri çalıştırmak silme mümkün değilse.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="f8b1d-221">Ancak, yeni tamsayı birincil anahtar veritabanı SQL kimlik özelliği olarak ayarlanmaması.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="f8b1d-222">Bir kimlik olarak ID sütunu el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b1d-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="f8b1d-223">Diğer kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f8b1d-223">Other resources</span></span>

- [<span data-ttu-id="f8b1d-224">ASP.NET kimliği için özel depolama sağlayıcıları genel bakış</span><span class="sxs-lookup"><span data-stu-id="f8b1d-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="f8b1d-225">Mevcut bir Web SQL üyeliğinden ASP.NET Identity geçirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="f8b1d-226">Üyelik ve ASP.NET kimliği için kullanıcı profilleri için evrensel sağlayıcısı verileri geçirme</span><span class="sxs-lookup"><span data-stu-id="f8b1d-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="f8b1d-227">[Örnek uygulama](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) değiştirilen birincil anahtara sahip</span><span class="sxs-lookup"><span data-stu-id="f8b1d-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
