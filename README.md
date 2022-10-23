## Установка ноды OLLO

###### OLLO — это суверенная сеть L1, построенная на сети Cosmos, предоставляющая торговые инструменты нового поколения и устойчивую токеномику, более подробно можно прочитать на официальном сайте. https://docs.ollo.zone/

#### Официальный дискорд проекта 

https://discord.gg/nqw8XypYaY

### Эксплорер 

https://ollo.explorers.guru/validators



## Рекомендуемые требования для сервера

#### 8GB RAM, 100GB_SSD of disk space, 2Cores (modern CPU's)

## Как всегда обновляем наш сервер

```
sudo apt update && sudo apt upgrade -y
```

## Устанавливаем доп. пакеты.

```
sudo apt install make clang pkg-config libssl-dev build-essential git gcc chrony curl jq ncdu bsdmainutils htop net-tools lsof fail2ban wget -y
```

## Устанавливаем go и проверяем версию

```
ver="1.18.4" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

## После этого скачиваем и устанавливаем бинарник

``` 
cd $HOME
git clone https://github.com/OllO-Station/ollo.git
cd ollo
make install

chmod +x /root/go/bin/ollod && sudo mv /root/go/bin/ollod /usr/local/bin/ollod
cd $HOME
```

####### Задаем переменные (CHAIN оставляем без изменений, в остальные вписываем свои данные) Что бы не путаться в дальнейшем задайте имя и имя кошелька одинаковыми

```
MONIKER="your_name"
CHAIN="ollo-testnet-0"
WALLET_NAME="your_name"
```

## Добавляем все в баш профиль

```
echo 'export MONIKER='${MONIKER} >> $HOME/.bash_profile
echo 'export CHAIN='${CHAIN} >> $HOME/.bash_profile
echo 'export WALLET_NAME='${WALLET_NAME} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## Инициализируем ноду

```
ollod init $MONIKER --chain-id $CHAIN
```

## Прописываем в конфиг имя сети

```
ollod config chain-id $CHAIN 
```

## Скачиваем файл генезис

```
curl https://raw.githubusercontent.com/OllO-Station/ollo/master/networks/ollo-testnet-0/genesis.json | jq .result.genesis > $HOME/.ollo/config/genesis.json
```

## Настраиваем прунинг

```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.ollo/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.ollo/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.ollo/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.ollo/config/app.toml
```

## Выключаем индексер (по желанию)

```
indexer="null" sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.ollo/config/config.toml
```

## Устанавливаем минимальную цену за газ

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0utollo\"/" $HOME/.ollo/config/app.toml
```

## Добавляем сиды и пиры

```
SEEDS=""
peers="6aa3e31cc85922be69779df9747d7a08326a44f2@ollo-testnet.nodejumper.io:28656,578a5986cd3b4c70451d91f96bfd585d23cacf11@65.21.133.125:32656,0617bc14e3c877e0ae2943a00cb80894ad18b689@65.108.206.56:38656,14d60656bb8104d45ba93a8d1b3833646b1ab392@185.205.244.21:32656,1a962c7ec644920c3f909abbba056b0cbcb9bbaf@185.188.250.66:11656,03c0ef3d849760c979b3003efac4d47c59598c81@135.181.5.47:26656,4dd3f3897ab77c7aa981bf4e928659f867093361@198.244.179.62:25456,fe83ff301a48a09fedb68dd7dfd5dc11005913fc@217.13.223.167:60556,6dd7e9c0afd1bd947351978713cae8e2379707f6@65.21.76.62:26656,9b24faa730b9d3542e445351a43465b8276be73d@65.21.156.187:26656,d5b72f42a88b60846d8c1884652bd87a4ffa0017@65.109.27.156:34656,19371004f2cfd91d2ead0fe8f8b548b2bbeac7eb@159.69.149.85:32656,8cf970aae503c66241072c63204b93cd9357cbfe@188.166.62.10:32656,329aea8611334a3437e3d28eee221b44c09482e2@194.163.163.224:26656,46fd4bfe2335c3f82df23111cbc2d62d942be909@178.18.247.249:26656,d7632ecbb247d8608ccbcbebb3235a586406e1a8@75.119.145.20:26656,6bbd163343785ddd636712bc4322699efaabaac7@149.102.139.103:26656,8b055b94e7e3eb1fd6413ae5036b76e61fd63234@159.69.65.97:35656,7cb4ed3a8e855d2280efad76a38aafcb13bf2129@45.134.226.207:15656,3a8a9083333ab450f6046be638b8433e5f313364@5.182.33.114:15656,969f3672d9d302c374102aa8ddb1c79672333127@95.216.149.119:32656,bf0bb12929a90e2505efde4a8de5c5074634658b@144.91.117.51:32656,c6f0b8944f31236b6a5f1713adcd12eb17f5f839@207.180.228.160:26656,6bdabdb92c19040910f0a882753cc50645ff421f@161.97.65.117:15656,dd08c333875b14931f4187d2361d40e7b0b0f324@38.242.133.241:32656,ae33ffb3697f83d59eca5666e71cd5449097a384@38.242.152.16:32656,83c109aacea2db21f46f9c4c5dbb5a7cbc81e6e1@178.62.62.90:26656,6609fd45964d3c83ba7f6b0780e154bb004387cd@142.93.153.163:32656,8e128f2cc04aa01e4b69d1b0a81e6f22dd78e617@65.109.52.156:46656,125b0e30f00df3ff2ee7b29b7992ed888998ad31@65.109.28.177:47656,d3967951a89123b2c883dd2d6dda6ff28db47e9a@135.181.90.219:15656,31f98e739ec4e2314bc2cdd0ecb3e59a09460a66@116.203.89.152:26656,8a1dac50e2da8032ad13f16cfb5126695a64be22@45.147.199.172:32656,dfb2bba31436bc6cde54f475204ff53c9440804e@149.102.147.59:32656,9c538c7faa0881052ff1cb21c031372ab510e064@134.209.91.16:26656,e1efa8953a5692053af9197a2adcf3b4258918b6@45.147.199.36:32656,97800e498716202bd3e50f986b3f6356c47fbc14@159.203.29.74:32656,30873a3c432c00e4a0b3741cb3e1c1eaeb0b30d9@209.126.2.211:26656,37a271ca0b1e0ab743a714c0d119cd2baba88af5@65.21.155.230:26656,adcc1fe9ce1f473560af5c760361e56b7ecff62c@65.108.195.235:14656,f0e337dde944763e6493eec4006c191f2cab163e@135.181.30.143:32656,b5d63299e45433978dc15a7cd3b26536e3d88e0a@65.21.51.120:26656,cfe46468047f12aca9441c8422c0277a4e1a0d39@38.242.132.221:32656,9afbbaa4eee6d478c338c98490d84a91e837f70c@141.95.65.26:27856,7c1c33cfd141b287ae4e94801a163a684ee1d060@38.242.159.65:26656,7ac41bff100a9aa1a2396f714dd933afe45dff95@62.171.147.252:32656,0d6ce9697433f59783f1be50071ecbd16da445f3@38.242.222.253:32656,5a675faf9468444e8d0e44d3df41bd3a7cd2985e@157.90.174.26:38656,34abe1b02fb9a38ab7b116897a32828073a8fe35@134.209.16.234:26656,938f28d1179d721396782e9f348d666405a494ac@142.93.145.223:32656,16f21fa249b4dd7ec6e796823e92dbcb94e99cbc@162.55.99.50:36656,53581b1c968fab62ee319f2d81fd1ad6da1c2230@65.21.138.123:30656,c2bd171c41e5ecb29d0344f72dafd6436493cb9b@38.242.221.88:15656,193e08f3fd44865c290ab031de8ce55005bafe5b@65.109.60.239:32656,ea4ac7cd220d9c820b8f0052b7ab96121a391eeb@38.242.245.172:26656,2d676480cf558a449eb3aa13d662b3dafd66eda8@154.38.161.255:26656,4ba791cc7657ca5f39aa4ba91e8c957f78924d8c@167.86.71.138:32656,d66601524903b7f9c568ada598c12976fe684bf2@68.183.237.199:32656,90ad9622ac54023fe4ee9824d77b5d3e3c25c245@162.55.234.70:54956,6b6e934ccda8c4be83b858a50de8c6ed494f401c@80.241.220.27:26656,a113c0984e319b44cfe5c848a0ed5591c2f004a8@78.47.75.185:32656,cf68a57a2fd39bef6b7e402830e1841e14722df6@45.147.199.219:32656,7f3603774a00c5d61db81b99ec97898e4c4eaf57@159.203.3.132:32656,84944a5bf48fa2d22758cb1724cf5dd0ec7903af@86.48.1.142:26656,27f13a633ec3108669593b979b31b1968fc29f31@86.48.0.218:32656,381571f357fd95089db89aa918e26e22e7a874f1@65.21.242.148:26656,40f8bbadfe8587d3e8ee0b69a8af16def37a9951@185.245.183.246:32656,c542f88877f150973981b0ab7aa25d66ed858d85@178.18.243.46:26656,fc75948254132757aeceb35fdae7f5f3a3d04870@144.91.80.32:32656,0cd6c7ad0504c091772c1ad5e5e0f14c8bc4629f@62.113.118.117:15656,a0e396f9aeb24d3efe7f834650f600f718af240a@85.114.142.242:60556,13d7eab4417344fe674a73951549764c4d844bf1@80.65.211.160:26656,6ce89d220345ebd0f503220d8b2a22d26ac0039d@86.48.5.63:15656,e6b829d5c7e25fd7bfc4ceaad8c7281606d5fabb@149.102.136.15:32656,b93b9ec4aa57189fe94e93deca00a419663cdd73@147.182.151.165:32656,d1a61729f2c42c6b4df37bde23517461a39d3878@135.181.253.107:15656,86cc52a859c469aaee73fe61838ce50f140989e5@92.119.112.248:32656,851a745e26d92bb6ec17e4782248bf734e7e11f1@38.242.151.30:15656,4f102e4e212bfe967505fe813dc2136afbe8eefa@80.65.211.249:26656,e3e682a18a86086cf83a759d9a525f9fa4c66bbd@65.109.5.217:15656,a4c37799fb6da5cfd076d7a56468bd61232e92cb@45.87.104.72:26656,b4a5eb58b6167a3ffaf2e3102f37b037ded77cbe@142.132.248.253:36656,e65fc5e2e547c07658026b6ea03a72728da83e77@147.182.151.187:32656,155c639c136b1b8195ae7cad9ca417aab6b3501f@167.235.52.83:26656,7cc385e39b94e57670359fce2c327d2ad262744f@46.101.30.164:32656,06089122df5c04afad4d42410be39651e8539462@194.146.12.252:15656,b807493dcb4437da3f67e2e74860f43a05ea7aba@167.235.147.187:26656,85c60bfa6fdca8b66ebe11ec826f1775c8968c67@154.53.53.202:32656,8d8fdebd2f7b6bfe9eebe5f0869bd115ace3e86f@38.242.158.85:15656,e7942a9562d1203b57185c0fbdfd5a8eb348d96d@185.190.140.81:32656,276705a7f41b6905576819802b5a00a301487e16@159.203.20.66:32656,74ad7619b99fcb3e4be0876bd0b69414d48400e2@109.205.181.162:36656,352654578c3e0f0087aefb41ee33a70fc7ad0c27@65.21.156.21:32656,22124714b8c2049e1a4a4dfda1cb3125f05f7f71@65.109.34.133:60756,dcc6f1a8105dcfe38da4711fe1d3acc646f70e44@167.99.176.181:32656,1f8b5d8414a2e5920e4ab53df53e2080b7f35f14@192.241.132.186:32656,eba3a05a130337ee878a91de257a4a17743b0ba3@167.86.81.96:32656,7f068d1c9295e22440067964dc463249e6d857dd@154.38.161.212:26656,e635b81ec5dd08126153c8aca5080afc00e6a6a0@149.102.157.96:46656,1200546f7452eb81aab9663ad8554c197d566225@138.197.169.119:32656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.ollo/config/config.toml
```

## Создаем сервис файл (целиком)

```
sudo tee /etc/systemd/system/ollod.service > /dev/null <<EOF
[Unit]
Description=ollo
After=network-online.target

[Service]
User=$USER
ExecStart=$(which ollod) start --home $HOME/.ollo
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

## И запускаем сервис

```
sudo systemctl daemon-reload
sudo systemctl enable ollod
sudo systemctl restart ollod
```

## Смотрим логи и ждем когда нода начнет синхронизироваться

```
sudo journalctl -u ollod -f -o cat
```
## !!!!! Если у вас стоят еще космо ноды на этом сервере то у вас обязательно будет ошибка по портам, что бы ее исправить воспользуйтесь этим примером изменения портов на космосе (спасибо автору гайда) https://nodes.mms.team/installing_multiple_nodes
После изменения портов вернитесь на шаг выше с __запускаем сервис___


```
https://nodes.mms.team/installing_multiple_nodes
```


## Или смотрим статус синхронизации (когда "catching_up": false то нода синхронизирована) 
Если нода долго не подключается к пирам то просим поделиться пирами или адресбуком в дискорде или тематических телеграм каналах.


```
curl localhost:26657/status
```

## После синхронизации создаем кошелек (не забываем сохранить мнемоник)

```
ollod keys add $WALLET_NAME
```

## Если вы восстанавливаете аккаунт то воспользуйтесь этой командой, если первый раз то пропускайте этот шаг.

```
ollod keys add $WALLET_NAME --recover
```

## Добавляем переменную с адресом кошелька

```
WALLET_ADDRESS=$(ollod keys show $WALLET_NAME -a)
```

## Добавляем переменную в баш профиль

```
echo 'export WALLET_ADDRESS='${WALLET_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

##### Теперь нам необходимо получить средства на кошелек. Для этого переходим по ссылке https://discord.gg/nqw8XypYaY
и запрашиваем токены на баланс кошелька, для того чтобы ветка #testnet-fauset стала доступной переходим в ветку #roles и выбираем роль Testnet Explorers.

## После запроса проверяем баланс

```
ollod query bank balances $WALLET_ADDRESS
```

##### Если средства успешно поступили, то создаем валидатора (сумму указывайте свою, сколько хотите делегировать с кошелька, указывайте чуть меньше что бы хватило на комиссию)

```
ollod tx staking create-validator \
  --amount 50000000utollo \
  --from $WALLET_NAME \
  --commission-max-change-rate "0.05" \
  --commission-max-rate "0.2" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey  $(ollod tendermint show-validator) \
  --moniker $MONIKER \
  --chain-id $CHAIN
```

## Задаем переменную с адресом валидатора

```
VALOPER=$(ollod keys show $WALLET_ADDRESS --bech val -a)
```

## И добавляем ее в баш профиль

```
echo 'export VALOPER='${VALOPER} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## Проверка статуса валидатора

```
ollod query staking validator $VALOPER
```

### Делегация средств с кошелька на валидатора (сумму вводите свою)

```
ollod tx staking delegate $VALOPER 50000000utollo --from $WALLET_NAME --chain-id $CHAIN
```

## Ну и если вдруг ваша нода попала в тюрьму, то выход

```
ollod tx slashing unjail --from $WALLET_NAME --chain-id $CHAIN
```

##### вся информация по этому проекту берется в дискорде проекта. Все там. Спасибо что воспользовались моим гайдом.

