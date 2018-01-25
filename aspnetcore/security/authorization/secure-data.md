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
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)

Bu öğretici yetkilendirme tarafından korunan kullanıcı verileri ile bir web uygulaması oluşturulacağını gösterir. Kimliği doğrulanmış (kayıtlı) kullanıcılar Kişiler listesini görüntüler oluşturdunuz. Üç güvenlik grupları vardır:

* Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.
* Kayıtlı kullanıcıların düzenleme/kullanıcıların kendi verilerini silme. 
* Yöneticileri onaylayın veya kişi verilerini reddedin. Yalnızca onaylanan kişilere kullanıcılar tarafından görülebilir.
* Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.

Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) imzalanır. Kullanıcı Rick can yalnızca görünümü kişiler onaylanmış ve kendi kişiler düzenleme/silme. Yalnızca son kaydı Rick tarafından oluşturulan, düzenleme görüntüler ve bağlantıları Sil

![Yukarıda açıklanan görüntüsü](secure-data/_static/rick.png)

Aşağıdaki görüntüde `manager@contoso.com` ve yöneticileri rolünde imzalanır. 

![Yukarıda açıklanan görüntüsü](secure-data/_static/manager1.png)

Aşağıdaki resimde yöneticileri kişi Ayrıntılar görünümünü gösterir.

![Yukarıda açıklanan görüntüsü](secure-data/_static/manager.png)

Yalnızca Yöneticiler ve yöneticilerin Onayla sahip ve düğmeleri reddedin.

Aşağıdaki görüntüde `admin@contoso.com` ve yöneticinin rolünde imzalanır. 

![Yukarıda açıklanan görüntüsü](secure-data/_static/admin.png)

Yönetici, tüm ayrıcalıklarına sahiptir. Depoladığından, herhangi bir kişiyi okuma/düzenleme/silme yapabilir ve kişiler durumunu değiştirebilirsiniz.

Uygulama tarafından oluşturulan [iskele](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) aşağıdaki `Contact` modeli:

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

A `ContactIsOwnerAuthorizationHandler` yetkilendirme işleyicisi sağlar kullanıcı verilerine yalnızca düzenleyebilirsiniz. A `ContactManagerAuthorizationHandler` yetkilendirme işleyici onaylamak veya reddetmek kişiler yöneticileri sağlar.  A `ContactAdministratorsAuthorizationHandler` yetkilendirme işleyicisini yöneticilerinin onaylama veya reddetme kişiler ve kişi düzenleme/silme sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Bu bir başlangıç Öğreticisi değildir. Hakkında bilginiz olması gerekir:

* [ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Tamamlanan uygulama ve başlangıç

[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) uygulama. [Test](#test-the-completed-app) tamamlanan uygulama ile güvenlik özellikleri hakkında bilgi sahibi olursunuz. 

### <a name="the-starter-app"></a>Başlangıç uygulaması

Tamamlanan örnek ile kodunuzu karşılaştırmak yararlıdır.

[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) uygulama. 

Bkz: [başlangıç uygulaması oluşturma](#create-the-starter-app) sıfırdan oluşturmak istiyorsanız.

Veritabanını güncelleştirme:

```none
   dotnet ef database update
```

Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.

Bu öğretici güvenli kullanıcı veri uygulaması oluşturmak için tüm ana adım vardır. Tamamlanan projesine başvurması daha yararlı olabilir.

## <a name="modify-the-app-to-secure-user-data"></a>Kullanıcı verileri güvenli hale getirmek için uygulama değiştirme

Aşağıdaki bölümlerde güvenli kullanıcı veri uygulaması oluşturmak için tüm önemli adımlar vardır. Tamamlanan projesine başvurması daha yararlı olabilir.

### <a name="tie-the-contact-data-to-the-user"></a>Kullanıcı kişi verilerini bağlayın

ASP.NET kullanan [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerini, ancak diğer kullanıcılar verileri düzenleyebilirsiniz. Ekleme `OwnerID` için `Contact` modeli:

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı. `Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler. 

Yeni bir geçişin iskelesini kurun ve veritabanı güncelleştirme:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>SSL ve kimliği doğrulanmış kullanıcılar gerektirir

İçinde `ConfigureServices` yöntemi *haline* dosya, ekleme [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) yetkilendirme Filtresi:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

HTTPS için HTTP isteklerini yeniden yönlendirmek için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting). Visual Studio Code kullanarak veya bir test sertifikası için SSL içermeyen yerel platformunda sınama istiyorsanız:

- Ayarlama `"LocalTest:skipSSL": true` içinde *appsettings.json* dosya.

### <a name="require-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar gerektirir

Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesini ayarlayın. Denetleyici veya eylem yöntemi ile kimlik doğrulama dışında seçebilirsiniz `[AllowAnonymous]` özniteliği. Bu yaklaşımda, eklenen her yeni denetleyicileri otomatik olarak eklemek için yeni denetleyicilerinde bağlı olan daha güvenli kimlik doğrulama gerektirecek `[Authorize]` özniteliği. Aşağıdakileri ekleyin `ConfigureServices` yöntemi *haline* dosyası:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

Ekleme `[AllowAnonymous]` bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri alabilmek için giriş denetleyicisi.

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>Test hesabı yapılandırın

`SeedData` Sınıfı, iki hesap, yönetici ve Yöneticisi oluşturur. Kullanım [gizli Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlayabilirsiniz. Proje dizinden bunu (dizinini içeren *Program.cs*).

```console
dotnet user-secrets set SeedUserPW <PW>
```

Güncelleştirme `Configure` test parola kullanmak için:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

Yönetici kullanıcı kimliği ekleyin ve `Status = ContactStatus.Approved` kişilere. Yalnızca bir kişinin gösterilen tüm kişiler için kullanıcı kimliği ekleyin:

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Sahibi, Yöneticisi ve yönetici yetkilendirme işleyicileri oluşturma

Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör. `ContactIsOwnerAuthorizationHandler` Kaynak üzerinde hareket kullanıcının sahip olduğu kaynak doğrular.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Çağrıları `context.Succeed` geçerli kimliği doğrulanmış kullanıcı kişi sahibi ise. Yetkilendirme işleyicileri genellikle dönmek `context.Succeed` zaman gereksinimleri karşılandıktan. Döndürmeleri `Task.FromResult(0)` zaman gereksinimleri karşılanmadığı. `Task.FromResult(0)`ne başarılı veya başarısız olan diğer yetkilendirme işleyici çalışmasına izin verir. Açıkça başarısız gerekiyorsa, dönüş `context.Fail()`.

Böylece biz gereksinim parametrede aktarılan işlemi denetleyin gerek kalmaz, kendi veri düzenleme/silme kişi sahipleri izin veriyoruz.

### <a name="create-a-manager-authorization-handler"></a>Bir yönetici yetkilendirme işleyicisi oluşturun

Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör. `ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrulayın. Yalnızca Yöneticiler onaylayabilir veya reddedebilirsiniz içerik değişikliklerini (yeni veya değiştirilmiş).

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Bir yönetici yetkilendirme işleyicisi oluşturun

Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör. `ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrulayın. Yönetici, tüm işlemleri yapabilir.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Yetkilendirme işleyicileri kaydetme

Entity Framework Çekirdek kullanarak Hizmetleri kayıtlı, için [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulmuştur. Kullanılabilir olacak işleyicileri hizmet Koleksiyonuyla Kaydet `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection). Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`ve `ContactManagerAuthorizationHandler` teklileri eklenir. Olup olmadıklarını teklileri EF kullanmayın ve gerekli tüm bilgileri de olduğundan `Context` parametresinin `HandleRequirementAsync` yöntemi.

Tam `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>Yetkilendirmeyi desteklemek için kodu güncelleştirme

Bu bölümde, denetleyici ve görünüm güncelleştirin ve işlemleri gereksinimleri sınıf ekleyin.

### <a name="update-the-contacts-controller"></a>Güncelleştirme kişiler denetleyicisi

Güncelleştirme `ContactsController` Oluşturucusu:

* Ekleme `IAuthorizationService` yetkilendirme işleyicilerine erişmek için hizmet. 
* Ekleme `Identity` `UserManager` hizmeti:

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>Bir kişi operations gereksinimleri sınıfı ekleme

Ekleme `ContactOperations` sınıfının *yetkilendirme* klasör. Bu sınıf içeren gereksinimleri bizim uygulama destekler:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>Güncelleştirme oluşturma

Güncelleştirme `HTTP POST Create` yöntemi için:

* Kullanıcı kimliği eklemek `Contact` modeli.
* Kullanıcının kişinin sahip olduğu doğrulamak için yetkilendirme işleyici çağırın.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>Güncelleştirmeyi Düzenle

Her ikisi de güncelleştirme `Edit` kullanıcıyı doğrulamak için yetkilendirme işleyicisi kullanmak için yöntemleri sahibi ile iletişime geçin. Kaynak Yetkilendirme performans gösterdiğini biz kullanamazsınız, çünkü `[Authorize]` özniteliği. Öznitelikleri değerlendirildiğinde biz kaynağa erişimi yok. Bağlı kaynak yetkilendirme kesinlik temelli olması gerekir. Bizim denetleyicisi yüklenirken, veya göre işleyici içinde yükleme kaynağına erişimi edindikten sonra denetimlerinin gerçekleştirilmesi gerekir. Sık sık kaynak anahtarı geçirerek kaynak erişir.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>Güncelleştirme Delete yöntemi

Her ikisi de güncelleştirme `Delete` kullanıcıyı doğrulamak için yetkilendirme işleyicisi kullanmak için yöntemleri sahibi ile iletişime geçin.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>Yetkilendirme hizmeti görünümlere ekleme

Şu anda UI gösterir düzenleyebilir ve kullanıcı değiştirilemiyor veri bağlantıları silebilirsiniz. Biz, görünümlere yetkilendirme işleyici uygulayarak düzeltmesi.

Yetkilendirme hizmetinde Ekle *Views/_ViewImports.cshtml* tüm görünümler kullanılabilir olacak şekilde dosya:

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

Güncelleştirme *Views/Contacts/Index.cshtml* yalnızca düzenleme görüntüleme ve düzenleme / kişi silme kullanıcılar için bağlantıları silmek için Razor görünüm.

Ekleme`@using ContactManager.Authorization;`

Güncelleştirme `Edit` ve `Delete` düzenlemek ve kişiyi silmek izne sahip kullanıcılar yalnızca çizilir şekilde bağlar.

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

Uyarı: Düzen veya veri silmek için izne sahip olmayan kullanıcılardan gelen bağlantılar gizleme uygulama güvenli değil. Bağlantılar gizleme uygulama daha fazla kullanıcı kolay yalnızca geçerli bağlantılar görüntüleyerek yapar. Kullanıcıları düzenleme çağırma ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'lerini korsan saldırılarına.  Denetleyici güvenli olması için erişim denetimleri yinelemeniz gerekir.

### <a name="update-the-details-view"></a>Güncelleştirme ayrıntıları görünümü

Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler şekilde güncelleştirin:

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>Tamamlanmış uygulamayı test etme

Visual Studio Code kullanarak veya bir test sertifikası için SSL içermeyen yerel platformunda sınama istiyorsanız:

- Ayarlama `"LocalTest:skipSSL": true` içinde *appsettings.json* dosya.

Uygulama çalıştıran ve kişiler varsa, tüm kayıtları silin `Contact` tablo ve veritabanı oluşturmak için uygulamayı yeniden başlatın. Visual Studio kullanıyorsanız, çıkmak ve IIS Express çekirdek veritabanı yeniden başlatmanız gerekir.

Bir kullanıcı kişiler göz atmak için kaydolun.

Üç farklı tarayıcılar (veya ıncognito/InPrivate sürümler) başlatmak için tamamlanan uygulama test etmek için kolay bir yol değil. Bir tarayıcıda, örneğin, yeni bir kullanıcı kaydı `test@example.com`. Her tarayıcıya farklı bir kullanıcı ile oturum açın. Aşağıdakileri doğrulayın:

* Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.
* Kayıtlı kullanıcıların düzenleme/kullanıcıların kendi verilerini silme. 
* Yöneticileri onaylayın veya kişi verilerini reddedin. `Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeler. 
* Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.

| Kullanıcı| Seçenekler |
| ------------ | ---------|
| test@example.com | Düzenle/kendi veri silmek |
| manager@contoso.com | Onayla/Reddet ve düzenleme/silme veri sahip olabilir  |
| admin@contoso.com | Düzenleme/silme ve tüm veri Onayla/Reddet kullanabilirsiniz|

Bir kişi Yöneticiler tarayıcıda oluşturun. Delete URL'sini kopyalayın ve yönetici kişiden düzenleyin. Bu bağlantıları test kullanıcısı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcıya yapıştırın.

## <a name="create-the-starter-app"></a>Başlangıç uygulaması oluşturma

Başlangıç uygulaması oluşturmak için bu yönergeleri izleyin.

* Oluşturma bir **ASP.NET çekirdek Web uygulaması** kullanarak [Visual Studio 2017](https://www.visualstudio.com/) "ContactManager" adlı

  * Uygulama oluşturma **bireysel kullanıcı hesapları**.
  * Ad alanınız örnek ad alanı kullanımda eşleşecek şekilde "ContactManager" adı.

* Aşağıdakileri ekleyin `Contact` modeli:

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* İskele `Contact` Entity Framework Çekirdek modelini ve `ApplicationDbContext` veri bağlamı. Tüm yapı iskelesi Varsayılanları kabul edin. Kullanarak `ApplicationDbContext` veri bağlamı sınıfı kişi tablo koyar [kimlik](xref:security/authentication/identity) veritabanı. Bkz: [bir modeli ekleme](xref:tutorials/first-mvc-app/adding-model) daha fazla bilgi için.

* Güncelleştirme **ContactManager** içinde yer alan bağlayıcı *Views/Shared/_Layout.cshtml* dosya `asp-controller="Home"` için `asp-controller="Contacts"` şekilde dokunarak **ContactManager** bağlantı Kişiler denetleyicisi çağırır. Özgün biçimlendirme:

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

Güncelleştirilmiş biçimlendirme:

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* İlk geçişin iskelesi ve veritabanı güncelleştirme

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* Oluşturma, düzenleme ve bir kişinin silinmesi uygulamayı test etme

### <a name="seed-the-database"></a>Çekirdek veritabanı

Ekleme `SeedData` sınıfının *veri* klasör. Örnek indirdiğiniz, kopyalayabilirsiniz *SeedData.cs* dosya *veri* başlangıç projesinin klasör.

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

Vurgulanmış kodu sonuna ekleyin `Configure` yönteminde *haline* dosyası:

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

Uygulama veritabanı sağlanmış sınayın. Seed yöntemi kişi DB herhangi bir satır varsa çalıştırmaz.

### <a name="create-a-class-used-in-the-tutorial"></a>Öğreticide kullanılan sınıf oluşturma

* Adlı bir klasör oluşturun *yetkilendirme*.
* Kopya *Authorization\ContactOperations.cs* dosya projeyi Merkezi'nden veya aşağıdaki kodu kopyalayın:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop). Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.
* [ASP.NET Core yetkilendirme: basit, rol, talep tabanlı ve özel](index.md)
* [Özel ilke tabanlı yetkilendirme](policies.md)
