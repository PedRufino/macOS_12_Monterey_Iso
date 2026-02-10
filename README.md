
# Link para Download da ISO do macOS 12 Monterey

> ⚠️ Este guia é experimental. O desempenho do macOS em VirtualBox é limitado e não substitui um Mac real.

Este repositório documenta o processo de instalação e execução do macOS 12 Monterey em máquina virtual utilizando VirtualBox.

O objetivo é auxiliar estudantes, desenvolvedores e pesquisadores que precisam testar compatibilidade de software (especialmente Safari/iOS) sem possuir um computador Apple físico.

*Se você for um representante da Apple Inc. ou de qualquer outra parte envolvida, leia a seção [Legal](#legal-) abaixo.*

## Sumário 📚

- [Obtenção da imagem do sistema](#obtenção-da-imagem-do-sistema-)
- [Perguntas Frequentes](#perguntas-frequentes-)
- [Criando a Máquina Virtual no VirtualBox](#criando-a-máquina-virtual-no-virtualbox)
- [Ajustes após criar a VM](#ajustes-após-criar-a-vm)
- [Configuração da Máquina Virtual (VirtualBox)](#configuração-da-máquina-virtual-virtualbox)
- [Instalando o macOS Monterey](#instalando-o-macos-monterey)
- [Ajustando a resolução da tela](#ajustando-a-resolução-da-tela)
- [Aviso Legal](#aviso-legal-)

## Obtenção da imagem do sistema 📥

> [!IMPORTANT]
> Você pode baixar as ISOs usando torrents, que também são muito mais rápidos, graças aos incríveis seeders!

  
  | Version                            | Download Link                         |
  |------------------------------------|---------------------------------------|
  | macOS 12 Monterey | [Torrent](https://raw.githubusercontent.com/PedRufino/macOS_12_Monterey_Iso/main/macOS_12_Monterey.iso.torrent) |


## Perguntas Frequentes ❓

### Como baixar a ISO?

> [!NOTE]
> Se você não sabe o que é um torrent, recomendo que assista a [este vídeo](https://www.youtube.com/watch?v=pQaVDmbQU_U) do [TecMundo](https://www.youtube.com/@tecmundo).

Mas para baixar os arquivos em si, você precisa de um cliente de torrent como o [qBittorrent](https://www.qbittorrent.org/download).

Lembre-se de que, após o download, você deve semear o arquivo por um tempo para ajudar outras pessoas a baixá-lo também.

## Criando a Máquina Virtual no VirtualBox

Faça o download do [VirtualBox](https://www.virtualbox.org/wiki/Downloads) abra e clique em **Novo** para criar uma nova máquina virtual.

Use as seguintes configurações:

### Configurações básicas

-   **VM Nome:** Mac OS
    
-   **ISO Image:** caminho/para/o/arquivo.iso
    
-   **OS:** Mac OS X

-  **OS Version:** Mac OS X (64-bit)
    

### Recursos mínimos recomendados

- Specify virtual hardware
	-   **CPU:** mínimo de 2 núcleos (2 CPUs)
    
	-   **Memória RAM:** mínimo de 4 GB - 4096 MB (recomendado 8 GB - 8192 MB)
	
- Specify virtual hard disk
	-   **Armazenamento:** mínimo de 80 GB
    

> Quanto mais RAM e CPU você disponibilizar, melhor será o desempenho do macOS.


## Ajustes após criar a VM

Após criar a máquina virtual, **não inicie ainda**.

Clique com o botão direito na VM → **Configurações**.

### Tela (Display)

Vá em **Tela → Memória de Vídeo (Video Memory)** e ajuste para:

```
128 MB
```

Isso é essencial para evitar tela preta ou travamentos gráficos.

----------

### USB (teclado e mouse não funcionam)

Caso o teclado ou mouse não respondam dentro do macOS feche a VM:

1.  Vá em **USB**
    
2.  Ative a controladora USB
    
3.  Selecione:
    

```
Controladora USB 3.0 (xHCI)
```

Isso normalmente corrige problemas de entrada.

----------

Depois disso, você pode prosseguir para a seção de **Configuração da Máquina Virtual (VirtualBox)** onde são executados os comandos `VBoxManage`.


## Configuração da Máquina Virtual (VirtualBox)

Após criar a máquina virtual no VirtualBox e adicionar o disco/ISO, é necessário aplicar algumas configurações extras para que o macOS consiga iniciar corretamente.

> ⚠️ **IMPORTANTE:**  
> O VirtualBox deve estar **fechado** antes de executar os comandos abaixo.

Abra um terminal no seu sistema operacional (Linux/Windows) e execute:

```bash
VBoxManage modifyvm "Mac OS" --cpuid-set 00000001 000106e5 00100800 0098e3fd bfebfbff

VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "MacBookPro15,1"
VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Mac-551B86E5744E2388"

VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1

VBoxManage setextradata "Mac OS" "VBoxInternal/TM/TSCMode" "RealTSCOffset"
```

Esses comandos fazem o VirtualBox emular um hardware Apple compatível (SMC + SMBIOS), necessário para o boot do macOS.

----------

### Processadores AMD

Se o seu computador possuir processador **AMD**, execute também:

```bash
VBoxManage modifyvm "Mac OS" --cpu-profile "Intel Core i7-6700K"
```

Isso força o VirtualBox a simular um processador Intel compatível com o kernel do macOS.

----------

### Nome da Máquina Virtual

O nome da VM precisa ser **exatamente igual** ao usado nos comandos:
```
Mac OS
```

Se você criou com outro nome (ex: `macOS` ou `MacOS Monterey`), altere o nome da VM no VirtualBox ou ajuste os comandos substituindo `"Mac OS"` pelo nome correto.

----------


## Instalando o macOS Monterey

Após concluir todas as configurações anteriores, inicie a máquina virtual.

Na primeira inicialização aparecerá o assistente de instalação do macOS.

----------

### 1. Seleção de idioma

Escolha o idioma desejado:

```
Português (Brasil)
```

Clique em **→ Continuar**.

----------

### 2. Preparar o disco (ETAPA MAIS IMPORTANTE)

Na tela seguinte aparecerão várias opções de utilitários do macOS.

Selecione:

```
Utilitário de Disco (Disk Utility)
```

e clique em **Continuar**.

Agora:

1.  No topo da janela, clique em **Visualizar (View)**
    
2.  Selecione **Mostrar Todos os Dispositivos (Show All Devices)**
    

No lado esquerdo aparecerão os discos disponíveis.

Na seção **Interno**, selecione o disco chamado:

```
VBOX HARDDISK Media
```

Com o disco selecionado:

1.  Clique em **Apagar (Erase)**
    
2.  Será exibida uma janela de configuração
    

Altere apenas:

-   **Nome:** (pode escolher qualquer nome, ex: `Macintosh HD`)
    

Não é necessário mudar mais nada.

Clique em **Apagar**  
Aguarde o processo terminar e depois clique em **OK**.

Agora feche o Utilitário de Disco.

----------

### 3. Instalar o macOS

De volta ao menu principal, selecione:

```
Instalação do macOS Monterey
```

Clique em **Continuar**.

Depois:

1.  Clique em **Continuar**
    
2.  Aceite os termos de uso
    
3.  Selecione o disco que você acabou de formatar
    
4.  Clique em **Continuar**
    

----------

### 4. Processo de instalação

O sistema começará a instalar.

> ⏳ A instalação pode demorar bastante (30 a 90 minutos dependendo do computador).

Durante esse processo:

-   A máquina virtual pode reiniciar várias vezes
    
-   A tela pode ficar parada por alguns minutos
    

Isso é **normal**.  
Não desligue a máquina virtual.

----------

### 5. Configuração inicial do macOS

Quando a instalação terminar, aparecerá a tela de configuração do sistema.

Siga os passos:

1.  **País ou Região:** selecione `Brasil`
    
2.  Configure as opções conforme preferir (pode aceitar ou pular)
    
3.  Aceite novamente os termos de uso
    

----------

### 6. Criar usuário

Crie sua conta do computador:

-   Nome completo
    
-   Nome da conta
    
-   Senha
    

Guarde essa senha — ela será usada para instalar programas no macOS.

----------

### 7. Fuso horário

Selecione:

```
São Paulo - Brasil
```

ou a sua região correspondente.

----------

### 8. Finalização

Continue avançando nas configurações:

-   Privacidade
    
-   Siri
    
-   Aparência (claro ou escuro)
    

Após finalizar, o macOS iniciará normalmente.

🎉 **Pronto! Sua máquina virtual com macOS Monterey está funcionando.**
    

É normal o macOS ficar um tempo parado na logo da Apple durante a primeira inicialização.

## Ajustando a resolução da tela

Caso a resolução do macOS fique muito pequena ou esticada, é possível defini-la manualmente.

1.  **Desligue completamente a máquina virtual**
    
    > Não use "Salvar estado".  
    > Escolha **Desligar a máquina (Power Off)**.
    
2.  Após desligar, abra um terminal no seu sistema operacional (Linux/Windows) e execute:
    

```bash
VBoxManage setextradata "Mac OS" VBoxInternal2/EfiGraphicsResolution 1920x1080

```

Você pode substituir `1920x1080` por qualquer resolução compatível com o seu monitor.

Recomenda-se utilizar a mesma resolução nativa da sua tela.

Exemplos:

```
1366x768
1600x900
1920x1080
2560x1440
```

3.  Inicie novamente a máquina virtual.
    

O macOS já deverá iniciar na nova resolução definida.

## Aviso Legal 📜

*Este repositório não possui qualquer vínculo com a Apple Inc. Apple, macOS e todos os outros produtos Apple são marcas registradas da Apple Inc., registradas nos EUA e em outros países.*

Não possuo os direitos de nenhuma versão do macOS listada aqui, nem promovo ou apoio a pirataria. Esta ISO fornecida estritamente para **fins educacionais** e **uso pessoal**.

Ao baixar ou usar estas ISOs, você **concorda em cumprir** os **Termos de Serviço (TOS)** da Apple e quaisquer **contratos de licença de software** aplicáveis. É sua responsabilidade garantir que seu uso do macOS esteja em conformidade com todas as leis aplicáveis, contratos de licença e os TOS da Apple.

**Se a Apple Inc. ou qualquer outra parte envolvida tiver algum problema com este repositório, abra uma [issue](https://github.com/PedRufino/macOS_12_Monterey_Iso/issues). Removerei este repositório, juntamente com quaisquer arquivos hospedados e torrents, o mais rápido possível!**
