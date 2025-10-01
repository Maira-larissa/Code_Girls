# Code_Girls

# Desafio DIO: Computação na Nuvem com AWS EC2, EBS e S3

Este repositório documenta a experiência prática e os *insights* adquiridos durante o laboratório de gerenciamento de instâncias Amazon EC2, EBS e S3, servindo como material de apoio e guia para futuras implementações na AWS.

## Objetivos de Aprendizagem

O foco deste laboratório foi consolidar a compreensão sobre os pilares fundamentais da computação e armazenamento em nuvem na AWS:

  * **Aplicar** os conceitos básicos de *cloud computing* em um ambiente prático.
  * **Documentar** processos técnicos de forma clara e estruturada.
  * **Utilizar** o GitHub como ferramenta de compartilhamento de conhecimento técnico.

## Anotações e Insights Adquiridos

### 1\. Amazon EC2 (Elastic Compute Cloud)

O EC2 é a espinha dorsal da computação na AWS, funcionando como um **servidor virtual (instância)** na nuvem.

| Conceito | Descrição | Insight Chave |
| :--- | :--- | :--- |
| **Instância** | O servidor virtual que hospeda o SO e a aplicação. | A escolha do tipo de instância define o poder de CPU, RAM e o custo.
| **AMI (Amazon Machine Image)** | Um *template* que contém o sistema operacional, software e configurações. | É a forma mais eficiente de **duplicar ambientes** ou restaurar estados de servidor. |

### 2\. Amazon EBS (Elastic Block Store)

Uma storage(armazenamento) altamente confiável que pode ser anexado em qualquer instância EC2.

| Conceito | Descrição | Insight Chave |
| :--- | :--- | :--- |
| **Volume EBS** | Um disco de rede que só pode ser anexado a uma única instância EC2 por vez, mas pode ser **desanexado** e **reatachado** a outra instância na mesma Zona de Disponibilidade (AZ). | É a chave para a **persistência de dados**. Se a instância EC2 falhar, o Volume EBS sobrevive. |
| **Snapshots** | Um backup pontual do Volume EBS armazenado no Amazon S3. | Snapshots são incrementais (armazenam apenas as mudanças desde o último snapshot), o que torna os **backups mais rápidos e econômicos**. |

### 3\. Amazon S3 (Simple Storage Service)

Amazon S3 (Amazon Simple Storage Service) é um serviços de armazenamento de objetos em nuvem oferecidos pela AWS. É ideal para armazenar, organizar e recuperar grandes volumes de dados de forma segura e escalável. Alta disponibilidade(entre 99% e 99.9%)

| Conceito | Descrição | Insight Chave |
| :--- | :--- | :--- |
| **Bucket** | O contêiner de nível superior para armazenar objetos. Os nomes devem ser **globalmente únicos**. | O S3 é usado para **arquivos estáticos** (imagens, vídeos, documentos). **Não** é um sistema de arquivos tradicional (como o EBS). |

-----

## Diagramas de Arquitetura com EC2, EBS e S3

Os diagramas a seguir ilustram como os serviços podem ser combinados para criar soluções na nuvem.

### 1\. Arquitetura: EBS junto com o EC2 (Servidor de Aplicação)

Este cenário foca na persistência dos dados da aplicação, utilizando o EBS como o disco da instância.

```mermaid
graph TD
    subgraph AWS Cloud (Region)
        subgraph EC2 Hosting (Availability Zone)
            EC2[AWS EC2 Instance: Servidor de Aplicação] -->|Anexo de Rede| EBS(AWS EBS Volume: Disco Persistente)
            EBS --> Data(Dados Transacionais/SO)
        end
        LB[AWS ELB/ALB: Load Balancer] --> EC2
    end
    User(Usuário/Cliente) --> LB
```

### 2\. Arquitetura: S3 junto com o Lambda (Processamento Serverless de Mídia)

Este cenário *serverless* demonstra uma arquitetura orientada a eventos, onde o armazenamento aciona a computação.

```mermaid
graph TD
    subgraph AWS Cloud (Serverless Architecture)
        S3(AWS S3 Bucket: Input de Imagens) -->|Gatilho: Evento PUT| Lambda(AWS Lambda Function: Redimensionamento/Análise)
        Lambda --> S3_OUTPUT[AWS S3 Bucket: Imagens Processadas]
        Lambda --> CloudWatch[AWS CloudWatch: Logs de Execução]
    end
    Uploader[Usuário/Serviço] --> S3
```

### 3\. Arquitetura Híbrida: EC2, S3 e EBS (Aplicação Web Completa)

Este diagrama combina os três serviços para uma aplicação web escalável, separando o armazenamento de objetos do armazenamento de bloco.

```mermaid
graph TD
    subgraph AWS Cloud (Web Application)
        subgraph Compute Layer (AZ)
            EC2(AWS EC2 Instance: Servidor da Aplicação)
            EC2 --> EBS(AWS EBS Volume: SO e Logs de Aplicação)
        end
        S3(AWS S3: Arquivos Estáticos/Mídia)
        
        ELB[AWS ELB/ALB: Load Balancer] --> EC2
        EC2 -->|Acesso a Conteúdo Estático| S3
        User(Usuário) --> ELB
        User -->|Conteúdo via CDN| S3
    end
```

-----

## Recursos Úteis Consultados

  * [Gerenciando EC2 instâncias da Amazon - Documentação AWS](https://www.google.com/search?q=link_para_documentacao)
  * [minhas próprias anotações do notion](https://www.notion.so/Computa-o-na-Nuvem-com-EC2-2699f90a75e2800d9ec8f04cd0464e49)

-----
