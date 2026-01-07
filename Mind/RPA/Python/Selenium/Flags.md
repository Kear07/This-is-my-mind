#### Configurações para o navegador:

.add_argument("--incognito")                  = Utiliza o modo anônimo
.add_argument("--disable-gpu")                = Desabilita o uso da GPU
.add_argument("--headless")                   = Executa em 2° plano
.add_argument("--window-size=[1920,1080]")    = Inicializa no tamanho desejado 
.add_argument("--start-maximized")            = Inicia o navegador maximizado 
.add_argument("--disable-extensions")         = Desativa extensões 
.add_argument("--disable-infobars")           = Remove a mensagem de automação 
.add_argument("--no-sandbox")                 = Evita problemas de permissões
.add_argument("--disable-dev-shm-usage")      = Reduz uso de memória
.add_argument("--log-level=[3,2,1]")          = Reduz mensagens no console
.add_argument("--lang=[pt-BR]")               = Define idioma  
.add_argument("--ignore-certificate-errors")  = Ignora erros de SSL 
.add_argument("--disable-popup-blocking")     = Desativa bloqueio de popups 
.add_argument("--disable-notifications")      = Desativa notificações push 
.add_argument("--mute-audio")                 = Silencia sons no navegador 
.add_argument("--hide-scrollbars")            = Oculta as barras de rolagem 
.add_argument("--disable-translate")          = Desativa o tradutor automático 
.add_argument("--disable-web-security")       = Desativa políticas de CORS 
.add_argument("--memory-pressure-off")        = Impede de economizar memória
.add_argument("--no-default-browser-check")   = Desativa o "navegador padrão" 
.add_argument("--no-first-run")               = Evita tela de 1° execução 

#### Evitar detecção: 

.add_argument('--user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36') 
.add_argument("--disable-blink-features=AutomationControlled")  