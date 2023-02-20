# Mustacchio_CTF
Mustacchio_CTF çözümü 

İlk olarak nmap taraması yapıyoruz.
![NmapScan](https://user-images.githubusercontent.com/103064152/220176415-5ccc0277-df4b-4ce3-9eb9-48d57fff70c9.png)

Daha sonra 80 portuna yani web servise gidiyoruz.
![web page](https://user-images.githubusercontent.com/103064152/220176532-dc22f6ea-1903-4c03-8b3f-5c4609323b8f.png)

Daha sonra enumaration yapıyouz.
![Enumarationcopy](https://user-images.githubusercontent.com/103064152/220176847-d7c71722-bd8b-4e79-bddf-c86d0b22fb22.png)

Bulduğumuz custom sayfasına gidiyoruz.

![custom](https://user-images.githubusercontent.com/103064152/220176964-cf547a37-a680-41e9-8859-8d22af71e2d1.png)

![sql file](https://user-images.githubusercontent.com/103064152/220177019-eb7f61e9-cf9c-42b1-b841-ddc9fbb45b29.png)

users.bak diye bir dosyayı indiriyoruz.

file komutunu kullanarak dosyanın ne tür dosya olduğunu bakıyoruz.
![file users bak](https://user-images.githubusercontent.com/103064152/220177149-db6235b9-8d20-4494-9a41-277736c3c52d.png)

Dosyanın sqlite3 dosyası olduğunu görüyoruz.

"sqlite3 users.bak" komutu ile dosyanın içeriğine bakıyoruz.

![dump](https://user-images.githubusercontent.com/103064152/220177358-23f44584-ed39-4fb7-978f-10aee430ff6d.png)

Dosyanın içindeki hash'li bir ipucu buluyoruz.Daha sonra Hashes.com da bu hash'i decoder yapıyoruz.

![HASH](https://user-images.githubusercontent.com/103064152/220177649-42c03be1-ef17-41c7-9f99-d333f81df97e.png)

"bulldog19"  Büyük ihtimalle bir password bulduk. İlk başta ssh login için admin'nin şifresi sandım.Ve ssh direk denedim ancak bir sonuç alamadım. 

Aklıma makineyi bir daha taramak geldi ve nmap aracı ile bir kez daha tarama işlemi yaptım ancak 1000 porttan sonrası için taradım.
![NmapScannfullport](https://user-images.githubusercontent.com/103064152/220178101-8fc5c891-d7b9-44c0-8c1d-57fb5a3be2cd.png)


"8765" portuna gidiyoruz.Ve karşımıza adminPanel çıkıyor.

![adminPanel](https://user-images.githubusercontent.com/103064152/220178410-7ea23eb7-494d-4186-8dc3-d5aa9f474dc9.png)

username:admin
password:bulldog19
İle giriş yapıyoruz

![Comment](https://user-images.githubusercontent.com/103064152/220178549-4b9239db-26f8-444f-bc01-5fff98c0e9b0.png)

Bir adet yorum ekle textbox geliyor. Burda xss ve xxe injection gibi zafiyetleri denedim ve xxe injection tespit ettim.

![Xxe injection](https://user-images.githubusercontent.com/103064152/220178657-fb0e6a5d-67e1-42e6-a2a8-9883ff5be258.png)

![xxe tespit](https://user-images.githubusercontent.com/103064152/220178663-546e1f12-69a9-4071-9f29-0d01139db14b.png)

XXE injection ile Directory travansel zafiyetini birleştirerek /etc/passwd dosyasının içeriğine ulaşıyoruz.

![directory trvansal](https://user-images.githubusercontent.com/103064152/220178934-f010d350-3a80-4931-bcf1-b9aa6fc1c9e3.png)

Tekradan directory travansel ile "/home/baary/.ssh/id_rsa" dosyasının içindeki id_rsa text dosyasını aldım.

![id_rsa](https://user-images.githubusercontent.com/103064152/220179258-25ecec4a-a71b-476b-ad39-fde366aad499.png)

"sshjohn" aracını kullanarak id_rsa hash kırdım ve "ssh baary@ip" ile ssh giriş yaptım 

![ssh login](https://user-images.githubusercontent.com/103064152/220179488-95d41668-01b4-4bc5-9e4c-11cee0b92930.png)

cat ile user.txt dosyasından flag1 aldım.

"ls -la" komutunu çalıştırdığımda "live_log" adlı bir klasör buldum.
"scp -i barry barry@10.10.152.83:/home/joe/live_log" çalıştırdığımda root hakkına sahip oldum.

