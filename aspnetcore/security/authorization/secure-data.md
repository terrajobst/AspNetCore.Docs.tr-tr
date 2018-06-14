---
title: Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma
author: rick-anderson
description: Kullanıcı veri yetkilendirme tarafından korunan bir Razor sayfalarının uygulaması oluşturmayı öğrenin. HTTPS, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir.
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 0b67d4aef198aa418b54fb92db76d331ffa2785a
ms.sourcegitcommit: 0d6f151e69c159d776ed0142773279e645edbc0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2018
ms.locfileid: "35415039"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)

Bu öğretici bir ASP.NET Core web uygulamasının yetkilendirme tarafından korunan kullanıcı verileri ile nasıl oluşturulacağını gösterir. Kimliği doğrulanmış (kayıtlı) kullanıcılar Kişiler listesini görüntüler oluşturdunuz. Üç güvenlik grupları vardır:

* **Kayıtlı kullanıcıların** tüm onaylanmış verilerini görüntüleyebilir ve düzenleme/kullanıcıların kendi verilerini silme.
* **Yöneticileri** onaylayabilir veya kişi verilerini reddedebilirsiniz. Yalnızca onaylanan kişilere kullanıcılar tarafından görülebilir.
* **Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.

Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) imzalanır. Rick yalnızca onaylanan kişilere görüntülemek ve **Düzenle**/**silmek**/**Yeni Oluştur** kendi kişiler için bağlantıları. Yalnızca son kaydı görüntüler Rick tarafından oluşturulan **Düzenle** ve **silmek** bağlantılar. Bir yöneticinin "Onaylandı" durumuna kadar diğer kullanıcılar son kaydı görmez.

![Önceki görüntü açıklanan](secure-data/_static/rick.png)

Aşağıdaki görüntüde `manager@contoso.com` ve yöneticileri rolünde imzalı:

![Önceki görüntü açıklanan](secure-data/_static/manager1.png)

Aşağıdaki resimde yöneticileri Ayrıntılar görünümü bir iletişim gösterilmiştir:

![Önceki görüntü açıklanan](secure-data/_static/manager.png)

**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.

Aşağıdaki görüntüde `admin@contoso.com` ve yöneticiler rolünde imzalı:

![Önceki görüntü açıklanan](secure-data/_static/admin.png)

Yönetici, tüm ayrıcalıklarına sahiptir. Depoladığından, herhangi bir kişiyi okuma/düzenleme/silme yapabilir ve kişiler durumunu değiştirebilirsiniz.

Uygulama tarafından oluşturulan [iskele](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) aşağıdaki `Contact` modeli:

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

Örnek aşağıdaki yetkilendirme işleyicileri içerir:

* `ContactIsOwnerAuthorizationHandler`: Bir kullanıcı yalnızca kendi veri düzenleme sağlar.
* `ContactManagerAuthorizationHandler`: Onaylamak veya reddetmek kişiler yöneticileri sağlar.
* `ContactAdministratorsAuthorizationHandler`: Yöneticilerinin onaylama veya reddetme kişiler ve kişi düzenleme/silme sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici Gelişmiş. Hakkında bilginiz olması gerekir:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Kimlik Doğrulaması](xref:security/authentication/index)
* [Hesap Onaylama ve Parola Kurtarma](xref:security/authentication/accconfirm)
* [Yetkilendirme](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Bkz: [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC sürüm için. Bu öğretici ASP.NET Core 1.1 sürümü [bu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) klasör. ASP.NET Core örnek konusu 1.1 [örnekleri](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

## <a name="the-starter-and-completed-app"></a>Tamamlanan uygulama ve başlangıç

[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) uygulama. [Test](#test-the-completed-app) tamamlanan uygulama ile güvenlik özellikleri hakkında bilgi sahibi olursunuz.

### <a name="the-starter-app"></a>Başlangıç uygulaması

[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) uygulama.

Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.

## <a name="secure-user-data"></a>Kullanıcı verileri güvenli

Aşağıdaki bölümlerde güvenli kullanıcı veri uygulaması oluşturmak için tüm önemli adımlar vardır. Tamamlanan projesine başvurması daha yararlı olabilir.

### <a name="tie-the-contact-data-to-the-user"></a>Kullanıcı kişi verilerini bağlayın

ASP.NET kullanan [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerini, ancak diğer kullanıcılar verileri düzenleyebilirsiniz. Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı. `Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.

Yeni geçiş oluştur ve veritabanı güncelleştirme:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a>HTTPS ve kimliği doğrulanmış kullanıcılar gerektirir

Ekleme [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) için `Startup`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

İçinde `ConfigureServices` yöntemi *haline* dosya, ekleme [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) yetkilendirme Filtresi:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

Visual Studio kullanıyorsanız, HTTPS etkinleştirin.

HTTPS için HTTP isteklerini yeniden yönlendirmek için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting). Visual Studio Code kullanarak ya da yerel bir platformda sınama, HTTPS için bir test sertifikası içermez:

  Ayarlama `"LocalTest:skipHTTPS": true` içinde *appsettings. Developement.JSON* dosya.

### <a name="require-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar gerektirir

Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesini ayarlayın. Kimlik doğrulaması ile Razor sayfasını, denetleyici veya eylem yöntemi düzeyinde dışında seçebilirsiniz `[AllowAnonymous]` özniteliği. Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfalarının ve denetleyiciler korur. Kimlik doğrulaması varsayılan olarak gerekli yeni denetleyicileri ve dahil etmek için Razor sayfalarının bağlı olan daha güvenli olan `[Authorize]` özniteliği. 

Kimliği doğrulanmış, tüm kullanıcılar gereksinimiyle [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ve [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) çağrıları gerekli değildir.

Güncelleştirme `ConfigureServices` aşağıdaki değişiklikleri:

* Out açıklama `AuthorizeFolder` ve `AuthorizePage`.
* Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesini ayarlayın.

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

Ekleme [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) dizine, bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri sınıflandırıp hakkında ve iletişim sayfaları. 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

Ekleme `[AllowAnonymous]` için [LoginModel ve RegisterModel](https://github.com/aspnet/templating/issues/238).

### <a name="configure-the-test-account"></a>Test hesabı yapılandırın

`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve Yöneticisi. Kullanım [gizli Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlayabilirsiniz. Proje dizinden parola ayarlayın (dizinini içeren *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Güncelleştirme `Main` test parola kullanmak için:

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Test hesapları oluşturabilir ve kişileri güncelleştir

Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturmak için sınıfı:

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

Yönetici kullanıcı kimliği ekleyin ve `ContactStatus` kişilere. "Gönderildi" ve bir "Reddedilen" kişilerden biri olun. Kullanıcı kimliği ve durum tüm kişileri ekleyin. Yalnızca bir kişinin gösterilir:

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Sahibi, Yöneticisi ve yönetici yetkilendirme işleyicileri oluşturma

Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör. `ContactIsOwnerAuthorizationHandler` Bir kaynakta hareket kullanıcı kaynağın sahibi doğrular.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) geçerli kimliği doğrulanmış kullanıcı kişi sahibi ise. Yetkilendirme işleyicileri genellikle:

* Dönüş `context.Succeed` zaman gereksinimleri karşılandıktan.
* Dönüş `Task.CompletedTask` zaman olmayan gereksinimlerin karşılanması. `Task.CompletedTask` ne başarılı veya başarısız olduğunu&mdash;çalıştırmak diğer yetkilendirme işleyicileri sağlar.

Açıkça başarısız gerekiyorsa, dönüş [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Uygulamanın kendi veri düzenleme/silme/oluşturmak için kişi sahipleri verir. `ContactIsOwnerAuthorizationHandler` gereksinim parametrede aktarılan işlemi denetlemek gerekmez.

### <a name="create-a-manager-authorization-handler"></a>Bir yönetici yetkilendirme işleyicisi oluşturun

Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör. `ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrular. Yalnızca Yöneticiler onaylayabilir veya reddedebilirsiniz içerik değişikliklerini (yeni veya değiştirilmiş).

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Bir yönetici yetkilendirme işleyicisi oluşturun

Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör. `ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrular. Yönetici, tüm işlemleri yapabilir.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Yetkilendirme işleyicileri kaydetme

Entity Framework Çekirdek kullanarak Hizmetleri kayıtlı, için [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulmuştur. Kullanılabilir hizmet koleksiyonuyla işleyicileri kaydolmayı `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection). Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` teklileri eklenir. Olup olmadıklarını teklileri EF kullanmayın ve gerekli tüm bilgileri de olduğundan `Context` parametresinin `HandleRequirementAsync` yöntemi.

## <a name="support-authorization"></a>Destek yetkilendirme

Bu bölümde, Razor sayfalarının güncelleştirin ve işlemleri gereksinimleri sınıf ekleyin.

### <a name="review-the-contact-operations-requirements-class"></a>Gözden geçirme kişi operations gereksinimleri sınıfı

Gözden geçirme `ContactOperations` sınıfı. Bu sınıf gereksinimlerini içeren uygulama destekler:

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>Razor sayfalar için temel sınıf oluşturma

Kişiler Razor sayfalarının kullanılan hizmetleri içeren temel bir sınıf oluşturun. Taban sınıfı başlatma kodun bir konuma yerleştirir:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

Önceki kod:

* Ekler `IAuthorizationService` yetkilendirme işleyicilerine erişmek için hizmet.
* Kimlik ekler `UserManager` hizmet.
* Ekleme `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Güncelleştirme CreateModel

Kullanılacak Oluştur sayfası modeli Oluşturucusu güncelleştirme `DI_BasePageModel` temel sınıfı:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Güncelleştirme `CreateModel.OnPostAsync` yöntemi için:

* Kullanıcı kimliği eklemek `Contact` modeli.
* Kullanıcının kişiler oluşturmak için izne sahip doğrulamak için yetkilendirme işleyici çağırın.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Güncelleştirme IndexModel

Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilen şekilde yöntemi:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Güncelleştirme EditModel

Kullanıcının kişinin sahip olduğu doğrulamak için bir yetkilendirme işleyici ekleyin. Kaynak Yetkilendirme doğrulandığı için `[Authorize]` özniteliği yeterli değil. Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok. Kaynak tabanlı bir yetkilendirme kesinlik temelli olması gerekir. Uygulama sayfası modelinde yükleme ya da işleyici içinde yükleme kaynağına erişimi olduğunda denetimlerinin gerçekleştirilmesi gerekir. Sık sık kaynak anahtarı geçirerek kaynak erişir.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Güncelleştirme DeleteModel

Kullanıcı, kişi hakkında silme iznine sahip doğrulamak için yetkilendirme işleyicisi kullanmak için silme sayfa modeli güncelleştirin.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Yetkilendirme hizmeti görünümlere ekleme

Şu anda, UI gösterir düzenleyebilir ve kullanıcı değiştirilemiyor veri bağlantıları silebilirsiniz. Kullanıcı arabirimini görünümlerine yetkilendirme işleyici uygulayarak düzeltilmiştir.

Yetkilendirme hizmetinde Ekle *Views/_ViewImports.cshtml* tüm görünümler kullanılabilir olacak şekilde dosya:

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

Önceki biçimlendirme birkaç ekler `using` deyimleri.

Güncelleştirme **Düzenle** ve **silmek** içinde bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar için işlenen için:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> Veri değiştirme iznine sahip olmayan kullanıcılardan gelen bağlantılar gizleme uygulama güvenli değil. Bağlantılar gizleme uygulama daha kullanıcı dostu yalnızca geçerli bağlantılar görüntüleyerek yapmaz. Kullanıcıları düzenleme çağırma ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'lerini korsan saldırılarına. Razor sayfasını veya denetleyicisi verileri korumak için erişim denetimleri zorunlu gerekir.

### <a name="update-details"></a>Güncelleştirme ayrıntıları

Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler şekilde güncelleştirin:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

Ayrıntılar sayfası modeli güncelleştirin:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>Tamamlanmış uygulamayı test etme

Visual Studio Code kullanarak ya da yerel bir platformda sınama, HTTPS için bir test sertifikası içermez:

* Ayarlama `"LocalTest:skipHTTPS": true` içinde *appsettings. Developement.JSON* HTTPS gereksinim atlamak için dosya. Bir geliştirme makinede yalnızca HTTPS atla.

Uygulamanın kişiler varsa:

* Tüm kayıtları silin `Contact` tablo.
* Veritabanını oluşturmak için uygulamayı yeniden başlatın.

Bir kullanıcı kişiler göz atmak için kaydolun.

Üç farklı tarayıcılar (veya ıncognito/InPrivate sürümler) başlatmak için tamamlanan uygulama test etmek için kolay bir yol değil. Bir tarayıcıda, yeni bir kullanıcı kaydı (örneğin, `test@example.com`). Her tarayıcıya farklı bir kullanıcı ile oturum açın. Aşağıdaki işlemleri doğrulayın:

* Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.
* Kayıtlı kullanıcıların düzenleme/kullanıcıların kendi verilerini silme.
* Yöneticileri onaylayın veya kişi verilerini reddedin. `Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeler.
* Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.

| Kullanıcı| Seçenekler |
| ------------ | ---------|
| test@example.com | Düzenle/kendi veri silmek |
| manager@contoso.com | Onayla/Reddet ve düzenleme/silme veri sahip olabilir |
| admin@contoso.com | Düzenleme/silme ve tüm veri Onayla/Reddet kullanabilirsiniz|

Bir kişi yöneticinin tarayıcıda oluşturun. Delete URL'sini kopyalayın ve yönetici kişiden düzenleyin. Bu bağlantıları test kullanıcısı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcıya yapıştırın.

## <a name="create-the-starter-app"></a>Başlangıç uygulaması oluşturma

* "ContactManager" adlı bir Razor sayfalarının uygulaması oluşturma

  * Uygulama oluşturma **bireysel kullanıcı hesapları**.
  * Aşağıdaki örnekte kullanılan ad ad alanınızı eşleşecek şekilde "ContactManager" adı.

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

  [!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * `-uld` SQLite yerine LocalDB belirtir

* Aşağıdakileri ekleyin `Contact` modeli:

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* İskele `Contact` modeli:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* Güncelleştirme **ContactManager** içinde yer alan bağlayıcı *Pages/_Layout.cshtml* dosyası:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* İlk geçişin iskelesi ve veritabanı güncelleştirme:

```console
dotnet ef migrations add initial
dotnet ef database update
```

* Oluşturma, düzenleme ve bir kişi silme uygulamayı test etme

### <a name="seed-the-database"></a>Çekirdek veritabanı

Ekleme `SeedData` sınıfının *veri* klasör. Örnek indirdiğiniz, kopyalayabilirsiniz *SeedData.cs* dosya *veri* başlangıç projesinin klasör.

Çağrı `SeedData.Initialize` gelen `Main`:

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

Uygulama veritabanı sağlanmış sınayın. Seed yöntemi kişi DB herhangi bir satır varsa, çalıştırmaz.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop). Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.
* [ASP.NET Core yetkilendirme: basit, rol, talep tabanlı ve özel](xref:security/authorization/index)
* [Özel ilke tabanlı yetkilendirme](xref:security/authorization/policies)
