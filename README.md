# Script passo a passo dos Checkpoints IaC
Ajuda para fazer os checkpoints do Kledson
<br>

## Checkpoint 04 

### Passo-a-passo

01. Fazer um **Fork** do repositório [app-static-site-ec2](https://github.com/kledsonbasso/app-static-site-ec2) 

02. Clone em uma pasta o repositório 

```
git clone https://github.com/NaderSouza/app-static-site-ec2-teste.git 
```

03. Crie a **Branch** develop

```
git checkout - b develop 
```

04. Entrar na **AWS** e dar start o **LAB** e pegar as credencias
e colocar no GitHub secrets 

![secrets](/images/secret.png)


5. Ajustar o código do Terraform para uma estrutura em módulos, contendo dois módulos (rede e compute).


6. Crias a pasta modules com as pastas **rede** e **compute**

7. Criar os arquivos de acordo com a imagem abaixo: 

![secrets](/images/modulos.png)


8. Entrar no repo do [Ian Soares](https://github.com/Ian-Soares/app-static-site-ec2/tree/develop/terraform/modules) 

9. Copiar todo codigo que esta nos arquivos **compute** e **rede** (esta como network) dele


10. Criar agora na pasta **terraform** o **provider.tf**

<br>

```
# PROVIDER
terraform {

  required_version = "~> 1.5.6"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.13"
    }
  }

    backend "s3" {
      bucket         = "app-static-site-ec2-tf-nadin"
      key            = "terraform.tfstate"
      dynamodb_table = "app-static-site-ec2-tf-nadin"
      region         = "us-east-1"
    }

}
```
11. Criar o pipeline - Crie uma pasta **.github/workflows** e dentro dela crie o arquivo **pipe.yaml**

![pipe](/images/pipe.png)


