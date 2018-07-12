> Uygulama kimlik veri deposu olarak SQLite kullanıyorsa, bazı komutlar desteklenmez. Veritabanı altyapısı, kısıtlamaları nedeniyle `Alter` komutları şu özel durum:
>
> "System.NotSupportedException: SQLite bu geçiş işlemi desteklemiyor." 
>
> Geçici bir çözüm, Code First geçişleri tabloları değiştirmek için veritabanında çalıştırın.
