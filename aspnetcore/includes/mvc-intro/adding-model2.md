## <a name="add-initial-migration-and-update-the-database"></a>İlk geçiş ekleyin ve veritabanını güncelleştirme

* Bir komut istemi açın ve proje dizinine gidin. (Dizinini içeren *haline* dosyası).

* Komut isteminde aşağıdaki komutları çalıştırın:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](https://docs.microsoft.com/dotnet/core/tools/index) .NET platformlar arası uygulamasıdır. Bu komutlar ne aşağıda verilmiştir:

  * [DotNet geri yükleme](/dotnet/core/tools/dotnet-restore): Belirtilen NuGet paketlerini indirir *.csproj* dosya.
  * `dotnet ef migrations add Initial` Entity Framework .NET Core CLI geçişler komutu çalıştırır ve ilk geçiş oluşturur. "Ekle" sonra parametresi geçiş atadığınız bir addır. Burada, ilk veritabanı geçiş olduğundan "Başlangıç" geçiş yeniden adlandırıyorsunuz. Bu işlem oluşturur *veri/geçişleri/\<tarih-saat > _Initial.cs* eklemek için geçiş komutları içeren bir dosya *film* veritabanına tablo.
  * `dotnet ef database update`  Veritabanını yeni oluşturduğumuz geçiş ile güncelleştirir.

Sonraki öğreticide veritabanı ve bağlantı dizesi hakkında bilgi edineceksiniz. Veri modeli değişiklikler hakkında bilgi edineceksiniz [bir alan ekleyebilmek](xref:tutorials/first-mvc-app/new-field) Öğreticisi.
