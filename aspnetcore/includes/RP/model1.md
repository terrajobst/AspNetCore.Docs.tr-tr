Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, bir veritabanında filmler yönetmek için sınıflar ekleyin. Bu sınıflar ile kullandığınız [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core). EF Core, yazmanız gereken veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.

EF Core üzerinde herhangi bir bağımlılığı olmadığından oluşturduğunuz modeli sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir. Bunlar, veritabanında depolanan verilerin özelliklerini tanımlayın.

Bu öğreticide model sınıfları ilk yazma ve EF Core veritabanı oluşturur. Burada ele alınmamaktadır alternatif bir yaklaşım [varolan bir veritabanından model sınıfları oluşturma](/ef/core/get-started/aspnetcore/existing-db).

[Görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) örnek.
