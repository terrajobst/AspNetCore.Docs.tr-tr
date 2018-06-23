> Uygulama kimlik veri deposu olarak SQLite kullanıyorsa bazı komutlar desteklenmez. Veritabanı Altyapısı'ndaki sınırlamaları nedeniyle `Alter` komutları şu özel durum:
>
> "System.NotSupportedException: SQLite bu geçiş işlemini desteklemiyor." 
>
> Geçici bir çözüm, Code First geçişleri tabloları değiştirmek için veritabanı üzerinde çalıştırın.
