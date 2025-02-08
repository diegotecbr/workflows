```mermaid
flowchart LR
    A([Dev Commit / Pull Request<br>no Repositório GitHub]) -->|Dispara Workflow| B([Job: Build (Matrix)])
    
    subgraph Build
      B --> C1[Build Lambda 1<br>Publicar ZIP em Artifactory]
      B --> C2[Build Lambda 2<br>Publicar ZIP em Artifactory]
    end
    
    C1 --> D([Job: Infra (Matrix)])
    C2 --> D
    
    subgraph Infra
      D --> E1[Terraform para Lambda 1<br>(infra/lambda1)]
      D --> E2[Terraform para Lambda 2<br>(infra/lambda2)]
    end
    
    E1 --> F([Job: Deploy (Matrix)])
    E2 --> F
    
    subgraph Deploy
      F --> G1[Recuperar ZIP Lambda 1<br>do Artifactory<br>aws lambda update-function-code]
      F --> G2[Recuperar ZIP Lambda 2<br>do Artifactory<br>aws lambda update-function-code]
    end
    
    G1 --> H([Status Check Lambda 1: OK])
    G2 --> H([Status Check Lambda 2: OK])
    
    H -->|Merge liberado<br>se todos verdes| I([Branch Protection<br>na main])
```
