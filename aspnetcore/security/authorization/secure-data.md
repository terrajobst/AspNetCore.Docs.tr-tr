---
title: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma
author: rick-anderson
description: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile Razor sayfaları uygulaması oluşturmayı öğrenin. HTTPS, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 225d0e3aa51745253d03e614b1c8568b3a6ba731
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017490"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="0d308-104">Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d308-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="0d308-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="0d308-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="0d308-106">Bkz: [bu PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC sürümü için.</span><span class="sxs-lookup"><span data-stu-id="0d308-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="0d308-107">Bu öğreticide ASP.NET Core 1.1 sürümü [bu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) klasör.</span><span class="sxs-lookup"><span data-stu-id="0d308-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="0d308-108">ASP.NET Core örnek konusu 1.1 [örnekleri](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="0d308-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0d308-109">Bkz: [bu pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="0d308-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0d308-110">Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0d308-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="0d308-111">Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="0d308-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="0d308-112">Üç güvenlik grubu vardır:</span><span class="sxs-lookup"><span data-stu-id="0d308-112">There are three security groups:</span></span>

* <span data-ttu-id="0d308-113">**Kayıtlı kullanıcıların** onaylanan tüm verileri görüntüleyebilir ve kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="0d308-114">**Yöneticileri** onaylayabilecek veya reddedebilecek kişi verilerini.</span><span class="sxs-lookup"><span data-stu-id="0d308-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="0d308-115">Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="0d308-116">**Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0d308-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="0d308-117">Bu belgedeki görüntüler en son şablonlarla tam olarak eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="0d308-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="0d308-118">Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) açmıştır.</span><span class="sxs-lookup"><span data-stu-id="0d308-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="0d308-119">Rick yalnızca onaylanan kişiler görüntüleyebilir ve **Düzenle**/**Sil**/**Yeni Oluştur** kendi kişiler için bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="0d308-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="0d308-120">Yalnızca en son kaydını görüntüler Rick tarafından oluşturulan **Düzenle** ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="0d308-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="0d308-121">Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.</span><span class="sxs-lookup"><span data-stu-id="0d308-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Oturum Rick gösteren ekran görüntüsü](secure-data/_static/rick.png)

<span data-ttu-id="0d308-123">Aşağıdaki görüntüde, `manager@contoso.com` ve yöneticisinin rolünde oturum açıldı:</span><span class="sxs-lookup"><span data-stu-id="0d308-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Gösteren ekran görüntüsü manager@contoso.com oturum açıldı](secure-data/_static/manager1.png)

<span data-ttu-id="0d308-125">Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0d308-125">The following image shows the managers details view of a contact:</span></span>

![Bir kişinin yöneticisinin görüntüle](secure-data/_static/manager.png)

<span data-ttu-id="0d308-127">**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0d308-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="0d308-128">Aşağıdaki görüntüde, `admin@contoso.com` ve yönetici rolünde oturum açıldı:</span><span class="sxs-lookup"><span data-stu-id="0d308-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Gösteren ekran görüntüsü admin@contoso.com oturum açıldı](secure-data/_static/admin.png)

<span data-ttu-id="0d308-130">Yöneticisinin tüm ayrıcalıklara sahip değil.</span><span class="sxs-lookup"><span data-stu-id="0d308-130">The administrator has all privileges.</span></span> <span data-ttu-id="0d308-131">Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0d308-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="0d308-132">Uygulama tarafından oluşturulan [yapı iskelesi](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) aşağıdaki `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="0d308-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="0d308-133">Örnek aşağıdaki yetkilendirme işleyicilerini içerir:</span><span class="sxs-lookup"><span data-stu-id="0d308-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="0d308-134">`ContactIsOwnerAuthorizationHandler`: Kullanıcının yalnızca verilerini düzenleyebilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0d308-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="0d308-135">`ContactManagerAuthorizationHandler`: Yöneticilerin kişileri onaylamasını veya reddetmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0d308-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="0d308-136">`ContactAdministratorsAuthorizationHandler`: Yöneticilerin kişileri onaylamasını veya reddetmesini ve kişileri düzenlemesini/silmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0d308-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d308-137">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="0d308-137">Prerequisites</span></span>

<span data-ttu-id="0d308-138">Bu öğreticide gelişmiştir.</span><span class="sxs-lookup"><span data-stu-id="0d308-138">This tutorial is advanced.</span></span> <span data-ttu-id="0d308-139">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="0d308-139">You should be familiar with:</span></span>

* [<span data-ttu-id="0d308-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d308-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="0d308-141">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="0d308-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="0d308-142">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="0d308-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="0d308-143">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="0d308-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="0d308-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0d308-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="0d308-145">Tamamlanmış uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="0d308-145">The starter and completed app</span></span>

<span data-ttu-id="0d308-146">[İndirme](xref:index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) uygulama.</span><span class="sxs-lookup"><span data-stu-id="0d308-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="0d308-147">[Test](#test-the-completed-app) tamamlanan uygulama güvenlik özellikleri ile ilgili bilgi sahibi olursunuz.</span><span class="sxs-lookup"><span data-stu-id="0d308-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="0d308-148">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="0d308-148">The starter app</span></span>

<span data-ttu-id="0d308-149">[İndirme](xref:index#how-to-download-a-sample) [başlangıç](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) uygulama.</span><span class="sxs-lookup"><span data-stu-id="0d308-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="0d308-150">Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="0d308-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="0d308-151">Kullanıcı verilerinin güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="0d308-151">Secure user data</span></span>

<span data-ttu-id="0d308-152">Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="0d308-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="0d308-153">Tamamlanan projeye başvuran daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="0d308-154">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="0d308-154">Tie the contact data to the user</span></span>

<span data-ttu-id="0d308-155">ASP.NET kullanmak [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerine, ancak diğer kullanıcıların verileri düzenleyebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="0d308-156">Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="0d308-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="0d308-157">`OwnerID` kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="0d308-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="0d308-158">`Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="0d308-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="0d308-159">Yeni geçiş oluştur ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="0d308-159">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="0d308-160">Kimlik için rol hizmetleri Ekle</span><span class="sxs-lookup"><span data-stu-id="0d308-160">Add Role services to Identity</span></span>

<span data-ttu-id="0d308-161">Append [ndeki](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) rol hizmetlerini eklemek için:</span><span class="sxs-lookup"><span data-stu-id="0d308-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="0d308-162">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="0d308-162">Require authenticated users</span></span>

<span data-ttu-id="0d308-163">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="0d308-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="0d308-164">Kimlik doğrulaması ile Razor sayfası, denetleyici veya eylem yöntemi düzeyinde dışında iyileştirilmiş `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0d308-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="0d308-165">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur.</span><span class="sxs-lookup"><span data-stu-id="0d308-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="0d308-166">Kimlik doğrulaması varsayılan olarak gerekli olan bağlı olan, yeni denetleyicileri ve dahil etmek için Razor sayfaları hakkında daha fazla güvenli `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0d308-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="0d308-167">Anonim kullanıcıların, kaydolmadan önce site hakkında bilgi alması için dizin ve gizlilik sayfalarına [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d308-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="0d308-168">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="0d308-168">Configure the test account</span></span>

<span data-ttu-id="0d308-169">`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve.</span><span class="sxs-lookup"><span data-stu-id="0d308-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="0d308-170">Kullanım [gizli dizi Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="0d308-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="0d308-171">Proje dizininden parolayı ayarlayın (dizinini içeren *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="0d308-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="0d308-172">Güçlü bir parola belirtilmediği takdirde, bir özel durum `SeedData.Initialize` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0d308-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="0d308-173">Güncelleştirme `Main` test parola kullanmak üzere:</span><span class="sxs-lookup"><span data-stu-id="0d308-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="0d308-174">Test hesapları oluşturabilir ve kişilerini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="0d308-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="0d308-175">Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturma sınıfı:</span><span class="sxs-lookup"><span data-stu-id="0d308-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="0d308-176">Yönetici kullanıcı Kimliğini ekleyin ve `ContactStatus` kişilere.</span><span class="sxs-lookup"><span data-stu-id="0d308-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="0d308-177">"Gönderildi" ve "Reddedildi" bir kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="0d308-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="0d308-178">Kullanıcı kimliği ve durumu tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d308-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="0d308-179">Tek bir kişi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0d308-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="0d308-180">Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d308-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="0d308-181">Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="0d308-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="0d308-182">`ContactIsOwnerAuthorizationHandler` Bir kaynakta çalışan kullanıcı kaynağın sahibi doğrular.</span><span class="sxs-lookup"><span data-stu-id="0d308-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="0d308-183">`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) kişi sahibi kimliği doğrulanmış geçerli kullanıcı ise.</span><span class="sxs-lookup"><span data-stu-id="0d308-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="0d308-184">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="0d308-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="0d308-185">Dönüş `context.Succeed` gereksinimleri karşılanmadığı zaman.</span><span class="sxs-lookup"><span data-stu-id="0d308-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="0d308-186">Dönüş `Task.CompletedTask` zaman olmayan gereksinimleri karşılanıyor.</span><span class="sxs-lookup"><span data-stu-id="0d308-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="0d308-187">`Task.CompletedTask`başarılı veya başarısız&mdash;değil, diğer yetkilendirme işleyicilerinin çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="0d308-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="0d308-188">Açıkça başarısız gerekiyorsa, iade [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="0d308-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="0d308-189">Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir.</span><span class="sxs-lookup"><span data-stu-id="0d308-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="0d308-190">`ContactIsOwnerAuthorizationHandler` gereksinim parametrede aktarılan işlemi denetleyin. gerekmez.</span><span class="sxs-lookup"><span data-stu-id="0d308-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="0d308-191">Bir yönetici yetkilendirme işleyici oluşturun</span><span class="sxs-lookup"><span data-stu-id="0d308-191">Create a manager authorization handler</span></span>

<span data-ttu-id="0d308-192">Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="0d308-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="0d308-193">`ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, bir yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="0d308-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="0d308-194">Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="0d308-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="0d308-195">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="0d308-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="0d308-196">Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="0d308-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="0d308-197">`ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="0d308-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="0d308-198">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="0d308-199">Yetkilendirme işleyicilerini kaydedin</span><span class="sxs-lookup"><span data-stu-id="0d308-199">Register the authorization handlers</span></span>

<span data-ttu-id="0d308-200">Entity Framework Core kullanan hizmetler için kaydedilmelidir [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="0d308-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="0d308-201">`ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0d308-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="0d308-202">Kullanılabilir bulunmaları işleyicilerini hizmet koleksiyonuyla kaydedin `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0d308-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0d308-203">Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0d308-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="0d308-204">`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` teklileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="0d308-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="0d308-205">Oldukları teklileri çünkü bunlar EF kullanmayın ve gereken tüm bilgiler `Context` parametresinin `HandleRequirementAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0d308-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="0d308-206">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="0d308-206">Support authorization</span></span>

<span data-ttu-id="0d308-207">Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d308-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="0d308-208">Gözden geçirme kişi işlem gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="0d308-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="0d308-209">Gözden geçirme `ContactOperations` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0d308-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="0d308-210">Bu sınıf gereksinimleri içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="0d308-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="0d308-211">Kişiler Razor sayfaları için bir temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d308-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="0d308-212">Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0d308-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="0d308-213">Temel sınıf başlatma kodunu tek bir yerde getirir:</span><span class="sxs-lookup"><span data-stu-id="0d308-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="0d308-214">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="0d308-214">The preceding code:</span></span>

* <span data-ttu-id="0d308-215">Ekler `IAuthorizationService` yetkilendirme işleyicilere erişmek için hizmet.</span><span class="sxs-lookup"><span data-stu-id="0d308-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="0d308-216">Kimlik ekler `UserManager` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="0d308-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="0d308-217">Ekleme `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="0d308-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="0d308-218">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="0d308-218">Update the CreateModel</span></span>

<span data-ttu-id="0d308-219">Kullanılacak Oluştur sayfası model Oluşturucu güncelleştirme `DI_BasePageModel` temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="0d308-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="0d308-220">Güncelleştirme `CreateModel.OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0d308-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="0d308-221">Kullanıcı Kimliğinize ekleme `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="0d308-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="0d308-222">Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="0d308-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="0d308-223">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="0d308-223">Update the IndexModel</span></span>

<span data-ttu-id="0d308-224">Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilecek şekilde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0d308-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="0d308-225">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="0d308-225">Update the EditModel</span></span>

<span data-ttu-id="0d308-226">Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d308-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="0d308-227">Kaynak Yetkilendirme Doğrulanmakta olan çünkü `[Authorize]` özniteliği yeterli değil.</span><span class="sxs-lookup"><span data-stu-id="0d308-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="0d308-228">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="0d308-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="0d308-229">Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0d308-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="0d308-230">Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="0d308-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="0d308-231">Sık sık geçirerek kaynak anahtarı kaynağa erişir.</span><span class="sxs-lookup"><span data-stu-id="0d308-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="0d308-232">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="0d308-232">Update the DeleteModel</span></span>

<span data-ttu-id="0d308-233">Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="0d308-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="0d308-234">Kimlik doğrulama servisi görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="0d308-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="0d308-235">Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="0d308-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="0d308-236">Tüm görünümlerde kullanılabilmesi için *Sayfalar/_Viewwimports. cshtml* dosyasına yetkilendirme hizmetini ekleme:</span><span class="sxs-lookup"><span data-stu-id="0d308-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="0d308-237">Birkaç önceki biçimlendirme ekler `using` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="0d308-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="0d308-238">Güncelleştirme **Düzenle** ve **Sil** bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar tarafından işlenen için:</span><span class="sxs-lookup"><span data-stu-id="0d308-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="0d308-239">Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="0d308-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="0d308-240">Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar.</span><span class="sxs-lookup"><span data-stu-id="0d308-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="0d308-241">Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack.</span><span class="sxs-lookup"><span data-stu-id="0d308-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="0d308-242">Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="0d308-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="0d308-243">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="0d308-243">Update Details</span></span>

<span data-ttu-id="0d308-244">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="0d308-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="0d308-245">Güncelleştirme ayrıntıları sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="0d308-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="0d308-246">Ekleme veya bir rolü için bir kullanıcıyı kaldırma</span><span class="sxs-lookup"><span data-stu-id="0d308-246">Add or remove a user to a role</span></span>

<span data-ttu-id="0d308-247">Bkz: [bu sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/8502) hakkında bilgi için:</span><span class="sxs-lookup"><span data-stu-id="0d308-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="0d308-248">Bir kullanıcıdan ayrıcalıklarının kaldırılması.</span><span class="sxs-lookup"><span data-stu-id="0d308-248">Removing privileges from a user.</span></span> <span data-ttu-id="0d308-249">Örneğin, bir sohbet uygulamasındaki kullanıcıyı kapatma.</span><span class="sxs-lookup"><span data-stu-id="0d308-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="0d308-250">Bir kullanıcı ayrıcalıkları ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="0d308-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="0d308-251">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="0d308-251">Test the completed app</span></span>

<span data-ttu-id="0d308-252">Kapsanan kullanıcı hesapları için bir parola belirlemediyseniz kullanın [gizli dizi Yöneticisi aracını](xref:security/app-secrets#secret-manager) parola ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="0d308-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="0d308-253">Güçlü bir parola seçin: Sekiz veya daha fazla karakter ve en az bir büyük harf karakter, sayı ve simge kullanın.</span><span class="sxs-lookup"><span data-stu-id="0d308-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="0d308-254">Örneğin, `Passw0rd!` güçlü parola gereksinimlerini karşılıyor.</span><span class="sxs-lookup"><span data-stu-id="0d308-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="0d308-255">Aşağıdaki komutu yürütün proje klasöründen burada `<PW>` parola:</span><span class="sxs-lookup"><span data-stu-id="0d308-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="0d308-256">Uygulamanın, kişiler varsa:</span><span class="sxs-lookup"><span data-stu-id="0d308-256">If the app has contacts:</span></span>

* <span data-ttu-id="0d308-257">Tüm kayıtları silin `Contact` tablo.</span><span class="sxs-lookup"><span data-stu-id="0d308-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="0d308-258">Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="0d308-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="0d308-259">Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate oturumları) başlatılmasıdır.</span><span class="sxs-lookup"><span data-stu-id="0d308-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="0d308-260">Yeni bir kullanıcı bir tarayıcıda kaydedin (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="0d308-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="0d308-261">Her bir tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="0d308-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="0d308-262">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="0d308-262">Verify the following operations:</span></span>

* <span data-ttu-id="0d308-263">Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="0d308-264">Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="0d308-265">Yöneticileri Onayla/kişi verilerini reddet.</span><span class="sxs-lookup"><span data-stu-id="0d308-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="0d308-266">`Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="0d308-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="0d308-267">Yöneticiler Onayla/Reddet ve tüm verileri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="0d308-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="0d308-268">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="0d308-268">User</span></span>                | <span data-ttu-id="0d308-269">Uygulama tarafından sağlanmış</span><span class="sxs-lookup"><span data-stu-id="0d308-269">Seeded by the app</span></span> | <span data-ttu-id="0d308-270">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="0d308-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="0d308-271">Hayır</span><span class="sxs-lookup"><span data-stu-id="0d308-271">No</span></span>                | <span data-ttu-id="0d308-272">Kendi veri düzenleme veya silme.</span><span class="sxs-lookup"><span data-stu-id="0d308-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="0d308-273">Evet</span><span class="sxs-lookup"><span data-stu-id="0d308-273">Yes</span></span>               | <span data-ttu-id="0d308-274">Onayla/Reddet ve kendi veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="0d308-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="0d308-275">Evet</span><span class="sxs-lookup"><span data-stu-id="0d308-275">Yes</span></span>               | <span data-ttu-id="0d308-276">Onayla/Reddet ve tüm verileri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="0d308-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="0d308-277">Bir kişi, yöneticinin tarayıcıda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0d308-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="0d308-278">Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="0d308-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="0d308-279">Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="0d308-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="0d308-280">Başlangıç uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="0d308-280">Create the starter app</span></span>

* <span data-ttu-id="0d308-281">"ContactManager" adlı bir Razor sayfaları uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d308-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="0d308-282">Uygulamayı oluşturma **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="0d308-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="0d308-283">Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.</span><span class="sxs-lookup"><span data-stu-id="0d308-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="0d308-284">`-uld` LocalDB yerine SQLite belirtir</span><span class="sxs-lookup"><span data-stu-id="0d308-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="0d308-285">*Modeller/Ilgili kişi ekle. cs*:</span><span class="sxs-lookup"><span data-stu-id="0d308-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="0d308-286">İskele `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="0d308-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="0d308-287">İlk geçiş oluşturun ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="0d308-287">Create initial migration and update the database:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

<span data-ttu-id="0d308-288">`dotnet aspnet-codegenerator razorpage` Komutuyla bir hata yaşarsanız, [Bu GitHub sorununa](https://github.com/aspnet/Scaffolding/issues/984)bakın.</span><span class="sxs-lookup"><span data-stu-id="0d308-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="0d308-289">*Pages/Shared/_Layout. cshtml* dosyasındaki **ContactManager** bağlayıcısını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="0d308-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="0d308-290">Oluşturma, düzenleme ve silme kişi uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="0d308-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="0d308-291">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d308-291">Seed the database</span></span>

<span data-ttu-id="0d308-292">*Veri* klasörüne [seeddata](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) sınıfını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="0d308-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="0d308-293">Çağrı `SeedData.Initialize` gelen `Main`:</span><span class="sxs-lookup"><span data-stu-id="0d308-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="0d308-294">Test, uygulama veritabanını sağlanmış.</span><span class="sxs-lookup"><span data-stu-id="0d308-294">Test that the app seeded the database.</span></span> <span data-ttu-id="0d308-295">DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="0d308-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="0d308-296">Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0d308-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="0d308-297">Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="0d308-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="0d308-298">Üç güvenlik grubu vardır:</span><span class="sxs-lookup"><span data-stu-id="0d308-298">There are three security groups:</span></span>

* <span data-ttu-id="0d308-299">**Kayıtlı kullanıcıların** onaylanan tüm verileri görüntüleyebilir ve kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="0d308-300">**Yöneticileri** onaylayabilecek veya reddedebilecek kişi verilerini.</span><span class="sxs-lookup"><span data-stu-id="0d308-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="0d308-301">Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="0d308-302">**Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0d308-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="0d308-303">Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) açmıştır.</span><span class="sxs-lookup"><span data-stu-id="0d308-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="0d308-304">Rick yalnızca onaylanan kişiler görüntüleyebilir ve **Düzenle**/**Sil**/**Yeni Oluştur** kendi kişiler için bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="0d308-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="0d308-305">Yalnızca en son kaydını görüntüler Rick tarafından oluşturulan **Düzenle** ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="0d308-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="0d308-306">Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.</span><span class="sxs-lookup"><span data-stu-id="0d308-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Oturum Rick gösteren ekran görüntüsü](secure-data/_static/rick.png)

<span data-ttu-id="0d308-308">Aşağıdaki görüntüde, `manager@contoso.com` ve yöneticisinin rolünde oturum açıldı:</span><span class="sxs-lookup"><span data-stu-id="0d308-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Gösteren ekran görüntüsü manager@contoso.com oturum açıldı](secure-data/_static/manager1.png)

<span data-ttu-id="0d308-310">Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0d308-310">The following image shows the managers details view of a contact:</span></span>

![Bir kişinin yöneticisinin görüntüle](secure-data/_static/manager.png)

<span data-ttu-id="0d308-312">**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0d308-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="0d308-313">Aşağıdaki görüntüde, `admin@contoso.com` ve yönetici rolünde oturum açıldı:</span><span class="sxs-lookup"><span data-stu-id="0d308-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Gösteren ekran görüntüsü admin@contoso.com oturum açıldı](secure-data/_static/admin.png)

<span data-ttu-id="0d308-315">Yöneticisinin tüm ayrıcalıklara sahip değil.</span><span class="sxs-lookup"><span data-stu-id="0d308-315">The administrator has all privileges.</span></span> <span data-ttu-id="0d308-316">Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0d308-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="0d308-317">Uygulama tarafından oluşturulan [yapı iskelesi](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) aşağıdaki `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="0d308-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="0d308-318">Örnek aşağıdaki yetkilendirme işleyicilerini içerir:</span><span class="sxs-lookup"><span data-stu-id="0d308-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="0d308-319">`ContactIsOwnerAuthorizationHandler`: Kullanıcının yalnızca verilerini düzenleyebilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0d308-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="0d308-320">`ContactManagerAuthorizationHandler`: Yöneticilerin kişileri onaylamasını veya reddetmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0d308-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="0d308-321">`ContactAdministratorsAuthorizationHandler`: Yöneticilerin kişileri onaylamasını veya reddetmesini ve kişileri düzenlemesini/silmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0d308-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d308-322">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="0d308-322">Prerequisites</span></span>

<span data-ttu-id="0d308-323">Bu öğreticide gelişmiştir.</span><span class="sxs-lookup"><span data-stu-id="0d308-323">This tutorial is advanced.</span></span> <span data-ttu-id="0d308-324">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="0d308-324">You should be familiar with:</span></span>

* [<span data-ttu-id="0d308-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d308-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="0d308-326">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="0d308-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="0d308-327">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="0d308-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="0d308-328">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="0d308-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="0d308-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0d308-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="0d308-330">Tamamlanmış uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="0d308-330">The starter and completed app</span></span>

<span data-ttu-id="0d308-331">[İndirme](xref:index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) uygulama.</span><span class="sxs-lookup"><span data-stu-id="0d308-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="0d308-332">[Test](#test-the-completed-app) tamamlanan uygulama güvenlik özellikleri ile ilgili bilgi sahibi olursunuz.</span><span class="sxs-lookup"><span data-stu-id="0d308-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="0d308-333">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="0d308-333">The starter app</span></span>

<span data-ttu-id="0d308-334">[İndirme](xref:index#how-to-download-a-sample) [başlangıç](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) uygulama.</span><span class="sxs-lookup"><span data-stu-id="0d308-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="0d308-335">Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="0d308-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="0d308-336">Kullanıcı verilerinin güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="0d308-336">Secure user data</span></span>

<span data-ttu-id="0d308-337">Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="0d308-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="0d308-338">Tamamlanan projeye başvuran daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="0d308-339">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="0d308-339">Tie the contact data to the user</span></span>

<span data-ttu-id="0d308-340">ASP.NET kullanmak [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerine, ancak diğer kullanıcıların verileri düzenleyebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="0d308-341">Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="0d308-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="0d308-342">`OwnerID` kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="0d308-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="0d308-343">`Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="0d308-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="0d308-344">Yeni geçiş oluştur ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="0d308-344">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="0d308-345">Kimlik için rol hizmetleri Ekle</span><span class="sxs-lookup"><span data-stu-id="0d308-345">Add Role services to Identity</span></span>

<span data-ttu-id="0d308-346">Append [ndeki](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) rol hizmetlerini eklemek için:</span><span class="sxs-lookup"><span data-stu-id="0d308-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="0d308-347">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="0d308-347">Require authenticated users</span></span>

<span data-ttu-id="0d308-348">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="0d308-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="0d308-349">Kimlik doğrulaması ile Razor sayfası, denetleyici veya eylem yöntemi düzeyinde dışında iyileştirilmiş `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0d308-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="0d308-350">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur.</span><span class="sxs-lookup"><span data-stu-id="0d308-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="0d308-351">Kimlik doğrulaması varsayılan olarak gerekli olan bağlı olan, yeni denetleyicileri ve dahil etmek için Razor sayfaları hakkında daha fazla güvenli `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0d308-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="0d308-352">Ekleme [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) dizine hakkında ve kişi sayfaları böylece bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0d308-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="0d308-353">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="0d308-353">Configure the test account</span></span>

<span data-ttu-id="0d308-354">`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve.</span><span class="sxs-lookup"><span data-stu-id="0d308-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="0d308-355">Kullanım [gizli dizi Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="0d308-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="0d308-356">Proje dizininden parolayı ayarlayın (dizinini içeren *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="0d308-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="0d308-357">Güçlü bir parola belirtilmediği takdirde, bir özel durum `SeedData.Initialize` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0d308-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="0d308-358">Güncelleştirme `Main` test parola kullanmak üzere:</span><span class="sxs-lookup"><span data-stu-id="0d308-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="0d308-359">Test hesapları oluşturabilir ve kişilerini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="0d308-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="0d308-360">Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturma sınıfı:</span><span class="sxs-lookup"><span data-stu-id="0d308-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="0d308-361">Yönetici kullanıcı Kimliğini ekleyin ve `ContactStatus` kişilere.</span><span class="sxs-lookup"><span data-stu-id="0d308-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="0d308-362">"Gönderildi" ve "Reddedildi" bir kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="0d308-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="0d308-363">Kullanıcı kimliği ve durumu tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d308-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="0d308-364">Tek bir kişi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0d308-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="0d308-365">Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d308-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="0d308-366">Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="0d308-366">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="0d308-367">`ContactIsOwnerAuthorizationHandler` Bir kaynakta çalışan kullanıcı kaynağın sahibi doğrular.</span><span class="sxs-lookup"><span data-stu-id="0d308-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="0d308-368">`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) kişi sahibi kimliği doğrulanmış geçerli kullanıcı ise.</span><span class="sxs-lookup"><span data-stu-id="0d308-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="0d308-369">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="0d308-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="0d308-370">Dönüş `context.Succeed` gereksinimleri karşılanmadığı zaman.</span><span class="sxs-lookup"><span data-stu-id="0d308-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="0d308-371">Dönüş `Task.CompletedTask` zaman olmayan gereksinimleri karşılanıyor.</span><span class="sxs-lookup"><span data-stu-id="0d308-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="0d308-372">`Task.CompletedTask`başarılı veya başarısız&mdash;değil, diğer yetkilendirme işleyicilerinin çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="0d308-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="0d308-373">Açıkça başarısız gerekiyorsa, iade [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="0d308-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="0d308-374">Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir.</span><span class="sxs-lookup"><span data-stu-id="0d308-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="0d308-375">`ContactIsOwnerAuthorizationHandler` gereksinim parametrede aktarılan işlemi denetleyin. gerekmez.</span><span class="sxs-lookup"><span data-stu-id="0d308-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="0d308-376">Bir yönetici yetkilendirme işleyici oluşturun</span><span class="sxs-lookup"><span data-stu-id="0d308-376">Create a manager authorization handler</span></span>

<span data-ttu-id="0d308-377">Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="0d308-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="0d308-378">`ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, bir yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="0d308-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="0d308-379">Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="0d308-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="0d308-380">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="0d308-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="0d308-381">Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="0d308-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="0d308-382">`ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="0d308-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="0d308-383">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="0d308-384">Yetkilendirme işleyicilerini kaydedin</span><span class="sxs-lookup"><span data-stu-id="0d308-384">Register the authorization handlers</span></span>

<span data-ttu-id="0d308-385">Entity Framework Core kullanan hizmetler için kaydedilmelidir [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="0d308-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="0d308-386">`ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0d308-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="0d308-387">Kullanılabilir bulunmaları işleyicilerini hizmet koleksiyonuyla kaydedin `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0d308-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0d308-388">Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0d308-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="0d308-389">`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` teklileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="0d308-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="0d308-390">Oldukları teklileri çünkü bunlar EF kullanmayın ve gereken tüm bilgiler `Context` parametresinin `HandleRequirementAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0d308-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="0d308-391">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="0d308-391">Support authorization</span></span>

<span data-ttu-id="0d308-392">Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d308-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="0d308-393">Gözden geçirme kişi işlem gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="0d308-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="0d308-394">Gözden geçirme `ContactOperations` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0d308-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="0d308-395">Bu sınıf gereksinimleri içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="0d308-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="0d308-396">Kişiler Razor sayfaları için bir temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d308-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="0d308-397">Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0d308-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="0d308-398">Temel sınıf başlatma kodunu tek bir yerde getirir:</span><span class="sxs-lookup"><span data-stu-id="0d308-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="0d308-399">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="0d308-399">The preceding code:</span></span>

* <span data-ttu-id="0d308-400">Ekler `IAuthorizationService` yetkilendirme işleyicilere erişmek için hizmet.</span><span class="sxs-lookup"><span data-stu-id="0d308-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="0d308-401">Kimlik ekler `UserManager` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="0d308-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="0d308-402">Ekleme `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="0d308-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="0d308-403">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="0d308-403">Update the CreateModel</span></span>

<span data-ttu-id="0d308-404">Kullanılacak Oluştur sayfası model Oluşturucu güncelleştirme `DI_BasePageModel` temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="0d308-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="0d308-405">Güncelleştirme `CreateModel.OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0d308-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="0d308-406">Kullanıcı Kimliğinize ekleme `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="0d308-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="0d308-407">Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="0d308-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="0d308-408">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="0d308-408">Update the IndexModel</span></span>

<span data-ttu-id="0d308-409">Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilecek şekilde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0d308-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="0d308-410">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="0d308-410">Update the EditModel</span></span>

<span data-ttu-id="0d308-411">Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0d308-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="0d308-412">Kaynak Yetkilendirme Doğrulanmakta olan çünkü `[Authorize]` özniteliği yeterli değil.</span><span class="sxs-lookup"><span data-stu-id="0d308-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="0d308-413">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="0d308-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="0d308-414">Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0d308-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="0d308-415">Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="0d308-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="0d308-416">Sık sık geçirerek kaynak anahtarı kaynağa erişir.</span><span class="sxs-lookup"><span data-stu-id="0d308-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="0d308-417">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="0d308-417">Update the DeleteModel</span></span>

<span data-ttu-id="0d308-418">Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="0d308-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="0d308-419">Kimlik doğrulama servisi görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="0d308-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="0d308-420">Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="0d308-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="0d308-421">Yetkilendirme hizmetinde ekleme *Views/_ViewImports.cshtml* tüm görünümlere kullanılabilir olacak şekilde dosya:</span><span class="sxs-lookup"><span data-stu-id="0d308-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="0d308-422">Birkaç önceki biçimlendirme ekler `using` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="0d308-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="0d308-423">Güncelleştirme **Düzenle** ve **Sil** bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar tarafından işlenen için:</span><span class="sxs-lookup"><span data-stu-id="0d308-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="0d308-424">Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="0d308-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="0d308-425">Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar.</span><span class="sxs-lookup"><span data-stu-id="0d308-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="0d308-426">Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack.</span><span class="sxs-lookup"><span data-stu-id="0d308-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="0d308-427">Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="0d308-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="0d308-428">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="0d308-428">Update Details</span></span>

<span data-ttu-id="0d308-429">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="0d308-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="0d308-430">Güncelleştirme ayrıntıları sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="0d308-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="0d308-431">Ekleme veya bir rolü için bir kullanıcıyı kaldırma</span><span class="sxs-lookup"><span data-stu-id="0d308-431">Add or remove a user to a role</span></span>

<span data-ttu-id="0d308-432">Bkz: [bu sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/8502) hakkında bilgi için:</span><span class="sxs-lookup"><span data-stu-id="0d308-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="0d308-433">Bir kullanıcıdan ayrıcalıklarının kaldırılması.</span><span class="sxs-lookup"><span data-stu-id="0d308-433">Removing privileges from a user.</span></span> <span data-ttu-id="0d308-434">Örneğin, bir sohbet uygulamasındaki kullanıcıyı kapatma.</span><span class="sxs-lookup"><span data-stu-id="0d308-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="0d308-435">Bir kullanıcı ayrıcalıkları ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="0d308-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="0d308-436">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="0d308-436">Test the completed app</span></span>

<span data-ttu-id="0d308-437">Kapsanan kullanıcı hesapları için bir parola belirlemediyseniz kullanın [gizli dizi Yöneticisi aracını](xref:security/app-secrets#secret-manager) parola ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="0d308-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="0d308-438">Güçlü bir parola seçin: Sekiz veya daha fazla karakter ve en az bir büyük harf karakter, sayı ve simge kullanın.</span><span class="sxs-lookup"><span data-stu-id="0d308-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="0d308-439">Örneğin, `Passw0rd!` güçlü parola gereksinimlerini karşılıyor.</span><span class="sxs-lookup"><span data-stu-id="0d308-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="0d308-440">Aşağıdaki komutu yürütün proje klasöründen burada `<PW>` parola:</span><span class="sxs-lookup"><span data-stu-id="0d308-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="0d308-441">Veritabanını bırakma ve güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="0d308-441">Drop and update the Database</span></span>

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* <span data-ttu-id="0d308-442">Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="0d308-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="0d308-443">Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate oturumları) başlatılmasıdır.</span><span class="sxs-lookup"><span data-stu-id="0d308-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="0d308-444">Yeni bir kullanıcı bir tarayıcıda kaydedin (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="0d308-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="0d308-445">Her bir tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="0d308-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="0d308-446">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="0d308-446">Verify the following operations:</span></span>

* <span data-ttu-id="0d308-447">Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="0d308-448">Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="0d308-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="0d308-449">Yöneticileri Onayla/kişi verilerini reddet.</span><span class="sxs-lookup"><span data-stu-id="0d308-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="0d308-450">`Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="0d308-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="0d308-451">Yöneticiler Onayla/Reddet ve tüm verileri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="0d308-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="0d308-452">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="0d308-452">User</span></span>                | <span data-ttu-id="0d308-453">Uygulama tarafından sağlanmış</span><span class="sxs-lookup"><span data-stu-id="0d308-453">Seeded by the app</span></span> | <span data-ttu-id="0d308-454">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="0d308-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="0d308-455">Hayır</span><span class="sxs-lookup"><span data-stu-id="0d308-455">No</span></span>                | <span data-ttu-id="0d308-456">Kendi veri düzenleme veya silme.</span><span class="sxs-lookup"><span data-stu-id="0d308-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="0d308-457">Evet</span><span class="sxs-lookup"><span data-stu-id="0d308-457">Yes</span></span>               | <span data-ttu-id="0d308-458">Onayla/Reddet ve kendi veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="0d308-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="0d308-459">Evet</span><span class="sxs-lookup"><span data-stu-id="0d308-459">Yes</span></span>               | <span data-ttu-id="0d308-460">Onayla/Reddet ve tüm verileri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="0d308-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="0d308-461">Bir kişi, yöneticinin tarayıcıda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0d308-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="0d308-462">Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="0d308-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="0d308-463">Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="0d308-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="0d308-464">Başlangıç uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="0d308-464">Create the starter app</span></span>

* <span data-ttu-id="0d308-465">"ContactManager" adlı bir Razor sayfaları uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d308-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="0d308-466">Uygulamayı oluşturma **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="0d308-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="0d308-467">Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.</span><span class="sxs-lookup"><span data-stu-id="0d308-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="0d308-468">`-uld` LocalDB yerine SQLite belirtir</span><span class="sxs-lookup"><span data-stu-id="0d308-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="0d308-469">*Modeller/Ilgili kişi ekle. cs*:</span><span class="sxs-lookup"><span data-stu-id="0d308-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="0d308-470">İskele `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="0d308-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="0d308-471">İlk geçiş oluşturun ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="0d308-471">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="0d308-472">Güncelleştirme **ContactManager** içinde yer işareti *Pages/_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0d308-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="0d308-473">Oluşturma, düzenleme ve silme kişi uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="0d308-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="0d308-474">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d308-474">Seed the database</span></span>

<span data-ttu-id="0d308-475">Ekleme [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) sınıfının *veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="0d308-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="0d308-476">Çağrı `SeedData.Initialize` gelen `Main`:</span><span class="sxs-lookup"><span data-stu-id="0d308-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="0d308-477">Test, uygulama veritabanını sağlanmış.</span><span class="sxs-lookup"><span data-stu-id="0d308-477">Test that the app seeded the database.</span></span> <span data-ttu-id="0d308-478">DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="0d308-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="0d308-479">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0d308-479">Additional resources</span></span>

* [<span data-ttu-id="0d308-480">Azure App Service'te .NET Core ve SQL veritabanı web uygulaması derleme</span><span class="sxs-lookup"><span data-stu-id="0d308-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="0d308-481">[ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="0d308-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="0d308-482">Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="0d308-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="0d308-483">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="0d308-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
