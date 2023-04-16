# Internal_Gitlab_SSL_Cert

# Şu linki takip ederek indirdik : https://docs.bitnami.com/general/apps/gitlab/administration/control-services/

Sırasıyla şu adımları takip ettik ;

1) Önce gitlab ı durdurduk;

sudo gitlab-ctl stop

2) Sonra şu linkte ki aşağıdaki komutu koştuk ;

https://docs.bitnami.com/general/apps/gitlab/administration/generate-configure-certificate-letsencrypt/

sudo /opt/bitnami/letsencrypt/lego --tls --email="admin@banct.com" --domains="git.banct-app.com" --domains="internal.git.banct-app.com" --path="/opt/bitnami/letsencrypt" run

3) 2. adımı yapmadan önce AWS de internal.git-banct-app.com isminde bir CNAME domain kaydı oluşturuyoruz. Value değerinede ilgili sunucunun public-ipv4 dns name ini yazıyoruz : " ec2-3-10-20-15.eu-west-2.compute.amazonaws.com "

3) Sonra şu linkteki https://docs.bitnami.com/general/apps/gitlab/administration/generate-configure-certificate-letsencrypt/ 3.step deki şu komutları çalıştırıyoruz ;

sudo mv /etc/gitlab/ssl/server.crt /etc/gitlab/ssl/server.crt.old
sudo mv /etc/gitlab/ssl/server.key /etc/gitlab/ssl/server.key.old
sudo mv /etc/gitlab/ssl/server.csr /etc/gitlab/ssl/server.csr.old

4) Sonra şunları koşuyoruz ;

sudo ln -sf /opt/bitnami/letsencrypt/certificates/git.banct-app.com.crt /etc/gitlab/ssl/server.key
sudo ln -sf /opt/bitnami/letsencrypt/certificates/git.banct-app.com.crt /etc/gitlab/ssl/server.crt
sudo chown root:root /etc/gitlab/ssl/server*


4) Sonrasında servisi yeniden başlatıyoruz.

sudo /opt/bitnami/ctlscript.sh start
