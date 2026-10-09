## GİT & VERSİYON KONTROLÜ

# Git Nedir?
Yazılım geliştirme ve dosya takibi süreçlerinde kullanılan, dünyada en yaygın dağıtık versiyon kontrol sistemidir (DVCS - Distributed Version Control System). 2005 yılında Linux çekirdeğinin yaratıcısı Linus Torvalds tarafından geliştirilmiştir.

# Git’in Mimarisi ve Temel Çalışma Mantığı
Git’i diğer geleneksel sistemlerden ayıran en büyük özellik dağıtık (distributed) olmasıdır. Merkezi bir sunucuya bağımlı değildir; projenin tam bir kopyası (tüm geçmişiyle birlikte) her geliştiricinin kendi bilgisayarında (yerel depoda) bulunur.

 > Git'in 3 Temel Yaşam Alanı (States)

- Working Directory (Çalışma Dizini): Dosyalarınız üzerinde doğrudan değişiklik yaptığınız, kod yazdığınız alandır.

- Staging Area / Index (Hazırlık Alanı): Bir sonraki kayda (commit) dahil etmek istediğiniz değişiklikleri topladığınız ara bölgedir (git add ile buraya aktarılır).

- Local Repository (Yerel Depo): Değişikliklerin sürüm snapshot'ı (anlık görüntüsü) olarak kalıcı şekilde .git klasörüne kaydedildiği yerdir (git commit ile yapılır).

> Git Temel Kavramları
- Repository (Depo / Repo): Projenizin tüm dosyalarını, klasör yapısını ve bütün versiyon geçmişini içeren veri tabanıdır.

- Commit: Projenin belirli bir andaki onaylanmış "fotoğrafı" (snapshot) gibidir. Her commit benzersiz bir benzersiz kimlik koduna (SHA-1 hash) ve bir açıklama mesajına sahiptir.

- Branch (Dal): Projenin ana hattından (main veya master) bağımsız olarak geliştirme yapmanızı sağlayan paralel çalışma hatlarıdır.

- Merge (Birleştirme): Farklı bir dalda yapılan geliştirmeleri ana dala güvenle aktarma işlemidir.

- Remote (Uzak Depo): Kodlarınızın internette veya bir sunucuda barındırıldığı yerdir (örneğin GitHub, GitLab).

> Git, dosyaların zaman içindeki değişimlerini farklar (diff) üzerinden değil, projenin o anki anlık görüntüleri (snapshot) üzerinden takip eden dağıtık bir versiyon kontrol sistemidir.

## 1. Snapshot (Anlık Görüntü) Mantığı

Geleneksel versiyon kontrol sistemleri (CVS, Subversion vb.) veriyi dosya tabanlı değişiklikler (delta-based / diff) olarak tutar. Yani "A dosyasında 5. satır değişti, B dosyasına yeni satır eklendi" şeklinde sadece farkları kaydederler.

# Git Snapshot Nasıl Çalışır?

- Her commit yaptığınızda, Git projenizin o anki tüm dosya yapısının anlık bir fotoğrafını (snapshot) çeker.

- Verimlilik & Performans: Eğer bir dosya iki commit arasında değişmediyse, Git o dosyayı tekrar kopyalamaz; sadece bir önceki versiyonuna bir bağlantı (pointer/link) oluşturur. Bu sayede depolama alanından büyük tasarruf sağlar.

- Bütünlük (Integrity): Git'teki her snapshot, iceriğindeki dosyalara göre SHA-1 (veya SHA-256) algoritması ile hesaplanan 40 karakterlik benzersiz bir hash kimliğine (a1b2c3d...) sahip olur. Bu sayede veride en ufak bir bozulma veya değişiklik olursa Git bunu anında tespit eder.

## 2. Commit (İşleme / Kayıt)

Bir Commit, projenizin tarih çizelgesindeki tek bir "snapshot" düğümüdür. Projenizde yaptığınız mantıksal bir değişikliği kalıcı hale getirme işlemidir.

# Commit İçi Mimarisi
Bir commit objesi arka planda şunları içerir:

- Tree Objesi: O anki dosya ve klasör yapısının (snapshot) SHA-1 referansı.

- Yazar ve Taahhüt Eden Bilgisi: Değişikliği kimin, ne zaman yaptığı (Name, Email, Timestamp).

- Commit Mesajı: Yapılan değişikliğin ne olduğunu açıklayan metin.

- Ebeveyn (Parent) Commit: Kendisinden bir önceki commit'in SHA-1 kimliği (İlk commit hariç her commit'in en az bir ebeveyni vardır; merge commit'lerinin 2 ebeveyni olur).

# Git'in 3 Temel Alanı (Three Trees)
Bir değişikliğin commit haline gelmesi 3 aşamadan geçer:

- Working Directory (Çalışma Dizini): Bilgisayarınızda dosyaları düzenlediğiniz canlı alan.

- Staging Area / Index (Hazırlık Alanı): git add yaptığınızda, bir sonraki snapshot'a nelerin dahil edileceğini seçtiğiniz ara katman.

- Repository (Depo / .git dizini): git commit yaptığınızda snapshot'ın kalıcı olarak veritabanına kaydedildiği yer.

## 3. Branch (Dal / Branş)
Pek çok versiyon kontrol sisteminde "branch" oluşturmak, tüm projenin dosyalarını başka bir klasöre kopyalamak anlamına geldiği için maliyetli ve yavaştır. Git'te ise branch oluşturmak neredeyse anlıktır (milisaniyeler sürer).

# Git'te Branch Aslında Nedir?
Git'te bir branch, belirli bir commit'i işaret eden basit, hareketli bir göstergedir (pointer). Sadece 41 baytlık bir metin dosyasıdır (içinde işaret ettiği commit'in SHA-1 hash'i yazar).

- main / master: Projenin ana hattını temsil eden varsayılan pointer'dır.

- HEAD: Git'in o anda hangi branch/commit üzerinde çalıştığınızı takip etmesini sağlayan özel bir pointer'dır.

# Yeni Branch Oluşturma ve Commit Atma

Yeni bir branch oluşturduğunuzda ("git branch feature"), Git yeni bir dosya oluşturup "HEAD'in" baktığı commit'i işaret ettirir.

Yeni branch'e geçip ("git checkout feature" veya "git switch feature") yeni bir commit attığınızda:

1- Yeni commit oluşturulur ve ebeveyni olarak eski commit ayarlanır.

2- "feature" pointer'ı otomatik olarak yeni commit'e ilerler.

3-"main" pointer'ı eski yerinde sabit kalır.

# Branch Birleştirme (Merging)
İşiniz bittiğinde branch'leri birleştirirken Git iki yöntem kullanır:

- > Fast-Forward: 
Eğer main branch'inde yeni bir commit atılmadıysa, Git sadece main pointer'ını feature'ın olduğu commit'e ileri sarar.

- > 3-Way Merge: 
-Eğer main ve feature ayrıştıktan sonra her ikisine de yeni commit'ler atıldıysa, Git iki branch'in ortak atası (Common Ancestor) ile iki uç commit'i karşılaştırarak yeni bir Merge Commit oluşturur.

- > Git versiyon kontrol sisteminde git merge ve git rebase, bir dalda (branch) yapılan değişiklikleri başka bir dala aktarma ve entegre etme amacına hizmet eden iki temel komuttur.

İki komut günün sonunda aynı nihai sonuca (kodların birleşmesine) ulaşsa da, bunu tarihçeyi (commit history) oluşturma biçimleri açısından tamamen farklı yollarla yaparlar.

# 1. Git Merge (Birleştirme)
"merge" , hedef daldaki geçmişi olduğu gibi koruyarak kaynak daldaki değişiklikleri yeni bir "Merge Commit" (birleştirme commiti) oluşturarak aktarır.

- >Nasıl Çalışır?
- İki dalın son durumlarını ve ortak ata (common ancestor) commit'lerini alır, bunları 3 yönlü bir birleştirme (3-way merge) ile bir araya getirir.

- >Tarihçe: 
-Tahribatsızdır (non-destructive). Projenin gerçek zaman çizelgesini, kimin ne zaman neyi birleştirdiğini eksiksiz korur.

- >Dezavantajı: 
Çok fazla dalın sık sık birleştirildiği projelerde commit geçmişi karmaşık, dallanıp budaklanan bir ağ görüntüsüne ("dallanma çöplüğü") dönüşebilir.

# 2. Git Rebase (Yeniden Konumlandırma)

"rebase", geliştirdiğiniz dalın başlangıç noktasını (base), hedef dalın en son commit'ine taşır. Bu işlem sırasında kendi commit'leriniz yeniden hesaplanır ve yeni hash'lerle sırayla hedef dalın üzerine eklenir.

- >Nasıl Çalışır? 
"feature" dalındaki commit'lerinizi geçici olarak bir kenara koyar, dalınızı hedef dalın (main) en son haline getirir, ardından kenara koyduğu commit'leri tek tek yeni tabanın üzerine uygular.

- >Tarihçe:
- Doğrusal (linear) ve çok temiz bir geçmiş oluşturur. Sanki tüm geliştirmeler sırayla ve tek bir çizgide yapılmış gibi görünür.

- >Dezavantajı: 
-Commit hash'leri değiştiği için projenin gerçek kronolojik geçmişini yeniden yazar.

# Rebase'i Nerede Kullanmamalısınız?
- Ortak kullanılan (public/shared) ana dallarda (örneğin main, master, dev) ASLA rebase yapmayın.

- Eğer başka geliştiricilerin de üzerinde çalıştığı veya kendi yerel kodlarını türettiği uzak bir dala rebase yaparsanız, geçmiş değiştiği için ekip arkadaşlarının çalışma ağaçları bozulur ve ciddi senkronizasyon krizleri yaşanır.

- > Ne Zaman Hangisi Tercih Edilmeli?

"git rebase" tercih edin:

- Kendi lokal Feature dalınızı, main dalındaki en güncel değişikliklerle tazelemek istediğinizde.

- PR (Pull Request) açmadan önce commit geçmişinizi temizlemek ve düzenlemek istediğinizde.

"git merge" tercih edin:

- Bir Feature dalını ana dala (main/production) tamamen entegre edeceğiniz zaman.

- Projenin tarihçesinin tarihsel olarak tam ve doğru kalması kritik olduğunda.

## Pull Request İnceleme Nasıl Yapılır?

Pull Request (PR) incelemesi (Code Review), yazılan kodun ana dala (main/master) eklenmeden önce kalite, güvenlik, performans ve mimari standartlar açısından kontrol edilmesini sağlar.

- > İyi bir PR incelemesi iki ana boyuttan oluşur: Teknik Kontrol Listesi ve İletişim & Etiketsel Süreç.

# 1. Context ve Dokümantasyon İncelemesi

- İlk aşama, kodun ne amaçla yazıldığını ve hangi problemi çözmeyi hedeflediğini anlamaktır.

- > İş Gereksinimlerinin Anlaşılması: 
PR başlığı, açıklaması ve bağlı olduğu görev/issue kartı okunarak yapılmak istenen değişiklik netleştirilir.

- > Kapsam Sınırlarının Belirlenmesi: 
Kodun sadece istenen işle ilgili olup olmadığı, gereksiz veya ilgisiz değişikliklerin (örneğin alakasız dosyalardaki format düzeltmeleri) PR'a dahil edilip edilmediği kontrol edilir.

## 2. Çalışma Ortamı ve Test Kontrolü
- Kodun teorik olarak doğru görünmesi yeterli değildir; pratikte de sorunsuz çalıştığından emin olunur.

- >Lokalde veya Önizleme Ortamında Çalıştırma:
 İhtiyaç halinde ilgili kod dalı (branch) yerele çekilir veya staging/preview ortamında canlı olarak test edilir.

- >Görsel ve Davranışsal Doğrulama: 
Kullanıcı arayüzü (UI) veya kullanıcı deneyimini (UX) etkileyen bir değişiklik varsa, tasarım standartlarına uygunluğu ve ekran çıktıları kontrol edilir.

## 3. Otomatik Tarama ve Otomasyon Kontrolleri

- Manuel incelemeye geçmeden önce sistemin otomatik olarak yaptığı testler değerlendirilir.

- >CI/CD Pipeline Durumu: 
Sürekli entegrasyon (CI) hattındaki otomatik testlerin (Unit, Integration) başarıyla geçip geçmediğine bakılır.

- >Statik Kod Analizi ve Linter: 
Kod formatı, girintileme kuralları, kullanılmayan değişkenler ve temel kodlama hatalarının Linter araçları tarafından onaylandığı doğrulanır.

## 4. Detaylı Satır Satır Kod İncelemesi
-Bu aşama, incelemenin çekirdeğini oluşturur. Kod dosyaları açılır ve satır satır aşağıdaki kriterlere göre değerlendirilir:

- > İşlevsellik ve Hata Tespiti:
 Mantıksal hatalar, uç durumlar (edge cases), boş/hatalı veri tipleri (null/undefined) ve sınır değerlerin doğru yönetilip yönetilmediği denetlenir.

-> Kod Kalitesi ve Standartlar:
 Değişken ve fonksiyon isimlerinin açıklayıcılığı, kod tekrarı (DRY prensibi) ve fonksiyonların tek bir sorumluluğa sahip olması (Single Responsibility) kontrol edilir.

- > Güvenlik ve Performans: 
Kod içinde unutulmuş hassas veriler (API anahtarları, şifreler), SQL Injection veya XSS riskleri araştırılır. Veritabanı sorgularının ve bellek kullanımının (örneğin gereksiz döngüler veya N+1 problemleri) optimize edilip edilmediği bakılır.

## 5. Geribildirim ve Karar Verilmesi
- Yapılan tespitler ve öneriler geliştiriciye iletilerek PR inceleme süreci sonlandırılır.

- > Yapıcı Yorumlar Ekleme: 
Tespit edilen eksikler veya geliştirme alanları net, saygılı ve kodun kendisine odaklanan bir dille ifade edilir. Değişiklik önerileri gerekçeleriyle sunulur.

- > Geri Bildirim Etiketleme: 
Yorumlar önem derecesine göre ayrıştırılır; düzeltilmesi zorunlu olan kritik noktalar (blocking/must fix), sadece bir öneri niteliğinde olanlar (suggestion) veya küçük stil hataları (nitpick) şeklinde netleştirilir.

- > Final Kararını Belirleme: 
Kodun durumuna göre sistem üzerinden Approve (Onayla), Request Changes (Değişiklik İstet) veya Comment (Sadece Yorum Yap) seçeneklerinden biri seçilerek işlem tamamlanır.


# Versiyon Kontrolü Nedir?
Versiyon kontrolü , projenizde (kodlar, raporlar, metin dosyaları) zaman içinde yapılan değişiklikleri adım adım kaydeden, kimin ne zaman neyi değiştirdiğini izleyen ve ihtiyaç duyulduğunda eski sürümlere sorunsuz dönmenizi sağlayan bir sistemdir.

- > Versiyon Kontrol Sistemi (VCS) Nedir ve Neden Gereklidir?

Versiyon kontrolü olmadan proje yürütmek, dosyaları "proje_son.zip", "proje_son_v2.zip", "proje_gercek_son.zip" gibi isimlerle saklamaya benzer. Bu yaklaşım karmaşaya, veri kaybına ve ekip çalışmalarında kod çakışmalarına yol açar.

# Versiyon Kontrolünün Sağladığı Temel Avantajlar

- >Zaman Yolculuğu (History & Rollback): 
- Projenizin 6 ay önceki veya 10 dakika önceki çalışan haline tek bir komutla geri dönebilirsiniz.

- >Geri Bildirim ve Sorumluluk: 
- Hangi satırın kimin tarafından, ne zaman ve hangi gerekçeyle değiştirildiğini görebilirsiniz.

- >Deneysel Çalışma Esnekliği: 
-Ana projenizi riske atmadan yeni özellikler (features) deneyebilir, beğenmezseniz tamamen silebilirsiniz.

- >Eşzamanlı İş Birliği: 
- Aynı dosya üzerinde birden fazla geliştirici aynı anda çalışabilir; sistem değişiklikleri çakışma olmadan birleştirir.