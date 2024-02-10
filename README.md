# hadoop
HADOOP NEDİR?

Apache hadoop çok sayıda sunucuları kullanarak oluşturulan cluster yapısı üzerinde büyük verilerin işlenerek kullanılacak uygulamalara bir yapı(altyapı)  sağlayan HDFS olarak adlandırılan bir dağıtık dosya sistemidir.

HDFS  hadoop modüllerinden biridir .Sunucuların disklerini kullanarak bunlar tek ve büyük bir  disk alanı oluşturur. Oluşturulan disk sayesinde çok büyük boyutta bir çok dosya sisteme aktarılabilir.Yapılan bu işlem sayesinde veri  yedeklenebilir ve verinin güvenliği sağlanmış olur.Aktarılan bu dosyalar bloklar halinde birden fazla sunucu üzerinde dağıtılır.HDFS bu şekilde çalışmasını temel olarak iki yapı oluşturur.

NAMENODE:

Genel olarak ana yani master olarak tanımlayabiliriz.Çünkü HDFS’ in kalbi,merkez noktasıdır diyebiliriz.
-HDFS’in tüm dosya ve dizinleri için meta bilgilerini,dosyanın ya da dizinin adı,boyutu,sahibi,izinleri gibi karakteristik bilgilerini içerir(meta data).Bu meta bilgileri sayesinde HDFS dosya ve dizinlerin durumlarını rahat bir şekilde takip edebiliriz.
-Dosya açma ,oluşturma silme,kopyalama gibi (dosya sistemlerine yönelik tüm işlemlerde diyebiliriz) işlevleri yapar.
- HDFS bloklar halinde veri dağıttını söylemiştik.Namenode bu blokların  datanode yardımıyla blokların sakladığı (yani verilerin depolandığı bloklar) takip edebilir ve onların erişimini kontrol eder.Aynı zamanda blokları koordine eder.
- Namenode kendisi gibi birden fazla namenode oluşturabilir.Bu sayede oluşan bir arızada diğer namenode işlemleri devralabilir.Bu da kesintisiz bir hizmet sunar.(bu yönüyle baktığımızda yüksek kullanılabilirlik gibi bir avantajı olduğunu da söyleyebiliriz.)
- Namenode bunlar dışında snapshot oluşturulduğunda namenode, tüm dosya ve dizinleri anlık durumunu görüntüleme tablosuna aktarır ve orada saklar. Bu tablo her dosya ve dizin için geçerli olan durum neyse onu kaydeder.
-Namenode bu görüntüleme tablosunu kullanarak kullanıcı herhangi bir snapshotu geri yükleyebilmesini de sağlar denebilir.
- Yani bu maddelerden de anlaşılacağı üzere snapshot’ ların oluşturulması ve yönetilmesinden sorumlu kontrol merkezi diyebilir.HDFS üzerindeki dosya ve dizinlerin durumunu izler ve bu durumları snapshotlar aracılığıyla korur.






DATANODE:

Temel olarak blokları saklamak olan bir süreç olarak görülür.Bunu biraz daha detaylandırırsak aslında gerçek veriyi depolayan node’ lardır.Burada ki gerçek veri terimi yerel diskinde olan veridir.her datanode kendi yerel diskinden sorumludur.Bunun dışında diğer datanodelarında verisini saklarlar.
Bu sayede veri kaybı ya da silinmesi durumda verinin daima 
yedeği bulunur.
-Namenode yönlendirmesiyle datanode üzerinde kopyalanan blokların HDFS üzerinde dosya oluşturmak için kullanılır.
-Snapshot’ lar  oluşturulduğunda datanode extra bir depolama alanı oluşur.Bu yeni alan sayesinde snapshot orjinal veriyi değiştirmeksiniz korur.
-Snapshot çalıştığı zaman,datanode dosyaların orjinal kopyalarını ve snapshot değişikliklerini saklar.bu saklamanın orjinal ve anlık veri olarak kullanıcıya görme imkanı sağlıyormuş gibi düşünebiliriz.
- Datanode snapshot üzerine yazılabilir yapıda çalışabilir.Şu şekilde düşünebiliriz.Bir snapshot alındığında  datanode üzerindeki veriyi değiştirebilir ancak snapshotlar bu durumdan etkilenmez. Bu sayede anlık durumu kaydeden snapshot da bozulma olmaz.
-Datanodelerin verilerini birbirine sakladığını daha önce söylemiştik.Aynı durumun snapshot için geçerli değil.Bu sayede datanode gelen bir sorun
 diğer datanode’ yi etkilemez. Bu da snapshot için bir değişiklik yaratılmadığından diğer datanodeler çalışmasını etkilemez ve sistem 
akışını ve güvenirliği daha sağlam hale getiren bir avantaj elde etmiş 
oluruz.
- Son olarak snapshot’ ların arasındaki veriyi paylaşmak için kullandığı stratejiyi  ele alalım.Copy on write strajtesi bir dosya değiştirildiğinde değiştirilen bloklar orjinal dosyadan ayrılır.Yani  orjinalden farklılaşır.
Eğer yeni bir snapshot oluşturulursa sadece değiştirilen bloklar kopyalar.
Bu sayede hem veri hem depolama alanında bir iyileştirme sağlanmış 
olur.











Peki bu bahsettiğimiz snapshot nedir?

Snapshot:

Genel tanımıyla snapshot HDFS üzerindeki dosya sistemlerinin belirli bir andaki durumu kopyalamak için kullanılan bir terimdir.Anlık görüntü oluşturma arama süresi dahil edilmeden maliyeti düşüktür( O(1) bu da hız açısından avantaj sağlar).
Peki bu anlık durumu kopyalamanın avantajları nelerdir:
-Ek bellek yanlızca snapshot değişiklik yapıldığında kullanılır. Bu yüzden bellek kullanımı azdır( O(M)’ m değiştirilen ve dosya dizini sayısıdır yani dosya dizin ve sayısına bağlı olarak bellek kullanımı performansı ve hızını etkiler)
-Anlık olarak görüntü almaya izin verir.Bu bahsettiğimiz kopyalama işlemidir.Dosya ve dizinlerin izinlerini ve içerikleri içinde barındırır.
-Snapshot kullanıcı verilerini isteğe göre belirli bir noktada dondurabilir ya da o noktaya geri de dönebilir.Bu erişim sayesinde istenildiği takdirde kopyalanan veriye erişim kolaylaşır.Bu da veriyi geri alma ve veri kaybı gibi konuları minimuma indirmekle birlikte veride yapılan yanlışlarında telafisini sağlar diyebiliriz.
- Bir dosya ya da dizinde bir hata durumu gerçekleşirse snapshot geriye alma durumu sayesinde bu hatayı geride bırakabilir ve önceki hatasız duruma geri dönebilir.Kısacası hatalara karşı bir özgürlüğümüz güvencemiz var diyebiliriz.
-Büyük veri setlerinde yedekleme işlemlerini rahatlıkla yapan snapshot veri güvenliğini sağlamakla birlikte sürekliliği de sağlar bu da hizmetin kesintisiz olması için büyük katkıda bulunur.
 
Avantajlarının yanında snapshotların benzersiz isimlerle tanımlanırlar.Genellikle tarih ve zaman olarak bir ibare bulunsada kullanıcı tarafından isimlendirilebilirler.
Kullanıcılar snapshotlar arasında geçiş yapabilir ve istediği snapshota dönebilir.

 Genel tanımlarıyla birlikte snapshot çalışma mimarisinin bileşenlerini açıkladım şimdi mimarinin işleyişini özetleyelim:












HADOOP SNAPSHOT

		HDFS(Hadoop Distributed File System) Architecture

Namenode:Bir merkez noktası.Dosya sistemi metalarını yönetir.
Snapshot görüntüleme tablosunu burada oluşturur.
Datanode:Snapshotların değişiklerini ve gerçek veriyi saklar:
Snapshot:İsimlendirme ,oluşturma ve yönetme işlemleri

Genel taslak mimarisi olarak bu bileşenlerden oluşur.




KURULUM ADIMLARI
-Virtual box indirip ubuntuyu kuruyoruz.
-sudo apt install default-jdk java paketimizi indiriyoruz(java -version paket kontrölü yapıyoruz.)
-sudo apt install openssh-server openssh-client pdsh(namenode datanode arasındaki iletişimi sağlayacak ağ protokolümüzü kuruyoruz.)
-sudo useradd -m -s /bin/bash hadoop(projenin kullanıcısını oluşturuyoruz,güvenlik için sudo passwd hadoop şifre eklenebilir.)
-sudo usermod -aG sudo hadoop(süper user yani root yetkilerini kullanabilecek değişiklik okuma yazma yürütme gibi(r.w.x) izinlerini veriyoruz.
-su hadoop son olarak oluşturuğumuz kullanıca geçiyoruz.
-ssh-keygen -t rsa                                                                                       
-ls ~/.ssh/
-cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
-chmod 600 ~/.ssh/authorized_keys(bu adımları sırayla yapıp  anahtar çiftimizi olutuyoruz. Bu anahtar çiftinin sahip olan tüm sunucular iletişim için hazır hale gelmiş olacak.)
-ssh localhost
-wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz( hadoop dosyamızı indiriyoruz)
-tar -xvzf hadoop-3.3.6.tar.gz(rardan çıkarıyoruz)
-sudo mv hadoop-3.3.6 /usr/local/hadoop( localimizde hadoop dizinini oluşturup rardan çıkardığımız dosyayı buraya taşıyorum)
-sudo chown -R hadoop:hadoop /usr/local/hadoop(gerekli yetkileri veriyoruz
-nano ~/.bashrc(ortam değişkenlerinini ayarlıyoruz: export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export HADOOP_HOME=/usr/local/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export HADOOP_YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native)
-source ~/.bashrc(değişiklikleri uyguluyoruz)
-nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh( #export$JAVA_HOME satırını export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64 bununla değiştiriyoruz.)
-hadoop version kurulumu doğrulamak amacıyla bu komutla sürümümü görüyoruz.
-sudo nano $HADOOP_HOME/etc/hadoop/core-site.xml(namenode ve datanode için core.xml dosyamızı konfigürasyonunu yapıyoruz: 
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://0.0.0.0:9000</value>
    </property>
</configuration>)
-sudo mkdir -p /home/hadoop/hdfs/{namenode,datanode}(kümelenme için yeni dizinleri oluşturuyorum.)
-sudo nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml(namenode ve datanode için hdfs.xml dosyamızı konfigürasyonunu yapıyoruz: 
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
   <property>
      <name>dfs.name.dir</name>
      <value>file:///home/hadoop/hdfs/namenode</value>
   </property>
   <property>
      <name>dfs.data.dir</name>
      <value>file:///home/hadoop/hdfs/datanode</value>
   </property>
</configuration>)

- hdfs namenode -format(dosya sistemini formatlayarak başlatmak için hazır hale getiriyoruz.)
- start-dfs.sh(namenode ve datanode çalıştırıyoruz.Burada permission denied hatasıyla çok karşılaştım bunun için pdsh remove  ederek bağımlılıkları kaldırır soruna çözüm olabilir.
- sudo nano $HADOOP_HOME/etc/hadoop/mapred-site.xml(
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
</configuration>)
- sudo nano $HADOOP_HOME/etc/hadoop/yarn-site.xml(
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME,PATH,LANG,TZ,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>) yarn konfigürasyonunu tamamlayıp start-yarn-sh yaparak başlatıyoruz.
-En son jpd komutuyla bileşenlerin doğru çalışıp çalışmadığına bakmamız gerekiyor.
Doğru görüntü şöyle olmalı:
 

-snapshotlar için kopya ve oluşturulma işlemi için yeni iki dizin eklenmeli.

-Snapshot adlarında mutlaka tarih damgası bulunmalıdır.



 




									


