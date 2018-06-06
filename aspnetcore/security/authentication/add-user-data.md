---
title: Ekle, indirin ve ASP.NET Core projesinde kimliğine özel kullanıcı verilerini sil
author: rick-anderson
description: Özel kullanıcı verilerini kimliğine ASP.NET Core projesinde eklemeyi öğrenin. Veri GDPR başına silin.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: 23fd792c0d93c038f31ce947e7885ad6e36d119e
ms.sourcegitcommit: d4cefc0c63550c64a8040b11867cc05efcfb7e86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34758806"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Ekle, indirin ve ASP.NET Core projesinde kimliğine özel kullanıcı verilerini sil

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu makalede gösterilmektedir nasıl yapılır:

* Özel kullanıcı verilerini bir ASP.NET Core web uygulaması'na ekleyin.
* Özel kullanıcı veri modeli ile işaretleme [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) indirme ve silme işlemini otomatik olarak kullanılabilir olacak şekilde özniteliği. İndirilen ve silinmiş verilerin sağlama yardımcı karşılayan [GDPR](xref:security/gdpr) gereksinimleri.

Proje örnek bir Razor sayfalarının web uygulamasından oluşturuldu, ancak bir ASP.NET Core MVC web uygulaması için benzer bir yönergelerdir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Bir Razor web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**. Proje adı **WebApp1** kendisine istiyorsanız, ad alanı eşleşen [örnek indirme](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kodu.
* Seçin **ASP.NET Core Web uygulaması** > **Tamam**
* Seçin **ASP.NET Core 2.1** açılır
* Seçin **Web uygulaması**  > **Tamam**
* Oluşturun ve projeyi çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

------

## <a name="run-the-identity-scaffolder"></a>Kimlik iskele kurucu çalıştırın

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.
* Sol bölmesinden **İskele Ekle** iletişim kutusunda **kimlik** > **eklemek**.
* İçinde **Ekle kimlik** iletişim kutusunda aşağıdaki seçenekleri:
  * Varolan düzeni dosyanızı seçmek *~/Pages/Shared/_Layout.cshtml*
  * Geçersiz kılmak için aşağıdaki dosyaları seçin:
    * **Hesap/kaydı**
    * **Hesap / / dizini Yönet**
  * Seçin **+** yeni düğmesi **veri bağlamı sınıfı**. Türünü kabul (**WebApp1.Models.WebApp1Context** proje adlandırırsanız **WebApp1**).
  * Seçin **+** yeni düğmesi **kullanıcı sınıfı**. Türünü kabul (**WebApp1User** proje adlandırırsanız **WebApp1**) > **Ekle**.
* Seçin **eklemek**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET iskele kurucu daha önce yüklemediyseniz şimdi yükle:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Paket için bir başvuru ekleyin [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) proje (.csproj) dosyasına. Proje dizininde aşağıdaki komutu çalıştırın:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Kimlik iskele kurucu seçeneklerinden listelemek için aşağıdaki komutu çalıştırın:

```cli
dotnet aspnet-codegenerator identity -h
```

Proje klasöründe kimlik iskele kurucu çalıştırın:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

İçindeki yönergeleri izleyin [geçişler, UseAuthentication ve düzeni](xref:security/authentication/scaffold-identity#efm) aşağıdaki adımları gerçekleştirmek için:

* Bir geçiş oluşturun ve veritabanı güncelleştirin.
* Add `UseAuthentication` to `Startup.Configure`.
* Ekleme `<partial name="_LoginPartial" />` düzeni dosyasına kayıt yapar.
* Uygulamayı test etme:
  * Bir kullanıcı kaydı
  * Yeni bir kullanıcı adı seçin (yanına **oturum kapatma** bağlantı). Pencereyi genişletin veya kullanıcı adı ve diğer bağlantıları göstermek için gezinti çubuğu simgesini seçin gerekebilir.
  * Seçin **kişisel veriler** sekmesi.
  * Seçin **karşıdan** düğmesine tıklayın ve incelenmesi *PersonalData.json* dosya.
  * Test **silmek** oturum açan kullanıcı siler düğmesi.

## <a name="add-custom-user-data-to-the-identity-db"></a>Özel kullanıcı verilerini kimlik DB ekleme

Güncelleştirme `IdentityUser` türetilmiş sınıf özel özelliklere sahip. Projeniz WebApp1 adlı, dosya adında *Areas/Identity/Data/WebApp1User.cs*. Dosya, aşağıdaki kod ile güncelleştirin:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Özellikler donatılmış ile [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) özniteliği şunlardır:

* Ne zaman silineceğini *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor sayfasını çağırır `UserManager.Delete`.
* Tarafından yüklenen veriler dahil *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor sayfası.

### <a name="update-the-accountmanageindexcshtml-page"></a>Güncelleştirme Account/Manage/Index.cshtml sayfası

Güncelleştirme `InputModel` içinde *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* aşağıdaki vurgulanmış kodu:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95)]

Güncelleştirme *Areas/Identity/Pages/Account/Manage/Index.cshtml* aşağıdaki vurgulanmış biçimlendirmeyi ile:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Güncelleştirme Account/Register.cshtml sayfası

Güncelleştirme `InputModel` içinde *Areas/Identity/Pages/Account/Register.cshtml.cs* aşağıdaki vurgulanmış kodu:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Güncelleştirme *Areas/Identity/Pages/Account/Register.cshtml* aşağıdaki vurgulanmış biçimlendirmeyi ile:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Projeyi oluşturun.

### <a name="add-a-migration-for-the-custom-user-data"></a>Özel kullanıcı verileri için bir geçiş ekleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio'da **Paket Yöneticisi Konsolu**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>Test oluşturun, görüntüleyin, indirin, özel kullanıcı verilerini sil

Uygulamayı test etme:

* Yeni bir kullanıcı kaydedin.
* Özel kullanıcı verilerini görüntülemek `/Identity/Account/Manage` sayfası.
* Karşıdan yükle ve kullanıcıların kişisel verileri görüntülemek `/Identity/Account/Manage/PersonalData` sayfası.