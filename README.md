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






# Definir hora RTC padrão, que automaticamente atualiza a hora do sistema

Agora, vamos alterar a hora do sistema para verificar o funcionamento do RTC. 
```bash
sudo timedatectl set-time "2024-03-06 18:23:00"
timedatectl status
```
