# RTC_Jetson_Orin_Nano
Configuração do RTC - Jetson Orin Nano
### Verifique o RTC padrão
```bash
ll /dev/rtc*
```

### Mudando o RTC padrão para rtc0
Para alterar o valor de `ATTR{hctosys}` de `1` para `0` no arquivo de regras do udev, você pode usar o comando `sudo sed`. Aqui está um exemplo de como fazer isso:

```bash
sudo sed -i 's/ATTR{hctosys}=="1"/ATTR{hctosys}=="0"/' /etc/udev/rules.d/50-udev-default.rules
```

Este comando usa o `sed` para substituir todas as ocorrências de `ATTR{hctosys}=="1"` por `ATTR{hctosys}=="0"` no arquivo `/etc/udev/rules.d/50-udev-default.rules`.

Após executar este comando, você pode reiniciar o sistema ou recarregar as regras do udev para aplicar as alterações.

```bash
sudo reboot
```
Certifique-se de verificar se as alterações funcionaram conforme esperado.
### Verifique o RTC padrão se foi alterado para rtc0
```bash
ll /dev/rtc*
```

## Alterar data e hora do sistema
Antes de tudo verifique o horário atual do sistema.
```bash
timedatectl status
```
Observe o horário `Local time`, `RTC time`.

Para gravar a data e hora vamos fazer isso com este comando:

```bash
sudo date --set="2024-03-06 18:23:00"
timedatectl status
```
### Configurar RTC
Com esses comandos configuramos o tempo rtc.

Grava a hora do Sistema no RTC
```bash
sudo hwclock -w -f /dev/rtc0
```

Se quiser conferir a data e hora do RTC
```bash
sudo hwclock -r -f /dev/rtc0
```

## Criando um serviço para atualizar o Sistema com a data do RTC ao inicializar o mesmo
O comando, `sudo hwclock --hctosys`, é usado para sincronizar o relógio do hardware (`hwclock`) com o sistema (`system clock`). E para garantir que o relógio do hardware seja sincronizado corretamente com o relógio do sistema a cada inicialização, criaremos um serviço.

1. Crie um arquivo de serviço. Por exemplo, você pode usar o comando `sudo nano` para editar um arquivo chamado `sync-hwclock.service`:

```bash
sudo nano /etc/systemd/system/sync-hwclock.service
```

2. Adicione o seguinte conteúdo ao arquivo:

```ini
   [Unit]
   Description=Sync Hardware Clock with System Clock

   [Service]
   Type=oneshot
   ExecStart=/sbin/hwclock --hctosys

   [Install]
   WantedBy=multi-user.target
```

3. Salve o arquivo e feche o editor.

4. Recarregue os serviços do systemd para que ele reconheça o novo serviço:

```bash
sudo systemctl daemon-reload
```

5. Ative o serviço para que ele seja iniciado durante a inicialização:

```bash
sudo systemctl enable sync-hwclock.service
```

6. Agora, você pode iniciar o serviço manualmente:

```bash
sudo systemctl start sync-hwclock.service
```

O serviço será automaticamente iniciado durante a próxima inicialização do sistema.
```bash
sudo reboot
```

# OUTRAS OPÇõES:
Temos como configurar o `rtc` padrão desativando o `ntp`, esse processo atualiza a data e hora do sistema e do rtc definido.
## Sincronização NTP

### Desabilitar NTP

Para configurar o RTC, devemos fechar a sincronização NTP. Vamos desativá-lo com estes comandos:

```bash
sudo timedatectl set-local-rtc 0
sudo timedatectl set-ntp 0
```

### Definir hora RTC padrão

Agora, vamos alterar a hora do sistema para verificar o funcionamento do RTC. 
```bash
sudo timedatectl set-time "2024-03-06 18:23:00"
```
### Habilitar NTP

Se quiser restaurar todas as alterações, você pode habilitar o NTP. 

```bash
sudo timedatectl set-ntp 1
```
