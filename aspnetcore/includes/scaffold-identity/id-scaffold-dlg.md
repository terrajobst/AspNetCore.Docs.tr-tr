Identity scaffolder öğesini çalıştırın:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.
* **Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.
* **Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.
  * Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır. Örneğin, MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`
  * Yeni bir **veri bağlamı sınıfı**oluşturmak için **+** düğmesini seçin.
* **Ekle**' yi seçin.

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Gerekli NuGet paket başvurularını proje (\*. csproj) dosyasına ekleyin. Proje dizininde aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın. Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
