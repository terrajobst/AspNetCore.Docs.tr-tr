::: moniker range=">= aspnetcore-3.0"

Identity scaffolder öğesini çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.
* **Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.
* **Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.
  * Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır. Varolan bir *\_Layout. cshtml* dosyası **seçildiğinde, üzerine yazılmaz.**

 Örneğin: MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`
* Mevcut veri bağlamınızı kullanmak için, geçersiz kılmak üzere en az bir dosya seçin. Veri bağlamınızı eklemek için en az bir dosya seçmeniz gerekir.
  * Veri bağlamı sınıfınızı seçin.
  * **Add (Ekle)** seçeneğini belirleyin.
* Yeni bir kullanıcı bağlamı oluşturmak ve muhtemelen kimlik için özel bir Kullanıcı sınıfı oluşturmak için:
  * Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.
  * **Add (Ekle)** seçeneğini belirleyin.

Note: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçmeniz gerekmez.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

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

Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın. Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın. DB içeriğiniz için doğru tam adı kullanın:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

PowerShell, bir komut ayırıcısı olarak noktalı virgül kullanır. PowerShell kullanırken, dosya listesinde noktalı virgül karakterini kaçış veya dosya listesini çift tırnak içine koyun. Örneğin:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Identity desteği `--files` bayrağını veya `--useDefaultUI` bayrağını belirtmeden çalıştırırsanız, tüm kullanılabilir kimlik Kullanıcı arabirimi sayfaları projenizde oluşturulur.

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Identity scaffolder öğesini çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.
* **Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.
* **Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.
  * Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır. Varolan bir *\_Layout. cshtml* dosyası **seçildiğinde, üzerine yazılmaz.**

 Örneğin: MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`
* Mevcut veri bağlamınızı kullanmak için, geçersiz kılmak üzere en az bir dosya seçin. Veri bağlamınızı eklemek için en az bir dosya seçmeniz gerekir.
  * Veri bağlamı sınıfınızı seçin.
  * **Add (Ekle)** seçeneğini belirleyin.
* Yeni bir kullanıcı bağlamı oluşturmak ve muhtemelen kimlik için özel bir Kullanıcı sınıfı oluşturmak için:
  * Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.
  * **Add (Ekle)** seçeneğini belirleyin.

Note: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçmeniz gerekmez.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Proje (\*. csproj) dosyasına [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) öğesine bir paket başvurusu ekleyin. Proje dizininde aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın. Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın. DB içeriğiniz için doğru tam adı kullanın:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

PowerShell, bir komut ayırıcısı olarak noktalı virgül kullanır. PowerShell kullanırken, dosya listesinde noktalı virgül karakterini kaçış veya dosya listesini çift tırnak içine koyun. Örneğin:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Identity desteği `--files` bayrağını veya `--useDefaultUI` bayrağını belirtmeden çalıştırırsanız, tüm kullanılabilir kimlik Kullanıcı arabirimi sayfaları projenizde oluşturulur.

---

::: moniker-end