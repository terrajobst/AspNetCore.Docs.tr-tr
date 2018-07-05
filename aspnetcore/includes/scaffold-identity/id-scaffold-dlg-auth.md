Kimlik iskele kurucu çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.
* Sol bölmeden **İskele Ekle** iletişim kutusunda **kimlik** > **ekleme**.
* İçinde **ADD kimliğini** iletişim kutusunda, istediğiniz seçenekleri seçin.
  * Varolan bir düzen sayfası seçin veya hatalı biçimlendirme Düzen dosyanızın üzerine yazılacak. Varolan _Layout.cshtml dosyasını seçildiğinde olduğu **değil** üzerine.

 Örneğin `~/Pages/Shared/_Layout.cshtml` Razor sayfaları için `~/Views/Shared/_Layout.cshtml` MVC projeleri
* Mevcut veri Bağlamınızı kullanmak için geçersiz kılmak için en az bir dosya seçin. Veri Bağlamınızı eklemek için en az bir dosya seçmelisiniz.
  * Veri bağlamı sınıfı seçin.
  * Seçin **ekleme**.
* Yeni bir kullanıcı bağlam oluşturur ve muhtemelen kimlik için bir özel kullanıcı sınıfı oluşturmak için:
  * Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.
  * Seçin **ekleme**.

Not: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçmek gerekmez.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Paket başvurusu ekleme [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) projesine (\*.csproj) dosyası. Proje dizininde aşağıdaki komutu çalıştırın:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:

```cli
dotnet aspnet-codegenerator identity -h
```

Proje klasöründe kimlik iskele kurucu istediğiniz seçeneği ile çalıştırın. Örneğin, kimliği ' % s'varsayılan kullanıcı Arabirimi ile ve en az sayıda dosya kurmak için aşağıdaki komutu çalıştırın. DB Bağlamınızı doğru tam adını kullanın:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
