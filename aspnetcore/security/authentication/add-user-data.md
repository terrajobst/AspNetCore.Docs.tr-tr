---
title: ASP.NET Core projesindeki Kullanıcı verilerini kimliğe ekleme, indirme ve silme
author: rick-anderson
description: ASP.NET Core projesindeki kimliğe özel kullanıcı verileri eklemeyi öğrenin. GDPR başına verileri silme.
ms.author: riande
ms.date: 06/18/2019
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 6daca5776930f80eec8d81132b5a5c4d4d5c13ad
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681168"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>ASP.NET Core projesindeki özel kullanıcı verilerini kimliğe ekleme, indirme ve silme

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

Bu makalede nasıl yapılacağı gösterilmektedir:

* ASP.NET Core Web uygulamasına özel kullanıcı verileri ekleyin.
* Özel Kullanıcı veri modelini <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> özniteliğiyle süsleyin, bu nedenle otomatik olarak indirilebilir ve silinmek üzere kullanılabilir. Verilerin indirilip silinebilmesini sağlamak, [GDPR](xref:security/gdpr) gereksinimlerini karşılamaya yardımcı olur.

Proje örneği bir Razor Pages Web uygulamasından oluşturulur, ancak yönergeler bir ASP.NET Core MVC web uygulaması için benzerdir.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Razor Web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin. [İndirme örnek](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kodunun ad alanıyla eşleştirmek Istiyorsanız projeyi **WebApp1** olarak adlandırın.
* **ASP.NET Core Web uygulaması** > **Tamam 'ı** seçin
* Açılan listede **ASP.NET Core 3,0** ' i seçin
* **Web uygulaması** > **Tamam 'ı** seçin
* Projeyi derleyin ve çalıştırın.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin. [İndirme örnek](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kodunun ad alanıyla eşleştirmek Istiyorsanız projeyi **WebApp1** olarak adlandırın.
* **ASP.NET Core Web uygulaması** > **Tamam 'ı** seçin
* Açılan listede **ASP.NET Core 2,2** ' i seçin
* **Web uygulaması** > **Tamam 'ı** seçin
* Projeyi derleyin ve çalıştırın.

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Identity desteği 'ı çalıştırın

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.
* **Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.
* **Kimlik Ekle** iletişim kutusunda aşağıdaki seçenekler:
  * Var olan düzen dosyasını seçin *~/Pages/Shared/_Layout. cshtml*
  * Geçersiz kılmak için aşağıdaki dosyaları seçin:
    * **Hesap/kayıt**
    * **Hesap/yönet/Dizin**
  * Yeni bir **veri bağlamı sınıfı**oluşturmak için **+** düğmesini seçin. Proje **WebApp1**olarak adlandırılmışsa türü (**WebApp1. modeller. WebApp1Context** ) kabul edin.
  * Yeni bir **Kullanıcı sınıfı**oluşturmak için **+** düğmesini seçin. **Ekle**> ( **projenin adı** **WebApp1User** ) öğesini kabul edin.
* **Ekle**' yi seçin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET Core scaffolder ' ı daha önce yüklemediyseniz, şimdi yükleyebilirsiniz:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Proje (. csproj) dosyasına [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) öğesine bir paket başvurusu ekleyin. Proje dizininde aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Identity desteği seçeneklerini listelemek için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

Proje klasöründe, kimlik scaffolder ' ı çalıştırın:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Aşağıdaki adımları gerçekleştirmek için [geçişler, UseAuthentication ve düzen](xref:security/authentication/scaffold-identity#efm) bölümündeki yönergeleri izleyin:

* Bir geçiş oluşturun ve veritabanını güncelleştirin.
* Add `UseAuthentication` to `Startup.Configure`.
* Düzen dosyasına `<partial name="_LoginPartial" />` ekleyin.
* Uygulamayı test etme:
  * Kullanıcı kaydetme
  * Yeni Kullanıcı adını ( **oturum kapatma** bağlantısının yanında) seçin. Kullanıcı adını ve diğer bağlantıları göstermek için pencereyi genişletmeniz veya gezinti çubuğu simgesini seçmeniz gerekebilir.
  * **Kişisel veri** sekmesini seçin.
  * **İndir** düğmesini seçin ve *persondata. JSON* dosyasını inceledi.
  * Oturum açan kullanıcıyı silen **Sil** düğmesini test edin.

## <a name="add-custom-user-data-to-the-identity-db"></a>Identity DB 'ye özel kullanıcı verileri ekleme

`IdentityUser` türetilmiş sınıfı özel özelliklerle güncelleştirin. Projeyi WebApp1 olarak adlandırdıysanız, dosya *Areas/Identity/Data/WebApp1User. cs*olarak adlandırılır. Dosyayı aşağıdaki kodla güncelleştirin:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

[Kişiselleştirme verisi](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) özniteliğiyle düzenlenmiş özellikler şunlardır:

* *Areas/Identity/Pages/Account/Manage/Deletepersonal Data. cshtml* Razor sayfası `UserManager.Delete`çağırdığında silinir.
* *Alan/kimlik/sayfa/hesap/Yönet/Downloadpersonal Data. cshtml* Razor sayfasında indirilen verilere dahildir.

### <a name="update-the-accountmanageindexcshtml-page"></a>Hesap/yönet/Index. cshtml sayfasını Güncelleştir

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

### <a name="update-the-accountregistercshtml-page"></a>Account/Register. cshtml sayfasını Güncelleştir

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

### <a name="add-a-migration-for-the-custom-user-data"></a>Özel Kullanıcı verileri için bir geçiş ekleyin

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio **Paket Yöneticisi konsolunda**:

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Test oluşturma, görüntüleme, indirme, özel kullanıcı verilerini silme

Uygulamayı test etme:

* Yeni bir Kullanıcı kaydedin.
* `/Identity/Account/Manage` sayfasındaki özel kullanıcı verilerini görüntüleyin.
* Kullanıcıların kişisel verilerini `/Identity/Account/Manage/PersonalData` sayfasından indirip görüntüleyin.
