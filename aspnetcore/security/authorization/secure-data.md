---
title: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma
author: rick-anderson
description: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile Razor sayfaları uygulaması oluşturmayı öğrenin. HTTPS, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir.
ms.author: riande
ms.date: 12/18/2018
ms.custom: seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: bdba706c1ef24ebe35129cb8bb2d9949196245a1
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098928"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)

::: moniker range="<= aspnetcore-1.1"

Bkz: [bu PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC sürümü için. Bu öğreticide ASP.NET Core 1.1 sürümü [bu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) klasör. ASP.NET Core örnek konusu 1.1 [örnekleri](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Bkz: [bu pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir. Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz. Üç güvenlik grubu vardır:

* **Kayıtlı kullanıcıların** onaylanan tüm verileri görüntüleyebilir ve kullanıcıların kendi verilerini düzenleme/silebilir.
* **Yöneticileri** onaylayabilecek veya reddedebilecek kişi verilerini. Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.
* **Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.

Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) açmıştır. Rick yalnızca onaylanan kişiler görüntüleyebilir ve **Düzenle**/**Sil**/**Yeni Oluştur** kendi kişiler için bağlantılar. Yalnızca en son kaydını görüntüler Rick tarafından oluşturulan **Düzenle** ve **Sil** bağlantıları. Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.

![Oturum Rick gösteren ekran görüntüsü](secure-data/_static/rick.png)

Aşağıdaki görüntüde, `manager@contoso.com` ve yöneticileri rolünde imzalanır:

![Gösteren ekran görüntüsü manager@contoso.com oturum açıldı](secure-data/_static/manager1.png)

Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:

![Bir kişinin yöneticisinin görüntüle](secure-data/_static/manager.png)

**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.

Aşağıdaki görüntüde, `admin@contoso.com` ve yöneticiler rolünde imzalanır:

![Gösteren ekran görüntüsü admin@contoso.com oturum açıldı](secure-data/_static/admin.png)

Yöneticisinin tüm ayrıcalıklara sahip değil. Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.

Uygulama tarafından oluşturulan [yapı iskelesi](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) aşağıdaki `Contact` modeli:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

Örnek aşağıdaki yetkilendirme işleyicilerini içerir:

* `ContactIsOwnerAuthorizationHandler`: Bir kullanıcı yalnızca kendi verilerini düzenleyebilir sağlar.
* `ContactManagerAuthorizationHandler`: Yöneticileri onaylayabilir veya kişiler sağlar.
* `ContactAdministratorsAuthorizationHandler`: Yöneticilerin onaylama veya reddetme kişiler ve kişiler düzenleme/silme olanak sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gelişmiştir. Sahibi olmalısınız:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Kimlik Doğrulaması](xref:security/authentication/identity)
* [Hesap Onaylama ve Parola Kurtarma](xref:security/authentication/accconfirm)
* [Yetkilendirme](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

ASP.NET Core 2.1 içinde `User.IsInRole` kullanırken başarısız `AddDefaultIdentity`. Bu öğreticide `AddDefaultIdentity` ve bu nedenle ASP.NET Core 2.2 veya sonraki bir sürümü gerektirir. Bkz: [bu GitHub sorunu](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) geçici için.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a>Tamamlanmış uygulama ve başlangıç

[İndirme](xref:index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) uygulama. [Test](#test-the-completed-app) tamamlanan uygulama güvenlik özellikleri ile ilgili bilgi sahibi olursunuz.

### <a name="the-starter-app"></a>Başlangıç uygulaması

[İndirme](xref:index#how-to-download-a-sample) [başlangıç](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) uygulama.

Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.

## <a name="secure-user-data"></a>Kullanıcı verilerinin güvenliğini sağlama

Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız. Tamamlanan projeye başvuran daha yararlı olabilir.

### <a name="tie-the-contact-data-to-the-user"></a>Kullanıcı kişi verilerini bağlayın

ASP.NET kullanmak [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerine, ancak diğer kullanıcıların verileri düzenleyebilir. Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı. `Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.

Yeni geçiş oluştur ve veritabanını güncelleştir:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Kimlik için rol hizmetleri Ekle

Append [ndeki](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) rol hizmetlerini eklemek için:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar gerektirir

Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 Kimlik doğrulaması ile Razor sayfası, denetleyici veya eylem yöntemi düzeyinde dışında iyileştirilmiş `[AllowAnonymous]` özniteliği. Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur. Kimlik doğrulaması varsayılan olarak gerekli olan bağlı olan, yeni denetleyicileri ve dahil etmek için Razor sayfaları hakkında daha fazla güvenli `[Authorize]` özniteliği.

Ekleme [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) dizine hakkında ve kişi sayfaları böylece bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri elde edebilirsiniz.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Test hesabı yapılandırın

`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve. Kullanım [gizli dizi Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlamak için. Proje dizininden parolayı ayarlayın (dizinini içeren *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Güçlü bir parola belirtilmediği takdirde, bir özel durum `SeedData.Initialize` çağrılır.

Güncelleştirme `Main` test parola kullanmak üzere:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Test hesapları oluşturabilir ve kişilerini güncelleştirme

Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturma sınıfı:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Yönetici kullanıcı Kimliğini ekleyin ve `ContactStatus` kişilere. "Gönderildi" ve "Reddedildi" bir kişilerden biri olun. Kullanıcı kimliği ve durumu tüm kişileri ekleyin. Tek bir kişi gösterilmektedir:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma

Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör. `ContactIsOwnerAuthorizationHandler` Bir kaynakta çalışan kullanıcı kaynağın sahibi doğrular.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) kişi sahibi kimliği doğrulanmış geçerli kullanıcı ise. Yetkilendirme işleyicileri genellikle:

* Dönüş `context.Succeed` gereksinimleri karşılanmadığı zaman.
* Dönüş `Task.CompletedTask` zaman olmayan gereksinimleri karşılanıyor. `Task.CompletedTask` ne success veya FAILURE olur&mdash;çalıştırmak, diğer yetkilendirme işleyicileri sağlar.

Açıkça başarısız gerekiyorsa, iade [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir. `ContactIsOwnerAuthorizationHandler` gereksinim parametrede aktarılan işlemi denetleyin. gerekmez.

### <a name="create-a-manager-authorization-handler"></a>Bir yönetici yetkilendirme işleyici oluşturun

Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör. `ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, bir yönetici yer almadığını doğrular. Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Bir yönetici yetkilendirme işleyicisi oluşturun

Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör. `ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, yönetici yer almadığını doğrular. Yönetici, tüm işlemleri yapabilir.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Yetkilendirme işleyicilerini kaydedin

Entity Framework Core kullanan hizmetler için kaydedilmelidir [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulur. Kullanılabilir bulunmaları işleyicilerini hizmet koleksiyonuyla kaydedin `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection). Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` teklileri eklenir. Oldukları teklileri çünkü bunlar EF kullanmayın ve gereken tüm bilgiler `Context` parametresinin `HandleRequirementAsync` yöntemi.

## <a name="support-authorization"></a>Destek yetkilendirme

Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.

### <a name="review-the-contact-operations-requirements-class"></a>Gözden geçirme kişi işlem gereksinimleri sınıfı

Gözden geçirme `ContactOperations` sınıfı. Bu sınıf gereksinimleri içeren uygulama destekler:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Kişiler Razor sayfaları için bir temel sınıf oluşturma

Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun. Temel sınıf başlatma kodunu tek bir yerde getirir:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Yukarıdaki kod:

* Ekler `IAuthorizationService` yetkilendirme işleyicilere erişmek için hizmet.
* Kimlik ekler `UserManager` hizmeti.
* Ekleme `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Güncelleştirme CreateModel

Kullanılacak Oluştur sayfası model Oluşturucu güncelleştirme `DI_BasePageModel` temel sınıfı:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Güncelleştirme `CreateModel.OnPostAsync` yöntemi:

* Kullanıcı Kimliğinize ekleme `Contact` modeli.
* Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Güncelleştirme IndexModel

Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilecek şekilde yöntemi:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Güncelleştirme EditModel

Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin. Kaynak Yetkilendirme Doğrulanmakta olan çünkü `[Authorize]` özniteliği yeterli değil. Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok. Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir. Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir. Sık sık geçirerek kaynak anahtarı kaynağa erişir.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Güncelleştirme DeleteModel

Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Kimlik doğrulama servisi görünümlere ekleme

Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.

Yetkilendirme hizmetinde ekleme *Views/_ViewImports.cshtml* tüm görünümlere kullanılabilir olacak şekilde dosya:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Birkaç önceki biçimlendirme ekler `using` deyimleri.

Güncelleştirme **Düzenle** ve **Sil** bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar tarafından işlenen için:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir. Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar. Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack. Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.

### <a name="update-details"></a>Güncelleştirme ayrıntıları

Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Güncelleştirme ayrıntıları sayfa modeli:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Ekleme veya bir rolü için bir kullanıcıyı kaldırma

Bkz: [bu sorunu](https://github.com/aspnet/Docs/issues/8502) hakkında bilgi için:

* Bir kullanıcıdan ayrıcalıklarının kaldırılması. Örneğin, bir kullanıcı bir sohbet uygulaması ses kapatma.
* Bir kullanıcı ayrıcalıkları ekleniyor.

## <a name="test-the-completed-app"></a>Tamamlanmış uygulamayı test etme

Kapsanan kullanıcı hesapları için bir parola belirlemediyseniz kullanın [gizli dizi Yöneticisi aracını](xref:security/app-secrets#secret-manager) parola ayarlamak için:

* Güçlü bir parola seçin: Sekiz kullanın veya daha fazla karakter ve en az bir büyük harf karakter, sayı ve simge. Örneğin, `Passw0rd!` güçlü parola gereksinimlerini karşılıyor.
* Aşağıdaki komutu yürütün proje klasöründen burada `<PW>` parola:

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

Uygulamanın, kişiler varsa:

* Tüm kayıtları silin `Contact` tablo.
* Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.

Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate oturumları) başlatılmasıdır. Yeni bir kullanıcı bir tarayıcıda kaydedin (örneğin, `test@example.com`). Her bir tarayıcıya farklı bir kullanıcı ile oturum açın. Aşağıdaki işlemleri doğrulayın:

* Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.
* Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.
* Yöneticileri Onayla/kişi verilerini reddet. `Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeleri.
* Yöneticiler Onayla/Reddet ve tüm verileri düzenleme/silme.

| Kullanıcı                | Uygulama tarafından sağlanmış | Seçenekler                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Hayır                | Kendi veri düzenleme veya silme.                |
| manager@contoso.com | Evet               | Onayla/Reddet ve kendi veri düzenleme/silme. |
| admin@contoso.com   | Evet               | Onayla/Reddet ve tüm verileri düzenleme/silme. |

Bir kişi, yöneticinin tarayıcıda oluşturur. Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin. Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.

## <a name="create-the-starter-app"></a>Başlangıç uygulaması oluşturun

* "ContactManager" adlı bir Razor sayfaları uygulaması oluşturma
  * Uygulamayı oluşturma **bireysel kullanıcı hesapları**.
  * Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.
  * `-uld` LocalDB yerine SQLite belirtir

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Ekleme *Models/Contact.cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* İskele `Contact` modeli.
* İlk geçiş oluşturun ve veritabanını güncelleştir:

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* Güncelleştirme **ContactManager** içinde yer işareti *Pages/_Layout.cshtml* dosyası:

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* Oluşturma, düzenleme ve silme kişi uygulamayı test etme

### <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

Ekleme [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) sınıfının *veri* klasör.

Çağrı `SeedData.Initialize` gelen `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Test, uygulama veritabanını sağlanmış. DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Ek kaynaklar

* [Azure App Service'te .NET Core ve SQL veritabanı web uygulaması derleme](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop). Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.
* <xref:security/authorization/introduction>
* [Özel ilke tabanlı yetkilendirme](xref:security/authorization/policies)

::: moniker-end
