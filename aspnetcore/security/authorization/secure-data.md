---
title: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma
author: rick-anderson
description: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile Razor sayfaları uygulaması oluşturmayı öğrenin. HTTPS, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: ba59e8d6243965188397c4ba7a130eec42acfb91
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055886"
---
::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="329a6-104">Bkz: [bu PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC sürümü için.</span><span class="sxs-lookup"><span data-stu-id="329a6-104">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="329a6-105">Bu öğreticide ASP.NET Core 1.1 sürümü [bu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) klasör.</span><span class="sxs-lookup"><span data-stu-id="329a6-105">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="329a6-106">ASP.NET Core örnek konusu 1.1 [örnekleri](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="329a6-106">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="329a6-107">Bkz. [Bu pdf] ()https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="329a6-107">See the [this pdf] (https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="329a6-108">Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="329a6-108">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="329a6-109">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="329a6-109">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="329a6-110">Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="329a6-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="329a6-111">Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="329a6-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="329a6-112">Üç güvenlik grubu vardır:</span><span class="sxs-lookup"><span data-stu-id="329a6-112">There are three security groups:</span></span>

* <span data-ttu-id="329a6-113">**Kayıtlı kullanıcıların** onaylanan tüm verileri görüntüleyebilir ve kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="329a6-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="329a6-114">**Yöneticileri** onaylayabilecek veya reddedebilecek kişi verilerini.</span><span class="sxs-lookup"><span data-stu-id="329a6-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="329a6-115">Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="329a6-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="329a6-116">**Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="329a6-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="329a6-117">Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) açmıştır.</span><span class="sxs-lookup"><span data-stu-id="329a6-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="329a6-118">Rick yalnızca onaylanan kişiler görüntüleyebilir ve **Düzenle**/**Sil**/**Yeni Oluştur** kendi kişiler için bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="329a6-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="329a6-119">Yalnızca en son kaydını görüntüler Rick tarafından oluşturulan **Düzenle** ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="329a6-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="329a6-120">Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.</span><span class="sxs-lookup"><span data-stu-id="329a6-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/rick.png)

<span data-ttu-id="329a6-122">Aşağıdaki görüntüde, `manager@contoso.com` ve yöneticileri rolünde imzalanır:</span><span class="sxs-lookup"><span data-stu-id="329a6-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/manager1.png)

<span data-ttu-id="329a6-124">Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="329a6-124">The following image shows the managers details view of a contact:</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/manager.png)

<span data-ttu-id="329a6-126">**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="329a6-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="329a6-127">Aşağıdaki görüntüde, `admin@contoso.com` ve yöneticiler rolünde imzalanır:</span><span class="sxs-lookup"><span data-stu-id="329a6-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/admin.png)

<span data-ttu-id="329a6-129">Yöneticisinin tüm ayrıcalıklara sahip değil.</span><span class="sxs-lookup"><span data-stu-id="329a6-129">The administrator has all privileges.</span></span> <span data-ttu-id="329a6-130">Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="329a6-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="329a6-131">Uygulama tarafından oluşturulan [yapı iskelesi](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) aşağıdaki `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="329a6-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="329a6-132">Örnek aşağıdaki yetkilendirme işleyicilerini içerir:</span><span class="sxs-lookup"><span data-stu-id="329a6-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="329a6-133">`ContactIsOwnerAuthorizationHandler`: Bir kullanıcı yalnızca kendi verilerini düzenleyebilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="329a6-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="329a6-134">`ContactManagerAuthorizationHandler`: Onaylama veya reddetme kişiler yöneticileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="329a6-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="329a6-135">`ContactAdministratorsAuthorizationHandler`: Yöneticilerin onaylama veya reddetme kişiler ve kişiler düzenleme/silme olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="329a6-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="329a6-136">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="329a6-136">Prerequisites</span></span>

<span data-ttu-id="329a6-137">Bu öğreticide gelişmiştir.</span><span class="sxs-lookup"><span data-stu-id="329a6-137">This tutorial is advanced.</span></span> <span data-ttu-id="329a6-138">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="329a6-138">You should be familiar with:</span></span>

* [<span data-ttu-id="329a6-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="329a6-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="329a6-140">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="329a6-140">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="329a6-141">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="329a6-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="329a6-142">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="329a6-142">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="329a6-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="329a6-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="329a6-144">Bu öğretici için indirme kod ASP.NET Core 2.2 Önizleme 1 veya üzerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="329a6-144">The download code as for this tutorial requires ASP.NET Core 2.2 preview 1 or later.</span></span> <span data-ttu-id="329a6-145">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) geçici için.</span><span class="sxs-lookup"><span data-stu-id="329a6-145">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="329a6-146">Tamamlanmış uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="329a6-146">The starter and completed app</span></span>

<span data-ttu-id="329a6-147">[İndirme](xref:tutorials/index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="329a6-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="329a6-148">[Test](#test-the-completed-app) tamamlanan uygulama güvenlik özellikleri ile ilgili bilgi sahibi olursunuz.</span><span class="sxs-lookup"><span data-stu-id="329a6-148">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="329a6-149">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="329a6-149">The starter app</span></span>

<span data-ttu-id="329a6-150">[İndirme](xref:tutorials/index#how-to-download-a-sample) [başlangıç](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="329a6-150">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="329a6-151">Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="329a6-151">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="329a6-152">Kullanıcı verilerinin güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="329a6-152">Secure user data</span></span>

<span data-ttu-id="329a6-153">Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="329a6-153">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="329a6-154">Tamamlanan projeye başvuran daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="329a6-154">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="329a6-155">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="329a6-155">Tie the contact data to the user</span></span>

<span data-ttu-id="329a6-156">ASP.NET kullanmak [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerine, ancak diğer kullanıcıların verileri düzenleyebilir.</span><span class="sxs-lookup"><span data-stu-id="329a6-156">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="329a6-157">Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="329a6-157">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="329a6-158">`OwnerID` kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="329a6-158">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="329a6-159">`Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="329a6-159">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="329a6-160">Yeni geçiş oluştur ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="329a6-160">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="329a6-161">Kimlik için rol hizmetleri Ekle</span><span class="sxs-lookup"><span data-stu-id="329a6-161">Add Role services to Identity</span></span>

<span data-ttu-id="329a6-162">Append [ndeki](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) rol hizmetlerini eklemek için:</span><span class="sxs-lookup"><span data-stu-id="329a6-162">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="329a6-163">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="329a6-163">Require authenticated users</span></span>

<span data-ttu-id="329a6-164">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="329a6-164">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="329a6-165">Kimlik doğrulaması ile Razor sayfası, denetleyici veya eylem yöntemi düzeyinde dışında iyileştirilmiş `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="329a6-165">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="329a6-166">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur.</span><span class="sxs-lookup"><span data-stu-id="329a6-166">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="329a6-167">Kimlik doğrulaması varsayılan olarak gerekli olan bağlı olan, yeni denetleyicileri ve dahil etmek için Razor sayfaları hakkında daha fazla güvenli `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="329a6-167">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="329a6-168">Ekleme [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) dizine hakkında ve kişi sayfaları böylece bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="329a6-168">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="329a6-169">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="329a6-169">Configure the test account</span></span>

<span data-ttu-id="329a6-170">`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve.</span><span class="sxs-lookup"><span data-stu-id="329a6-170">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="329a6-171">Kullanım [gizli dizi Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="329a6-171">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="329a6-172">Proje dizininden parolayı ayarlayın (dizinini içeren *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="329a6-172">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="329a6-173">Güçlü bir parola belirtilmediği takdirde, bir özel durum `SeedData.Initialize` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="329a6-173">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="329a6-174">Güncelleştirme `Main` test parola kullanmak üzere:</span><span class="sxs-lookup"><span data-stu-id="329a6-174">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="329a6-175">Test hesapları oluşturabilir ve kişilerini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="329a6-175">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="329a6-176">Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturma sınıfı:</span><span class="sxs-lookup"><span data-stu-id="329a6-176">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="329a6-177">Yönetici kullanıcı Kimliğini ekleyin ve `ContactStatus` kişilere.</span><span class="sxs-lookup"><span data-stu-id="329a6-177">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="329a6-178">"Gönderildi" ve "Reddedildi" bir kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="329a6-178">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="329a6-179">Kullanıcı kimliği ve durumu tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="329a6-179">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="329a6-180">Tek bir kişi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="329a6-180">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="329a6-181">Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="329a6-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="329a6-182">Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="329a6-182">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="329a6-183">`ContactIsOwnerAuthorizationHandler` Bir kaynakta çalışan kullanıcı kaynağın sahibi doğrular.</span><span class="sxs-lookup"><span data-stu-id="329a6-183">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="329a6-184">`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) kişi sahibi kimliği doğrulanmış geçerli kullanıcı ise.</span><span class="sxs-lookup"><span data-stu-id="329a6-184">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="329a6-185">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="329a6-185">Authorization handlers generally:</span></span>

* <span data-ttu-id="329a6-186">Dönüş `context.Succeed` gereksinimleri karşılanmadığı zaman.</span><span class="sxs-lookup"><span data-stu-id="329a6-186">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="329a6-187">Dönüş `Task.CompletedTask` zaman olmayan gereksinimleri karşılanıyor.</span><span class="sxs-lookup"><span data-stu-id="329a6-187">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="329a6-188">`Task.CompletedTask` ne success veya FAILURE olur&mdash;çalıştırmak, diğer yetkilendirme işleyicileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="329a6-188">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="329a6-189">Açıkça başarısız gerekiyorsa, iade [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="329a6-189">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="329a6-190">Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir.</span><span class="sxs-lookup"><span data-stu-id="329a6-190">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="329a6-191">`ContactIsOwnerAuthorizationHandler` gereksinim parametrede aktarılan işlemi denetleyin. gerekmez.</span><span class="sxs-lookup"><span data-stu-id="329a6-191">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="329a6-192">Bir yönetici yetkilendirme işleyici oluşturun</span><span class="sxs-lookup"><span data-stu-id="329a6-192">Create a manager authorization handler</span></span>

<span data-ttu-id="329a6-193">Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="329a6-193">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="329a6-194">`ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, bir yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="329a6-194">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="329a6-195">Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="329a6-195">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="329a6-196">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="329a6-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="329a6-197">Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="329a6-197">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="329a6-198">`ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="329a6-198">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="329a6-199">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="329a6-199">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="329a6-200">Yetkilendirme işleyicilerini kaydedin</span><span class="sxs-lookup"><span data-stu-id="329a6-200">Register the authorization handlers</span></span>

<span data-ttu-id="329a6-201">Entity Framework Core kullanan hizmetler için kaydedilmelidir [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="329a6-201">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="329a6-202">`ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="329a6-202">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="329a6-203">Kullanılabilir bulunmaları işleyicilerini hizmet koleksiyonuyla kaydedin `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="329a6-203">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="329a6-204">Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="329a6-204">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="329a6-205">`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` teklileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="329a6-205">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="329a6-206">Oldukları teklileri çünkü bunlar EF kullanmayın ve gereken tüm bilgiler `Context` parametresinin `HandleRequirementAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="329a6-206">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="329a6-207">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="329a6-207">Support authorization</span></span>

<span data-ttu-id="329a6-208">Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="329a6-208">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="329a6-209">Gözden geçirme kişi işlem gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="329a6-209">Review the contact operations requirements class</span></span>

<span data-ttu-id="329a6-210">Gözden geçirme `ContactOperations` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="329a6-210">Review the `ContactOperations` class.</span></span> <span data-ttu-id="329a6-211">Bu sınıf gereksinimleri içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="329a6-211">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="329a6-212">Kişiler Razor sayfaları için bir temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="329a6-212">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="329a6-213">Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="329a6-213">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="329a6-214">Temel sınıf başlatma kodunu tek bir yerde getirir:</span><span class="sxs-lookup"><span data-stu-id="329a6-214">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="329a6-215">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="329a6-215">The preceding code:</span></span>

* <span data-ttu-id="329a6-216">Ekler `IAuthorizationService` yetkilendirme işleyicilere erişmek için hizmet.</span><span class="sxs-lookup"><span data-stu-id="329a6-216">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="329a6-217">Kimlik ekler `UserManager` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="329a6-217">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="329a6-218">Ekleme `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="329a6-218">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="329a6-219">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="329a6-219">Update the CreateModel</span></span>

<span data-ttu-id="329a6-220">Kullanılacak Oluştur sayfası model Oluşturucu güncelleştirme `DI_BasePageModel` temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="329a6-220">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="329a6-221">Güncelleştirme `CreateModel.OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="329a6-221">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="329a6-222">Kullanıcı Kimliğinize ekleme `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="329a6-222">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="329a6-223">Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="329a6-223">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="329a6-224">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="329a6-224">Update the IndexModel</span></span>

<span data-ttu-id="329a6-225">Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilecek şekilde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="329a6-225">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="329a6-226">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="329a6-226">Update the EditModel</span></span>

<span data-ttu-id="329a6-227">Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="329a6-227">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="329a6-228">Kaynak Yetkilendirme Doğrulanmakta olan çünkü `[Authorize]` özniteliği yeterli değil.</span><span class="sxs-lookup"><span data-stu-id="329a6-228">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="329a6-229">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="329a6-229">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="329a6-230">Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="329a6-230">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="329a6-231">Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="329a6-231">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="329a6-232">Sık sık geçirerek kaynak anahtarı kaynağa erişir.</span><span class="sxs-lookup"><span data-stu-id="329a6-232">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="329a6-233">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="329a6-233">Update the DeleteModel</span></span>

<span data-ttu-id="329a6-234">Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="329a6-234">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="329a6-235">Kimlik doğrulama servisi görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="329a6-235">Inject the authorization service into the views</span></span>

<span data-ttu-id="329a6-236">Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="329a6-236">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="329a6-237">Yetkilendirme hizmetinde ekleme *Views/_ViewImports.cshtml* tüm görünümlere kullanılabilir olacak şekilde dosya:</span><span class="sxs-lookup"><span data-stu-id="329a6-237">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="329a6-238">Birkaç önceki biçimlendirme ekler `using` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="329a6-238">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="329a6-239">Güncelleştirme **Düzenle** ve **Sil** bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar tarafından işlenen için:</span><span class="sxs-lookup"><span data-stu-id="329a6-239">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="329a6-240">Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="329a6-240">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="329a6-241">Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar.</span><span class="sxs-lookup"><span data-stu-id="329a6-241">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="329a6-242">Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack.</span><span class="sxs-lookup"><span data-stu-id="329a6-242">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="329a6-243">Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="329a6-243">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="329a6-244">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="329a6-244">Update Details</span></span>

<span data-ttu-id="329a6-245">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="329a6-245">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="329a6-246">Güncelleştirme ayrıntıları sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="329a6-246">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="329a6-247">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="329a6-247">Test the completed app</span></span>

<span data-ttu-id="329a6-248">Uygulamanın, kişiler varsa:</span><span class="sxs-lookup"><span data-stu-id="329a6-248">If the app has contacts:</span></span>

* <span data-ttu-id="329a6-249">Tüm kayıtları silme `Contact` tablo.</span><span class="sxs-lookup"><span data-stu-id="329a6-249">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="329a6-250">Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="329a6-250">Restart the app to seed the database.</span></span>

<span data-ttu-id="329a6-251">Bir kullanıcı kişiler göz atmak için kaydedin.</span><span class="sxs-lookup"><span data-stu-id="329a6-251">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="329a6-252">Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate sürümleri) başlatılmasıdır.</span><span class="sxs-lookup"><span data-stu-id="329a6-252">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="329a6-253">Yeni bir kullanıcı bir tarayıcıda kaydedin (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="329a6-253">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="329a6-254">Her bir tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="329a6-254">Sign in to each browser with a different user.</span></span> <span data-ttu-id="329a6-255">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="329a6-255">Verify the following operations:</span></span>

* <span data-ttu-id="329a6-256">Kayıtlı kullanıcılar, tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="329a6-256">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="329a6-257">Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="329a6-257">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="329a6-258">Yöneticileri onaylayabilir veya kişi verilerinizi reddedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="329a6-258">Managers can approve or reject contact data.</span></span> <span data-ttu-id="329a6-259">`Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="329a6-259">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="329a6-260">Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="329a6-260">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="329a6-261">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="329a6-261">User</span></span>| <span data-ttu-id="329a6-262">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="329a6-262">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="329a6-263">Kendi veri düzenleme/silebilir</span><span class="sxs-lookup"><span data-stu-id="329a6-263">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="329a6-264">Onayla/Reddet ve düzenleme/silme, veri sahip olabilir</span><span class="sxs-lookup"><span data-stu-id="329a6-264">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="329a6-265">Düzenleme/silme ve tüm verileri Onayla/Reddet kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="329a6-265">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="329a6-266">Bir kişi, yöneticinin tarayıcıda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="329a6-266">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="329a6-267">Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="329a6-267">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="329a6-268">Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="329a6-268">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="329a6-269">Başlangıç uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="329a6-269">Create the starter app</span></span>

* <span data-ttu-id="329a6-270">"ContactManager" adlı bir Razor sayfaları uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="329a6-270">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="329a6-271">Uygulamayı oluşturma **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="329a6-271">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="329a6-272">Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.</span><span class="sxs-lookup"><span data-stu-id="329a6-272">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="329a6-273">`-uld` LocalDB yerine SQLite belirtir</span><span class="sxs-lookup"><span data-stu-id="329a6-273">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="329a6-274">Ekleme *Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="329a6-274">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="329a6-275">İskele `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="329a6-275">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="329a6-276">İlk geçiş oluşturun ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="329a6-276">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="329a6-277">Güncelleştirme **ContactManager** içinde yer işareti *Pages/_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="329a6-277">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="329a6-278">Oluşturma, düzenleme ve silme kişi uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="329a6-278">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="329a6-279">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="329a6-279">Seed the database</span></span>

<span data-ttu-id="329a6-280">Ekleme [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) sınıfının *veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="329a6-280">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="329a6-281">Çağrı `SeedData.Initialize` gelen `Main`:</span><span class="sxs-lookup"><span data-stu-id="329a6-281">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="329a6-282">Test, uygulama veritabanını sağlanmış.</span><span class="sxs-lookup"><span data-stu-id="329a6-282">Test that the app seeded the database.</span></span> <span data-ttu-id="329a6-283">DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="329a6-283">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="329a6-284">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="329a6-284">Additional resources</span></span>

* <span data-ttu-id="329a6-285">[ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="329a6-285">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="329a6-286">Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="329a6-286">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="329a6-287">ASP.NET Core yetkilendirme: basit, rol, talep tabanlı ve özel</span><span class="sxs-lookup"><span data-stu-id="329a6-287">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="329a6-288">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="329a6-288">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end