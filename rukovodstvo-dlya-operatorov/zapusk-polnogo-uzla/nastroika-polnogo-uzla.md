# Настройка полного узла

Перед запуском полного узла потребуется уникальный идентификатор `chain-id`, который будет выдан, как только будет готов файл genesis.

### Присоединение к сети

После того как идентификатор `chain-id` был распространен, можно присоединиться к сети с помощью `CHAIN_ID`:

```rust
  export CHAIN_ID="namada-mainnet" ## (replace with the actual chain-id)
  namada client utils join-network --chain-id $CHAIN_ID
```

### Запуск узла и синхронизация

```rust
  CMT_LOG_LEVEL=p2p:none,pex:error namada node ledger run
```

Дополнительно: Если требуется большее количество журналов, можно вместо этого выполнить команду

```rust
NAMADA_LOG=info CMT_LOG_LEVEL=p2p:none,pex:error NAMADA_CMT_STDOUT=true namada node ledger run
```

А если необходимо сохранить журналы в файл, то можно выполнить команду:

```rust
TIMESTAMP=$(date +%s)
NAMADA_LOG=info CMT_LOG_LEVEL=p2p:none,pex:error NAMADA_CMT_STDOUT=true namada node ledger run &> logs-${TIMESTAMP}.txt
tail -f -n 20 logs-${TIMESTAMP}.txt ## (in another shell)
```

### Запуск namada в качестве службы systemd

{% hint style="info" %}
<mark style="color:blue;">Приведенный ниже скрипт является вкладом сообщества, сделанным Encipher88, и в настоящее время работает только на машинах Ubuntu. Он был полезен для многих валидаторов.</mark>
{% endhint %}

Ниже предполагается, что namada была установлена из исходного кода с помощью `make install`. По крайней мере, предполагается, что соответствующие двоичные файлы находятся в каталоге `/usr/local/bin/`.

```rust
which namada ## (should return /usr/local/bin/namada)
```

Ниже приведен служебный файл для systemd, который будет запускать namada как службу. Это удобно для работы узла в фоновом режиме, а также для автоматического перезапуска узла в случае его сбоя.

```rust
sudo tee /etc/systemd/system/namadad.service > /dev/null <<EOF
[Unit]
Description=namada
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.local/share/namada
Environment=CMT_LOG_LEVEL=p2p:none,pex:error
Environment=NAMADA_CMT_STDOUT=true
ExecStart=/usr/local/bin/namada node ledger run 
StandardOutput=syslog
StandardError=syslog
Restart=always
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

Включите службу с помощью следующих команд:

```rust
sudo systemctl daemon-reload
sudo systemctl enable namadad
```

Теперь вы можете управлять узлом с помощью команд systemd:

* Запустите узел

```rust
sudo systemctl start namadad
```

* Остановите узел

```rust
sudo systemctl stop namadad
```

* Перезапустите узел

```rust
sudo systemctl restart namadad
```

* Просмотрите логи

```rust
sudo journalctl -u namadad -f -o cat
```
