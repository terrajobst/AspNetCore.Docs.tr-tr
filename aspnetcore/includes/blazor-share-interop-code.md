## <a name="share-interop-code-in-a-class-library"></a>Birlikte çalışma kodunu bir sınıf kitaplığında paylaşma

JS birlikte çalışma kodu bir NuGet paketindeki kodu paylaşmanıza olanak sağlayan bir sınıf kitaplığına dahil edilebilir.

Sınıf kitaplığı, yerleşik derlemede JavaScript kaynaklarını katıştırmayı işler. JavaScript dosyaları *Wwwroot* klasörüne yerleştirilir. Araç, kitaplık oluşturulduğunda kaynakları katıştırmaya önem kazanır.

Oluşturulan NuGet paketine, uygulamanın proje dosyasında herhangi bir NuGet paketiyle aynı şekilde başvurulur. Paket geri yüklendikten sonra, uygulama kodu JavaScript 'e, gibi çağrı yapabilir C#.

Daha fazla bilgi için bkz. <xref:blazor/class-libraries>.
