CC14 区块链修改至 Nxt Blockchain Creation Kit 版本号 1.12.2.

Ubuntu Linux 请登陆终端后做以下更新：
sudo apt update
sudo apt upgrade -y
sudo apt install zip -y
sudo apt install dos2unix
sudo apt install openjdk-11-jre-headless -y
sudo apt install openjdk-11-jdk-headless -y

克隆CC14，如果没有Git请安装
git clone http://www.github.com/ditinow/cc14/
cd cc14
sh ./compile.sh
sudo ufw allow 19884:19886/tcp
sudo ufw allow 6874:6876/tcp
chmod 751 run.sh
chmod 751 stop.sh
./run.sh --daemon

Windows用户请下载后运行:
run.bat
默认 JAVA 在windows path里。如果CC14不能正常启动，请在CMD输入以下命令：
java.exe -cp classes;lib\*;conf;addons\classes;addons\lib\* -Dnxt.runtime.mode=daemon nxt.Nxt

Windows path 设置JAVA可以参考以下链接。或者拆卸并重装JAVA JRE。
https://www.yuque.com/ican/canjava/path-classpath

Linux与Windows都使用以下地址接入钱包。
http://localhost:19886/
或者所在计算机的IP地址
http://ipaddress:19886/

具体修改如下
 src/java/nxt/Nxt.java
	line 56 APPLICATION = "CryptoC14"

vim src/java/nxt/Constants.java
	line 29 COIN_SYMBOL = "CC14"
	line 30 ACCOUNT_PREFIX = "CC14" 
	line 31 PROJECT_NAME = "CryptoC14"

vim src/java/nxt/BlockchainProcessorImpl.java
	line 1369 
                    accept(block, validPhasedTransactions, invalidPhasedTransactions, duplicates);
  添加              BlockDb.commit(block);
                    Db.db.commitTransaction();
                 } catch (Exception e) {

vim src/java/nxt/peer/Peers.java
	line 107 DEFAULT_PEER_PORT =19884

vim contrib/Dockerfile
	line 40 EXPOSE 19884 19886

vim Wallet.url
	line 2 localhost:19886

vim conf/nxt-default.properties
	line  39 nxt.peerServerPort=19884
	line  70 nxt.wellKnownPeers=52.87.243.104; 13.52.217.197; 40.124.110.187;
	line 190 nxt.allowedBotHosts=*
	line 193 nxt.apiServerPort=19886
	line 197 nxt.apiServerSSLPort=19886
	line 205 nxt.apiServerHost=0.0.0.0
	line 275 nxt.maxUploadFileSize=512000
	line 438 nxt.maxPrunableLifetime=-1

vim conf/nxt.properties
	#nxt.adminPassword=123456789
	#nxt.isTestnet=false
	#nxt.maxPrunableLifetime=7776000
	#nxt.maxUploadFileSize=512000

vim jar.sh
	line 2 APPLICATION="cc14"

vim package.sh
	line 2 VERSION=1.12.2
	line 8 APPLICATION="cc14"

vim release-package.sh
	line 2 VERSION=1.12.2
	line 8 APPLICATION="cc14"

vim stop.sh
	line 2 APPLICATION="nxt"
  
  
