---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: ASP.NET ıdentity'de kullanıcılar için birincil anahtarı değiştirme | Microsoft Docs
author: tfitzmac
description: Visual Studio 2013'te varsayılan web uygulaması için kullanıcı hesapları için anahtar bir dize değeri kullanır. ASP.NET Identity türünü değiştirmenizi sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 20e6b86f50a6ea62f188ae592e0b302c7ef77177
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373334"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="a0b8e-104">ASP.NET ıdentity'de kullanıcılar için birincil anahtarı değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="a0b8e-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a0b8e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a0b8e-106">Visual Studio 2013'te varsayılan web uygulaması için kullanıcı hesapları için anahtar bir dize değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="a0b8e-107">ASP.NET Identity veri gereksinimlerinizi karşılamak için anahtar türünü değiştirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="a0b8e-108">Örneğin, anahtar türünü dizeden tamsayıya değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="a0b8e-109">Bu konuda, varsayılan web uygulaması ve kullanıcı hesap anahtarı değiştirmek için bir tamsayı başlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="a0b8e-110">Aynı değişiklikleri, projenizdeki herhangi bir türde anahtar uygulamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="a0b8e-111">Varsayılan web uygulamasında bu değişiklikleri yapmak nasıl gösterir, ancak benzer değişiklikler özelleştirilmiş bir uygulama için geçerli olabilir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="a0b8e-112">Bu, MVC veya Web Forms ile çalışırken, gereken değişiklikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a0b8e-113">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="a0b8e-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a0b8e-114">Visual Studio 2013 güncelleştirme 2 (veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="a0b8e-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="a0b8e-115">ASP.NET Identity 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a0b8e-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="a0b8e-116">Bu öğreticideki adımları gerçekleştirmek için Visual Studio 2013 güncelleştirme 2 (veya üzeri) ve ASP.NET Web uygulaması şablondan oluşturulan bir web uygulaması olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="a0b8e-117">Güncelleştirme 3'te değiştirilmiş şablonu.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-117">The template changed in Update 3.</span></span> <span data-ttu-id="a0b8e-118">Bu konu, güncelleştirme 2 ve güncelleştirme 3'te şablonu değiştirmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="a0b8e-119">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="a0b8e-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="a0b8e-120">Kimlik kullanıcı sınıfı anahtar türünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="a0b8e-121">Anahtar türünü kullanan özelleştirilmiş kimlik sınıfları Ekle</span><span class="sxs-lookup"><span data-stu-id="a0b8e-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="a0b8e-122">Anahtar türü kullanmak için bağlam sınıfı ve kullanıcı Yöneticisini Değiştir</span><span class="sxs-lookup"><span data-stu-id="a0b8e-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="a0b8e-123">Anahtar türü kullanmak için başlangıç yapılandırmasını değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="a0b8e-124">Güncelleştirme 2 ile MVC için anahtar türü geçirilecek AccountController değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="a0b8e-125">Güncelleştirme 3 ile MVC için değişiklik AccountController ve ManageController anahtar türü geçirmek için</span><span class="sxs-lookup"><span data-stu-id="a0b8e-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="a0b8e-126">Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="a0b8e-127">Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="a0b8e-128">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="a0b8e-128">Run application</span></span>](#run)
- [<span data-ttu-id="a0b8e-129">Diğer kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a0b8e-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="a0b8e-130">Kimlik kullanıcı sınıfı anahtar türünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="a0b8e-131">ASP.NET Web uygulaması şablondan oluşturulan projenizde ApplicationUser sınıfı, kullanıcı hesapları için anahtar tamsayı kullandığını belirtin.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="a0b8e-132">Bir tür olan IdentityUser devralacak şekilde ApplicationUser sınıfı IdentityModels.cs içinde değiştirmek **int** TKey genel parametresi için.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="a0b8e-133">Henüz uygulamaya olmayan üç özelleştirilmiş sınıfı adları ayrıca geçirdiğiniz.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="a0b8e-134">Anahtar türünü değiştirdiniz, ancak varsayılan olarak, uygulamanın rest hala anahtar bir dize olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="a0b8e-135">Ayrıca, anahtar bir dize varsayan kod türünü açıkça belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="a0b8e-136">İçinde **ApplicationUser** sınıfı, değişiklik **GenerateUserIdentityAsync** yöntemine aşağıdaki vurgulanmış kodu gösterildiği gibi int, dahil etmek için.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="a0b8e-137">Bu değişiklik, güncelleştirme 3 şablonu ile Web formları projeleri için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="a0b8e-138">Anahtar türünü kullanan özelleştirilmiş kimlik sınıfları Ekle</span><span class="sxs-lookup"><span data-stu-id="a0b8e-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="a0b8e-139">Diğer kimlik sınıfları IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, yine de bir dize anahtarı kullanacak şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="a0b8e-140">Anahtar için bir tamsayı belirtin. Bu sınıfların yeni sürümlerini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="a0b8e-141">Bu sınıfların kadar uygulama kodunda sağlamak gerekmez, int anahtar olarak yalnızca birincil olarak ayarlama.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="a0b8e-142">Aşağıdaki sınıflar IdentityModels.cs dosyanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="a0b8e-143">Anahtar türü kullanmak için bağlam sınıfı ve kullanıcı Yöneticisini Değiştir</span><span class="sxs-lookup"><span data-stu-id="a0b8e-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="a0b8e-144">IdentityModels.cs içinde tanımını değiştirme **ApplicationDbContext** yeni kullanılacak sınıfı özelleştirilmiş sınıfları ve **int** vurgulanan kodda gösterildiği gibi anahtar.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="a0b8e-145">ThrowIfV1Schema parametresi artık oluşturucuda geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="a0b8e-146">Oluşturucu, ThrowIfV1Schema değerine geçmiyor şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="a0b8e-147">IdentityConfig.cs açın ve değiştirmek **ApplicationUserManger** , yeni bir kullanıcı için sınıf sınıf kalıcı veri depolamak ve **int** anahtarı.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="a0b8e-148">Güncelleştirme 3'ü şablonunda ApplicationSignInManager sınıfı değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="a0b8e-149">Anahtar türü kullanmak için başlangıç yapılandırmasını değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="a0b8e-150">Startup.Auth.cs içinde Onvalidateıdentity kod aşağıda vurgulandığı gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="a0b8e-151">GetUserIdCallback tanımı ayrıştırma dize değeri bir tamsayıya dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="a0b8e-152">Projenizin genel uygulamasını tanımıyor, **getuserıd öğesini** yöntemi, ASP.NET Identity NuGet paketini 2.1 sürümüne güncelleştirmeniz gerekebilir</span><span class="sxs-lookup"><span data-stu-id="a0b8e-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="a0b8e-153">ASP.NET Identity tarafından kullanılan altyapı sınıflar için çok sayıda değişiklik yapıldı.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="a0b8e-154">Projeyi derlemeyi denerseniz, çok sayıda hata fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="a0b8e-155">Neyse ki, kalan hatalar tüm benzerdir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="a0b8e-156">Identity sınıfı anahtarı için bir tamsayı bekliyor, ancak denetleyici (veya Web formu) bir dize değeri geçiyor.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="a0b8e-157">Her durumda, bir dizeye ve tamsayı çağırarak dönüştürmeniz gerekir **getuserıd öğesini&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="a0b8e-158">Derleme hatası listeden çalışmak veya aşağıdaki değişiklikleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="a0b8e-159">Oluşturmakta olduğunuz ve hangi güncelleştirme Visual Studio'da yüklediğiniz projenin türünü kalan değişiklikleri bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="a0b8e-160">Aşağıdaki bağlantılar üzerinden doğrudan ilgili bölümüne Git</span><span class="sxs-lookup"><span data-stu-id="a0b8e-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="a0b8e-161">Güncelleştirme 2 ile MVC için anahtar türü geçirilecek AccountController değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="a0b8e-162">Güncelleştirme 3 ile MVC için değişiklik AccountController ve ManageController anahtar türü geçirmek için</span><span class="sxs-lookup"><span data-stu-id="a0b8e-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="a0b8e-163">Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="a0b8e-164">Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="a0b8e-165">Güncelleştirme 2 ile MVC için anahtar türü geçirilecek AccountController değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="a0b8e-166">AccountController.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="a0b8e-167">Aşağıdaki yöntemlerden değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-167">You need to change the following methods.</span></span>

<span data-ttu-id="a0b8e-168">**ConfirmEmail** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="a0b8e-169">**İlişkisini** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="a0b8e-170">**Manage(ManageUserViewModel)** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="a0b8e-171">**LinkLoginCallback** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="a0b8e-172">**RemoveAccountList** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="a0b8e-173">**HasPassword** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="a0b8e-174">Artık [uygulamayı çalıştırmak](#run) ve yeni bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="a0b8e-175">Güncelleştirme 3 ile MVC için değişiklik AccountController ve ManageController anahtar türü geçirmek için</span><span class="sxs-lookup"><span data-stu-id="a0b8e-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="a0b8e-176">AccountController.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="a0b8e-177">Aşağıdaki yöntemi değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-177">You need to change the following method.</span></span>

<span data-ttu-id="a0b8e-178">**ConfirmEmail** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="a0b8e-179">**SendCode** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="a0b8e-180">ManageController.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="a0b8e-181">Aşağıdaki yöntemlerden değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-181">You need to change the following methods.</span></span>

<span data-ttu-id="a0b8e-182">**Dizin** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="a0b8e-183">**RemoveLogin** yöntemleri</span><span class="sxs-lookup"><span data-stu-id="a0b8e-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="a0b8e-184">**AddPhoneNumber** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="a0b8e-185">**EnableTwoFactorAuthentication** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="a0b8e-186">**DisableTwoFactorAuthentication** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="a0b8e-187">**VerifyPhoneNumber** yöntemleri</span><span class="sxs-lookup"><span data-stu-id="a0b8e-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="a0b8e-188">**RemovePhoneNumber** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="a0b8e-189">**ChangePassword** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="a0b8e-190">**SetPassword** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="a0b8e-191">**ManageLogins** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="a0b8e-192">**LinkLoginCallback** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="a0b8e-193">**HasPassword** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="a0b8e-194">**HasPhoneNumber** yöntemi</span><span class="sxs-lookup"><span data-stu-id="a0b8e-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="a0b8e-195">Artık [uygulamayı çalıştırmak](#run) ve yeni bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="a0b8e-196">Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="a0b8e-197">Güncelleştirme 2 ile Web formları için şu sayfalara değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="a0b8e-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="a0b8e-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="a0b8e-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="a0b8e-201">Artık [uygulamayı çalıştırmak](#run) ve yeni bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="a0b8e-202">Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="a0b8e-203">Güncelleştirme 3 ile Web formları için şu sayfalara değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="a0b8e-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="a0b8e-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="a0b8e-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="a0b8e-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="a0b8e-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="a0b8e-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="a0b8e-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="a0b8e-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a0b8e-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="a0b8e-212">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="a0b8e-212">Run application</span></span>

<span data-ttu-id="a0b8e-213">Varsayılan Web uygulaması şablonu için gerekli tüm değişiklikleri tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="a0b8e-214">Uygulamayı çalıştırın ve yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-214">Run the application and register a new user.</span></span> <span data-ttu-id="a0b8e-215">Kullanıcı kaydedildikten sonra AspNetUsers tablo, bir tamsayı kimlik sütunu olduğunu fark edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![Yeni birincil anahtar](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="a0b8e-217">Daha önce ASP.NET Identity tablolar ile farklı bir birincil anahtar oluşturduysanız, bazı ek değişiklikler yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="a0b8e-218">Mümkünse, varolan bir veritabanını silmeniz yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="a0b8e-219">Web uygulamasını çalıştırın ve yeni kullanıcı ekleme veritabanı doğru tasarım ile yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="a0b8e-220">Silme yapılamıyorsa, code first geçişleri tabloları değiştirmek için çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="a0b8e-221">Ancak, yeni tamsayı birincil anahtar veritabanı SQL kimlik özelliği olarak ayarlanır değil.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="a0b8e-222">Kimlik sütunu bir kimlik olarak el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0b8e-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="a0b8e-223">Diğer kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a0b8e-223">Other resources</span></span>

- [<span data-ttu-id="a0b8e-224">ASP.NET Identity için Özel Depolama Sağlayıcılarına Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="a0b8e-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="a0b8e-225">Mevcut Bir Web Sitesini SQL Üyeliğinden ASP.NET Identity’ye Geçirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="a0b8e-226">Üyelik ve ASP.NET ıdentity'ye kullanıcı profilleri için evrensel sağlayıcı verilerini geçirme</span><span class="sxs-lookup"><span data-stu-id="a0b8e-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="a0b8e-227">[Örnek uygulama](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) değiştirilen birincil anahtara sahip</span><span class="sxs-lookup"><span data-stu-id="a0b8e-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
