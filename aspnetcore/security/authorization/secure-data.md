---
title: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma
author: rick-anderson
description: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile Razor sayfaları uygulaması oluşturmayı öğrenin. HTTPS, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 7710a8965771db02e601dafb7da752906bcd43e5
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659581"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma

By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ali Audette](https://twitter.com/joeaudette)

::: moniker range="<= aspnetcore-1.1"

ASP.NET Core MVC sürümü için [Bu PDF 'ye](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) bakın. Bu öğreticinin ASP.NET Core 1,1 sürümü [Bu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) klasöründedir. 1,1 ASP.NET Core örnek [örnekleri](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[Bu PDF 'ye](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf) bakın

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir. Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz. Üç güvenlik grubu vardır:

* **Kayıtlı kullanıcılar** tüm onaylanan verileri görüntüleyebilir ve kendi verilerini düzenleyebilir/silebilir.
* **Yöneticiler** , kişi verilerini onaylayabilir veya reddedebilir. Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.
* **Yöneticiler** , verileri onaylayabilir/reddedebilir ve düzenleyebilir/silebilir.

Bu belgedeki görüntüler en son şablonlarla tam olarak eşleşmez.

Aşağıdaki görüntüde, User Rick (`rick@example.com`) oturum açtı. Rick, onaylanan kişileri görüntüleyebilir ve kişiler için **yeni bağlantılar oluşturmak**//**silebilir** ve **düzenleyebilir** . Yalnızca Rick tarafından oluşturulan son kayıt, **Düzenle** ve **Sil** bağlantılarını görüntüler. Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.

![Oturum Rick gösteren ekran görüntüsü](secure-data/_static/rick.png)

Aşağıdaki görüntüde `manager@contoso.com`, ve yöneticisinin rolünde oturum açtı:

![manager@contoso.com oturum açmış olduğunu gösteren ekran görüntüsü](secure-data/_static/manager1.png)

Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:

![Bir kişinin yöneticisinin görüntüle](secure-data/_static/manager.png)

**Onaylama** ve **reddetme** düğmeleri yalnızca Yöneticiler ve yöneticiler için görüntülenir.

Aşağıdaki görüntüde `admin@contoso.com`, ve yönetici rolünde oturum açtı:

![admin@contoso.com oturum açmış olduğunu gösteren ekran görüntüsü](secure-data/_static/admin.png)

Yöneticisinin tüm ayrıcalıklara sahip değil. Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.

Uygulama, aşağıdaki `Contact` modeli için [Yapı iskelesi](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) tarafından oluşturulmuştur:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

Örnek aşağıdaki yetkilendirme işleyicilerini içerir:

* `ContactIsOwnerAuthorizationHandler`: kullanıcının yalnızca verilerini düzenleyebilmesini sağlar.
* `ContactManagerAuthorizationHandler`: yöneticilerin kişileri onaylamasını veya reddetmesini sağlar.
* `ContactAdministratorsAuthorizationHandler`: yöneticilerin kişileri onaylamasını veya reddetmesini ve kişileri düzenlemesini/silmesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gelişmiştir. Sahibi olmalısınız:

* [ASP.NET Çekirdeği](xref:tutorials/first-mvc-app/start-mvc)
* [Kimlik doğrulaması](xref:security/authentication/identity)
* [Hesap Onaylama ve Parola Kurtarma](xref:security/authentication/accconfirm)
* [Yetkilendirme](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Tamamlanmış uygulama ve başlangıç

[Tamamlanmış](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) uygulamayı [indirin](xref:index#how-to-download-a-sample) . Tamamlanmış uygulamayı [Test](#test-the-completed-app) edin, böylece güvenlik özellikleri hakkında bilgi sahibi olun.

### <a name="the-starter-app"></a>Başlangıç uygulaması

[Başlangıç](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) uygulamasını [indirin](xref:index#how-to-download-a-sample) .

Uygulamayı çalıştırın, **ContactManager** bağlantısına dokunun ve bir kişi oluşturup silemdiğinizi doğrulayın.

## <a name="secure-user-data"></a>Kullanıcı verilerinin güvenliğini sağlama

Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız. Tamamlanan projeye başvuran daha yararlı olabilir.

### <a name="tie-the-contact-data-to-the-user"></a>Kullanıcı kişi verilerini bağlayın

Kullanıcıların verilerini düzenleyebilmeleri, ancak diğer kullanıcıların verilerini düzenleyebilmeleri için ASP.NET [Identity](xref:security/authentication/identity) Kullanıcı kimliğini kullanın. `Contact` modeline `OwnerID` ve `ContactStatus` ekleyin:

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

[kimlik](xref:security/authentication/identity) veritabanındaki `AspNetUser` TABLOSUNDAN kullanıcının kimliği `OwnerID`. `Status` alanı, bir kişinin genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.

Yeni geçiş oluştur ve veritabanını güncelleştir:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Kimlik için rol hizmetleri Ekle

Rol hizmetleri eklemek için [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) ekleyin:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar gerektirir

Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 `[AllowAnonymous]` özniteliğiyle Razor sayfasında, denetleyicide veya eylem yöntemi düzeyinde kimlik doğrulamasının devre dışı bırakabilirsiniz. Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur. Varsayılan olarak kimlik doğrulamanın gerekli olması, yeni denetleyicilere bağlı olandan daha güvenlidir ve `[Authorize]` özniteliğini eklemek Razor Pages.

Anonim kullanıcıların, kaydolmadan önce site hakkında bilgi alması için dizin ve gizlilik sayfalarına [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) ekleyin.

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a>Test hesabı yapılandırın

`SeedData` sınıfı iki hesap oluşturur: yönetici ve yönetici. Bu hesaplara bir parola ayarlamak için [gizli dizi Yöneticisi aracını](xref:security/app-secrets) kullanın. Parolayı proje dizininden ayarlayın ( *program.cs*içeren dizin):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Güçlü bir parola belirtilmemişse `SeedData.Initialize` çağrıldığında bir özel durum oluşturulur.

Sınama parolasını kullanmak için `Main` güncelleştirin:

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Test hesapları oluşturabilir ve kişilerini güncelleştirme

Test hesapları oluşturmak için `SeedData` sınıfındaki `Initialize` yöntemi güncelleştirin:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

Yönetici kullanıcı KIMLIĞINI ve `ContactStatus` kişilere ekleyin. "Gönderildi" ve "Reddedildi" bir kişilerden biri olun. Kullanıcı kimliği ve durumu tüm kişileri ekleyin. Tek bir kişi gösterilmektedir:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma

*Yetkilendirme* klasöründe bir `ContactIsOwnerAuthorizationHandler` sınıfı oluşturun. `ContactIsOwnerAuthorizationHandler`, bir kaynağın kaynak üzerinde davranan kullanıcının kaynağın sahip olduğunu doğrular.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` bağlamını çağırır [. ](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)Kimliği doğrulanmış geçerli kullanıcı kişi sahibsiyse başarılı olur. Yetkilendirme işleyicileri genellikle:

* Gereksinimler karşılandığında `context.Succeed` döndürün.
* Gereksinimler karşılanmazsa `Task.CompletedTask` döndürün. `Task.CompletedTask`, diğer yetkilendirme işleyicilerinin çalışmasına izin veren&mdash;başarılı veya başarısız değildir.

Açıkça başarısız olması gerekiyorsa, [bağlamı döndürün. Başarısız oldu](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir. `ContactIsOwnerAuthorizationHandler` gereksinim parametresine geçirilen işlemi denetlemesi gerekmez.

### <a name="create-a-manager-authorization-handler"></a>Bir yönetici yetkilendirme işleyici oluşturun

*Yetkilendirme* klasöründe bir `ContactManagerAuthorizationHandler` sınıfı oluşturun. `ContactManagerAuthorizationHandler`, kaynak üzerinde davranan kullanıcının bir yönetici olduğunu doğrular. Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Bir yönetici yetkilendirme işleyicisi oluşturun

*Yetkilendirme* klasöründe bir `ContactAdministratorsAuthorizationHandler` sınıfı oluşturun. `ContactAdministratorsAuthorizationHandler`, kaynak üzerinde davranan kullanıcının bir yönetici olduğunu doğrular. Yönetici, tüm işlemleri yapabilir.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Yetkilendirme işleyicilerini kaydedin

Entity Framework Core kullanan hizmetlerin, [Addkapsamlıdır](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)kullanılarak [bağımlılık ekleme](xref:fundamentals/dependency-injection) için kayıtlı olması gerekir. `ContactIsOwnerAuthorizationHandler`, Entity Framework Core oluşturulan ASP.NET Core [kimliğini](xref:security/authentication/identity)kullanır. İşleyicileri, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla `ContactsController` için kullanılabilir olmaları için hizmet koleksiyonuyla kaydedin. `ConfigureServices`sonuna aşağıdaki kodu ekleyin:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` tekton olarak eklenir. Bunlar, EF kullanmadığından ve gereken tüm bilgiler `HandleRequirementAsync` yönteminin `Context` parametresinde olduğundan tektonlar vardır.

## <a name="support-authorization"></a>Destek yetkilendirme

Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.

### <a name="review-the-contact-operations-requirements-class"></a>Gözden geçirme kişi işlem gereksinimleri sınıfı

`ContactOperations` sınıfını gözden geçirin. Bu sınıf gereksinimleri içeren uygulama destekler:

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Kişiler Razor sayfaları için bir temel sınıf oluşturma

Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun. Temel sınıf başlatma kodunu tek bir yerde getirir:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

Yukarıdaki kod:

* Yetkilendirme işleyicilerine erişim için `IAuthorizationService` hizmetini ekler.
* Kimlik `UserManager` hizmetini ekler.
* `ApplicationDbContext`ekleyin.

### <a name="update-the-createmodel"></a>Güncelleştirme CreateModel

Create Page model oluşturucusunu `DI_BasePageModel` temel sınıfını kullanacak şekilde güncelleştirin:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

`CreateModel.OnPostAsync` yöntemini şu şekilde güncelleştirin:

* Kullanıcı KIMLIĞINI `Contact` modeline ekleyin.
* Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Güncelleştirme IndexModel

`OnGetAsync` yöntemini yalnızca onaylanan kişilerin genel kullanıcılara gösterilmesi için güncelleştirin:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Güncelleştirme EditModel

Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin. Kaynak yetkilendirmesi doğrulanamadığından `[Authorize]` özniteliği yeterli değildir. Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok. Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir. Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir. Sık sık geçirerek kaynak anahtarı kaynağa erişir.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Güncelleştirme DeleteModel

Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Kimlik doğrulama servisi görünümlere ekleme

Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.

Yetkilendirme hizmetini *Sayfalar/_ViewImports. cshtml* dosyasına ekleme, bu nedenle tüm görünümlerde kullanılabilir hale gelir:

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

Yukarıdaki biçimlendirme birkaç `using` deyimi ekler.

*Sayfalar/kişiler/Index. cshtml* 'deki **Düzenle** ve **Sil** bağlantılarını yalnızca uygun izinlere sahip kullanıcılar için işlendikleri şekilde güncelleştirin:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir. Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar. Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack. Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.

### <a name="update-details"></a>Güncelleştirme ayrıntıları

Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

Güncelleştirme ayrıntıları sayfa modeli:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Ekleme veya bir rolü için bir kullanıcıyı kaldırma

Hakkında bilgi için [Bu soruna](https://github.com/dotnet/AspNetCore.Docs/issues/8502) bakın:

* Bir kullanıcıdan ayrıcalıklarının kaldırılması. Örneğin, bir sohbet uygulamasındaki kullanıcıyı kapatma.
* Bir kullanıcı ayrıcalıkları ekleniyor.

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a>Challenge ve Fordeklarasyon arasındaki farklar

Bu uygulama, varsayılan ilkeyi [kimliği doğrulanmış kullanıcıları gerektirecek](#require-authenticated-users)şekilde ayarlar. Aşağıdaki kod anonim kullanıcılara izin verir. Anonim kullanıcılara, çekişme ile Fordeklarasyon arasındaki farkları gösterme izni verilir.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

Önceki kodda:

* Kullanıcının kimliği **doğrulanmadığı** zaman, bir `ChallengeResult` döndürülür. Bir `ChallengeResult` döndürüldüğünde, Kullanıcı oturum açma sayfasına yönlendirilir.
* Kullanıcının kimliği doğrulandığında, ancak yetkilendirilmediğinde, bir `ForbidResult` döndürülür. Bir `ForbidResult` döndürüldüğünde, Kullanıcı erişim reddedildi sayfasına yönlendirilir.

## <a name="test-the-completed-app"></a>Tamamlanmış uygulamayı test etme

Önceden oluşturulmuş kullanıcı hesapları için bir parola ayarlamadıysanız, parola ayarlamak için [gizli dizi Yöneticisi aracını](xref:security/app-secrets#secret-manager) kullanın:

* Güçlü bir parola seçin: kullanım sekiz veya daha fazla karakter ve en az bir büyük harf karakter, sayı ve simge. Örneğin, `Passw0rd!` güçlü parola gereksinimlerini karşılar.
* Projenin klasöründen aşağıdaki komutu yürütün, burada `<PW>` paroladır:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

Uygulamanın, kişiler varsa:

* `Contact` tablosundaki tüm kayıtları silin.
* Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.

Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate oturumları) başlatılmasıdır. Tek bir tarayıcıda yeni bir Kullanıcı kaydedin (örneğin, `test@example.com`). Her bir tarayıcıya farklı bir kullanıcı ile oturum açın. Aşağıdaki işlemleri doğrulayın:

* Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.
* Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.
* Yöneticileri Onayla/kişi verilerini reddet. `Details` görünümü **onaylama** ve **reddetme** düğmelerini gösterir.
* Yöneticiler Onayla/Reddet ve tüm verileri düzenleme/silme.

| Kullanıcı                | Uygulama tarafından sağlanmış | Seçenekler                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Hayır                | Kendi veri düzenleme veya silme.                |
| manager@contoso.com | Yes               | Onayla/Reddet ve kendi veri düzenleme/silme. |
| admin@contoso.com   | Yes               | Onayla/Reddet ve tüm verileri düzenleme/silme. |

Bir kişi, yöneticinin tarayıcıda oluşturur. Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin. Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.

## <a name="create-the-starter-app"></a>Başlangıç uygulaması oluşturun

* "ContactManager" adlı bir Razor sayfaları uygulaması oluşturma
  * **Ayrı kullanıcı hesaplarıyla**uygulamayı oluşturun.
  * Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.
  * `-uld`, SQLite yerine LocalDB belirtir

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* *Modeller/Ilgili kişi ekle. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* `Contact` modeli için yapı iskelesi yapın.
* İlk geçiş oluşturun ve veritabanını güncelleştir:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

`dotnet aspnet-codegenerator razorpage` komutuyla bir hata yaşarsanız, [Bu GitHub sorununa](https://github.com/aspnet/Scaffolding/issues/984)bakın.

* *Pages/Shared/_Layout. cshtml* dosyasındaki **ContactManager** bağlayıcısını güncelleştirin:

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* Oluşturma, düzenleme ve silme kişi uygulamayı test etme

### <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

*Veri* klasörüne [seeddata](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) sınıfını ekleyin:

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

`Main``SeedData.Initialize` çağrısı:

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

Test, uygulama veritabanını sağlanmış. DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir. Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz. Üç güvenlik grubu vardır:

* **Kayıtlı kullanıcılar** tüm onaylanan verileri görüntüleyebilir ve kendi verilerini düzenleyebilir/silebilir.
* **Yöneticiler** , kişi verilerini onaylayabilir veya reddedebilir. Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.
* **Yöneticiler** , verileri onaylayabilir/reddedebilir ve düzenleyebilir/silebilir.

Aşağıdaki görüntüde, User Rick (`rick@example.com`) oturum açtı. Rick, onaylanan kişileri görüntüleyebilir ve kişiler için **yeni bağlantılar oluşturmak**//**silebilir** ve **düzenleyebilir** . Yalnızca Rick tarafından oluşturulan son kayıt, **Düzenle** ve **Sil** bağlantılarını görüntüler. Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.

![Oturum Rick gösteren ekran görüntüsü](secure-data/_static/rick.png)

Aşağıdaki görüntüde `manager@contoso.com`, ve yöneticisinin rolünde oturum açtı:

![manager@contoso.com oturum açmış olduğunu gösteren ekran görüntüsü](secure-data/_static/manager1.png)

Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:

![Bir kişinin yöneticisinin görüntüle](secure-data/_static/manager.png)

**Onaylama** ve **reddetme** düğmeleri yalnızca Yöneticiler ve yöneticiler için görüntülenir.

Aşağıdaki görüntüde `admin@contoso.com`, ve yönetici rolünde oturum açtı:

![admin@contoso.com oturum açmış olduğunu gösteren ekran görüntüsü](secure-data/_static/admin.png)

Yöneticisinin tüm ayrıcalıklara sahip değil. Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.

Uygulama, aşağıdaki `Contact` modeli için [Yapı iskelesi](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) tarafından oluşturulmuştur:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

Örnek aşağıdaki yetkilendirme işleyicilerini içerir:

* `ContactIsOwnerAuthorizationHandler`: kullanıcının yalnızca verilerini düzenleyebilmesini sağlar.
* `ContactManagerAuthorizationHandler`: yöneticilerin kişileri onaylamasını veya reddetmesini sağlar.
* `ContactAdministratorsAuthorizationHandler`: yöneticilerin kişileri onaylamasını veya reddetmesini ve kişileri düzenlemesini/silmesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gelişmiştir. Sahibi olmalısınız:

* [ASP.NET Çekirdeği](xref:tutorials/first-mvc-app/start-mvc)
* [Kimlik doğrulaması](xref:security/authentication/identity)
* [Hesap Onaylama ve Parola Kurtarma](xref:security/authentication/accconfirm)
* [Yetkilendirme](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Tamamlanmış uygulama ve başlangıç

[Tamamlanmış](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) uygulamayı [indirin](xref:index#how-to-download-a-sample) . Tamamlanmış uygulamayı [Test](#test-the-completed-app) edin, böylece güvenlik özellikleri hakkında bilgi sahibi olun.

### <a name="the-starter-app"></a>Başlangıç uygulaması

[Başlangıç](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) uygulamasını [indirin](xref:index#how-to-download-a-sample) .

Uygulamayı çalıştırın, **ContactManager** bağlantısına dokunun ve bir kişi oluşturup silemdiğinizi doğrulayın.

## <a name="secure-user-data"></a>Kullanıcı verilerinin güvenliğini sağlama

Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız. Tamamlanan projeye başvuran daha yararlı olabilir.

### <a name="tie-the-contact-data-to-the-user"></a>Kullanıcı kişi verilerini bağlayın

Kullanıcıların verilerini düzenleyebilmeleri, ancak diğer kullanıcıların verilerini düzenleyebilmeleri için ASP.NET [Identity](xref:security/authentication/identity) Kullanıcı kimliğini kullanın. `Contact` modeline `OwnerID` ve `ContactStatus` ekleyin:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

[kimlik](xref:security/authentication/identity) veritabanındaki `AspNetUser` TABLOSUNDAN kullanıcının kimliği `OwnerID`. `Status` alanı, bir kişinin genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.

Yeni geçiş oluştur ve veritabanını güncelleştir:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Kimlik için rol hizmetleri Ekle

Rol hizmetleri eklemek için [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) ekleyin:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar gerektirir

Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 `[AllowAnonymous]` özniteliğiyle Razor sayfasında, denetleyicide veya eylem yöntemi düzeyinde kimlik doğrulamasının devre dışı bırakabilirsiniz. Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur. Varsayılan olarak kimlik doğrulamanın gerekli olması, yeni denetleyicilere bağlı olandan daha güvenlidir ve `[Authorize]` özniteliğini eklemek Razor Pages.

Anonim kullanıcıların, kayıt yaptırmadan önce site hakkında bilgi alması için dizine, hakkında ve Iletişim sayfalarına [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) ekleyin.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Test hesabı yapılandırın

`SeedData` sınıfı iki hesap oluşturur: yönetici ve yönetici. Bu hesaplara bir parola ayarlamak için [gizli dizi Yöneticisi aracını](xref:security/app-secrets) kullanın. Parolayı proje dizininden ayarlayın ( *program.cs*içeren dizin):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Güçlü bir parola belirtilmemişse `SeedData.Initialize` çağrıldığında bir özel durum oluşturulur.

Sınama parolasını kullanmak için `Main` güncelleştirin:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Test hesapları oluşturabilir ve kişilerini güncelleştirme

Test hesapları oluşturmak için `SeedData` sınıfındaki `Initialize` yöntemi güncelleştirin:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Yönetici kullanıcı KIMLIĞINI ve `ContactStatus` kişilere ekleyin. "Gönderildi" ve "Reddedildi" bir kişilerden biri olun. Kullanıcı kimliği ve durumu tüm kişileri ekleyin. Tek bir kişi gösterilmektedir:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma

Bir *Yetkilendirme* klasörü oluşturun ve içinde bir `ContactIsOwnerAuthorizationHandler` sınıfı oluşturun. `ContactIsOwnerAuthorizationHandler`, bir kaynağın kaynak üzerinde davranan kullanıcının kaynağın sahip olduğunu doğrular.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` bağlamını çağırır [. ](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)Kimliği doğrulanmış geçerli kullanıcı kişi sahibsiyse başarılı olur. Yetkilendirme işleyicileri genellikle:

* Gereksinimler karşılandığında `context.Succeed` döndürün.
* Gereksinimler karşılanmazsa `Task.CompletedTask` döndürün. `Task.CompletedTask`, diğer yetkilendirme işleyicilerinin çalışmasına izin veren&mdash;başarılı veya başarısız değildir.

Açıkça başarısız olması gerekiyorsa, [bağlamı döndürün. Başarısız oldu](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir. `ContactIsOwnerAuthorizationHandler` gereksinim parametresine geçirilen işlemi denetlemesi gerekmez.

### <a name="create-a-manager-authorization-handler"></a>Bir yönetici yetkilendirme işleyici oluşturun

*Yetkilendirme* klasöründe bir `ContactManagerAuthorizationHandler` sınıfı oluşturun. `ContactManagerAuthorizationHandler`, kaynak üzerinde davranan kullanıcının bir yönetici olduğunu doğrular. Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Bir yönetici yetkilendirme işleyicisi oluşturun

*Yetkilendirme* klasöründe bir `ContactAdministratorsAuthorizationHandler` sınıfı oluşturun. `ContactAdministratorsAuthorizationHandler`, kaynak üzerinde davranan kullanıcının bir yönetici olduğunu doğrular. Yönetici, tüm işlemleri yapabilir.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Yetkilendirme işleyicilerini kaydedin

Entity Framework Core kullanan hizmetlerin, [Addkapsamlıdır](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)kullanılarak [bağımlılık ekleme](xref:fundamentals/dependency-injection) için kayıtlı olması gerekir. `ContactIsOwnerAuthorizationHandler`, Entity Framework Core oluşturulan ASP.NET Core [kimliğini](xref:security/authentication/identity)kullanır. İşleyicileri, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla `ContactsController` için kullanılabilir olmaları için hizmet koleksiyonuyla kaydedin. `ConfigureServices`sonuna aşağıdaki kodu ekleyin:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` tekton olarak eklenir. Bunlar, EF kullanmadığından ve gereken tüm bilgiler `HandleRequirementAsync` yönteminin `Context` parametresinde olduğundan tektonlar vardır.

## <a name="support-authorization"></a>Destek yetkilendirme

Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.

### <a name="review-the-contact-operations-requirements-class"></a>Gözden geçirme kişi işlem gereksinimleri sınıfı

`ContactOperations` sınıfını gözden geçirin. Bu sınıf gereksinimleri içeren uygulama destekler:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Kişiler Razor sayfaları için bir temel sınıf oluşturma

Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun. Temel sınıf başlatma kodunu tek bir yerde getirir:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Yukarıdaki kod:

* Yetkilendirme işleyicilerine erişim için `IAuthorizationService` hizmetini ekler.
* Kimlik `UserManager` hizmetini ekler.
* `ApplicationDbContext`ekleyin.

### <a name="update-the-createmodel"></a>Güncelleştirme CreateModel

Create Page model oluşturucusunu `DI_BasePageModel` temel sınıfını kullanacak şekilde güncelleştirin:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

`CreateModel.OnPostAsync` yöntemini şu şekilde güncelleştirin:

* Kullanıcı KIMLIĞINI `Contact` modeline ekleyin.
* Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Güncelleştirme IndexModel

`OnGetAsync` yöntemini yalnızca onaylanan kişilerin genel kullanıcılara gösterilmesi için güncelleştirin:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Güncelleştirme EditModel

Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin. Kaynak yetkilendirmesi doğrulanamadığından `[Authorize]` özniteliği yeterli değildir. Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok. Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir. Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir. Sık sık geçirerek kaynak anahtarı kaynağa erişir.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Güncelleştirme DeleteModel

Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Kimlik doğrulama servisi görünümlere ekleme

Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.

Yetkilendirme hizmetini *Görünümler/_ViewImports. cshtml* dosyasında, tüm görünümlerde kullanılabilir olacak şekilde ekleme:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Yukarıdaki biçimlendirme birkaç `using` deyimi ekler.

*Sayfalar/kişiler/Index. cshtml* 'deki **Düzenle** ve **Sil** bağlantılarını yalnızca uygun izinlere sahip kullanıcılar için işlendikleri şekilde güncelleştirin:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir. Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar. Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack. Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.

### <a name="update-details"></a>Güncelleştirme ayrıntıları

Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Güncelleştirme ayrıntıları sayfa modeli:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Ekleme veya bir rolü için bir kullanıcıyı kaldırma

Hakkında bilgi için [Bu soruna](https://github.com/dotnet/AspNetCore.Docs/issues/8502) bakın:

* Bir kullanıcıdan ayrıcalıklarının kaldırılması. Örneğin, bir sohbet uygulamasındaki kullanıcıyı kapatma.
* Bir kullanıcı ayrıcalıkları ekleniyor.

## <a name="test-the-completed-app"></a>Tamamlanmış uygulamayı test etme

Önceden oluşturulmuş kullanıcı hesapları için bir parola ayarlamadıysanız, parola ayarlamak için [gizli dizi Yöneticisi aracını](xref:security/app-secrets#secret-manager) kullanın:

* Güçlü bir parola seçin: kullanım sekiz veya daha fazla karakter ve en az bir büyük harf karakter, sayı ve simge. Örneğin, `Passw0rd!` güçlü parola gereksinimlerini karşılar.
* Projenin klasöründen aşağıdaki komutu yürütün, burada `<PW>` paroladır:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* Veritabanını bırakma ve güncelleştirme

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.

Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate oturumları) başlatılmasıdır. Tek bir tarayıcıda yeni bir Kullanıcı kaydedin (örneğin, `test@example.com`). Her bir tarayıcıya farklı bir kullanıcı ile oturum açın. Aşağıdaki işlemleri doğrulayın:

* Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.
* Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.
* Yöneticileri Onayla/kişi verilerini reddet. `Details` görünümü **onaylama** ve **reddetme** düğmelerini gösterir.
* Yöneticiler Onayla/Reddet ve tüm verileri düzenleme/silme.

| Kullanıcı                | Uygulama tarafından sağlanmış | Seçenekler                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Hayır                | Kendi veri düzenleme veya silme.                |
| manager@contoso.com | Yes               | Onayla/Reddet ve kendi veri düzenleme/silme. |
| admin@contoso.com   | Yes               | Onayla/Reddet ve tüm verileri düzenleme/silme. |

Bir kişi, yöneticinin tarayıcıda oluşturur. Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin. Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.

## <a name="create-the-starter-app"></a>Başlangıç uygulaması oluşturun

* "ContactManager" adlı bir Razor sayfaları uygulaması oluşturma
  * **Ayrı kullanıcı hesaplarıyla**uygulamayı oluşturun.
  * Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.
  * `-uld`, SQLite yerine LocalDB belirtir

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* *Modeller/Ilgili kişi ekle. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* `Contact` modeli için yapı iskelesi yapın.
* İlk geçiş oluşturun ve veritabanını güncelleştir:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* *Pages/_Layout. cshtml* dosyasındaki **ContactManager** bağlayıcısını güncelleştirin:

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* Oluşturma, düzenleme ve silme kişi uygulamayı test etme

### <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

*Veri* klasörüne [seeddata](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) sınıfını ekleyin.

`Main``SeedData.Initialize` çağrısı:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Test, uygulama veritabanını sağlanmış. DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Ek kaynaklar

* [Azure App Service bir .NET Core ve SQL veritabanı Web uygulaması oluşturun](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core yetkilendirme Laboratuvarı](https://github.com/blowdart/AspNetAuthorizationWorkshop). Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.
* <xref:security/authorization/introduction>
* [Özel ilke tabanlı yetkilendirme](xref:security/authorization/policies)
