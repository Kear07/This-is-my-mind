---

---
---
#### Comandos Linux

apt-get update                     = Atualiza o sistema 
apt-get upgrade software           = Atualiza uma única coisa 
apt-get install software           = Instala um software

pwd                                = Exibe o diretório atual 
ls                                 = Exibe arquivos do diretório 
ls -l                              = Exibe arquivos com detalhes 
cd /                               = Volta a pasta principal 
cd pasta                           = Acessa a pasta  
cd ..                              = Volta para o diretório anterior

date                               = Exibe a data da maquina 
date > data.txt                    = Coloca data dentro de um arquivo 
date >> data.txt                   = Adiciona data ao arquivo

history                            = Exibe o histórico de comandos 
clear                              = Limpa a tela 

mkdir pasta                        = Cria uma nova pasta 
touch arquivo.txt                  = Cria um arquivo vazio 
cat arquivo                        = Exibe o conteúdo do arquivo

cat > arquivo                      = Criar um arquivo com texto
cat >> arquivo                     = Adiciona texto ao arquivo

cat /etc/passwd                    = Exibe os usuários do sistema
cat /etc/group                     = Exibe os grupos do sistema
 
rm pasta_arquivo                   = Exclui pasta/arquivo 
rm -r *                            = Exclui o sistema antes de /home 

find -name "text*"                 = Procura o arquivo/pasta 
find -name /. "text*"              = Procura em todo o sistema 

cp conteudo destino/               = Copia o arquivo para o destino 

mv arquivo /pasta                  = Move o arquivo para a pasta 
mv arqu arquivo                    = Renomeia o arquivo  

grep -n "texto" text.txt           = Procura a linha no arquivo

adduser user                       = Cria um usuário 
passwd senha                       = Altera senha de um usuário 
login user                         = Faz login no usuário 
logout                             = Volta como root 

ps -all                            = Exibe as tarefas/processos 
tail -3 /etc/group                 = Exibe as 3 ultimas linhas 

usermod user -G group              = Adiciona um usuário a um grupo 
usermod user -G root               = Concede permissão de root

groupdel group                     = Exclui um grupo 
userdel user                       = Exclui o usuário 
 
chmod 013 arquivo/pasta            = Altera a permissão 
chown aluno arquivo                = Define o user como dono 
chown :grupo arquivo               = Altera o grupo do arquivo  

-----
#### Gerenciar permissões

-- Entidades
U = Dono 
G = Grupo 
O = Outros 
A = Todos 

-- Arquivos/Pastas
R = Ler 
W = Modificar  
X = Executar 

-- Permissão padrão
![[permissões especiais.png]]

-- Permissão especial
![[permissões.png]]

-- Outro método
R = 4 | W = 2 | X = 1

r w x | - - - | r w x
4 2 1 | 0 0 0 | 4 2 1

7 0 7

rwx --- rwx = 707

-- Dica extra
Coloque letra maiúscula quando estiver vazio