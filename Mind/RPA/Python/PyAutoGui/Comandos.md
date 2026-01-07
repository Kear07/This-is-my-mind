
---
### Comandos: 

.hotkey(['key'], ['key'])                 = Pressiona diversas teclas  
.press(['key'])                           = Pressiona uma tecla 

.write(['text'])                          = Digita um texto 
.write(['text'], interval = 0.25 )        = Digita um texto com um intervalo

.keyDown(['key'])                         = Mantém uma tecla pressionada 
.keyUp(['key'])                           = Solta a tecla  

.scroll([200])                            = Rola a tela para cima
.scroll([-200])                           = Rola a tela para baixo

.click()                                  = Clica onde estiver o mouse 
.click([x],[y])                           = Clica no local 

.doubleClick()                            = Clica 2 vezes 
.doubleClick([x],[y])                     = Clica 2 vezes na posição 

.tripleClick()                            = Clica três vezes no local  
.tripleClick([x],[y])                     = Clica três vezes no local  

.rightClick()                             = Clica com o botão direito 
.rightClick([x],[y])                      = Clica com o botão direito no local 

.middleClick()                            = Clica com o botão do meio 
.middleClick([x],[y])                     = Clica com o botão do meio no local

.moveTo([x],[y])                          = Move o mouse para o local 
.moveTo([x],[y], duration=2)              = Move o mouse com duração de tempo

.position()                               = Retorna o local do mouse 
.onScreen([x],[y])                        = Retorna se estiver no local 
  
.screenshot([img].png)                    = Printa e salva o em img.png 
.screenshot(region=([x],[y],[w],[h]))     = Printa com especificações

---
