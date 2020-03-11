---
title: Ekleme, indirmek ve bir ASP.NET Core projesi kimliği için kullanıcı verilerini silme
author: rick-anderson
description: Bir ASP.NET Core projesi içinde kimlik için özel kullanıcı veri eklemeyi öğrenin. GDPR başına verileri silin.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 7a67f55da0e685ed3fd5badb30e8be683411a5ae
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662689"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Ekleme, indirmek ve kimlik için bir ASP.NET Core projesi özel kullanıcı verilerini silme

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu makale nasıl yapılır:

* ASP.NET Core web uygulaması için özel kullanıcı verilerini ekleyin.
* Özel Kullanıcı veri modelini <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> özniteliğiyle işaretleyin, bu nedenle otomatik olarak indirilebilir ve silinirler. Verilerin indirilip silinebilmesini sağlamak, [GDPR](xref:security/gdpr) gereksinimlerini karşılamaya yardımcı olur.

Bir Razor sayfaları web uygulaması proje örneği oluşturulur, ancak yönergeleri ASP.NET Core MVC web uygulaması için benzerdir.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Razor web uygulaması oluşturma

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin. [İndirme örnek](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kodunun ad alanıyla eşleştirmek Istiyorsanız projeyi **WebApp1** olarak adlandırın.
* **ASP.NET Core Web uygulaması** > **Tamam 'ı** seçin
* Açılan listede **ASP.NET Core 3,0** ' i seçin
* **Web uygulaması** > **Tamam 'ı** seçin
* Derleme ve projeyi çalıştırın.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin. [İndirme örnek](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kodunun ad alanıyla eşleştirmek Istiyorsanız projeyi **WebApp1** olarak adlandırın.
* **ASP.NET Core Web uygulaması** > **Tamam 'ı** seçin
* Açılan listede **ASP.NET Core 2,2** ' i seçin
* **Web uygulaması** > **Tamam 'ı** seçin
* Derleme ve projeyi çalıştırın.

::: moniker-end


# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Kimlik iskele kurucu çalıştırın

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.
* **Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.
* **Kimlik Ekle** iletişim kutusunda aşağıdaki seçenekler:
  * Var olan düzen dosyasını seçin *~/Pages/Shared/_Layout. cshtml*
  * Aşağıdaki dosyalar geçersiz kılmak için seçin:
    * **Hesap/kayıt**
    * **Hesap/yönet/Dizin**
  * Yeni bir **veri bağlamı sınıfı**oluşturmak için **+** düğmesini seçin. Proje **WebApp1**olarak adlandırılmışsa türü (**WebApp1. modeller. WebApp1Context** ) kabul edin.
  * Yeni bir **Kullanıcı sınıfı**oluşturmak için **+** düğmesini seçin. **Ekle**> ( **projenin adı** **WebApp1User** ) öğesini kabul edin.
* **Add (Ekle)** seçeneğini belirleyin.

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Proje (. csproj) dosyasına [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) öğesine bir paket başvurusu ekleyin. Proje dizininde aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

Proje klasöründe kimlik iskele kurucu çalıştırın:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Aşağıdaki adımları gerçekleştirmek için [geçişler, UseAuthentication ve düzen](xref:security/authentication/scaffold-identity#efm) bölümündeki yönergeleri izleyin:

* Bir geçiş oluşturmak ve veritabanı güncelleştirin.
* Add `UseAuthentication` to `Startup.Configure`.
* Düzen dosyasına `<partial name="_LoginPartial" />` ekleyin.
* Uygulamayı test edin:
  * Bir kullanıcı kaydı
  * Yeni Kullanıcı adını ( **oturum kapatma** bağlantısının yanında) seçin. Penceresini genişletin veya kullanıcı adı ve diğer bağlantıları göstermek için gezinti çubuğu simgesini gerekebilir.
  * **Kişisel veri** sekmesini seçin.
  * **İndir** düğmesini seçin ve *persondata. JSON* dosyasını inceledi.
  * Oturum açan kullanıcıyı silen **Sil** düğmesini test edin.

## <a name="add-custom-user-data-to-the-identity-db"></a>Özel kullanıcı verilerini kimlik Veritabanına Ekle

`IdentityUser` türetilmiş sınıfı özel özelliklerle güncelleştirin. Projeyi WebApp1 olarak adlandırdıysanız, dosya *Areas/Identity/Data/WebApp1User. cs*olarak adlandırılır. Dosya, aşağıdaki kod ile güncelleştirin:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

[Personal Data](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) özniteliğiyle birlikte bulunan özellikler şunlardır:

* *Areas/Identity/Pages/Account/Manage/Deletepersonal Data. cshtml* Razor sayfası `UserManager.Delete`çağırdığında silinir.
* *Alan/kimlik/sayfa/hesap/Yönet/Downloadpersonal Data. cshtml* Razor sayfasında indirilen verilere dahildir.

### <a name="update-the-accountmanageindexcshtml-page"></a>Account/Manage/Index.cshtml sayfası

*Areas/kimlik/sayfa/hesap/Manage/Index. cshtml. cs* içindeki `InputModel` şu vurgulanmış kodla güncelleştirin:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

*Bölgeleri/kimliği/sayfaları/hesabı/Yönet/Index. cshtml* 'yi aşağıdaki vurgulanmış işaretlerle güncelleştirin:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

*Bölgeleri/kimliği/sayfaları/hesabı/Yönet/Index. cshtml* 'yi aşağıdaki vurgulanmış işaretlerle güncelleştirin:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>Account/Register.cshtml sayfası

*Areas/kimlik/sayfa/hesap/kayıt. cshtml. cs* ' deki `InputModel` aşağıdaki vurgulanmış kodla güncelleştirin:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

*Areas/Identity/Pages/Account/Register. cshtml* ' i aşağıdaki vurgulanmış işaretlerle güncelleştirin:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

*Areas/Identity/Pages/Account/Register. cshtml* ' i aşağıdaki vurgulanmış işaretlerle güncelleştirin:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


Projeyi oluşturun.

### <a name="add-a-migration-for-the-custom-user-data"></a>Özel kullanıcı verileri için bir geçiş ekleyin

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio **Paket Yöneticisi konsolunda**:

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Test oluşturun, görüntüleyin, indirin, özel kullanıcı verilerini sil

Uygulamayı test edin:

* Yeni bir kullanıcı kaydedin.
* `/Identity/Account/Manage` sayfasındaki özel kullanıcı verilerini görüntüleyin.
* Kullanıcıların kişisel verilerini `/Identity/Account/Manage/PersonalData` sayfasından indirip görüntüleyin.
