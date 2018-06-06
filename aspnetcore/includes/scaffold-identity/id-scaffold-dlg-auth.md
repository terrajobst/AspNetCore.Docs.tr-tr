Kimlik iskele kurucu çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.
* Sol bölmesinden **İskele Ekle** iletişim kutusunda **kimlik** > **eklemek**.
* İçinde **Ekle kimlik** iletişim kutusunda, kullanmak istediğiniz seçenekleri seçin.
  * Var olan düzen sayfası seçin veya düzeni dosyanızı olan hatalı biçimlendirme üzerine yazılır. Var olan bir _Layout.cshtml dosyasını seçildiğinde olduğu **değil** üzerine.

 Örneğin `~/Pages/Shared/_Layout.cshtml` Razor sayfalarının `~/Views/Shared/_Layout.cshtml` MVC projeleri için
* Mevcut veri içeriğiniz kullanmak için geçersiz kılmak için en az bir dosya seçin. Veri bağlamı eklemek için en az bir dosya seçmelisiniz.
  * Veri bağlamı sınıfı seçin.
  * Seçin **eklemek**.
* Yeni kullanıcı bağlamı oluşturmak ve büyük olasılıkla bir özel kullanıcı sınıfı için kimlik oluşturmak için:
  * Seçin **+** yeni düğmesi **veri bağlamı sınıfı**.
  * Seçin **eklemek**.

Not: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçin gerekmez.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET iskele kurucu daha önce yüklemediyseniz şimdi yükle:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Paket için bir başvuru ekleyin [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) projeye (\*.csproj) dosyası. Proje dizininde aşağıdaki komutu çalıştırın:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Kimlik iskele kurucu seçeneklerinden listelemek için aşağıdaki komutu çalıştırın:

```cli
dotnet aspnet-codegenerator identity -h
```

Proje klasöründe kimlik iskele kurucu istediğiniz seçenekleriyle çalıştırın. Örneğin, varsayılan UI kimlikle ve dosyaları minimum sayısını ayarlamak için aşağıdaki komutu çalıştırın. DB bağlamı için doğru tam ad kullanın:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
