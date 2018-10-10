---
title: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma
author: rick-anderson
description: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile Razor sayfaları uygulaması oluşturmayı öğrenin. HTTPS, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 71b7855958b530b8bac32843a8d1e7db0113ffd9
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912637"
---
::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="e9197-104">Bkz: [bu PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC sürümü için.</span><span class="sxs-lookup"><span data-stu-id="e9197-104">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="e9197-105">Bu öğreticide ASP.NET Core 1.1 sürümü [bu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) klasör.</span><span class="sxs-lookup"><span data-stu-id="e9197-105">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="e9197-106">ASP.NET Core örnek konusu 1.1 [örnekleri](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="e9197-106">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e9197-107">Bkz: [bu pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="e9197-107">See [this pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="e9197-108">Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e9197-108">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="e9197-109">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="e9197-109">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="e9197-110">Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e9197-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="e9197-111">Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="e9197-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="e9197-112">Üç güvenlik grubu vardır:</span><span class="sxs-lookup"><span data-stu-id="e9197-112">There are three security groups:</span></span>

* <span data-ttu-id="e9197-113">**Kayıtlı kullanıcıların** onaylanan tüm verileri görüntüleyebilir ve kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="e9197-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="e9197-114">**Yöneticileri** onaylayabilecek veya reddedebilecek kişi verilerini.</span><span class="sxs-lookup"><span data-stu-id="e9197-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="e9197-115">Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="e9197-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="e9197-116">**Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9197-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="e9197-117">Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) açmıştır.</span><span class="sxs-lookup"><span data-stu-id="e9197-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="e9197-118">Rick yalnızca onaylanan kişiler görüntüleyebilir ve **Düzenle**/**Sil**/**Yeni Oluştur** kendi kişiler için bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="e9197-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="e9197-119">Yalnızca en son kaydını görüntüler Rick tarafından oluşturulan **Düzenle** ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="e9197-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="e9197-120">Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.</span><span class="sxs-lookup"><span data-stu-id="e9197-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/rick.png)

<span data-ttu-id="e9197-122">Aşağıdaki görüntüde, `manager@contoso.com` ve yöneticileri rolünde imzalanır:</span><span class="sxs-lookup"><span data-stu-id="e9197-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/manager1.png)

<span data-ttu-id="e9197-124">Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="e9197-124">The following image shows the managers details view of a contact:</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/manager.png)

<span data-ttu-id="e9197-126">**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e9197-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="e9197-127">Aşağıdaki görüntüde, `admin@contoso.com` ve yöneticiler rolünde imzalanır:</span><span class="sxs-lookup"><span data-stu-id="e9197-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/admin.png)

<span data-ttu-id="e9197-129">Yöneticisinin tüm ayrıcalıklara sahip değil.</span><span class="sxs-lookup"><span data-stu-id="e9197-129">The administrator has all privileges.</span></span> <span data-ttu-id="e9197-130">Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e9197-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="e9197-131">Uygulama tarafından oluşturulan [yapı iskelesi](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) aşağıdaki `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="e9197-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="e9197-132">Örnek aşağıdaki yetkilendirme işleyicilerini içerir:</span><span class="sxs-lookup"><span data-stu-id="e9197-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="e9197-133">`ContactIsOwnerAuthorizationHandler`: Bir kullanıcı yalnızca kendi verilerini düzenleyebilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="e9197-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="e9197-134">`ContactManagerAuthorizationHandler`: Onaylama veya reddetme kişiler yöneticileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="e9197-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="e9197-135">`ContactAdministratorsAuthorizationHandler`: Yöneticilerin onaylama veya reddetme kişiler ve kişiler düzenleme/silme olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e9197-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9197-136">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e9197-136">Prerequisites</span></span>

<span data-ttu-id="e9197-137">Bu öğreticide gelişmiştir.</span><span class="sxs-lookup"><span data-stu-id="e9197-137">This tutorial is advanced.</span></span> <span data-ttu-id="e9197-138">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="e9197-138">You should be familiar with:</span></span>

* [<span data-ttu-id="e9197-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9197-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="e9197-140">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e9197-140">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="e9197-141">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="e9197-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="e9197-142">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="e9197-142">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="e9197-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e9197-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="e9197-144">ASP.NET Core 2.1 içinde `User.IsInRole` kullanırken başarısız `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="e9197-144">In ASP.NET Core 2.1, `User.IsInRole` fails when using `AddDefaultIdentity`.</span></span> <span data-ttu-id="e9197-145">Bu öğreticide `AddDefaultIdentity` ve bu nedenle ASP.NET Core 2.2 Önizleme 1 veya üzerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e9197-145">This tutorial uses `AddDefaultIdentity` and therefore requires ASP.NET Core 2.2 preview 1 or later.</span></span> <span data-ttu-id="e9197-146">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) geçici için.</span><span class="sxs-lookup"><span data-stu-id="e9197-146">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="e9197-147">Tamamlanmış uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="e9197-147">The starter and completed app</span></span>

<span data-ttu-id="e9197-148">[İndirme](xref:tutorials/index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="e9197-148">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="e9197-149">[Test](#test-the-completed-app) tamamlanan uygulama güvenlik özellikleri ile ilgili bilgi sahibi olursunuz.</span><span class="sxs-lookup"><span data-stu-id="e9197-149">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="e9197-150">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="e9197-150">The starter app</span></span>

<span data-ttu-id="e9197-151">[İndirme](xref:tutorials/index#how-to-download-a-sample) [başlangıç](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="e9197-151">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="e9197-152">Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e9197-152">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="e9197-153">Kullanıcı verilerinin güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="e9197-153">Secure user data</span></span>

<span data-ttu-id="e9197-154">Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="e9197-154">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="e9197-155">Tamamlanan projeye başvuran daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="e9197-155">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="e9197-156">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="e9197-156">Tie the contact data to the user</span></span>

<span data-ttu-id="e9197-157">ASP.NET kullanmak [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerine, ancak diğer kullanıcıların verileri düzenleyebilir.</span><span class="sxs-lookup"><span data-stu-id="e9197-157">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="e9197-158">Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="e9197-158">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="e9197-159">`OwnerID` kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="e9197-159">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="e9197-160">`Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="e9197-160">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="e9197-161">Yeni geçiş oluştur ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="e9197-161">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="e9197-162">Kimlik için rol hizmetleri Ekle</span><span class="sxs-lookup"><span data-stu-id="e9197-162">Add Role services to Identity</span></span>

<span data-ttu-id="e9197-163">Append [ndeki](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) rol hizmetlerini eklemek için:</span><span class="sxs-lookup"><span data-stu-id="e9197-163">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="e9197-164">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="e9197-164">Require authenticated users</span></span>

<span data-ttu-id="e9197-165">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="e9197-165">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="e9197-166">Kimlik doğrulaması ile Razor sayfası, denetleyici veya eylem yöntemi düzeyinde dışında iyileştirilmiş `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e9197-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="e9197-167">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur.</span><span class="sxs-lookup"><span data-stu-id="e9197-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="e9197-168">Kimlik doğrulaması varsayılan olarak gerekli olan bağlı olan, yeni denetleyicileri ve dahil etmek için Razor sayfaları hakkında daha fazla güvenli `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e9197-168">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="e9197-169">Ekleme [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) dizine hakkında ve kişi sayfaları böylece bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9197-169">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="e9197-170">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="e9197-170">Configure the test account</span></span>

<span data-ttu-id="e9197-171">`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve.</span><span class="sxs-lookup"><span data-stu-id="e9197-171">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="e9197-172">Kullanım [gizli dizi Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="e9197-172">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="e9197-173">Proje dizininden parolayı ayarlayın (dizinini içeren *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="e9197-173">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="e9197-174">Güçlü bir parola belirtilmediği takdirde, bir özel durum `SeedData.Initialize` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e9197-174">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="e9197-175">Güncelleştirme `Main` test parola kullanmak üzere:</span><span class="sxs-lookup"><span data-stu-id="e9197-175">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="e9197-176">Test hesapları oluşturabilir ve kişilerini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="e9197-176">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="e9197-177">Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturma sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e9197-177">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="e9197-178">Yönetici kullanıcı Kimliğini ekleyin ve `ContactStatus` kişilere.</span><span class="sxs-lookup"><span data-stu-id="e9197-178">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="e9197-179">"Gönderildi" ve "Reddedildi" bir kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="e9197-179">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="e9197-180">Kullanıcı kimliği ve durumu tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e9197-180">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="e9197-181">Tek bir kişi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="e9197-181">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="e9197-182">Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="e9197-182">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="e9197-183">Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="e9197-183">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e9197-184">`ContactIsOwnerAuthorizationHandler` Bir kaynakta çalışan kullanıcı kaynağın sahibi doğrular.</span><span class="sxs-lookup"><span data-stu-id="e9197-184">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="e9197-185">`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) kişi sahibi kimliği doğrulanmış geçerli kullanıcı ise.</span><span class="sxs-lookup"><span data-stu-id="e9197-185">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="e9197-186">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="e9197-186">Authorization handlers generally:</span></span>

* <span data-ttu-id="e9197-187">Dönüş `context.Succeed` gereksinimleri karşılanmadığı zaman.</span><span class="sxs-lookup"><span data-stu-id="e9197-187">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="e9197-188">Dönüş `Task.CompletedTask` zaman olmayan gereksinimleri karşılanıyor.</span><span class="sxs-lookup"><span data-stu-id="e9197-188">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="e9197-189">`Task.CompletedTask` ne success veya FAILURE olur&mdash;çalıştırmak, diğer yetkilendirme işleyicileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="e9197-189">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="e9197-190">Açıkça başarısız gerekiyorsa, iade [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="e9197-190">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="e9197-191">Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir.</span><span class="sxs-lookup"><span data-stu-id="e9197-191">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="e9197-192">`ContactIsOwnerAuthorizationHandler` gereksinim parametrede aktarılan işlemi denetleyin. gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e9197-192">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="e9197-193">Bir yönetici yetkilendirme işleyici oluşturun</span><span class="sxs-lookup"><span data-stu-id="e9197-193">Create a manager authorization handler</span></span>

<span data-ttu-id="e9197-194">Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="e9197-194">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e9197-195">`ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, bir yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="e9197-195">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="e9197-196">Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="e9197-196">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="e9197-197">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="e9197-197">Create an administrator authorization handler</span></span>

<span data-ttu-id="e9197-198">Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="e9197-198">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e9197-199">`ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="e9197-199">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="e9197-200">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="e9197-200">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="e9197-201">Yetkilendirme işleyicilerini kaydedin</span><span class="sxs-lookup"><span data-stu-id="e9197-201">Register the authorization handlers</span></span>

<span data-ttu-id="e9197-202">Entity Framework Core kullanan hizmetler için kaydedilmelidir [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="e9197-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="e9197-203">`ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e9197-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="e9197-204">Kullanılabilir bulunmaları işleyicilerini hizmet koleksiyonuyla kaydedin `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e9197-204">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e9197-205">Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e9197-205">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="e9197-206">`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` teklileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="e9197-206">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="e9197-207">Oldukları teklileri çünkü bunlar EF kullanmayın ve gereken tüm bilgiler `Context` parametresinin `HandleRequirementAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e9197-207">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="e9197-208">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="e9197-208">Support authorization</span></span>

<span data-ttu-id="e9197-209">Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e9197-209">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="e9197-210">Gözden geçirme kişi işlem gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="e9197-210">Review the contact operations requirements class</span></span>

<span data-ttu-id="e9197-211">Gözden geçirme `ContactOperations` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e9197-211">Review the `ContactOperations` class.</span></span> <span data-ttu-id="e9197-212">Bu sınıf gereksinimleri içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="e9197-212">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="e9197-213">Kişiler Razor sayfaları için bir temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="e9197-213">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="e9197-214">Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e9197-214">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="e9197-215">Temel sınıf başlatma kodunu tek bir yerde getirir:</span><span class="sxs-lookup"><span data-stu-id="e9197-215">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="e9197-216">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="e9197-216">The preceding code:</span></span>

* <span data-ttu-id="e9197-217">Ekler `IAuthorizationService` yetkilendirme işleyicilere erişmek için hizmet.</span><span class="sxs-lookup"><span data-stu-id="e9197-217">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="e9197-218">Kimlik ekler `UserManager` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="e9197-218">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="e9197-219">Ekleme `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="e9197-219">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="e9197-220">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="e9197-220">Update the CreateModel</span></span>

<span data-ttu-id="e9197-221">Kullanılacak Oluştur sayfası model Oluşturucu güncelleştirme `DI_BasePageModel` temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e9197-221">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="e9197-222">Güncelleştirme `CreateModel.OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e9197-222">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="e9197-223">Kullanıcı Kimliğinize ekleme `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="e9197-223">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="e9197-224">Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="e9197-224">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="e9197-225">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="e9197-225">Update the IndexModel</span></span>

<span data-ttu-id="e9197-226">Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilecek şekilde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e9197-226">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="e9197-227">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="e9197-227">Update the EditModel</span></span>

<span data-ttu-id="e9197-228">Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e9197-228">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="e9197-229">Kaynak Yetkilendirme Doğrulanmakta olan çünkü `[Authorize]` özniteliği yeterli değil.</span><span class="sxs-lookup"><span data-stu-id="e9197-229">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="e9197-230">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="e9197-230">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="e9197-231">Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e9197-231">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="e9197-232">Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e9197-232">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="e9197-233">Sık sık geçirerek kaynak anahtarı kaynağa erişir.</span><span class="sxs-lookup"><span data-stu-id="e9197-233">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="e9197-234">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="e9197-234">Update the DeleteModel</span></span>

<span data-ttu-id="e9197-235">Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="e9197-235">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="e9197-236">Kimlik doğrulama servisi görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="e9197-236">Inject the authorization service into the views</span></span>

<span data-ttu-id="e9197-237">Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="e9197-237">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="e9197-238">Yetkilendirme hizmetinde ekleme *Views/_ViewImports.cshtml* tüm görünümlere kullanılabilir olacak şekilde dosya:</span><span class="sxs-lookup"><span data-stu-id="e9197-238">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="e9197-239">Birkaç önceki biçimlendirme ekler `using` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="e9197-239">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="e9197-240">Güncelleştirme **Düzenle** ve **Sil** bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar tarafından işlenen için:</span><span class="sxs-lookup"><span data-stu-id="e9197-240">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="e9197-241">Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="e9197-241">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="e9197-242">Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar.</span><span class="sxs-lookup"><span data-stu-id="e9197-242">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="e9197-243">Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack.</span><span class="sxs-lookup"><span data-stu-id="e9197-243">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="e9197-244">Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="e9197-244">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="e9197-245">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="e9197-245">Update Details</span></span>

<span data-ttu-id="e9197-246">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="e9197-246">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="e9197-247">Güncelleştirme ayrıntıları sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="e9197-247">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="e9197-248">Ekleme veya bir rolü için bir kullanıcıyı kaldırma</span><span class="sxs-lookup"><span data-stu-id="e9197-248">Add or remove a user to a role</span></span>

<span data-ttu-id="e9197-249">Bkz: [bu sorunu](https://github.com/aspnet/Docs/issues/8502) hakkında bilgi için:</span><span class="sxs-lookup"><span data-stu-id="e9197-249">See [this issue](https://github.com/aspnet/Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="e9197-250">Bir kullanıcıdan ayrıcalıklarının kaldırılması.</span><span class="sxs-lookup"><span data-stu-id="e9197-250">Removing privileges from a user.</span></span> <span data-ttu-id="e9197-251">Örneğin, bir kullanıcı bir sohbet uygulaması ses kapatma.</span><span class="sxs-lookup"><span data-stu-id="e9197-251">For example muting a user in a chat app.</span></span>
* <span data-ttu-id="e9197-252">Bir kullanıcı ayrıcalıkları ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="e9197-252">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="e9197-253">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="e9197-253">Test the completed app</span></span>

<span data-ttu-id="e9197-254">Uygulamanın, kişiler varsa:</span><span class="sxs-lookup"><span data-stu-id="e9197-254">If the app has contacts:</span></span>

* <span data-ttu-id="e9197-255">Tüm kayıtları silme `Contact` tablo.</span><span class="sxs-lookup"><span data-stu-id="e9197-255">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="e9197-256">Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="e9197-256">Restart the app to seed the database.</span></span>

<span data-ttu-id="e9197-257">Bir kullanıcı kişiler göz atmak için kaydedin.</span><span class="sxs-lookup"><span data-stu-id="e9197-257">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="e9197-258">Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate sürümleri) başlatılmasıdır.</span><span class="sxs-lookup"><span data-stu-id="e9197-258">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="e9197-259">Yeni bir kullanıcı bir tarayıcıda kaydedin (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="e9197-259">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="e9197-260">Her bir tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="e9197-260">Sign in to each browser with a different user.</span></span> <span data-ttu-id="e9197-261">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="e9197-261">Verify the following operations:</span></span>

* <span data-ttu-id="e9197-262">Kayıtlı kullanıcılar, tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="e9197-262">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="e9197-263">Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="e9197-263">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="e9197-264">Yöneticileri onaylayabilir veya kişi verilerinizi reddedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9197-264">Managers can approve or reject contact data.</span></span> <span data-ttu-id="e9197-265">`Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="e9197-265">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="e9197-266">Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="e9197-266">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="e9197-267">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="e9197-267">User</span></span>| <span data-ttu-id="e9197-268">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="e9197-268">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="e9197-269">Kendi veri düzenleme/silebilir</span><span class="sxs-lookup"><span data-stu-id="e9197-269">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="e9197-270">Onayla/Reddet ve düzenleme/silme, veri sahip olabilir</span><span class="sxs-lookup"><span data-stu-id="e9197-270">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="e9197-271">Düzenleme/silme ve tüm verileri Onayla/Reddet kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="e9197-271">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="e9197-272">Bir kişi, yöneticinin tarayıcıda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e9197-272">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="e9197-273">Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="e9197-273">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="e9197-274">Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="e9197-274">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="e9197-275">Başlangıç uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="e9197-275">Create the starter app</span></span>

* <span data-ttu-id="e9197-276">"ContactManager" adlı bir Razor sayfaları uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e9197-276">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="e9197-277">Uygulamayı oluşturma **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="e9197-277">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="e9197-278">Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.</span><span class="sxs-lookup"><span data-stu-id="e9197-278">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="e9197-279">`-uld` LocalDB yerine SQLite belirtir</span><span class="sxs-lookup"><span data-stu-id="e9197-279">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="e9197-280">Ekleme *Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="e9197-280">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="e9197-281">İskele `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="e9197-281">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="e9197-282">İlk geçiş oluşturun ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="e9197-282">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="e9197-283">Güncelleştirme **ContactManager** içinde yer işareti *Pages/_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="e9197-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="e9197-284">Oluşturma, düzenleme ve silme kişi uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="e9197-284">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="e9197-285">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="e9197-285">Seed the database</span></span>

<span data-ttu-id="e9197-286">Ekleme [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) sınıfının *veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="e9197-286">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="e9197-287">Çağrı `SeedData.Initialize` gelen `Main`:</span><span class="sxs-lookup"><span data-stu-id="e9197-287">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="e9197-288">Test, uygulama veritabanını sağlanmış.</span><span class="sxs-lookup"><span data-stu-id="e9197-288">Test that the app seeded the database.</span></span> <span data-ttu-id="e9197-289">DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="e9197-289">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="e9197-290">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e9197-290">Additional resources</span></span>

* [<span data-ttu-id="e9197-291">Azure App Service'te .NET Core ve SQL veritabanı web uygulaması derleme</span><span class="sxs-lookup"><span data-stu-id="e9197-291">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="e9197-292">[ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="e9197-292">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="e9197-293">Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="e9197-293">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="e9197-294">ASP.NET Core yetkilendirme: basit, rol, talep tabanlı ve özel</span><span class="sxs-lookup"><span data-stu-id="e9197-294">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="e9197-295">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="e9197-295">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end
