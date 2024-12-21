**Minimum Sistem Gereksinimleri**


250 GB SSD storage

Kurulumu kendi cihazınıza yapıyorsanız eğer 10 yaşından küçük dört çekirdekli CPU, 16 GB ram, 4+ GB Virtual Memory

Sunucu kiralıyorsanız da çift çekirdekli CPU, daha yeni Xeons / EPYC ile barındırılıyorsa çalışır, 8 GB RAM + 8 GB Virtual Memory

**NOT: Ben sunucularımı genellikle Hetzner üzerinden kiraladığım Cloud VPS sunuculara kuruyorum. Ben bu validatörü CX42'ye kurdum, CPX41 ya da CX52'ye de kurulabilir. Maliyeti göz önünde bulundurarak CX42'ye kurulum gerçekleştirdim. Ubuntu 22.04 seçiyorum işletim sistemi olarak.**

# Shardeum Atomium Stage 4 Validator Node nasıl kurulur?

Bu rehber, sisteminize devam eden Shardeum Atomium Stage 4 Validator Node kurma ve çalıştırma sürecinde size yol gösterecektir. Lütfen aşağıdaki adımları dikkatlice izleyin.

## Önkoşullar

Başlamadan önce, sisteminizde aşağıdaki önkoşulların yüklü olduğundan emin olun:

1. Paket Yöneticilerini Kurma

**Linux İçin:**

```bash
sudo apt-get install curl
```

**MacOS İçin:**

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Homebrewu `PATH`a ekleme: (Linux Olanların Yapmasına Gerek Yok)

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"'
eval "$(/opt/homebrew/bin/brew shellenv)"
```

2. Paket Yöneticilerini Güncelleme:

Linux İçin:

```bash
sudo apt update
```

MacOS İçin:

```bash
brew update
```

3. Docker'ı Yükleme

Linux İçin:

Docker'ı docker.io ile Kurma

```bash
sudo apt install docker.io
```

MacOS İçin:

```bash
brew install docker
```

> Docker kurulumunu `docker --version` çalıştırarak doğrulayın (20.10.12 veya daha yüksek bir sürüm görmelisiniz).

4. Docker-compose Yükleme

**Linux İçin:**

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Docker-compose kullanımı için kurulum izni ayarlama

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

**MacOS İçin:**

```bash
brew install docker-compose
```

> `docker-compose --version` çalıştırarak docker-compose kurulumunu doğrulayın (1.29.2 veya daha yüksek bir sürüm görmelisiniz).

## Kurulum Komut Dosyasını İndirin ve Çalıştırın

Kurulum komut dosyasını indirmek ve çalıştırmak için aşağıdaki yöntemlerden birini seçin: **Ben kurulum için `curl` kullanmayı tercih ediyorum.**

`curl` kullanarak kurmak için

```bash
curl -O https://raw.githubusercontent.com/shardeum/validator-dashboard/main/installer.sh && chmod +x installer.sh && ./installer.sh
```

`wget` kullanarak kurmak için

```bash
wget https://raw.githubusercontent.com/shardeum/validator-dashboard/main/installer.sh && chmod +x installer.sh && ./installer.sh
```

Bu aşamadan sonra aşağıdaki sorulara belirttiğim yanıtları vermelisiniz kurulumun başarıyla tamamlanması için:

- By running this installer, you agree to allow the Shardeum team to collect this data. (y/n)?: -> Y + ENTER
- Do you want to run the web based Dashboard? (y/n): -> Y + ENTER
- Set the password to access the Dashboard: -> Dashboard'a erişim için bir şifre belirlemeniz gerekiyor. **NOT: Şifrenizde mutlaka bir büyük bir küçük harf bir adet özel harf (!@? gibi) olmalı ve minimum 8 karakter olmalı. Aksi halde panele giriş yaparken hata alırsınız.**
- Enter the port (1025-65536) to access the web based Dashboard (default 8080): -> ENTER
- If you wish to set an explicit external IP, enter an IPv4 address (default=auto): -> ENTER
- If you wish to set an explicit internal IP, enter an IPv4 address (default=auto): -> ENTER
- To run a validator on the Sphinx Atomium network, you will need to open two ports in your firewall.
  This allows p2p communication between nodes.
  Enter the first port (1025-65536) for p2p communication (default 9001): -> ENTER
- Enter the second port (1025-65536) for p2p communication (default 10001): -> ENTER
- What base directory should the node use (defaults to ~/.shardeum): -> ENTER

Bu aşamadan sonra kurulumunuz başlayacak. Tamamlanması sunucu performansına bağlı olmakla birlikte bende ortalama 5 dakika sürdü.

### Cüzdanınıza Ağı Eklemek İçin

<https://docs.shardeum.org/docs/network/endpoints> sayfasını açın ve Atomium ağını cüzdanınıza ekleyin.

### Faucet'ten Token Almak İçin

[Shardeum Discord'a katılın](https://discord.gg/shardeum) ve `/faucet <CÜZDAN ADRESİ>` komutunu çalıştırarak faucetten token talep edin. **Yoğunluk sebebiyle tokenin gelmesi birkaç saati bulabilir.**

## Validatörü Başlatmak

Yükleme işlemi tamamlandıktan sonra, web tabanlı kontrol panelini veya komut satırını kullanarak doğrulayıcıyı başlatabilirsiniz:

Web Arayüzüne Erişmek İçin:

- Tarayıcınızı açın ve eğer sunucuya kurduysanız https://SERVER_IP:8080 uzantısına, kendi cihazınıza kurduysanız https://localhost:8080 adresine giriş yapın.
- Sunucu kurulumunu gerçekleştirirken girdiğiniz şifreyi girerek panele erişim sağlayın. 
- Sağ üstteki beyaz kutuda bulunan `Start Node` düğmesine tıklayın.
- Validator başladıktan sonra cüzdanınızı bağlayın.
- Faucet'ten temin ettiğiniz $SHM tokenlerden minimum 10 adet $SHM stake edin ve onaylayın.

NOT: Eğer arayüzden başlatmak konusunda problem yaşıyorsanız terminal ekranınızdan şu komutu girerek de başlatabilirsiniz.

``` cd shardeum ```

``` bash ./shell.sh ```

``` operator-cli start ```

Start işlemini ve stake işlemini tamamladıktan sonra arayüzde "Waiting for Network ya da On Standby" şeklinde bir uyarı görüyorsanız bir problem yok. Ağ bir süre sonra sizi onaylayacaktır.

Shardeum Validator Node çalıştırmak isteyen kullanıcı için talimatlar burada bulunabilir: <https://docs.shardeum.org/docs/node/run/validator>
