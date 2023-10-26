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

![secrets](/images/secrets.png)


5. Ajustar o código do Terraform para uma estrutura em módulos, contendo dois módulos (rede e compute).


6. Crias a pasta modules com as pastas **rede** e **compute**

7. Criar os arquivos de acordo com a imagem abaixo: 

![secrets](/images/modulos.png)


