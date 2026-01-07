-------------

docker --help                          = Exibe ajuda 
docker --version                       = Exibe a versão do docker 

docker info                            = Exibe informações de execução 
docker inspect [objeto]                = Exibe informações por id ou nome 
docker stats                           = Exibe informações dos containers 

docker pull [img]                      = Baixa uma imagem 
docker images -a                       = Exibe todas as imagens baixadas 
docker image ls                        = Exibe todas as imagens baixadas 

docker container ls                    = Exibe todos os containers 
docker container ls -a                 = Exibe todos os container 

docker container create [img]          = Cria um container 
docker rename [antes depois]           = Renomeia um container 

docker run [img]                       = Cria e inicia um container 
docker run --name [nome  img]          = Cria e inicia um container

docker container restart [nome]        = Reinicia um container
docker container stop [nome]           = Para a execução do container 
docker kill [nome]                     = Para a execução do container 

docker container pause [nome]          = Pausa um container 
docker container unpause [nome]        = Despausa um container 

docker rm [nome]                       = Deleta um container 
docker rm -f [nome]                    = Deleta um container 

docker rmi [img]                       = Deleta uma imagem 
docker rmi -f [img]                    = Deleta uma imagem

docker logs [nome]                     = Exibe os logs do container 
docker logs -f [nome]                  = Exibe os logs do container                       
docker container ls -a

|**Coluna**|**Descrição**|
|---|---|
|**CONTAINER ID**|Identificador único do container.|
|**IMAGE**|Nome da imagem usada para criar o container.|
|**COMMAND**|Comando que foi executado quando o container foi iniciado.|
|**CREATED**|Tempo decorrido desde a criação do container.|
|**STATUS**|Estado atual do container (ex: "Up" para em execução, "Exited" para parado).|
|**PORTS**|Portas mapeadas entre o host e o container.|
|**NAMES**|Nome do container (gerado automaticamente ou definido pelo usuário).|
