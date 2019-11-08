### <a name="use-sqlite-for-development-sql-server-for-production"></a>Geliştirme için SQLite kullanın, üretim için SQL Server

SQLite seçildiğinde, şablon tarafından oluşturulan kod geliştirme için hazırlayın. Aşağıdaki kod, <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> başlangıca nasıl ekleneceğini gösterir. `IWebHostEnvironment`, geliştirme ve üretimde SQL Server `ConfigureServices` SQLite kullanabilmesi için eklenir.

[!code-csharp[](~/includes/RP/code/StartupDevProd.cs?name=snippet&highlight=5,10,14)]
