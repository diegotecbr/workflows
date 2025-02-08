```mermaid
flowchart LR
    A([Dev Commit / Pull Request<br>no Repositório GitHub]) -->|Dispara Workflow| B([Job: Build (Matrix)])
    
    B --> BuildStart
    subgraph Build
      BuildStart --> C1[Build Lambda 1<br>Publicar ZIP em Artifactory]
      BuildStart --> C2[Build Lambda 2<br>Publicar ZIP em Artifactory]
    end
    
    C1 --> D([Job: Infra (Matrix)])
    C2 --> D
    
    D --> InfraStart
    subgraph Infra
      InfraStart --> E1[Terraform para Lambda 1<br>(infra/lambda1)]
      InfraStart --> E2[Terraform para Lambda 2<br>(infra/lambda2)]
    end
    
    E1 --> F([Job: Deploy (Matrix)])
    E2 --> F
    
    F --> DeployStart
    subgraph Deploy
      DeployStart --> G1[Recuperar ZIP Lambda 1<br>do Artifactory<br>aws lambda update-function-code]
      DeployStart --> G2[Recuperar ZIP Lambda 2<br>do Artifactory<br>aws lambda update-function-code]
    end
    
    G1 --> H([Status Check Lambda 1: OK])
    G2 --> H([Status Check Lambda 2: OK])
    
    H -->|Merge liberado<br>se todos verdes| I([Branch Protection<br>na main])
```
