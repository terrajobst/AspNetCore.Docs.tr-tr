---
title: Ekleme, indirmek ve bir ASP.NET Core projesi kimliği için kullanıcı verilerini silme
author: rick-anderson
description: Bir ASP.NET Core projesi içinde kimlik için özel kullanıcı veri eklemeyi öğrenin. GDPR başına verileri silin.
ms.author: riande
ms.date: 6/16/2018
ms.custom: seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 8c0413a16d92b717619387748ee78f0d14d6c852
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516215"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Ekleme, indirmek ve kimlik için bir ASP.NET Core projesi özel kullanıcı verilerini silme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu makale nasıl yapılır:

* ASP.NET Core web uygulaması için özel kullanıcı verilerini ekleyin.
* Özel kullanıcı veri modeli ile donatmak [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) indirme ve silme için otomatik olarak kullanılabilir olacak şekilde özniteliği. Verileri silinir ve indirilen mümkün hale yardımcı karşılamak [GDPR](xref:security/gdpr) gereksinimleri.

Bir Razor sayfaları web uygulaması proje örneği oluşturulur, ancak yönergeleri ASP.NET Core MVC web uygulaması için benzerdir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/2.2-SDK.md)]

## <a name="create-a-razor-web-app"></a>Razor web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**. Projeyi adlandırın **WebApp1** için istiyorsanız ad alanı eşleşen [örneği indirin](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kod.
* Seçin **ASP.NET Core Web uygulaması** > **Tamam**
* Seçin **ASP.NET Core 2.2** açılır
* Seçin **Web uygulaması**  > **Tamam**
* Derleme ve projeyi çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Kimlik iskele kurucu çalıştırın

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.
* Sol bölmeden **İskele Ekle** iletişim kutusunda **kimlik** > **ekleme**.
* İçinde **ADD kimliğini** iletişim kutusunda, aşağıdaki seçenekleri:
  * Varolan bir düzen dosyası seçin *~/Pages/Shared/_Layout.cshtml*
  * Aşağıdaki dosyalar geçersiz kılmak için seçin:
    * **Hesabı/kaydı**
    * **Hesabı/yönetmek/dizin**
  * Seçin **+** yeni bir düğme **veri bağlamı sınıfının**. Tür kabul et (**WebApp1.Models.WebApp1Context** proje adlandırılmışsa **WebApp1**).
  * Seçin **+** yeni bir düğme **kullanıcı sınıfı**. Tür kabul et (**WebApp1User** proje adlandırılmışsa **WebApp1**) > **Ekle**.
* Seçin **ekleme**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Paket başvurusu ekleme [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) proje (.csproj) dosyasına. Proje dizininde aşağıdaki komutu çalıştırın:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:

```cli
dotnet aspnet-codegenerator identity -h
```

Proje klasöründe kimlik iskele kurucu çalıştırın:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Verilen yönergeleri izleyin [düzeni geçişleri ve UseAuthentication](xref:security/authentication/scaffold-identity#efm) aşağıdaki adımları gerçekleştirmek için:

* Bir geçiş oluşturmak ve veritabanı güncelleştirin.
* Add `UseAuthentication` to `Startup.Configure`.
* Ekleme `<partial name="_LoginPartial" />` Düzen dosyası için.
* Uygulamayı test edin:
  * Bir kullanıcı kaydı
  * Yeni kullanıcı adını seçin (yanındaki **oturum kapatma** bağlantı). Penceresini genişletin veya kullanıcı adı ve diğer bağlantıları göstermek için gezinti çubuğu simgesini gerekebilir.
  * Seçin **kişisel verileri** sekmesi.
  * Seçin **indirme** düğmesine tıklayın ve denetlenen *PersonalData.json* dosya.
  * Test **Sil** düğmesi, oturum açmış kullanıcıyı siler.

## <a name="add-custom-user-data-to-the-identity-db"></a>Özel kullanıcı verilerini kimlik Veritabanına Ekle

Güncelleştirme `IdentityUser` türetilmiş sınıf özel özelliklere sahip. ' % S'proje WebApp1 adlı, dosyanın nasıl adlandırıldığı *Areas/Identity/Data/WebApp1User.cs*. Dosya, aşağıdaki kod ile güncelleştirin:

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Data/WebApp1User.cs)]

Düzenlenmiş özellikler ile [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) öznitelik şunlardır:

* Ne zaman silinmiş *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor sayfası çağırır `UserManager.Delete`.
* Tarafından yüklenen veriler dahil *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor sayfası.

### <a name="update-the-accountmanageindexcshtml-page"></a>Account/Manage/Index.cshtml sayfası

Güncelleştirme `InputModel` içinde *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* aşağıdaki vurgulanmış kodu:

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Güncelleştirme *Areas/Identity/Pages/Account/Manage/Index.cshtml* aşağıdaki vurgulanmış biçimlendirmeye sahip:

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

### <a name="update-the-accountregistercshtml-page"></a>Account/Register.cshtml sayfası

Güncelleştirme `InputModel` içinde *Areas/Identity/Pages/Account/Register.cshtml.cs* aşağıdaki vurgulanmış kodu:

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Güncelleştirme *Areas/Identity/Pages/Account/Register.cshtml* aşağıdaki vurgulanmış biçimlendirmeye sahip:

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Projeyi oluşturun.

### <a name="add-a-migration-for-the-custom-user-data"></a>Özel kullanıcı verileri için bir geçiş ekleyin

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio **Paket Yöneticisi Konsolu**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Test oluşturun, görüntüleyin, indirin, özel kullanıcı verilerini sil

Uygulamayı test edin:

* Yeni bir kullanıcı kaydedin.
* Özel kullanıcı veri görünümünde `/Identity/Account/Manage` sayfası.
* İndirme ve görüntüleme kullanıcıların kişisel verilerden `/Identity/Account/Manage/PersonalData` sayfası.
