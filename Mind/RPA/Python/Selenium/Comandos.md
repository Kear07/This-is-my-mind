---------------
### Inicialização

nav = webdriver.Chrome()                        = Inicia o navegador
nav.get(["URL"])                                = Inicia uma guia com a URL  

------
### Configurações de exibição

.fullscreen_window()                            = Expande para tela cheia  
.maximize_window()                              = Maximiza a tela 
.minimize_window()                              = Minimiza a tela 

.set_window_size([x],[y])                       = Define o tamanha da tela 

---------
### Alertas e Frames: 

.switch_to.alert                                = Altera o foco para um alerta 

.alerta.accept()                                = Confirma o alerta (OK) 
.alerta.dismiss()                               = Cancela o alerta 

.alerta.text                                    = Captura o texto do alerta

.switch_to.frame([loc])                         = Altera o foco para um iframe 

.switch_to.default_content()                    = Volta para a tela principal 

.switch_to.window(nav.window_handles[x])        = Alterna entre abas 

-------
### Funções comuns:

.find_element(["type"], ["name"])       = Encontra um único elemento 
.find_elements(["type"], ["name"])      = Encontra uma lista de elementos 

.click()                                = Clica em um elemento 
.send_keys([text])                      = Digita texto dentro do elemento 
.clear()                                = Apaga o texto do campo 

.title                                  = Retorna o título da página 
.current_url                            = Retorna a URL atual 
.page_source                            = Retorna o código-fonte 

.back()                                 = Volta para a página anterior 
.forward()                              = Avança para página seguinte 
.refresh()                              = Recarrega a página 
.close()                                = Encerra a aba ativa
.quit()                                 = Encerra o navegador 

---------
### Seletores:

.find_element("id", [meuId]) 
.find_element("name", [meuNome]) 
.find_element("xpath", "//*[@id='meuElemento']") 
.find_element("link text", [Clique Aqui]) 
.find_element("partial link text", [Clique]) 
.find_element("tag name", [input]) 
.find_element("class name", [minhaClasse]) 
.find_element("css selector", [.minhaClasse]) 

#### Dica para XPATH:

1°: Encontre a tag do elemento que procura
2°: Pegue a class, id, qualquer coisa dentro do elemento
3°: No inspecionar pressione "ctrl" + "f"
4°: Digite "//[tag [@class='...']]"
5°: Pronto, agora temos um seletor XPATH

----

### Shadow Root

https://cosmocode.io/how-to-interact-with-shadow-dom-in-selenium/ 