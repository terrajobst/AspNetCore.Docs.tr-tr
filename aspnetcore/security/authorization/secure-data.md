---
title: "Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 7404b8ec20ed6a00554c8a7ade9a282362b9a186
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="59e82-102">Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="59e82-102">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="59e82-103">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="59e82-103">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="59e82-104">Bu öğretici yetkilendirme tarafından korunan kullanıcı verileri ile bir web uygulaması oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="59e82-104">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="59e82-105">Kimliği doğrulanmış (kayıtlı) kullanıcılar Kişiler listesini görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="59e82-105">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="59e82-106">Üç güvenlik grupları vardır:</span><span class="sxs-lookup"><span data-stu-id="59e82-106">There are three security groups:</span></span>

* <span data-ttu-id="59e82-107">Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="59e82-107">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="59e82-108">Kayıtlı kullanıcıların düzenleme/kullanıcıların kendi verilerini silme.</span><span class="sxs-lookup"><span data-stu-id="59e82-108">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="59e82-109">Yöneticileri onaylayın veya kişi verilerini reddedin.</span><span class="sxs-lookup"><span data-stu-id="59e82-109">Managers can approve or reject contact data.</span></span> <span data-ttu-id="59e82-110">Yalnızca onaylanan kişilere kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="59e82-110">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="59e82-111">Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="59e82-111">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="59e82-112">Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) imzalanır.</span><span class="sxs-lookup"><span data-stu-id="59e82-112">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="59e82-113">Kullanıcı Rick can yalnızca görünümü kişiler onaylanmış ve kendi kişiler düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="59e82-113">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="59e82-114">Yalnızca son kaydı Rick tarafından oluşturulan, düzenleme görüntüler ve bağlantıları Sil</span><span class="sxs-lookup"><span data-stu-id="59e82-114">Only the last record, created by Rick, displays edit and delete links</span></span>

![Yukarıda açıklanan görüntüsü](secure-data/_static/rick.png)

<span data-ttu-id="59e82-116">Aşağıdaki görüntüde `manager@contoso.com` ve yöneticileri rolünde imzalanır.</span><span class="sxs-lookup"><span data-stu-id="59e82-116">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![Yukarıda açıklanan görüntüsü](secure-data/_static/manager1.png)

<span data-ttu-id="59e82-118">Aşağıdaki resimde yöneticileri kişi Ayrıntılar görünümünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="59e82-118">The following image shows the  managers details view of a contact.</span></span>

![Yukarıda açıklanan görüntüsü](secure-data/_static/manager.png)

<span data-ttu-id="59e82-120">Yalnızca Yöneticiler ve yöneticilerin Onayla sahip ve düğmeleri reddedin.</span><span class="sxs-lookup"><span data-stu-id="59e82-120">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="59e82-121">Aşağıdaki görüntüde `admin@contoso.com` ve yöneticinin rolünde imzalanır.</span><span class="sxs-lookup"><span data-stu-id="59e82-121">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![Yukarıda açıklanan görüntüsü](secure-data/_static/admin.png)

<span data-ttu-id="59e82-123">Yönetici, tüm ayrıcalıklarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="59e82-123">The administrator has all privileges.</span></span> <span data-ttu-id="59e82-124">Depoladığından, herhangi bir kişiyi okuma/düzenleme/silme yapabilir ve kişiler durumunu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="59e82-124">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="59e82-125">Uygulama tarafından oluşturulan [iskele](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) aşağıdaki `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="59e82-125">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="59e82-126">A `ContactIsOwnerAuthorizationHandler` yetkilendirme işleyicisi sağlar kullanıcı verilerine yalnızca düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="59e82-126">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="59e82-127">A `ContactManagerAuthorizationHandler` yetkilendirme işleyici onaylamak veya reddetmek kişiler yöneticileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="59e82-127">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="59e82-128">A `ContactAdministratorsAuthorizationHandler` yetkilendirme işleyicisini yöneticilerinin onaylama veya reddetme kişiler ve kişi düzenleme/silme sağlar.</span><span class="sxs-lookup"><span data-stu-id="59e82-128">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="59e82-129">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="59e82-129">Prerequisites</span></span>

<span data-ttu-id="59e82-130">Bu bir başlangıç Öğreticisi değildir.</span><span class="sxs-lookup"><span data-stu-id="59e82-130">This isn't a beginning tutorial.</span></span> <span data-ttu-id="59e82-131">Hakkında bilginiz olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="59e82-131">You should be familiar with:</span></span>

* [<span data-ttu-id="59e82-132">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="59e82-132">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="59e82-133">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="59e82-133">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="59e82-134">Tamamlanan uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="59e82-134">The starter and completed app</span></span>

<span data-ttu-id="59e82-135">[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) uygulama.</span><span class="sxs-lookup"><span data-stu-id="59e82-135">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="59e82-136">[Test](#test-the-completed-app) tamamlanan uygulama ile güvenlik özellikleri hakkında bilgi sahibi olursunuz.</span><span class="sxs-lookup"><span data-stu-id="59e82-136">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="59e82-137">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="59e82-137">The starter app</span></span>

<span data-ttu-id="59e82-138">Tamamlanan örnek ile kodunuzu karşılaştırmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="59e82-138">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="59e82-139">[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) uygulama.</span><span class="sxs-lookup"><span data-stu-id="59e82-139">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="59e82-140">Bkz: [başlangıç uygulaması oluşturma](#create-the-starter-app) sıfırdan oluşturmak istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="59e82-140">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="59e82-141">Veritabanını güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="59e82-141">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="59e82-142">Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="59e82-142">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="59e82-143">Bu öğretici güvenli kullanıcı veri uygulaması oluşturmak için tüm ana adım vardır.</span><span class="sxs-lookup"><span data-stu-id="59e82-143">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="59e82-144">Tamamlanan projesine başvurması daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="59e82-144">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="59e82-145">Kullanıcı verileri güvenli hale getirmek için uygulama değiştirme</span><span class="sxs-lookup"><span data-stu-id="59e82-145">Modify the app to secure user data</span></span>

<span data-ttu-id="59e82-146">Aşağıdaki bölümlerde güvenli kullanıcı veri uygulaması oluşturmak için tüm önemli adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="59e82-146">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="59e82-147">Tamamlanan projesine başvurması daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="59e82-147">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="59e82-148">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="59e82-148">Tie the contact data to the user</span></span>

<span data-ttu-id="59e82-149">ASP.NET kullanan [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerini, ancak diğer kullanıcılar verileri düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="59e82-149">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="59e82-150">Ekleme `OwnerID` için `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="59e82-150">Add `OwnerID` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="59e82-151">`OwnerID`kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="59e82-151">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="59e82-152">`Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="59e82-152">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="59e82-153">Yeni bir geçişin iskelesini kurun ve veritabanı güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="59e82-153">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="59e82-154">SSL ve kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="59e82-154">Require SSL and authenticated users</span></span>

<span data-ttu-id="59e82-155">İçinde `ConfigureServices` yöntemi *haline* dosya, ekleme [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) yetkilendirme Filtresi:</span><span class="sxs-lookup"><span data-stu-id="59e82-155">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

<span data-ttu-id="59e82-156">HTTPS için HTTP isteklerini yeniden yönlendirmek için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="59e82-156">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="59e82-157">Visual Studio Code kullanarak veya bir test sertifikası için SSL içermeyen yerel platformunda sınama istiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="59e82-157">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="59e82-158">Ayarlama `"LocalTest:skipSSL": true` içinde *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="59e82-158">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="59e82-159">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="59e82-159">Require authenticated users</span></span>

<span data-ttu-id="59e82-160">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="59e82-160">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="59e82-161">Denetleyici veya eylem yöntemi ile kimlik doğrulama dışında seçebilirsiniz `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="59e82-161">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="59e82-162">Bu yaklaşımda, eklenen her yeni denetleyicileri otomatik olarak eklemek için yeni denetleyicilerinde bağlı olan daha güvenli kimlik doğrulama gerektirecek `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="59e82-162">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="59e82-163">Aşağıdakileri ekleyin `ConfigureServices` yöntemi *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="59e82-163">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

<span data-ttu-id="59e82-164">Ekleme `[AllowAnonymous]` bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri alabilmek için giriş denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="59e82-164">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="59e82-165">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="59e82-165">Configure the test account</span></span>

<span data-ttu-id="59e82-166">`SeedData` Sınıfı, iki hesap, yönetici ve Yöneticisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="59e82-166">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="59e82-167">Kullanım [gizli Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="59e82-167">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="59e82-168">Proje dizinden bunu (dizinini içeren *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="59e82-168">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="59e82-169">Güncelleştirme `Configure` test parola kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="59e82-169">Update `Configure` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

<span data-ttu-id="59e82-170">Yönetici kullanıcı kimliği ekleyin ve `Status = ContactStatus.Approved` kişilere.</span><span class="sxs-lookup"><span data-stu-id="59e82-170">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="59e82-171">Yalnızca bir kişinin gösterilen tüm kişiler için kullanıcı kimliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="59e82-171">Only one contact is shown, add the user ID to all contacts:</span></span>

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="59e82-172">Sahibi, Yöneticisi ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="59e82-172">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="59e82-173">Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="59e82-173">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="59e82-174">`ContactIsOwnerAuthorizationHandler` Kaynak üzerinde hareket kullanıcının sahip olduğu kaynak doğrular.</span><span class="sxs-lookup"><span data-stu-id="59e82-174">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="59e82-175">`ContactIsOwnerAuthorizationHandler` Çağrıları `context.Succeed` geçerli kimliği doğrulanmış kullanıcı kişi sahibi ise.</span><span class="sxs-lookup"><span data-stu-id="59e82-175">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="59e82-176">Yetkilendirme işleyicileri genellikle dönmek `context.Succeed` zaman gereksinimleri karşılandıktan.</span><span class="sxs-lookup"><span data-stu-id="59e82-176">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="59e82-177">Döndürmeleri `Task.FromResult(0)` zaman gereksinimleri karşılanmadığı.</span><span class="sxs-lookup"><span data-stu-id="59e82-177">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="59e82-178">`Task.FromResult(0)`ne başarılı veya başarısız olan diğer yetkilendirme işleyici çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="59e82-178">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="59e82-179">Açıkça başarısız gerekiyorsa, dönüş `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="59e82-179">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="59e82-180">Böylece biz gereksinim parametrede aktarılan işlemi denetleyin gerek kalmaz, kendi veri düzenleme/silme kişi sahipleri izin veriyoruz.</span><span class="sxs-lookup"><span data-stu-id="59e82-180">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="59e82-181">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="59e82-181">Create a manager authorization handler</span></span>

<span data-ttu-id="59e82-182">Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="59e82-182">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="59e82-183">`ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="59e82-183">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="59e82-184">Yalnızca Yöneticiler onaylayabilir veya reddedebilirsiniz içerik değişikliklerini (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="59e82-184">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="59e82-185">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="59e82-185">Create an administrator authorization handler</span></span>

<span data-ttu-id="59e82-186">Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="59e82-186">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="59e82-187">`ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="59e82-187">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="59e82-188">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="59e82-188">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="59e82-189">Yetkilendirme işleyicileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="59e82-189">Register the authorization handlers</span></span>

<span data-ttu-id="59e82-190">Entity Framework Çekirdek kullanarak Hizmetleri kayıtlı, için [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="59e82-190">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="59e82-191">`ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="59e82-191">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="59e82-192">Kullanılabilir olacak işleyicileri hizmet Koleksiyonuyla Kaydet `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="59e82-192">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="59e82-193">Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="59e82-193">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

<span data-ttu-id="59e82-194">`ContactAdministratorsAuthorizationHandler`ve `ContactManagerAuthorizationHandler` teklileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="59e82-194">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="59e82-195">Olup olmadıklarını teklileri EF kullanmayın ve gerekli tüm bilgileri de olduğundan `Context` parametresinin `HandleRequirementAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="59e82-195">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="59e82-196">Tam `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="59e82-196">The complete `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="59e82-197">Yetkilendirmeyi desteklemek için kodu güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="59e82-197">Update the code to support authorization</span></span>

<span data-ttu-id="59e82-198">Bu bölümde, denetleyici ve görünüm güncelleştirin ve işlemleri gereksinimleri sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="59e82-198">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="59e82-199">Güncelleştirme kişiler denetleyicisi</span><span class="sxs-lookup"><span data-stu-id="59e82-199">Update the Contacts controller</span></span>

<span data-ttu-id="59e82-200">Güncelleştirme `ContactsController` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="59e82-200">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="59e82-201">Ekleme `IAuthorizationService` yetkilendirme işleyicilerine erişmek için hizmet.</span><span class="sxs-lookup"><span data-stu-id="59e82-201">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="59e82-202">Ekleme `Identity` `UserManager` hizmeti:</span><span class="sxs-lookup"><span data-stu-id="59e82-202">Add the `Identity` `UserManager` service:</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="59e82-203">Bir kişi operations gereksinimleri sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="59e82-203">Add a contact operations requirements class</span></span>

<span data-ttu-id="59e82-204">Ekleme `ContactOperations` sınıfının *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="59e82-204">Add the `ContactOperations` class to the *Authorization* folder.</span></span> <span data-ttu-id="59e82-205">Bu sınıf içeren gereksinimleri bizim uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="59e82-205">This class  contain the requirements our app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a><span data-ttu-id="59e82-206">Güncelleştirme oluşturma</span><span class="sxs-lookup"><span data-stu-id="59e82-206">Update Create</span></span>

<span data-ttu-id="59e82-207">Güncelleştirme `HTTP POST Create` yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="59e82-207">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="59e82-208">Kullanıcı kimliği eklemek `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="59e82-208">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="59e82-209">Kullanıcının kişinin sahip olduğu doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="59e82-209">Call the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a><span data-ttu-id="59e82-210">Güncelleştirmeyi Düzenle</span><span class="sxs-lookup"><span data-stu-id="59e82-210">Update Edit</span></span>

<span data-ttu-id="59e82-211">Her ikisi de güncelleştirme `Edit` kullanıcıyı doğrulamak için yetkilendirme işleyicisi kullanmak için yöntemleri sahibi ile iletişime geçin.</span><span class="sxs-lookup"><span data-stu-id="59e82-211">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="59e82-212">Kaynak Yetkilendirme performans gösterdiğini biz kullanamazsınız, çünkü `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="59e82-212">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="59e82-213">Öznitelikleri değerlendirildiğinde biz kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="59e82-213">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="59e82-214">Bağlı kaynak yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="59e82-214">Resource based authorization must be imperative.</span></span> <span data-ttu-id="59e82-215">Bizim denetleyicisi yüklenirken, veya göre işleyici içinde yükleme kaynağına erişimi edindikten sonra denetimlerinin gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="59e82-215">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="59e82-216">Sık sık kaynak anahtarı geçirerek kaynak erişir.</span><span class="sxs-lookup"><span data-stu-id="59e82-216">Frequently you will access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a><span data-ttu-id="59e82-217">Güncelleştirme Delete yöntemi</span><span class="sxs-lookup"><span data-stu-id="59e82-217">Update the Delete method</span></span>

<span data-ttu-id="59e82-218">Her ikisi de güncelleştirme `Delete` kullanıcıyı doğrulamak için yetkilendirme işleyicisi kullanmak için yöntemleri sahibi ile iletişime geçin.</span><span class="sxs-lookup"><span data-stu-id="59e82-218">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="59e82-219">Yetkilendirme hizmeti görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="59e82-219">Inject the authorization service into the views</span></span>

<span data-ttu-id="59e82-220">Şu anda UI gösterir düzenleyebilir ve kullanıcı değiştirilemiyor veri bağlantıları silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="59e82-220">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="59e82-221">Biz, görünümlere yetkilendirme işleyici uygulayarak düzeltmesi.</span><span class="sxs-lookup"><span data-stu-id="59e82-221">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="59e82-222">Yetkilendirme hizmetinde Ekle *Views/_ViewImports.cshtml* tüm görünümler kullanılabilir olacak şekilde dosya:</span><span class="sxs-lookup"><span data-stu-id="59e82-222">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

<span data-ttu-id="59e82-223">Güncelleştirme *Views/Contacts/Index.cshtml* yalnızca düzenleme görüntüleme ve düzenleme / kişi silme kullanıcılar için bağlantıları silmek için Razor görünüm.</span><span class="sxs-lookup"><span data-stu-id="59e82-223">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="59e82-224">Ekleme`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="59e82-224">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="59e82-225">Güncelleştirme `Edit` ve `Delete` düzenlemek ve kişiyi silmek izne sahip kullanıcılar yalnızca çizilir şekilde bağlar.</span><span class="sxs-lookup"><span data-stu-id="59e82-225">Update the `Edit` and `Delete` links so they're only rendered for users with permission to edit and delete the contact.</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

<span data-ttu-id="59e82-226">Uyarı: Düzen veya veri silmek için izne sahip olmayan kullanıcılardan gelen bağlantılar gizleme uygulama güvenli değil.</span><span class="sxs-lookup"><span data-stu-id="59e82-226">Warning: Hiding links from users that don't have permission to edit or delete data doesn't secure the app.</span></span> <span data-ttu-id="59e82-227">Bağlantılar gizleme uygulama daha fazla kullanıcı kolay yalnızca geçerli bağlantılar görüntüleyerek yapar.</span><span class="sxs-lookup"><span data-stu-id="59e82-227">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="59e82-228">Kullanıcıları düzenleme çağırma ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'lerini korsan saldırılarına.</span><span class="sxs-lookup"><span data-stu-id="59e82-228">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="59e82-229">Denetleyici güvenli olması için erişim denetimleri yinelemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="59e82-229">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="59e82-230">Güncelleştirme ayrıntıları görünümü</span><span class="sxs-lookup"><span data-stu-id="59e82-230">Update the Details view</span></span>

<span data-ttu-id="59e82-231">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="59e82-231">Update the details view so managers can approve or reject contacts:</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a><span data-ttu-id="59e82-232">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="59e82-232">Test the completed app</span></span>

<span data-ttu-id="59e82-233">Visual Studio Code kullanarak veya bir test sertifikası için SSL içermeyen yerel platformunda sınama istiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="59e82-233">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="59e82-234">Ayarlama `"LocalTest:skipSSL": true` içinde *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="59e82-234">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="59e82-235">Uygulama çalıştıran ve kişiler varsa, tüm kayıtları silin `Contact` tablo ve veritabanı oluşturmak için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="59e82-235">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="59e82-236">Visual Studio kullanıyorsanız, çıkmak ve IIS Express çekirdek veritabanı yeniden başlatmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="59e82-236">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="59e82-237">Bir kullanıcı kişiler göz atmak için kaydolun.</span><span class="sxs-lookup"><span data-stu-id="59e82-237">Register a user to browse the contacts.</span></span>

<span data-ttu-id="59e82-238">Üç farklı tarayıcılar (veya ıncognito/InPrivate sürümler) başlatmak için tamamlanan uygulama test etmek için kolay bir yol değil.</span><span class="sxs-lookup"><span data-stu-id="59e82-238">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="59e82-239">Bir tarayıcıda, örneğin, yeni bir kullanıcı kaydı `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="59e82-239">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="59e82-240">Her tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="59e82-240">Sign in to each browser with a different user.</span></span> <span data-ttu-id="59e82-241">Aşağıdakileri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="59e82-241">Verify the following:</span></span>

* <span data-ttu-id="59e82-242">Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="59e82-242">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="59e82-243">Kayıtlı kullanıcıların düzenleme/kullanıcıların kendi verilerini silme.</span><span class="sxs-lookup"><span data-stu-id="59e82-243">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="59e82-244">Yöneticileri onaylayın veya kişi verilerini reddedin.</span><span class="sxs-lookup"><span data-stu-id="59e82-244">Managers can approve or reject contact data.</span></span> <span data-ttu-id="59e82-245">`Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeler.</span><span class="sxs-lookup"><span data-stu-id="59e82-245">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="59e82-246">Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="59e82-246">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="59e82-247">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="59e82-247">User</span></span>| <span data-ttu-id="59e82-248">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="59e82-248">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="59e82-249">Düzenle/kendi veri silmek</span><span class="sxs-lookup"><span data-stu-id="59e82-249">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="59e82-250">Onayla/Reddet ve düzenleme/silme veri sahip olabilir</span><span class="sxs-lookup"><span data-stu-id="59e82-250">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="59e82-251">Düzenleme/silme ve tüm veri Onayla/Reddet kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="59e82-251">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="59e82-252">Bir kişi Yöneticiler tarayıcıda oluşturun.</span><span class="sxs-lookup"><span data-stu-id="59e82-252">Create a contact in the administrators browser.</span></span> <span data-ttu-id="59e82-253">Delete URL'sini kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="59e82-253">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="59e82-254">Bu bağlantıları test kullanıcısı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcıya yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="59e82-254">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="59e82-255">Başlangıç uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="59e82-255">Create the starter app</span></span>

<span data-ttu-id="59e82-256">Başlangıç uygulaması oluşturmak için bu yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="59e82-256">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="59e82-257">Oluşturma bir **ASP.NET çekirdek Web uygulaması** kullanarak [Visual Studio 2017](https://www.visualstudio.com/) "ContactManager" adlı</span><span class="sxs-lookup"><span data-stu-id="59e82-257">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="59e82-258">Uygulama oluşturma **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="59e82-258">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="59e82-259">Ad alanınız örnek ad alanı kullanımda eşleşecek şekilde "ContactManager" adı.</span><span class="sxs-lookup"><span data-stu-id="59e82-259">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="59e82-260">Aşağıdakileri ekleyin `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="59e82-260">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="59e82-261">İskele `Contact` Entity Framework Çekirdek modelini ve `ApplicationDbContext` veri bağlamı.</span><span class="sxs-lookup"><span data-stu-id="59e82-261">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="59e82-262">Tüm yapı iskelesi Varsayılanları kabul edin.</span><span class="sxs-lookup"><span data-stu-id="59e82-262">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="59e82-263">Kullanarak `ApplicationDbContext` veri bağlamı sınıfı kişi tablo koyar [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="59e82-263">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="59e82-264">Bkz: [bir modeli ekleme](xref:tutorials/first-mvc-app/adding-model) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="59e82-264">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="59e82-265">Güncelleştirme **ContactManager** içinde yer alan bağlayıcı *Views/Shared/_Layout.cshtml* dosya `asp-controller="Home"` için `asp-controller="Contacts"` şekilde dokunarak **ContactManager** bağlantı Kişiler denetleyicisi çağırır.</span><span class="sxs-lookup"><span data-stu-id="59e82-265">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="59e82-266">Özgün biçimlendirme:</span><span class="sxs-lookup"><span data-stu-id="59e82-266">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="59e82-267">Güncelleştirilmiş biçimlendirme:</span><span class="sxs-lookup"><span data-stu-id="59e82-267">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="59e82-268">İlk geçişin iskelesi ve veritabanı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="59e82-268">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="59e82-269">Oluşturma, düzenleme ve bir kişinin silinmesi uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="59e82-269">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="59e82-270">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="59e82-270">Seed the database</span></span>

<span data-ttu-id="59e82-271">Ekleme `SeedData` sınıfının *veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="59e82-271">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="59e82-272">Örnek indirdiğiniz, kopyalayabilirsiniz *SeedData.cs* dosya *veri* başlangıç projesinin klasör.</span><span class="sxs-lookup"><span data-stu-id="59e82-272">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

<span data-ttu-id="59e82-273">Vurgulanmış kodu sonuna ekleyin `Configure` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="59e82-273">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

<span data-ttu-id="59e82-274">Uygulama veritabanı sağlanmış sınayın.</span><span class="sxs-lookup"><span data-stu-id="59e82-274">Test that the app seeded the database.</span></span> <span data-ttu-id="59e82-275">Seed yöntemi kişi DB herhangi bir satır varsa çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="59e82-275">The seed method doesn't run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="59e82-276">Öğreticide kullanılan sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="59e82-276">Create a class used in the tutorial</span></span>

* <span data-ttu-id="59e82-277">Adlı bir klasör oluşturun *yetkilendirme*.</span><span class="sxs-lookup"><span data-stu-id="59e82-277">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="59e82-278">Kopya *Authorization\ContactOperations.cs* dosya projeyi Merkezi'nden veya aşağıdaki kodu kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="59e82-278">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="59e82-279">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="59e82-279">Additional resources</span></span>

* <span data-ttu-id="59e82-280">[ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="59e82-280">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="59e82-281">Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="59e82-281">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="59e82-282">ASP.NET Core yetkilendirme: basit, rol, talep tabanlı ve özel</span><span class="sxs-lookup"><span data-stu-id="59e82-282">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="59e82-283">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="59e82-283">Custom policy-based authorization</span></span>](policies.md)
