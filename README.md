# Script passo a passo dos Checkpoints IaC



### Checkpoint 04 - 2º Semestre

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


8. Entrar no repo do [Ian Soares](https://github.com/Ian-Soares/app-static-site-ec2/tree/develop/terraform/modules) como referência

9. Copiar todo codigo que esta nos arquivos **compute** e **rede** (esta como network o dele) 

![ian](/images/ian.png)

10. Criar na AWS o **S3** e **DynamoDB**  coloque um nome que seja facil para reconhecer


11. Criar agora na pasta **terraform** o **provider.tf** com esses códigos

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

12. Criar agora na pasta **terraform** o **orch.tf** com esses códigos:

```
module "rede" {
    source      = "./modules/rede"
    rede_cidr   = "20.0.0.0/16"
    subnet_cidr = "20.0.1.0/24"
}

module "compute" {
    source    = "./modules/compute"
    rede_id   = "${module.rede.vpc_id}"
    rede_cidr = "20.0.0.0/16"
    ami       = "ami-02e136e904f3da870"
    subnet_id = "${module.rede.subnet_id}"
}

```


> **Note**: Não esqueca de trocar a AMI do **vars.tf** para a mesma do **orch.tf**





13. Criar o pipeline - Crie uma pasta **.github/workflows** e dentro dela crie o arquivo **pipe.yaml**

![pipe](/images/pipe.png)

14. Coloque o código do **Pipeline** dessa forma:

```
name: Terraform Pipeline

on:
  push:
    branches:
    - develop

jobs:

  tf-run:
    name: Terraform Run
    runs-on: ubuntu-latest
 
    steps:

    - name: Step 01 - Terraform Setup
      uses: hashicorp/setup-terraform@v2
      with:
        terraform_version: 1.5.6

    - name: Step 02 - Terraform Version
      run : terraform --version

    - name: Step 03 - CheckOut GitHub Repo
      uses: actions/checkout@v3

    - name: Step 04 - Set AWS Account
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id    : ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-session-token    : ${{ secrets.AWS_SESSION_TOKEN }}
        aws-region           : us-east-1

    - name: Step 05 - Terraform Init
      run : terraform -chdir=./terraform init -input=false

    - name: Step 06 - Terraform Unit Tests
      run : terraform -chdir=./terraform validate

    - name: Step 07 - Terraform Plan with Contract Tests
      run : terraform -chdir=./terraform plan -input=false -out tfplan
      # run : terraform -chdir=./terraform plan -input=false -destroy -out tfplan

    - name: Step 08 - Terraform Security Scannning Setup
      run: pip3 install checkov

    - name: Step 09 - Terraform Security Scannning Run
      run: checkov --directory ./terraform

    - name: Step 10 - Terraform Apply
      run : terraform -chdir=./terraform apply -auto-approve -input=false tfplan

    - name: Step 11 - Terraform Show
      run : terraform -chdir=./terraform show
      
      
```
15. Fazer o comandos para subir o repositório

```
git add .
```
```
git commit -m "cp-1"
```
```
git push origin develop
```
> **Note**: Se não aparecer o **Actions**, suba mais um **commit** - exemplo: dar algum espaçamento em algum arquivo e salvar 