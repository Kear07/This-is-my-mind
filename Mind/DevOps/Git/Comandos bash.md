-----------
#### Git bash comandos

git help                         = Exibe o manual do git 
git config                       = Conecta sua conta Git-Hub

git init                         = Inicia um repositório local
git status                       = Exibe o estado dos arquivos

git add .                        = Prepara todos os arquivos
git add                          = Prepara um arquivo 
git clean -n	                 = Simula uma limpeza
git clean -f	                 = Remove arquivos não rastreados
git clean -d	                 = Remove pastas não rastreados

git reset [arquivo]              = Volta o arquivo para modificado
git reset --soft HEAD~1          = Desfaz commit, mantém em preparado
git reset --mixed HEAD~1         = Desfaz commit, mantém arquivos
git reset --hard HEAD~1          = Apaga commit e arquivos

git revert [hash]	             = Desfaz um commit

git commit -m "msg"	             = Commita com mensagem
git commit --amend	             = Altera o último commit
git commit --allow-empty -m 	 = Commita sem mudanças

git restore [arquivo]	         = Desfaz alterações no arquivo
git restore --staged [arquivo]	 = Remove do commit
git restore .	                 = Desfaz alterações em tudo

git clone [link]                 = Clona o repositório do GitHub
 
git remote                       = Exibe as branches  
git remote -v                    = Exibe as branches com detalhes  

git branch -m [nome]	         = Renomeia branch atual
git branch -m [antes depois]	 = Renomeia outra branch
git branch -M [nome]	         = Renomeia (força se necessário)

git branch	                     = Exibe as branches locais
git branch -a	                 = Exiba as branches locais e remotas

git branch [nome]                = Cria uma nova branch
git checkout -b [nome]	         = Cria e troca para a branch
git switch -c [nome]             = Cria e troca para a branch

git branch -d [nome]	         = Deleta a branch
git branch -D [nome]	         = Força a deleção da branch
git push origin --delete [nome]	 = Deleta a branch remota

git checkout [nome]	             = Troca de branch
git switch [nome]	             = Troca para branch

git remote add origin [link]     = Conecta com o repositório GitHub
 
git push origin [class]          = Envia as mudanças para o repositório GitHub
git pull origin [class]          = Recebe as mudanças do repositório GitHub                
git remote	                     = Exibe os nomes das conexões
git remote -v	                 = Exibe as URLs das conexões
git remote add origin [URL]	     = Adiciona um nova conexão
git remote remove origin	     = Remove uma conexão remota
git remote rename old new	     = Renomeia uma conexão remota
git remote show origin	         = Exibe os detalhes da conexão

git log                          = Exibe todas os commits 
git log --oneline                = Exibe todos os commits simplificados 

git merge [class]	             = Mescla com outra branch
git merge --squash [class]	     = Junta commits em um só
git merge --abort  [class]       = Cancela merge com conflito

git config --global user.email "seu-email@exemplo.com"   = Configura seu email
git config --global user.name "Seu Nome"                 = Configura seu nome