# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulaması için bir model ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Tom Dykstra](https://github.com/tdykstra)

Bu bölümde, bir veritabanında filmler yönetmek için bazı sınıflar ekleyeceksiniz. Bu sınıflar olacaktır "**M**odel" parçası **M**VC uygulama.

Bu sınıflar ile kullandığınız [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core). EF Core, yazmanız gereken veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir. [EF Core birçok veritabanı altyapılarını destekleyen](/ef/core/providers/).

EF Core üzerinde herhangi bir bağımlılığı olmadığından oluşturacağınız model sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir. Bunlar yalnızca veritabanında depolanan verilerin özelliklerini tanımlayın.

Bu öğreticide, model sınıfları ilk yazacaksınız ve EF Core veritabanı oluşturacaksınız. Burada ele alınmamaktadır alternatif bir yaklaşım, model sınıfları zaten varolan bir veritabanı oluşturmaktır. Bu yaklaşımı hakkında daha fazla bilgi için bkz. [ASP.NET Core - mevcut veritabanı](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Bir veri modeli sınıfı Ekle
