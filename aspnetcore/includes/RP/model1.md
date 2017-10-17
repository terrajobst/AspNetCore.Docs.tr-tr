Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, bir veritabanında filmler yönetmek için sınıfları ekleyin. Bu sınıflarla kullandığınız [Entity Framework Çekirdek](https://docs.microsoft.com/ef/core) (EF bir veritabanıyla çalışmak için temel). EF çekirdek yazmak zorunda veri erişim kodu basitleştiren bir nesne ilişkisel eşleme (ORM) çerçevedir.

EF çekirdeği üzerinde herhangi bir bağımlılığı olmadığından oluşturduğunuz modeli sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir. Bunlar veritabanında depolanan veriler için özellikleri tanımlayın.

Bu öğreticide modeli sınıfları ilk yazma ve veritabanı EF çekirdek oluşturur. Burada kapsamında olmayan alternatif bir yaklaşım [varolan bir veritabanından modeli sınıfları oluşturmak](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

[Görüntülemek veya karşıdan](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) örnek.