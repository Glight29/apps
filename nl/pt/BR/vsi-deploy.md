---
copyright:

  years: 2018

lastupdated: "2018-06-28"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Implementando em um servidor virtual
{: #vsi-deploy}

Implemente os apps {{site.data.keyword.cloud}} [App Service ![Ícone de link externo](../icons/launch-glyph.svg)](https://console.bluemix.net/developer/appservice/starter-kits){: new_window} em instâncias do servidor virtual para permitir que suas atividades de desenvolvedor de plataforma e infraestrutura funcionem juntas e aumentem o controle e a flexibilidade do app.
{: shortdesc}

Uma instância de servidor virtual oferece melhor transparência, previsibilidade e automação para todos os tipos de carga de trabalho quando comparada com outras configurações. Combine-a com um servidor bare metal para criar combinações de carga de trabalho exclusivas. Por exemplo, é possível criar a lógica de banco de dados de alto desempenho ou o aprendizado de máquina com configurações bare metal e GPU executando um sistema operacional baseado em Linux Debian.

## Antes de começar
Para usar instâncias virtuais, sua conta do {{site.data.keyword.cloud_notm}} deve estar ativada para infraestrutura. Para obter mais informações, consulte [Fazer upgrade para a infraestrutura ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/dashboard/ibm-iaas-g1){: new_window}.

## Implementando por meio do Terraform
Qualquer um dos kits do iniciador do App Service pode ser implementado em uma instância virtual criada dinamicamente por meio do [Terraform ![Ícone de link externo](../icons/launch-glyph.svg)](https://ibm-cloud.github.io/tf-ibm-docs/v0.10.0/){: new_window}, uma estrutura de software livre de infraestrutura como código. Para obter mais informações, consulte [Documentação do Terraform ![Ícone de link externo](../icons/launch-glyph.svg)](https://www.terraform.io/docs/index.html){: new_window}.

## Ativando sua implementação de pipeline

Ao criar um kit do iniciador que usa o {{site.data.keyword.cloud_notm}} [App Service ![Ícone de link externo](../icons/launch-glyph.svg)](https://console.bluemix.net/developer/appservice/starter-kits){: new_window}, a instância do servidor virtual é ativada. Após o app ser criado, será possível escolher onde deseja implementá-lo. Os kits do iniciador são ativados para suportar a implementação usando uma cadeia de ferramentas de Continous Delivery. Os kits do iniciador podem ter como destino o Kubernetes, o Cloud Foundry e as Instâncias do Virtual Server. A cadeia de ferramentas inclui um repositório de código-fonte e um pipeline de implementação.

A opção do servidor virtual funciona em fases. Primeiramente, o código do app é preparado e armazenado em um repositório Git GITLab e o código-fonte cria uma cadeia de ferramentas com um pipeline. O pipeline é definido para construir o código e o empacotá-lo em um formato de gerenciador de pacote do Debian. Em seguida, Terraform provisões uma instância virtual. Finalmente, o app é implementado, instalado e iniciado dentro da imagem virtual em execução e seu funcionamento é validado.

O pipeline falha no primeiro uso porque ele requer que um conjunto de propriedades da conta do usuário seja configurado manualmente. Essas propriedades não podem ser transmitidas para a cadeia de ferramentas que usa o código-fonte GIT por razões de segurança. Deve-se configurar manualmente esses valores para permitir que a cadeia de ferramentas seja concluída com êxito.

Siga estas etapas para obter a visualização de propriedades do ambiente e ativar seu pipeline com as propriedades da conta do usuário.

1. Na página Detalhes do app, clique em **Visualizar cadeia de ferramentas**.
2. Clique no ladrilho **Delivery Pipeline**.
3. Clique em **Configurar estágio** no estágio de construção.
4. Clique na guia **Variáveis de ambiente** para visualizar as propriedades.
5. Mude essas propriedades na guia Variáveis de ambiente para permitir que a cadeia de ferramentas seja executada com êxito.

| Propriedade   | Descrição   |
|---|---|
| `TF_VAR_ibm_sl_api_key` | A [chave API de infraestrutura](#iaas-key) é do console de infraestrutura. |
| `TF_VAR_ibm_sl_username` | O [nome do usuário da infraestrutura](#user-key) que identifica a conta de infraestrutura |
| `TF_VAR_ibm_cloud_api_key` | A [chave API da plataforma](#platform-key) do {{site.data.keyword.cloud_notm}} é usada para ativar a criação de serviço. |
| `PUBLIC_KEY` | [Chave pública](#public-key) que é definida para ativar o acesso à instância de servidor virtual. |
| `PRIVATE_KEY` | [Chave privada](#public-key) que é definida para ativar o acesso à instância de servidor virtual. |
| `VI_INSTANCE_NAME` | Nome gerado automaticamente para a instância do servidor virtual |
| `GIT_USER` | Se você configurar o [estado do Terraform](#tform-state) para armazenar o estado do comando apply, o nome do usuário GITLab será necessário. |
| `GIT_PASSWORD` | Se você configurar o [estado do Terraform](#tform-state) para armazenar o estado do comando apply, a senha GITLab será necessária. |
{: caption="Tabela 1. Variáveis de ambiente a serem mudadas para ativação" caption-side="top"}

### Recuperando a chave API de infraestrutura
{: #iaas-key}

O Terraform requer uma chave API de infraestrutura para criar recursos de infraestrutura. Siga estas etapas para recuperar uma chave de infraestrutura.

1. Acesse a [lista de usuários da infraestrutura ![Ícone de link externo](../icons/launch-glyph.svg)](https://control.bluemix.net/account/users){: new_window} ou **Menu** > **Infraestrutura** > **Conta** > **Lista de usuários**.
2. Localize os detalhes do usuário que estão criando a cadeia de ferramentas e clique em `Visualizar` na coluna Chave API ou clique em `Gerar`, ambas as etapas exibirão a chave API em um diálogo.
3. Copie a chave API e substitua o valor na configuração da Cadeia de Ferramentas `TF_VAR_ibm_sl_api_key`.

### Recuperando o nome do usuário da infraestrutura
{: #user-key}

Para recuperar o nome do usuário da infraestrutura, siga estas etapas.

1. Acesse a [lista de usuários da infraestrutura ![Ícone de link externo](../icons/launch-glyph.svg)](https://control.bluemix.net/account/users){: new_window} ou **Menu** > **Infraestrutura** > **Conta** > **Lista de usuários**.
2. Clique no usuário para o qual você deseja criar a cadeia de ferramentas.
3. Role para baixo para a propriedade **Nome do usuário da VPN**.
4. Recorte e cole esse valor e substitua a configuração da Cadeia de Ferramentas `TF_VAR_ibm_sl_username`.

### Recuperando a chave API da plataforma
{: #platform-key}

Para ativar o Terraform para criar serviços de nível de plataforma como bancos de dados e editar serviços, a chave API de plataforma é necessária. Siga estas etapas para recuperar uma chave de plataforma.

1. Clique em **Menu** > **Painel** para certificar-se de que o usuário esteja no painel da plataforma.
2. Clique em **Gerenciar** > **Segurança** > **Chaves API da Plataforma**.
3. Clique em **Criar**.
4. Insira um nome e uma descrição e clique em **Criar**.
5. Quando o diálogo for aberto, clique em *Mostrar* para revisar a chave.
6. Copie e cole a chave dentro da área de transferência ou faça download da chave.
7. Substitua o valor na configuração da cadeia de ferramentas `TF_VAR_ibm_cloud_api_key` pelo valor que foi gerado.

### Gerando a chave pública e privada
{: #public-key}

Para ativar a cadeia de ferramentas para instalar o pacote Debian na instância do Virtual Server, siga estas etapas.

1. Em seu cálculo de cliente, use as instruções a seguir para criar um [par de chaves pública e privada ![Ícone de link externo](../icons/launch-glyph.svg)](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/){: new_window}.
2. Acesse a [visualização de chaves SSH de infraestrutura ![Ícone de link externo](../icons/launch-glyph.svg)](https://control.bluemix.net/devices/sshkeys){: new_window} ou **Menu** > **Infraestrutura** > **Dispositivos** > **Gerenciar** > **Chaves SSH**.
3. Clique em **Incluir**.
4. Copie o conteúdo da chave privada que você criou anteriormente e cole-o no conteúdo da chave.
5. Forneça um nome à chave e clique em **Incluir**.

### Ativando o estado do Terraform
{: #tform-state}

O Terraform suporta o armazenamento do estado do comando `apply` do Terraform. Esse arquivo de estado executa o comando `apply` uma segunda vez para determinar se algum dos arquivos de configuração do Terraform mudou. É possível armazenar o estado no repositório GIT que o app e a configuração gerenciam.

Para ativar o armazenamento de estado, configure as propriedades `GIT_USER` e `GIT_PASSWORD` nas variáveis de ambiente da cadeia de ferramentas. Para recuperar os valores, siga estas etapas:

1. Na visualização Cadeias de ferramentas, acesse o repositório GIT por meio da Ferramenta de Código.
2. No GIT Lab, clique no **Ícone do perfil** e, em seguida, clique em **Configurações**.
3. Clique em **Conta**, copie o nome do usuário e substitua o valor `GIT_USER`.
4. Clique em **Token de acesso**.
5. Defina um nome para seu token e, em seguida, clique em **Criar token de acesso pessoal**.
6. Clique no ícone de cópia no campo `Seu novo token de acesso pessoal` e substitua o valor de `GIT_PASSWORD` nas propriedades da cadeia de ferramentas.

O estado do Terraform é armazenado em uma ramificação chamada `terraform` e não aciona o pipeline para execução caso ele tenha sido mudado.


### Ativando operações GIT
{: #git-repo}

Quando o aplicativo é implementado no {{site.data.keyword.cloud_notm}}, um repositório GIT Lab é criado para hospedar o código para gerenciamento de código-fonte. É possível usar operações do GIT para permitir que equipes trabalhem e entreguem mudanças para seu app. As pastas incluídas nesse repositório e uma explicação de seus conteúdos.

#### Pasta Debian
{: #debian-folder}

A pasta `debian` retém a configuração necessária para ativar o empacotamento do app em um [pacote Debian. ![Ícone de link externo](../icons/launch-glyph.svg)](https://www.debian.org/doc/manuals/debian-faq/ch-pkgtools.en.html){: new_window}

#### Pasta do Terraform
{: #terraform-folder}

A pasta `terraform` retém a configuração para a infraestrutura como código que pode ser usada para provisionar recursos de infraestrutura. O arquivo `main.tf` é a origem principal para mudar as opções para a configuração do Terraform.

```
resource "ibm_compute_vm_instance" "vm1" {
    hostname = "${var.vi_instance_name}"
    domain = "example.com"
    os_reference_code = "DEBIAN_8_64"
    datacenter = "${var.datacenter}"
    network_speed = 100
    hourly_billing = true
    private_network_only = false
    cores = 1
    memory = 1024
    disks = [25]
    local_disk = false
    ssh_key_ids = [
        "${ibm_compute_ssh_key.ssh_key_gip.id}"
    ]
}
```

Também é possível provisionar servidores bare metal com o Terraform. Para obter mais informações, consulte [Documentação do IBM Terraform Provider ![Ícone de link externo](../icons/launch-glyph.svg)](https://ibm-cloud.github.io/tf-ibm-docs/v0.10.0/){: new_window} e [Repositório GIT do IBM Terraform Provider ![Ícone de link externo](../icons/launch-glyph.svg)](https://github.com/IBM-Cloud/terraform-provider-ibm){: new_window}.

O `variables.tf` pode ser usado para mudar o data center que você deseja ter como destino para criar a instância virtual. Para ver a lista de data centers definidos na plataforma, consulte [Data centers ![Ícone de link externo](../icons/launch-glyph.svg)](https://www.ibm.com/cloud-computing/bluemix/data-centers){: new_window}.

Por padrão, o arquivo terraform é configurado para Washington e `wdc04`.

```
variable "datacenter" {
  description = "Washington"
  default = "wdc04"
}
```

### Entendendo os estágios da cadeia de ferramentas
{: #toolchain-stages}

A cadeia de ferramentas mostra a infraestrutura e a implementação do app em um pipeline simples. Quebre a parte da infraestrutura como código em um pipeline e a implementação do app em outro pipeline.

A cadeia de ferramentas é quebrada nos estágios a seguir:

1. O Estágio de Compilação clona o repositório GIT e empacota o código em um pacote Debian.
2. O Estágio de Plano do Terraform prepara um plano do Terraform.

  ```
  terraform init -input=false
  terraform validate
  terraform plan -var "ssh_public_key=$PUBLIC_KEY" -input=false -out tfplan
  ```

3. O Estágio de Aplicação do Terraform aplica a configuração do Terraform e aguarda até que o endereço IP do servidor virtual esteja disponível.

  ```
  terraform apply -auto-approve -input=false tfplan
  terraform output "host ip" > hostip.txt
  ```

4. O Estágio Implementar, Instalar, Iniciar move o pacote do Debian que é construído no primeiro estágio para o servidor virtual em execução, instala-o e, em seguida, inicia-o.
5. O Estágio de Verificação de Funcionamento valida se o terminal de funcionamento está disponível no app e, em seguida, conclui o pipeline.

Para acessar o app depois de iniciado, verifique os logs do Estágio de Verificação de Funcionamento. Clique nos links de URL listados nos logs que mostram o endereço IP e a porta do app.
