# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Model bir ASP.NET Core MVC uygulamasına ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [zel Dykstra](https://github.com/tdykstra)

Bu bölümde, bir veritabanında filmler yönetmek için bazı sınıfları ekleyeceksiniz. Bu sınıfların olacak "**M**odel" parçası **M**VC uygulama.

Bu sınıflarla kullandığınız [Entity Framework Çekirdek](/ef/core) (EF bir veritabanıyla çalışmak için temel). EF çekirdek yazmak zorunda veri erişim kodu basitleştiren bir nesne ilişkisel eşleme (ORM) çerçevedir. [EF çekirdek destekleyen birçok veritabanı motoru](/ef/core/providers/).

EF çekirdeği üzerinde herhangi bir bağımlılığı olmadığından oluşturacağınız modeli sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir. Bunlar yalnızca veritabanında depolanan verilerin özelliklerini tanımlayın.

Bu öğreticide, modeli sınıfları ilk yazacaksınız ve EF çekirdek veritabanını oluşturur. Burada kapsamında olmayan alternatif bir yaklaşım, zaten varolan bir veritabanından modeli sınıfları oluşturmaktır. Bu yaklaşımı hakkında daha fazla bilgi için bkz: [ASP.NET Core - var olan veritabanı](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Bir veri modeli sınıfı ekleme
